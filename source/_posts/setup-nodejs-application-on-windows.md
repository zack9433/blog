---
title: 在 Windows 上開發 Nodejs 應用程式注意事項
sitemap: false
date: 2016-05-13 14:26:06
description: 紀錄在 Windows 上開發 Node 程式所碰到的問題。
tags:
  - nodejs
  - windows
---

最近公司打算要讓產品做成跨平台的，所以原本只在 Linux 上跑得 Node 程式要嘗試在 Windows 上跑了。在建立開發環境就遇到一些問題，就順手紀錄一下。

> 心得：我還是喜歡在 Linux 上開發就好....

# 開發環境
由於有裝 `bcryt` 這個 module 需要在安裝時才進行編譯的動作，所以需要用到 `node-gyp` 進行 compile 的動作，而在 Windows 7 64bit 上就需要裝以下的東西：
- [Microsoft Visual Studio C++ 2013 Express](https://www.microsoft.com/en-gb/download/details.aspx?id=44914)
- [Windows 7 64-bit SDK](https://www.microsoft.com/en-us/download/details.aspx?id=8279)
- python 2.7

另外用 [nvm-windows](https://github.com/coreybutler/nvm-windows) 來作 node.js 的版本控管，另外在安裝好 node 之後要確保 npm 是使用 v3.x.x 的版本，原因是在 windows 安裝 node modules 會碰到路徑過長的問題，詳情可以參考保哥文章的介紹 - [如何在 Windows 平台變更 Node.js / npm 全域模組的預設安裝路徑](http://blog.miniasp.com/post/2015/09/01/Change-npm-default-global-installation-directory-for-nodejs-modules-in-Windows.aspx)。

# 設置環境變數
通常我啟動 node 應用程式都是透過 npm script 來啟動(`npm start`)，順便會設定環境變數，例如：`NODE_ENV=development`，但這樣設定在 windows 上是行不通的，必須改成 `set NODE_ENV=development`，另外在設定多個環境變數的時候要注意有無空白，例如：`set NODE_ENV=development && set FOO=bar`，在 `&&` 前面有一個空白，所以 `NODE_ENV` 也會有並不會被 trim 掉，但可以考慮用 [cross-env](https://www.npmjs.com/package/cross-env) 來解決。
