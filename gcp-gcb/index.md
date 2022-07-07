# Google Cloud Platform (GCP) 百科全書  - Cloud Build [ EP.5 ]


本篇是我們進入 GCP 的第五篇文章，詳細的文章列表大家可以到[這一篇查看](https://pin-yi.me/gcp-introduce/) ～ 跟大家介紹一下今天的主題 Cloud Build，Cloud Build 可以幫我們做持續建構、測試和部署，我們可以把它想成簡易版的 Jenkins，從整個映像檔案打包到部署，也就幾分鐘的事情，且內建許多指令。

<br>

我們今天文章，需要使用前幾天提到的 [Cloud Source Repositories](https://pin-yi.me/gcp-gcsr/) 、[Compute Engine](https://pin-yi.me/gcp-gce/)、[Container Registry](https://pin-yi.me/gcp-gcr/)，我們需要先透過 GitLab 將程式鏡像到 Cloud Source Repositories，再透過 Cloud Build 觸發將 GitLab 上面的 Dockerfile 建置到 Container Registry 中，再部署到 Compute Engine VM 上。大家可以參考流程圖，會更清楚今天的流程！那我們就開始囉 🥸

<br>

{{< image src="/images/gcp-gcb/1.png"  width="1200" caption="流程圖" src_s="/images/gcp-gcb/1.png" src_l="/images/gcp-gcb/1.png" >}}

<br>		


## Cloud Source Repositories 測試

前面 GitLab 鏡像設定，請先參考上一篇 [Google Cloud Platform (GCP) 百科全書 - Cloud Source Repositories [ EP.3 ]](https://pin-yi.me/gcp-gcsr/)，上一篇會帶大家從 GitLab 鏡像到 Cloud Source Repositories，所以我們就接續上一篇的內容，繼續往下開始學習吧～

<br>

1. 開啟 GCP，選擇左側的 menu > 點擊 **Cloud Build** > 選擇 **觸發條件**，點擊 **建立觸發條件** 按鈕。

<br>

2. 輸入觸發條件的名稱，事件可以設定我們要怎麼進行觸發，我們這邊先選擇 **推送至分支的版本** 來觸發，在來源選擇上一篇建立好的 Cloud Source Repositories 存放區，分支版本我們先使用預設的 master，也就是推程式到 master 他會就觸發 Cloud Build：

<br>

{{< image src="/images/gcp-gcb/2.png"  width="1000" caption="建立觸發條件 1" src_s="/images/gcp-gcb/2.png" src_l="/images/gcp-gcb/2.png" >}}

<br>

3. 設定類型我們選擇 Cloud Build 設定檔，他也是 Cloud Build 專用的設定檔，後面會帶大家寫一份 Cloud Build，位置當然是使用我們 Cloud Source Repositories 存放區，以及可以依照專案來修改 cloudbuild.yaml 放在專案的哪裡，最後都沒問題，就按下建立：

<br>

{{< image src="/images/gcp-gcb/3.png"  width="1000" caption="建立觸發條件 2" src_s="/images/gcp-gcb/3.png" src_l="/images/gcp-gcb/3.png" >}}

<br>

## 撰寫 cloudbuild.yaml 設定檔

在開始建立檔案前，先來跟大家說說檔案內有哪些設定吧：

<br>

首先 Cloud Build 建構器是裝有常用的程式語言和工具的容器映像。我們可以配置 Cloud Build，讓建構器中運行特定命令，我舉個例子讓大家了解：

以下程式碼是來自 [Docker Hub](https://hub.docker.com/search?q=&type=image) 的 `ubnutu` 映像檔中所執行的命令：

```
steps:
- name: 'ubuntu'
  args: ['echo', 'hello world']
```

可以看到我們配置文件中 `steps` 參數是指我們要建構的步驟， `name` 字段指定 Docker 映像檔的位置，以及 `args` 字段中是指定映像檔運行的命令。

我們的 Cloud Build 一樣會需要用 `name` 來指定建構容器的映像檔，以及使用 `args` 來執行我們映像檔所要運行的命令。

<br>

我們的 Cloud Build  設定檔中的 `name` 常用的建構器映像檔如下：

| Builder | 名稱 |
| :---: | :---: |
| bazel | gcr.io/cloud-builders/bazel |
| docker | gcr.io/cloud-builders/docker |
| git | gcr.io/cloud-builders/git |
| gcloud | gcr.io/cloud-builders/gcloud	 |
| gke-deploy | gcr.io/cloud-builders/gke-deploy |

<br>

接著我們來試著寫一個 cloudbuild.yaml 來建構我們的 nginx 服務，並部署到 GCE 上。

<br>

1. 我們先回到 Gitlab 該專案下的目錄，新增 cloudbuild.yaml 檔案，將複製以下內容：

```

```



<br>

## 參考資料


[Cloud Builder](https://cloud.google.com/build/docs/cloud-builders)

[Day27 - 用 Cloud Build 實作 CI 部分](https://ithelp.ithome.com.tw/articles/10224727)

