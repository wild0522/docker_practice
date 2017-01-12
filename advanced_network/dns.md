## 設定 DNS
Docker 沒有為每個容器專門訂製映像檔，那麼怎麼自訂設定容器的主電腦名稱和 DNS 設定呢？
秘訣就是它利用虛擬檔案來載入到來容器的 3 個相關設定檔。

在容器中使用 mount 指令可以看到載入資訊：
```
$ mount
...
/dev/disk/by-uuid/1fec...ebdf on /etc/hostname type ext4 ...
/dev/disk/by-uuid/1fec...ebdf on /etc/hosts type ext4 ...
tmpfs on /etc/resolv.conf type tmpfs ...
...
```
這種機制可以讓宿主主電腦 DNS 資訊發生更新後，所有 Docker 容器的 dns 設定透過 `/etc/resolv.conf` 檔案立刻得到更新。

如果使用者想要手動指定容器的設定，可以利用下面的選項。

`-h HOSTNAME or --hostname=HOSTNAME`
設定容器的主電腦名稱，它會被寫到容器內的 `/etc/hostname` 和 `/etc/hosts`。但它在容器外部看不到，既不會在 `docker ps` 中顯示，也不會在其它的容器的 `/etc/hosts` 看到。

`--link=CONTAINER_NAME:ALIAS`
選項會在建立容器的時候，加入一個其它容器的主電腦名稱到 `/etc/hosts` 檔案中，讓新容器的處理序可以使用主電腦名稱 ALIAS 就可以連線它。

`--dns=IP_ADDRESS`
加入 DNS 伺服器到容器的 `/etc/resolv.conf` 中，讓容器用這個伺服器來解析所有不在 `/etc/hosts` 中的主電腦名稱。

`--dns-search=DOMAIN`
設定容器的搜尋網域，當設定搜尋網域為 `.example.com` 時，在搜尋一個名為 host 的主電腦時，DNS 不僅搜尋host，還會搜尋 `host.example.com`。
注意：如果沒有上述最後 2 個選項，Docker 會 預設用主電腦上的 `/etc/resolv.conf` 來設定容器。
