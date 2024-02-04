# 怎麼部署 React 應用程式到 GitHub Pages
<br> 
[English (original)](https://github.com/gitname/react-gh-pages)
<br>
\* 使用 `create-react-app` 進行

# 引言

在這個教學中，我會告訴你怎麼建立、部署一個 React  到 GitHub Pages 上。

我會使用 [`create-react-app`](https://create-react-app.dev/) 來建立一個 React 應用程式 。這個工具能夠幫我們從零開始打造一個 React 應用程式。至於部署的步驟，我會使用一個 npm 套件 [`gh-pages`](https://github.com/tschaub/gh-pages) 來部署應用程式到 GitHub提供的免費代管服務 GitHub Pages 上。

在跟著教學完成之後，你會得到一個由 Github Pages 代管的、可以再任你編輯的 React 應用程式。

# 教學

## 前置作業

1. 安裝好 [Node 以及 npm](https://nodejs.org/en/download/)，這個教學將會使用以下版本:

    ```shell
    $ node --version
    v16.13.2

    $ npm --version
    8.1.2
    ```
    
    >安裝 npm 會在你的系統中加入 `npm`、`npx` 指令，接下來我們會一直使用到它們。

2. 安裝好 [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)，這個教學將會使用以下版本:

    ```shell
    $ git --version
    git version 2.29.1.windows.1
    ```

3. 建立好自己的 [GitHub](https://github.com/signup) 帳號。 :octocat:

## 建立、部屬步驟

### 1. 在 GitHub 建立一個*空的*存放庫 

1. 登入你的 GitHub 帳號。
2. 前往 [Create a new repository](https://github.com/new)。
3. 依照下面的方式填入資料：
    - **存放庫名稱** 可以任意填入\*.
        > \*如果要建立的是 [專案網頁](https://pages.github.com/#project-site)，任何名稱都沒有問題，但如果你要建立[個人網頁](https://pages.github.com/#user-site)，GitHub [要求](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites) 存放庫名稱要以 `{username}.github.io` 的格式命名 (如 `gitname.github.io`)

        >這個名字會出現在： (1) GitHub 上提到這個存放庫的所有時機 、 (2) 連結到這個存放庫的網址 、 (3) 部署之後，連結到這個應用程式的網址。


        > 在這裡，我將以專頁網頁為例進行教學。

        我在這裡填入 `react-gh-pages`
        
   - **存放庫隱私設定** 選擇 _公開(Public)_ (或是 _不公開(Private)_\*).

        > \* 對 [免費使用](https://docs.github.com/en/get-started/learning-about-github/githubs-products#github-free-for-user-accounts) 帳號而言, 只有*公開*存放庫可以用來部署 GitHub Pages。但 [GitHub Pro](https://docs.github.com/en/get-started/learning-about-github/githubs-products#github-pro) 或是其他類型的付費使用者則可以自行選擇要*公開*存放庫還是*不公開*存放庫。

        這裡我選擇 _公開_

   - **初始化存放庫** 不必選取任何其他核取方塊。
        >這麼做才會讓 GitHub 才不會先幫你建立 `README.md`, `.gitignore`, 或是 `LICENSE`，而是建立一個空白、乾淨的的存放庫。
4. 按下建立。

現在，你已經設定好一個存放在 GitHub 上的空白存放庫了。

### 2. 建立一個 React 應用程式。

1. 建立一個叫做 `my-app` 的應用程式:
    >如果你想用 `my-app` 以外的名字，比如說 `web-ui`，就將以下提到 `my-app` 的所有地方替換成你選擇的名字就好 (如 `my-app` --> `web-ui`)

  
    ```shell
    $ npx create-react-app my-app
    ```
    >上面的指令會幫你建立一個 javascript 語言的 React 應用程式。如果你想要使用 [TypeScript](https://create-react-app.dev/docs/adding-typescript/#installation) 來建立 React 應用程式，就改為使用下面的指令：

    > ```shell
    > $ npx create-react-app my-app --template typescript
    > ```
    這兩個指令都會幫你建立一個叫 `my-app` 的資料夾，裡面包含一個 React 應用程式的原始碼。

    > 除了包含 React 應用程式的原始碼之外，該資料夾也是一個 Git 存放庫。在第 6 步中，我們將會利用這個特性。

    #### 分支名稱: `master` vs. `main`

    >Git存放庫將有一個被命名為(a) master，這是新安裝Git的預設；或者(b) 如果您的電腦運行的是Git 2.28或更高版本，並且你在Git配置中有設定 [`init.defaultBranch`](https://github.blog/2020-07-27-highlights-from-git-2-28/#introducing-init-defaultbranch) 這個變數的值，則分支將被命名為 init.defaultBranch 的值（如以 $ git config --global init.defaultBranch main 設定）。

    >
    >由於我沒有在我的 Git 安裝中設定該變數，因此我的存放庫中的分支將被命名為 `master`。如果您的存放庫中的分支有不同的名稱（可以通過執行 `$ git branch` 來檢查），例如 `main`，你可以**替換**本教程其餘部分中所有出現的 `master`，將其替換為其他名稱（例如 `master` → `main`）。

2. 進入剛剛建立好的資料夾 :
  
    ```shell
    $ cd my-app
    ```

到這個時候，你的電腦上已經有一個 React 應用程式，而您正在包含其原始碼的資料夾中。本教學中接下來的所有指令都能從該資料夾中執行。

### 3. 安裝 `gh-pages` npm 套件

1. 安裝 [`gh-pages`](https://github.com/tschaub/gh-pages) npm 套件 並將它指定成 [依賴套件](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file):
 
    ```shell
    $ npm install gh-pages --save-dev
    ```


到這個時候，`gh-pages` npm 套件已經安裝在你的電腦上，且 React 應用程式對它的依賴已經紀錄在 React 應用程式的 `package.json` 中。

### 4. 增加 `homepage` 屬性到 `package.json` 中

1. 在文字編輯器中打開 `package.json` 檔案。
   
    ```shell
    $ vi package.json
    ```

    > 這個教學將會使用 [vi](https://www.vim.org/) 文字編輯器。 但你也能使用任何你喜歡的編輯器。例如 [Visual Studio Code](https://code.visualstudio.com/)。

2. 以下面的格式新增 `homepage` 屬性
 \*: `https://{username}.github.io/{repo-name}`

  > \* 對於 [專案網頁](https://pages.github.com/#project-site)而言，你可以使用這樣的格式，但對 [使用者網頁](https://pages.github.com/#user-site)來說，你應該使用 `https://{username}.github.io`. 你可以在 `create-react-app` 文件中的["GitHub Pages" 章節](https://create-react-app.dev/docs/deployment/#github-pages) 了解更多關於 `homepage` 的資訊。

  ```diff
    {
      "name": "my-app",
      "version": "0.1.0",
  +   "homepage": "https://gitname.github.io/react-gh-pages",
      "private": true,
  ```

這步之後，React 應用程式中的 `package.json` 新增了一個 `homepage` 屬性。

### 5. 將開發腳本加入 `package.json` 中

1. 用編輯器打開 `package.json`
   
    ```shell
    $ vi package.json
    ```

2. 新增 `predeploy` (預部屬) 和 `deploy` (部屬)屬性到 `scripts` 物件底下:

    ```diff
    "scripts": {
    +   "predeploy": "npm run build",
    +   "deploy": "gh-pages -d build",
        "start": "react-scripts start",
        "build": "react-scripts build",
    ```

至此，React 應用程式中的 `package.json` 檔案中已經包含部署所需要的腳本了。

### 6. 新增一個指向 Github 存放庫的遠端存放庫

1. 在本地存放庫中加入 "[remote](https://git-scm.com/docs/git-remote)" 

    你可以使用下面的指令：
    
    ```shell
    $ git remote add origin https://github.com/{username}/{repo-name}.git
    ```
    
    如果要自訂該指令，將「{username}」替換為您的 GitHub 使用者名稱，並將「{repo-name}」替換為步驟 1 中建立的 GitHub 存放庫的名稱

    在我的範例中，我會使用：

    ```shell
    $ git remote add origin https://github.com/gitname/react-gh-pages.git
    ```

    > That command tells Git where I want it to push things whenever I—or the `gh-pages` npm package acting on my behalf—issue the `$ git push` command from within this local Git repository.
    >此指令告訴 git 我(或是 `gh-page` 套件的以我的身分執行時)要把檔案從本地存放庫推送到哪裡。

    至此，本地存放庫中有了一個指向步驟一中的存放庫的 `remote`

### 7. 將 React 應用程式推送到 Github 上

1. 將 React 應用程式推送到 Github 存放庫

    ```shell
    $ npm run deploy
    ```

    >這個指令會執行剛才在 `package.json` 中的預部屬、部屬指令。
    >
    > 在後台，`predeploy` 腳本將構建 React 應用程式的可分發版本，並將其儲存在名為 `build` 的資料夾中。然後，`deploy` 腳本會將該資料夾的內容推送到 GitHub 存放庫的 `gh-pages` 分支中。如果該分支尚不存在，則此指令會建立該分支。

    >預設情況下，`gh-pages` 中的提交中都帶有 `Updates` 的訊息。不過你也能以 `-m` 選項 [自訂提交訊息](https://github.com/gitname/react-gh-pages/issues/80#issuecomment-1042449820)。如：

    > ```shell
    > $ npm run deploy -- -m "Deploy React app to GitHub Pages"
    > ```


此時，GitHub 存放庫包含一個名為 `gh-pages` 的分支，其中包含構成 React 應用程式可分發版本的檔案。但是，我們仍需使 GitHub Pages 設定成可以 _供應_ 這些網站的狀態。

### 8. 設定 GitHub Pages

1. 前往 **GitHub Pages** 設定網站
   1. 前往你的 Github 存放庫
   2. 點擊檔案瀏覽器上面的設定(Settings)分頁
   3. 點擊 程式碼與自動化(Code and automation) 之下的 "Pages"
1. 將 "Build and deployment" 設定如下: 
   1. **Source**: 由分支部署(Deploy from a branch)
   2. **Branch**: 
      - Branch: `gh-pages`
      - Folder: `/ (根目錄)`
1. 點擊 儲存(Save) 按鈕

**完成!** React 應用程式成功部署到 GitHub Pages 上了！ :rocket:

至此，你的 React 應用程式已經可以被所有來到第四部中 `homepage` 頁面的人看見。舉例來說，我部署的應用程式可以在https://gitname.github.io/react-gh-pages 被看見。

### 9. (非必要)將應用程式的原始碼存放在 GitHub 上

在前一步中，`gh-pages` npm 套件將可分發的 React 應用程式版本推送到了存放庫中的 `gh-pages` 分支，不過，其 _原始碼_ 仍未被儲存在 GitHub 上。

在這一步中，我將教你把 React 應用程式原始碼儲存在 GitHub 上的方法。

1. 將擬在本教學中做出的所有修改提供到本地的 `master` 分支中；並將此分支推送到 GitHub 上的 `master` 分支。

    ```shell
    $ git add .
    $ git commit -m "Configure React app for deployment to GitHub Pages"
    $ git push origin master
    ```
    >我建議你在此時探索 GitHub 存放庫。它將會有兩個分支：`master` 和`gh-pages`。`master` 分支中包含 React 應用程式的原始碼；而 `gh-pages` 分支將包含 React 應用程式的可分發版本。

# 參考資料

1. [官方的 `create-react-app` 部署指引](https://create-react-app.dev/docs/deployment/#github-pages)
2. [GitHub blog: 從任何分支建立並部署 GitHub Pages](https://github.blog/changelog/2020-09-03-build-and-deploy-github-pages-from-any-branch/)
3. [如何使用自訂網域且留下 `CNAME` 檔案](https://github.com/gitname/react-gh-pages/issues/89#issuecomment-1207271670)

# 後記

-特別感謝 GitHub (the company) 給了我們 GitHub Pages 這個免費的網頁代管平台
- 現在是時候將`create-react-app` 為我們創建的預設網頁變成獨特的樣子了！
- 此存放庫中有兩個分支: 
    - `master` - React 應用程式的原始碼
    - `gh-pages` - 由原始碼 _建立_ 的 React 應用程式

 # 貢獻者們

謝謝這些為此存放庫貢獻的人們

<!--

Template:
---------

<a href="https://github.com/____" target="_blank" title="____">
  <img src="https://github.com/____.png?size=40" height="40" width="40" alt="____" />
</a>

Instructions:
-------------

1. Copy the template and paste it below.
2. Replace the four "____" strings with the contributor's GitHub username.

Note: I specified the avatars using HTML because, when I did so using Markdown,
      only the _custom_ avatars appeared at the size I specified via the URL
      (e.g. 40px squared, for `https://github.com/gitname.png?size=40`);
      the GitHub-generated avatars seemed to ignore the size parameter and,
      instead, appear at their full size (approximately 420px squared).
      By using HTML, I can force _both_ types to appear at 40px squared.

-->

<a href="https://github.com/gitname" target="_blank" title="gitname">
  <img src="https://github.com/gitname.png?size=40" height="40" width="40" alt="gitname" />
</a>
<a href="https://github.com/rhulse" target="_blank" title="rhulse">
  <img src="https://github.com/rhulse.png?size=40" height="40" width="40" alt="rhulse" />
</a>
<a href="https://github.com/AbhishekCode" target="_blank" title="AbhishekCode">
  <img src="https://github.com/AbhishekCode.png?size=40" height="40" width="40" alt="AbhishekCode" />
</a>
<a href="https://github.com/adnjoo" target="_blank" title="adnjoo">
  <img src="https://github.com/adnjoo.png?size=40" height="40" width="40" alt="adnjoo" />
</a>
<a href="https://github.com/thebeatlesphan" target="_blank" title="thebeatlesphan">
  <img src="https://github.com/thebeatlesphan.png?size=40" height="40" width="40" alt="thebeatlesphan" />
</a>
<a href="https://github.com/valerio-pescatori" target="_blank" title="valerio-pescatori">
  <img src="https://github.com/valerio-pescatori.png?size=40" height="40" width="40" alt="valerio-pescatori" />
</a>
<a href="https://github.com/jackweyhrich" target="_blank" title="jackweyhrich">
  <img src="https://github.com/jackweyhrich.png?size=40" height="40" width="40" alt="jackweyhrich" />
</a>

This list is maintained manually—for now—and includes (a) each person who submitted a pull request that was eventually merged into `master`, and (b) each person who contributed in a different way (e.g. providing constructive feedback) and who approved of me including them in this list.
這個清單是手動維護的——目前為止——他們做出了以下貢獻： （a） 提交成功被合併到 `master` 中的 Pull Request 的每個人，以及 （b） 以其他方式做出貢獻（例如提供建設性建議）並願意讓我將他們包含在此清單中的每個人。