## 核心能力機制

能力機制（Capability）是 Linux 核心一個強大的內容屬性，可以提供細粒度的權限存取控制。
Linux 核心自 2.2 版本起就支援能力機制，它將權限劃分為更加細粒度的作業能力，既可以作用在處理序上，也可以作用在檔案上。

例如，一個 Web 服務處理序只需要繫結一個低於 1024 的連接埠的權限，並不需要 root 權限。那麼它只需要被授權 `net_bind_service` 能力即可。此外，還有很多其它的類似能力來避免處理序取得 root 權限。

 預設情況下，Docker 啟動的容器被嚴格限制只允許使用核心的一部分能力。

使用能力機制對加強 Docker 容器的安全有很多好處。通常，在伺服器上會執行一堆需要特權權限的處理序，包括有 ssh、cron、syslogd、硬體管理工具模組（例如負載模組）、網路設定工具等等。容器跟這些處理序是不同的，因為幾乎所有的特權處理序都由容器以外的支援系統來進行管理。
* ssh 存取被主電腦上ssh服務來管理；
* cron 通常應該作為使用者處理序執行，權限交給使用它服務的應用來處理；
* 日誌系統可由 Docker 或第三方服務管理；
* 硬體管理無關緊要，容器中也就無需執行 udevd 以及類似服務；
* 網路管理也都在主電腦上設定，除非特殊需求，容器不需要對網路進行設定。

從上面的例子可以看出，大部分情況下，容器並不需要「真正的」 root 權限，容器只需要少數的能力即可。為了加強安全，容器可以禁用一些沒必要的權限。
* 完全禁止任何 mount 作業；
* 禁止直接存取本地主電腦的通訊端；
* 禁止存取一些檔案系統的作業，比如建立新的裝置、修改檔案屬性等；
* 禁止模組載入。

這樣，就算攻擊者在容器中取得了 root 權限，也不能獲得本地主電腦的較高權限，能進行的破壞也有限。

 預設情況下，Docker採用 [白名單](https://github.com/docker/docker/blob/master/daemon/execdriver/native/template/default_template.go) 機制，禁用 [必需功能](https://github.com/docker/docker/blob/master/daemon/execdriver/native/template/default_template.go) 之外的其它權限。
當然，使用者也可以根據自身需求來為 Docker 容器啟動額外的權限。
