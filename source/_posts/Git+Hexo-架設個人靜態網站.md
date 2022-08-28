---
title: Git+Hexo-架設個人靜態網站
tags:
  - web
  - hexo
categories:
  - website網頁設計
date: 2020-02-10
---

categories: `website網頁設計`
tags: `web` `hexo`


### Git+Hexo-架設個人靜態網站-前言
因為心血來潮，想說是時候該架設自己的網站了，可以記錄自己一些學習心得，在建立環境的時候也是反反覆覆重建了好多次嗚嗚，當然最後還是順利的建完了。

接下來就開始介紹 hexo 的基本安裝，並且架設在自己的github上。

環境：mscOS mojave (當然windows也是可以)

<!--more-->

### Github repo設定 & Hexo搭建

設置 github page 方式，以 `<user_name>.github.io` 為專案名稱，利用 branch 建構，這樣生成的網址將會是 `https://<user_name>.github.io/<repo_name>` 

### Step 1: 建立repository

在個人的 github 頁面上新增一個 repository (此處我就使用 blog 作為專案名稱)，不需建立 README.md


{% if 1 == 1 %}
    {% asset_img step1_git.png step1_git %}
{% else %}
    ![step1_git](https://i.imgur.com/tj82ztP.png)
{% endif %}


### Step 2: 安裝套件(node.js/npm/nvm)
還有很多重要的事情，中間可能會遇到一些問題，接下來的部分我就簡單介紹一下，上網找一下應該都會有很多解決方式。

Hexo 是一個用 Node.js 所寫成的部落格框架，因此在安裝前必須先確定我們是否有 Node.js 套件管理工具 npm ( 本文使用 )

我們可以從 [Node.js官網](https://nodejs.org/en/?source=post_page-----317beefdf182----------------------) 下載安裝，安裝完後至終端機輸入
```bash
$ npm -v
```
輸出版本後便表示安裝已完成

### Step 3: 安裝HEXO
```bash
$ npm install -g hexo-cli
# 安裝 hexo

$ hexo init <repo_name>
# 初始化安裝 hexo 的資料夾

$ git clone https://github.com/<user_name>/<repo_name>.git
# 將安裝 hexo 資料夾內的檔案全部複製到這裡
```
這樣我們就可以從這邊修改部落格內容，並且推上 github 專案中。

像我這裡就遇到 `command not found: hexo bash` 的情形，這裡有個方法提供參考:

```bash
# 下載nvm套件
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash

# This loads nvm
$ export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" 
```

載完nvm，還有一個路徑問題(for mac)，首先 `open ~/.zshrc` ，然後在.zshrc檔案新增hexo路徑:

```bash
#path to HEXO
export PATH=~/.npm-global/lib/node_modules/hexo-cli/bin:$PATH
```

完成後再重新安裝Hexo即可。

這裡有幾個常用指令要記住，之後再進行部落格修改的時候會很常使用到 :

```bash
$ hexo clean
# 清除舊 publish設定

$ hexo g
# 建立新 publish

$ hexo deploy
# deploy 到伺服器

$ hexo server
# 在本地瀏覽網站內容 (預設 http://localhost:4000)

$ hexo server -p 5000
# 如果port:4000被佔用，可以用這個
```

備註：
在 github 下的 branch 我設定成 master/hexofile
在 hexofile 進行操作，利用 git 指令載入到 `hexofile` 上；如果是利用hexo指令則會自動部署到 `master` 裡（因為_config.yml裡的branch設定成master)

hexo指令進行部署厚的檔案是經過轉換的，並非原始檔，所以使用 branch 維護是比姣好的。

主要是為了不讓原始檔只保留在本機，如果電腦壞了或者換電腦，你還可以從github找到。

### STEP4 : 安裝Theme(Next)

主題可改可不改，看個人喜好囉。
因為主題的版本不同，有些檔案以及程式碼都不太一樣(但大同小異)，每行程式註解都看過，大概就能知道他在幹嘛。

version: 5.1.4

```bash
$ cd <repo_name>
# 切換到要部屬至 github 的資料夾路徑

$ git clone https://github.com/iissnan/hexo-theme-next themes/next
# clone Next 主題專案到站點裡的themes資料夾中
```
接下來要到設定檔中將主題改為 next ( 預設是 landscape )

主要的設定檔案有兩個 : 
1. `\_config.yml`
2. `\themes\next\_config.yml`

#### \_config.yml 

網站的基本資訊設置，比較要注意的地方是 language 的部分，之後要配合 Next 的語言統一做設定。

```bash
# Site
title: #網站/部落格名稱
subtitle: #網站/部落格副標題
description: #網站/部落格基本描述 
keywords:
author: #作者
language: #語言 zh-tw
timezone: #時區 Asia/Taipei
```

設定文章網址型態，這裡主要是讓某些地方會需要顯示文章網址 (例如版權宣告區塊)的部分可以正確顯示。

```bash
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://<user_name>.github.io/<repo_name>
root: /<repo_name>/
permalink: :year/:month/:day/:title/
permalink_defaults:
```

將主題改為 next
```bash
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

最後這裡最重要，之後我們要利用 hexo 指令進行部屬，就會調用這部份來進行部屬，如果這部分沒有正確便無法部屬上去。

```bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/<user_name>/<repo_name>.git
  branch: master
```

如果不是利用 master 來進行網站設置，那麼 branch 的部分就要改成你的 <branch_name>。

#### \themes\next\_config.yml
Next 一共有四種版型可以選擇，大家可以自行選擇版型，將不要用的版型用 # 註釋掉即可。( 我自己使用的是 Gemini 版型 )
```bash
# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini
```
### Step 5: 動態背景
可以在 `\⁨themes⁩\⁨next⁩\⁨source⁩\lib⁩\canvas-nest⁩.js` 做修改


```bash
color: "255, 80, 219" # RGB values, use `,` to separate #線條顏色
opacity: 1 # The opacity of line: 0~1 #線條透明度
zIndex: -1 # z-index property of the background #位在哪一層
count: 100 # The number of lines #線條數量
```
### Step 6: 調整及部署

在每一個設定更動後，也有一個標準 SOP :

```bash
$ hexo clean
$ hexo g # generator
$ hexo s # server
```

先在本機上看看是否有什麼問題，持續地進行微調，待最後設定都沒有問題後再進行部屬

```bash
$ hexo d # deploy
```

### Step 7: 文章發佈
Hexo 支援 markdown 編輯。
基本上有在使用hackmd的人都並不陌生，有興趣可以去學習 [markdown語法](https://github.com/othree/markdown-syntax-zhtw)

`<!--more-->`是用來縮短首頁每篇文章的長度，會顯示`閱讀全文`

接下來，我們要在網站中建立第一篇新文章，您可以直接從現有的範例文章「Hello World」改寫，也可以學習 new 指令。

```bash
$ hexo new [layout] <title>
```
您可以在指令中指定文章的佈局（layout），預設為 `post` ，您可以透過修改 `_config.yml` 中的 `default_layout` 設定來指定預設佈局。

#### 佈局（Layout）
Hexo 有三種預設佈局：post、page 和 draft，它們分別對應不同的路徑，而您所自定的其他佈局和 post 相同，都儲存至 source/_posts 資料夾。

| 佈局| 路徑| 
| --- | --- |
| post | source/_posts | 
| page | source | 
| draft | source/_posts | 


#### 首頁摘要的部分
一般在首頁會希望每一篇文章只要呈現一小部分內容以及一張代表性圖片即可，藉由 “ 閱讀全文 “ 按鈕來閱讀每一篇文章完整內容，在 hexo 官方的建議作法是在文章中插入 `<!-- more -->` 便可以在首頁僅呈現此代碼之前的文章內容。

至於閱讀全文的按鈕樣式，我們可以在 `\themes\next\source\css\_schemes\<選擇的版型>\index.styl` 最後加入以下內容來做調整

```bash
//閱讀全文樣式設置
.post-button {
    margin-top: 30px;
    text-align: center;
}

.post-button .btn {
    color: white;
    font-size: 15px;
    background: #000d33;
    border-radius: 16px;
    line-height: 2;
    margin: 0 4px 8px 4px;
    padding: 0 20px;
    border:none;
    -webkit-box-shadow: 0px 5px 30px -3px rgba(0,0,0,0.75);
    -moz-box-shadow: 0px 5px 30px -3px rgba(0,0,0,0.75);
}

.post-button .btn:hover {
    color: #000d33;
    background: #ffffff;
}
```

### Step 8: 結論

現在網路上真的很多教學範例，但是實際操作起來，因為每個主題不同，甚至於主題的版本也不同，在查詢的時候也會遇到一些問題，一開始因為路徑問題以及套件版本問題花了很多時間在建立環境，大家遇到的問題我遇到了，沒遇到的問題我也遇到了，俗話說的好，“頭過身就過”，~~老天幫你挖了一個坑，就是要你跳下去~~，期許自己未來在這個地方能夠分享很多自己學習到的新知識與心得。

另外還有一些配置，像是留言板、背景顏色、字體大小、分類標籤的設置我就不一一贅述，有時間會再把東西都補上來的。

### Step 9: reference
* [https://shizsun0609tw.github.io/2019/05/29/git-hexo-輕鬆架網站/](https://shizsun0609tw.github.io/2019/05/29/git-hexo-%E8%BC%95%E9%AC%86%E6%9E%B6%E7%B6%B2%E7%AB%99/)
* https://allen108108.github.io/blog/
* https://blog.csdn.net/lichenliang666/article/details/88218551
* https://ithelp.ithome.com.tw/articles/10208619
* [https://sh2ocn.top/2019/05/31/Hexo-NexT主题-设置圆形旋转头像/](https://sh2ocn.top/2019/05/31/Hexo-NexT%E4%B8%BB%E9%A2%98-%E8%AE%BE%E7%BD%AE%E5%9C%86%E5%BD%A2%E6%97%8B%E8%BD%AC%E5%A4%B4%E5%83%8F/)
* https://extremegtr.github.io/2017/09/27/Customize-NexT-Gemini-theme/
* https://jacksonleon.gitee.io/posts/1540132352.html
*  https://blog.csdn.net/Wonz5130/article/details/84666519
* [https://linlif.github.io/2017/05/27/Hexo使用攻略-添加分类及标签/](https://linlif.github.io/2017/05/27/Hexo%E4%BD%BF%E7%94%A8%E6%94%BB%E7%95%A5-%E6%B7%BB%E5%8A%A0%E5%88%86%E7%B1%BB%E5%8F%8A%E6%A0%87%E7%AD%BE/)
* [文章連結唯一化](http://muyunyun.cn/posts/f55182c5/#more)
* [...etc](https://www.google.com/search?q=hexo&rlz=1C5CHFA_enTW848TW848&oq=hexo&aqs=chrome..69i57j69i59l3j69i60l4.2945j0j7&sourceid=chrome&ie=UTF-8)


### 後記： Debug
記錄一些後續再換電腦後重新部署遇到的問題
#### Hexo
* [sudo權限問題](https://blog.csdn.net/Andrewb/article/details/103324279)
* [Hexo Deploy 失敗](https://blog.myctw.cc/post/25cd.html)
  * nodejs版本問題，需要透過nvm降版本 
* [hexo使用theme出現"  extends '_layout.swig' import '_macro/post.swig' as post_template"問題](https://blog.csdn.net/qq_39898645/article/details/109181736)
  * 因為我把package.json裡的套件都更新成最新的
  
#### Git
* [../.DS_Store](https://blog.csdn.net/qq_32907195/article/details/114941510)