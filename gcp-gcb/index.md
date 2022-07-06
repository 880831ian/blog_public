# Google Cloud Platform (GCP) 百科全書  - Cloud Build [ EP.4 ]


本篇是我們進入 GCP 的第四篇文章，詳細的文章列表大家可以到[這一篇查看](https://pin-yi.me/gcp-introduce/) ～ 跟大家介紹一下今天的主題 Cloud Build，Cloud Build 可以幫我們做持續建構、測試和部署，我們可以把它想成簡易版的 Jenkins，從整個映像檔案打包到部署，也就幾分鐘的事情，且內建許多指令。

<br>

我們今天文章，需要使用前幾天提到的 [Cloud Source Repositories](https://pin-yi.me/gcp-gcsr/) 以及 [Compute Engine](https://pin-yi.me/gcp-gce/)，我們需要先透過 GitLab 將程式鏡像到 Cloud Source Repositories，再透過 Cloud Build 觸發部署到 Compute Engine VM 上。大家可以參考一下流程圖，會更清楚今天要做得呦！那我們就開始囉 🥸

<br>

{{< image src="/images/gcp-gcb/1.png"  width="1200" caption="流程圖" src_s="/images/gcp-gcb/1.png" src_l="/images/gcp-gcb/1.png" >}}

<br>		

## 官方提供內建的指令

我們整理一些比較常用的指令，其他的可以再去官網查看：

* gcloud
* docker
* kubectl
* gsutil
* git
* curl
* mvn
* wget
* npm

<br>

## 參考資料

[Day27 - 用 Cloud Build 實作 CI 部分](https://ithelp.ithome.com.tw/articles/10224727)

