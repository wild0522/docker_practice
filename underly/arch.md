## 基本架構
Docker 採用了 C/S架構，包括用戶端和服務端。
Docker daemon 作為服務端接受來自客戶的請求，並處理這些請求（建立、執行、分發容器）。
用戶端和服務端既可以執行在一個機器上，也可透過 socket 或是 RESTful API 來進行通信。

![Docker 基本架構](../_images/docker_arch.png)


Docker daemon 一般在宿主主電腦後台執行，等待接收來自用戶端的信息。
Docker 用戶端則為使用者提供一系列可執行指令，使用者用這些指令實作跟 Docker daemon 互動。
