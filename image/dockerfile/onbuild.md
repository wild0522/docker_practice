### ONBUILD 為他人做嫁衣裳

格式：`ONBUILD <其它指令>`。

`ONBUILD` 是一個特殊的指令，它後面跟的是其它指令，比如 `RUN`, `COPY` 等，而這些指令，在目前映像檔建構時並不會被執行。只有當以目前映像檔為基礎映像檔，去建構下一級映像檔的時候才會被執行。

`Dockerfile` 中的其它指令都是為了訂製目前映像檔而準備的，唯有 `ONBUILD` 是為了說明別人訂製自己而準備的。

假設我們要製作 Node.js 所寫的應用的映像檔。我們都知道 Node.js 使用 `npm` 進行包管理，所有依賴、設定、啟動資訊等會放到 `package.json` 檔案裡。在拿到程式程式碼後，需要先進行 `npm install` 才可以獲得所有需要的依賴。然後就可以透過 `npm start` 來啟動應用。因此，一般來說會這樣寫 `Dockerfile`：

```Dockerfile
FROM node:slim
RUN "mkdir /app"
WORKDIR /app
COPY ./package.json /app
RUN [ "npm", "install" ]
COPY . /app/
CMD [ "npm", "start" ]
```

把這個 `Dockerfile` 放到 Node.js 項目的根目錄，建構好映像檔後，就可以直接拿來啟動容器執行。但是如果我們還有第二個 Node.js 項目也差不多呢？好吧，那就再把這個 `Dockerfile` 複製到第二個項目裡。那如果有第三個項目呢？再複製麼？檔案的副本越多，版本控制就越困難，讓我們繼續看這樣的場景維護的問題。

如果第一個 Node.js 項目在開發程序中，發現這個 `Dockerfile` 裡存在問題，比如敲錯字了、或是需要安裝額外的包，然後開發人員修復了這個 `Dockerfile`，再次建構，問題解決。第一個項目沒問題了，但是第二個項目呢？雖然最初 `Dockerfile` 是複製、貼上自第一個項目的，但是並不會因為第一個項目修復了他們的 `Dockerfile`，而第二個項目的 `Dockerfile` 就會被自動修復。

那麼我們可不可以做一個基礎映像檔，然後各個項目使用這個基礎映像檔呢？這樣基礎映像檔更新，各個項目不用同步 `Dockerfile` 的變化，重新建構後就繼承了基礎映像檔的更新？好吧，可以，讓我們看看這樣的結果。那麼上面的這個 `Dockerfile` 就會變為：

```Dockerfile
FROM node:slim
RUN "mkdir /app"
WORKDIR /app
CMD [ "npm", "start" ]
```

這裡我們把項目相關的建構指令拿出來，放到次基碼目裡去。假設這個基礎映像檔的名字為 `my-node` 的話，各個項目內的自己的 `Dockerfile` 就變為：

```Dockerfile
FROM my-node
COPY ./package.json /app
RUN [ "npm", "install" ]
COPY . /app/
```

基礎映像檔變化後，各個項目都用這個 `Dockerfile` 重新建構映像檔，會繼承基礎映像檔的更新。

那麼，問題解決了麼？沒有。準確說，只解決了一半。如果這個 `Dockerfile` 裡面有些東西需要調整呢？比如 `npm install` 都需要加一些參數，那怎麼辦？這一行 `RUN` 是不可能放入基礎映像檔的，因為涉及到了目前項目的 `./package.json`，難道又要一個個修改麼？所以說，這樣製作基礎映像檔，只解決了原來的 `Dockerfile` 的前4條指令的變化問題，而後面三條指令的變化則完全沒辦法處理。

`ONBUILD` 可以解決這個問題。讓我們用 `ONBUILD` 重新寫一下基礎映像檔的 `Dockerfile`:

```Dockerfile
FROM node:slim
RUN "mkdir /app"
WORKDIR /app
ONBUILD COPY ./package.json /app
ONBUILD RUN [ "npm", "install" ]
ONBUILD COPY . /app/
CMD [ "npm", "start" ]
```

這次我們回到原始的 `Dockerfile`，但是這次將項目相關的指令加上 `ONBUILD`，這樣在建構基礎映像檔的時候，這三行並不會被執行。然後各個項目的 `Dockerfile` 就變成了簡單地：

```Dockerfile
FROM my-node
```

是的，只有這麼一行。當在各個專案目錄中，用這個只有一行的 `Dockerfile` 建構映像檔時，之前基礎映像檔的那三行 `ONBUILD` 就會開始執行，成功的將目前項目的程式碼複製進映像檔、並且針對本項目執行 `npm install`，產生應用映像檔。
