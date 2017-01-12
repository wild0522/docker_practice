## Compose 簡介

![Docker Compose 項目](../_images/docker_compose.jpg)

Compose 項目是 Docker 官方的開源項目，負責實作對 Docker 容器叢集的快速編排。從功能上看，跟 OpenStack 中的 Heat 十分類似。

其程式碼目前在 [https://github.com/docker/compose](https://github.com/docker/compose) 上開源。

Compose 定位是 「定義和執行多個 Docker 容器的應用（Defining and running multi-container Docker applications）」，其前身是開源項目 Fig，目前仍然相容 Fig 格式的 範本檔案。

透過第一部分中的介紹，我們知道使用一個 Dockerfile  範本檔案，可以讓使用者很方便的定義一個單獨的應用容器。然而，在日常工作中，經常會碰到需要多個容器相互配合來完成某項工作的情況。例如要實作一個 Web 項目，除了 Web 服務容器本身，往往還需要再加上後端的資料庫服務容器，甚至還包括負載均衡容器等。

Compose 恰好滿足了這樣的需求。它允許使用者透過一個單獨的 `docker-compose.yml`  範本檔案（YAML 格式）來定義一組相關聯的應用容器為一個項目（project）。

Compose 中有兩個重要的概念：

* 服務（service）：一個應用的容器，實際上可以包括若干執行相同映像檔的容器實例。
* 項目(project)：由一組關聯的應用容器組成的一個完整業務單元，在 `docker-compose.yml` 檔案中定義。

Compose 的 預設管理對象是項目，透過子指令對項目中的一組容器進行便捷地生命週期管理。

Compose 項目由 Python 編輯，實作上叫用了 Docker 服務提供的 API 來對容器進行管理。因此，只要所作業的平台支援 Docker API，就可以在其上利用 Compose 來進行編排管理。
