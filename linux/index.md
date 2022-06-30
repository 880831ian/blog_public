# Linux 常用指令


因為最近在管理機器時，常常會使用各式各樣的指令來協助管理，所以把常用的指令依照不同類別整理在底下呦 😘 會定期更新該文章呦 ~

<br>

## 網路類

### 查詢遠端 Port 是否開放

#### nc (netcat)

nc (netcat)，可以讀取經過 TCP 及 UDP 的網路連線資料，是一套很實用的網路除錯工具。

<br>

安裝指令：

```
yum install nc
```

<br>

檢查 Port 是否有開放，可以用以下指令來查詢：

```
nc -zvw3 <IP:Port>
```
`-z` 只進行掃描，不進行任何的資料傳輸

`-v` 顯示掃描訊息

`-w3` 等待 3 秒

<br>

如果 Port 有開放，會回傳以下內容：

```
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Connected to <IP:Port>.
Ncat: 0 bytes sent, 0 bytes received in 0.01 seconds.
```

<br>

如果沒有開放，會回傳以下內容：

```
Ncat: No route to host.
```

<br>

#### nmap

nmap (Network Mapper) 是另一個可以檢查 Port 的工具，安裝語法是這樣：

```
yum install nmap
```

<br>

檢查 Port 是否有開放，可以用以下指令來查詢：

```
nmap <IP> -p <Port>
```

<br>

如果 Port 有開放，會回傳以下內容：

```
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-17 16:06 CST
Nmap scan report for<Domain> (<IP>)
Host is up (0.0039s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 1.09 seconds
```

<br>

如果沒有開放，會回傳以下內容：

```
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-17 16:06 CST
Nmap scan report for<Domain> (<IP>)
Host is up (0.0039s latency).

PORT   STATE SERVICE
80/tcp filtered  http

Nmap done: 1 IP address (1 host up) scanned in 1.09 seconds
```

<br>

#### Telnet

Telnet 也是一個可以檢查 Port 的工具，安裝語法是這樣：

```
yum install telnet
```

<br>

檢查 Port 是否有開放，可以用以下指令來查詢：

```
telnet <IP> <Port>
```

<br>

如果 Port 有開放，會回傳以下內容：

```
Trying <IP>…
Connected to <IP>.
Escape character is ‘^]’.
^CConnection closed by foreign host.
```

<br>

如果沒有開放，會回傳以下內容：

```
Trying <IP>…
telnet: Unable to connect to remote host: Connection refused
```

<br>

## 系統類

### 顯示當前進程狀態

#### ps

```
ps [參數]
```

* \-A 列出所有的進程
* \-w 可以加寬顯示較多的訊息
* \-au 顯示更詳細的訊息
* \-aux 顯示所有包含其他用戶的進程

<br>

au(x) 輸出的格式：

```
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
```

* USER：行程的擁有者
* PID：pid
* %CPU：佔用的 CPU 使用率
* %MEM：佔用的記憶體使用率
* VSZ：佔用的虛擬記憶體大小
* RSS：佔用的記憶體大小
* TTY：終端的次要裝置號碼
* STAT：該行程的狀態：
	* D：無法中斷的休眠狀態 (通常都是 IO 進程) 
	* R：正在執行中
	* S：靜止狀態
	* T：暫停執行
	* Z：不存在但暫時無法消除
	* W：沒有足夠的記憶體可以分配
	* <：高優先序的進程
	* N：低優先序的進程
* START：進程開始時間
* TIME：執行的時間
* COMMAND：所執行的指令

<br>

### 刪除執行中的進程

#### kill

```
kill [-s <訊息名稱或編號>] [程序] or [-l <訊息編號>]
``` 
* -l <訊息編號>：若不加 <訊息編號>選項，-l 會列出全部的訊息名稱
* -s <訊息名稱或編號>：指定要送出的訊息

<br>

最常用的訊息是

* 1 (HUP)：重新加載進程
* 9 (KILL)：刪除一個進程
* 15 (TERM)：正常停止一個進程

<br>

## 參考資料

[3 種檢查遠端埠號是否開啟的方法](https://www.ltsplus.com/linux/3-ways-check-remote-server-open-port)
