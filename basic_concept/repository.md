## Docker Registry

映像檔建構完成後，可以很容易的在目前宿主上執行，但是，如果需要在其它伺服器上使用這個映像檔，我們就需要一個集中的儲存、分發映像檔的服務，[Docker Registry](https://docs.docker.com/registry/) 就是這樣的服務。

一個 **Docker Registry** 中可以包含多個**倉庫**（Repository）；每個倉庫可以包含多個**標籤**（Tag）；每個標籤對應一個映像檔。

一般而言，一個倉庫包含的是同一個軟體的不同版本的映像檔，而標籤則用於對應於軟體的的不同版本。我們可以透過 `<倉庫名>:<標籤>` 的格式來指定具體是哪個版本的映像檔。如果不給出標籤，將以 `latest` 作為 預設標籤。

以 [Ubuntu 映像檔](https://hub.docker.com/_/ubuntu/) 為例，`ubuntu` 是倉庫的名字，其內包含有不同的版本標籤，如，`14.04`, `16.04`。我們可以透過 `ubuntu:14.04`，或是 `ubuntu:16.04` 來具體指定所需哪個版本的映像檔。如果忽略了標籤，比如 `ubuntu`，那將視為 `ubuntu:latest`。

倉庫名經常以 *兩段式路徑* 形式出現，比如 `jwilder/nginx-proxy`，前者往往意味著 Docker Registry 多使用者環境下的使用者名稱，後者則往往是對應的軟體名。但這並非絕對，取決於所使用的具體 Docker Registry 的軟體或服務。

### Docker Registry 公開服務

Docker Registry 公開服務是開放給使用者使用、允許使用者管理映像檔的 Registry 服務。一般這類公開服務允許使用者免費上傳、下載公開的映像檔，並可能提供收費服務供使用者管理私有映像檔。

最常使用的 Registry 公開服務是官方的 [Docker Hub](https://hub.docker.com/)，這也是 預設的 Registry，並擁有大量的高品質的官方映像檔。除此以外，還有 [CoreOS](https://coreos.com/) 的 [Quay.io](https://quay.io/repository/)，CoreOS 相關的映像檔儲存在這裡；Google 的 [Google Container Registry](https://cloud.google.com/container-registry/)，[Kubernetes](http://kubernetes.io/) 的映像檔使用的就是這個服務。

由於某些原因，在國內存取這些服務可能會比較慢。國內的一些雲服務商提供了針對 Docker Hub 的映像檔服務（Registry Mirror），這些映像檔服務被稱為**加速器**。常見的有 [阿里雲加速器](https://cr.console.aliyun.com/#/accelerator)、[DaoCloud 加速器](https://www.daocloud.io/mirror#accelerator-doc)、[靈雀雲加速器](http://docs.alauda.cn/feature/accelerator.html)等。使用加速器會直接從國內的位址下載 Docker Hub 的映像檔，比直接從官方網站下載速度會提高很多。在後面的章節中會有進一步如何設定加速器的講解。

國內也有一些雲服務商提供類似於 Docker Hub 的公開服務。比如 [時速雲映像檔倉庫](https://hub.tenxcloud.com/)、[網易雲映像檔服務](https://c.163.com/hub#/m/library/)、[DaoCloud 映像檔市場](https://hub.daocloud.io/)、[阿里雲映像檔庫](https://cr.console.aliyun.com)等。

### 私有 Docker Registry

除了使用公開服務外，使用者還可以在本地搭建私有 Docker Registry。Docker 官方提供了 [Docker Registry 映像檔](https://hub.docker.com/_/registry/)，可以直接使用做為私有 Registry 服務。在後續的相關章節中，會有進一步的搭建私有 Registry 服務的講解。

開源的 Docker Registry 映像檔只提供了 [Docker Registry API](https://docs.docker.com/registry/spec/api/) 的服務端實作，足以支援 `docker` 指令，不影響使用。但不包含圖形界面，以及映像檔維護、使用者管理、存取控制等進階功能。在官方的商業化版本 [Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/2.0/) 中，提供了這些進階功能。

除了官方的 Docker Registry 外，還有第三方軟體實作了 Docker Registry API，甚至提供了使用者界面以及一些進階功能。比如，[VMWare Harbor](http://vmware.github.io/harbor/index_cn.html) 和 [Sonatype Nexus](https://www.sonatype.com/docker)。
