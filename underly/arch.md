## 基本架構
Docker 採用了 C/S架構，包括客戶端和服務端。
Docker daemon 作為服務端接受來自客戶的請求，並處理這些請求（創建、運行、分發容器）。
客戶端和服務端既可以運行在一個機器上，也可通過 socket 或者 RESTful API 來進行通信。

![Docker 基本架構](../_images/docker_arch.png)


Docker daemon 一般在宿主主機後台運行，等待接收來自客戶端的消息。
Docker 客戶端則為用戶提供一系列可執行命令，用戶用這些命令實現跟 Docker daemon 交互。
