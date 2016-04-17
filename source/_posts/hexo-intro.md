---
title: Hexo + Github + Travis 架站心路歷程 - 第一步：Hexo 介紹
sitemap: false
date: 2016-04-13 22:40:30
tags: hexo
description: 主要紀錄如何透過 Hexo, Github 和 Travis 來架設並自動部屬更新部落格。分成三個部分來說明。
---

> 主要紀錄如何透過 Hexo, Github 和 Travis 來架設並自動部屬更新部落格。分成三個部分來說明。
> 第二篇 [Hexo + Github + Travis 架站心路歷程 - 第二步：Github Page 設定](http://zack9433.github.io/2016/04/14/github-page-intro/)
> 第三篇 [Hexo + Github + Travis 架站心路歷程 - 第三步：Travis 自動佈署](http://zack9433.github.io/2016/04/17/travis-for-blog/)

突然心血來潮，想要筆記一下自己工作上所解決的問題，人老了記憶力衰退...，所以就來架個部落格吧！

# Hexo 快速架站
耳聞 Hexo 架站之快，親自體驗過後，除了快還頗方便的，算是很完整的方案，毫不猶豫就選擇用 Hexo。

# 安裝
要先安裝 [Node.js](https://nodejs.org/en/)
```sh
npm install hexo-cli -g
```
# 建立
```sh
hexo init blog
cd blog
npm install
```
# 設定
Hexo 設定檔是 `_config.yml`
## 基本設定
```yml
# Site
title: RD 日常
subtitle: Code, Life, Coffee, That's it.
description: RD 日常的 murmur
author: Zack Yang
language: zh-tw
timezone:

# URL
url: https://zack9433.github.io
```
## Theme 設定
先下載想要的 [Theme](https://hexo.io/themes/)，我選擇 [Next](https://github.com/iissnan/hexo-theme-next)
```sh
git submodule add https://github.com/iissnan/hexo-theme-next.git themes/next
```
再根據 [Next](https://github.com/iissnan/hexo-theme-next) 所提供的設定進行調整，
```yml
# hexo next theme config
avatar: https://secure.gravatar.com/avatar/f56f0914fead2d6040a68b9fff0f39be
since: 2016
# 要到 disqus 註冊帳號
disqus_shortname: zack9433
creative_commons: by-nc-sa
social:
  github: https://github.com/zack9433
  twitter: https://twitter.com/zack9433
menu:
  home: /
  archives: /archives
  tags: /tags
```

# 撰寫文章
先啟動 Hexo Server
```sh
hexo server
```
再來新增文章
```sh
hexo new test
```
這動作會建立 `ssource/_posts/test.md`，然後就可以開始寫囉！

# 發佈
我是透過 [Github Page](https://pages.github.com/) 建立的，建立 [Github Page](https://pages.github.com/) 會在另一篇說明。在發佈前先安裝 [hexo-deployer-git](https://github.com/iissnan/hexo-theme-next)
```sh
npm install hexo-deployer-git --save
```
再來到 `_config.yml` 搞一下發佈設定
```yml
# Deployment
deploy:
  type: git
  repo: https://github.com/zack9433/zack9433.github.io.git
  branch: master
  message:
```
最後下達發佈指令就搞定了，輕輕鬆鬆啊～～
```sh
hexo d -g
```
