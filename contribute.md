## 如何貢獻項目

* 在 GitHub 上 `fork` 到自己的倉庫，如 `docker_user/docker_practice`，然後 `clone` 到本地，並設定使用者資訊。

  ```sh
  $ git clone git@github.com:docker_user/docker_practice.git
  $ cd docker_practice
  $ git config user.name "yourname"
  $ git config user.email "your email"
  ```

* 修改程式碼後送出，並推送到自己的倉庫。

  ```sh
  $ #do some change on the content
  $ git commit -am "Fix issue #1: change helo to hello"
  $ git push
  ```

* 在 GitHub 網站上送出 pull request。

* 定期使用項目倉庫內容更新自己倉庫內容。
  ```sh
  $ git remote add upstream https://github.com/yeasy/docker_practice
  $ git fetch upstream
  $ git checkout master
  $ git rebase upstream/master
  $ git push -f origin master
  ```

