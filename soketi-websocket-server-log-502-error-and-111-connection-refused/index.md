# Soketi WebSocket Server LOG 不定時出現 502 error 以及 connect() failed (111: Connection refused)


此文章要來記錄一下 RD 同仁前陣子有反應使用 Soketi 這個 WebSocket Server 會不定時在 LOG 出現 502 error 錯誤訊息以及 connect() failed (111: Connection refused) while connecting to upstream，雖然說服務使用上不會影響很大，但還是希望我們可以協助找出 502 的原因。

<br>

{{< image src="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/1.png"  width="950" caption="出錯的 LOG" src_s="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/1.png" src_l="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/1.png" >}}

<br>

在開始找問題前，先簡單介紹一下 Soketi 是什麼東西好了，根據官網的說明，他是簡單、快速且有彈性的開源 WebSockets server，想要了解更多的可以到它[官網](https://docs.soketi.app/)去查看。

<br>

## 解決過程

我們可以看到上方錯誤 LOG 中，發現有出現 502 error 以及 connect() failed (111: Connection refused) while connecting to upstream，這兩個錯誤都是由 Nginx 所產生的，那我們先來理解一下，Nginx 與 Soketi 之間的關係。

在使用上，RD 的程式會打 Soketi 專用的 Subdomain 來使用這個 WebSocket 服務，而在我們的架構上，這個 Subdomain 會經過用 nginx proxy server，來轉發到 Soketi WebSocket Server (走 k8s svc)，設定檔如下圖：

<br>

{{< image src="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/2.png"  width="700" caption="入口 nginx 設定" src_s="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/2.png" src_l="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/2.png" >}}

<br>

然後會出現 `connect() failed (111: Connection refused) while connecting to upstream` 的錯誤訊息，代表我們的 Nginx 設定少了一個重要的一行設定，就是 `proxy_http_version 1.1;`，這個設定要讓 Nginx 作為 proxy 可以和 upstream 的後端服務也是用 keepalive，必須使用 http 1.1，但如果沒有設定預設是 1.0，也要記得設定 `proxy_set_header Upgrade`、`proxy_set_header Connection`。調整過後就變成：

<br>

```
server {
  server_name socket.XXX.com;
  listen 80 ;
  listen [::]:80 ;
  listen 443 ssl;
  listen [::]:443 ssl;
  ssl_certificate /etc/nginx/ingress.gcp.cert;
  ssl_certificate_key /etc/nginx/ingress.gcp.key;
  access_log /var/log/nginx/access.log main;
  location / {
    proxy_pass http://soketi-ws-ci:6001;
    proxy_connect_timeout 10s;
    proxy_read_timeout 1800s;

    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header X-Real-IP $remote_addr;
  }
}
```

<br>

解決完 `connect() failed (111: Connection refused)` 這個問題後，接下來就是要解決 `502 error` 這個問題，會導致 502 代表 Nginx 這個 proxy server 連不上後端的 Soketi WebSocket Server，再觀察 LOG 以及測試後發現，當 Pod 自動重啟，或是手動重啟 Deployment 的時候，就會有 502 的錯誤，代表 Nginx 在 proxy 到後面的 Soketi svc 再到 Pod 的時候，有一段時間是連不上的，所以就會出現 502 的錯誤，可以推測是流量進到正在關閉的 Pod 或是進到還沒有啟動好的 Pod 才導致的。

那我們先來看一下 Soketi WebSocket Server 的服務 yaml 檔案：

<br>

```yaml
    spec:
      terminationGracePeriodSeconds: 30
      securityContext: {}
      containers:
        - name: soketi
          securityContext: {}
          image: "quay.io/soketi/soketi::1.6.0-16-alpine"

          ... 省略 (可以到 github 看 code)...

          livenessProbe:
            failureThreshold: 3
            httpGet:
              httpHeaders:
                - name: X-Kube-Healthcheck
                  value: "Yes"
              path: /
              port: 6001
            initialDelaySeconds: 5
            periodSeconds: 2
            successThreshold: 1
```

<br>

可以看到原來的設定只有 `livenessProbe` 而已，因此我們為了要避免流量進到正在關閉的 Pod 或是進到還沒有啟動好的 Pod，所以我們需要加上 `readinessProbe` 以及 `preStop`，讓 Pod 確定啟動完畢，或是等待 Service 的 endpoint list 中移除 Pod，才開始接收流量，這樣就可以避免出現 502 的錯誤。

<br>

```yaml
    spec:
      terminationGracePeriodSeconds: 30
      securityContext: {}
      containers:
        - name: soketi
          securityContext: {}
          image: "quay.io/soketi/soketi::1.6.0-16-alpine"

          ... 省略 (可以到 github 看 code)...

          livenessProbe:
            failureThreshold: 3
            httpGet:
              httpHeaders:
                - name: X-Kube-Healthcheck
                  value: "Yes"
              path: /
              port: 6001
            initialDelaySeconds: 5
            periodSeconds: 2
            successThreshold: 1
          readinessProbe:
            failureThreshold: 3
            httpGet:
              httpHeaders:
                - name: X-Kube-Healthcheck
                  value: "Yes"
              path: /ready
              port: 6001
            initialDelaySeconds: 5
            periodSeconds: 2
            successThreshold: 1
          lifecycle:
            preStop:
              exec:
                command: ["/bin/sh", "-c", "sleep 20"]
```

<br>

{{< image src="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/3.png"  width="600" caption="Pod 終止的過程" src_s="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/3.png" src_l="/images/soketi-websocket-server-log-502-error-and-111-connection-refused/3.png" >}}

<br>

## 參考

[[Nginx] 解決 connect() failed (111: Connection refused) while connecting to upstream](https://wshs0713.github.io/posts/8c1276a7/)

[WebSocket proxying](http://nginx.org/en/docs/http/websocket.html)

[day 10 Pod(3)-生命週期, 容器探測](https://ithelp.ithome.com.tw/articles/10236314)
