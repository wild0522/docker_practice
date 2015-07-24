## 簡介
Compose 項目目前在 [Github](https://github.com/docker/compose) 上進行維護，目前最新版本是 1.2.0。

Compose 定位是「defining and running complex applications with Docker」，前身是 Fig，兼容 Fig 的模板文件。

Dockerfile 可以讓用戶管理一個單獨的應用容器；而 Compose 則允許用戶在一個模板（YAML 格式）中定義一組相關聯的應用容器（被稱為一個 `project`，即項目），例如一個 Web 服務容器再加上後端的數據庫服務容器等。

![](../_images/docker_compose.jpg)

該項目由 Python 編寫，實際上調用了 Docker 提供的 API 來實現。
