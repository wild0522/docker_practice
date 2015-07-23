# 項目簡介

![](../_images/kubernetes_logo.svg)

Kubernetes 是 Google 團隊發起的開源項目，它的目標是管理跨多個主機的容器，提供基本的部署，維護以及運用伸縮，主要實現語言為Go語言。Kubernetes是：
* 易學：輕量級，簡單，容易理解
* 便攜：支持公有雲，私有雲，混合雲，以及多種雲平台
* 可拓展：模塊化，可插拔，支持鉤子，可任意組合
* 自修復：自動重調度，自動重啟，自動複製

Kubernetes構建於Google數十年經驗，一大半來源於Google生產環境規模的經驗。結合了社區最佳的想法和實踐。

在分佈式系統中，部署，調度，伸縮一直是最為重要的也最為基礎的功能。Kubernets就是希望解決這一序列問題的。

Kubernets 目前在[github.com/GoogleCloudPlatform/kubernetes](https://github.com/GoogleCloudPlatform/kubernetes)進行維護，截至定稿最新版本為 0.7.2 版本。

### Kubernetes 能夠運行在任何地方！

雖然Kubernets最初是為GCE定制的，但是在後續版本中陸續增加了其他雲平台的支持，以及本地數據中心的支持。
