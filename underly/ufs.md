## 聯合檔案系統
聯合檔案系統（[UnionFS](http://en.wikipedia.org/wiki/UnionFS)）是一種分層、輕量級並且高效能的檔案系統，它支援對檔案系統的修改作為一次送出來一層層的疊加，同時可以將不同目錄載入到同一個虛擬檔案系統下(unite several directories into a single virtual filesystem)。

聯合檔案系統是 Docker 映像檔的基礎。映像檔可以透過分層來進行繼承，基於基礎映像檔（沒有父映像檔），可以製作各種具體的應用映像檔。

另外，不同 Docker 容器就可以共享一些基礎的檔案系統層，同時再加上自己獨有的改動層，大大提高了儲存的效率。

Docker 中使用的 AUFS（AnotherUnionFS）就是一種聯合檔案系統。 AUFS 支援為每一個成員目錄（類似 Git 的分支）設定唯讀（readonly）、讀寫（readwrite）和寫出（whiteout-able）權限, 同時 AUFS 裡有一個類似分層的概念, 對唯讀權限的分支可以邏輯上進行增量地修改(不影響唯讀部分的)。

Docker 目前支援的聯合檔案系統種類包括 AUFS, btrfs, vfs 和 DeviceMapper。
