---
title: Hexo + Github + Travis 架站心路歷程 - 第三步：Travis 自動佈署
sitemap: false
date: 2016-04-16 10:41:28
tags: travis
description: 由於想要達到自動佈署，所以可以用一些 CI 來達到，而我選擇用 Travis CI，也是可以用其他的 CI service 來達到，例如：Circle CI。
---

> 由於想要達到自動佈署，所以可以用一些 CI 來達到，而我選擇用 Travis
CI，也是可以用其他的 CI service 來達到，例如：Circle CI。
> 第一篇 [Hexo + Github + Travis 架站心路歷程 - 第一步：Hexo 介紹](http://zack9433.github.io/2016/04/13/hexo-intro/)
> 第二篇 [Hexo + Github + Travis 架站心路歷程 - 第二步：Github Page 設定](http://zack9433.github.io/2016/04/14/github-page-intro/)

# 申請 Github Personal Access Tokens
由於我們要達到自動佈署，也就是讓 Travis 能將 Hexo 產生的結果寫到 Github
repo，所以 Travis 必須要拿到寫入的授權，我們可以透過 Personal Access Tokens
來達到，Personal Access Tokens 路徑如下:

`Settings -> Personal Access Tokens -> Generate new token`

P.S. 在產生的 token 設定中勾選 `repo` 之後產生 token 後要 copy 起來，如果離開畫面就看不到了，必須再重新產生。

# Travis 設定

## Setup
將 Travis 與 Github 同步後(按下同步的按鈕)找到 `<帳號名稱>.github.io` 的
repo，啟用此 repo 要進行 CI 的任務並且設定要進行 CI 時的環境變數設定。

## 設定 Token
設定一組環境變數，key 值`DEPLOY_REPO`(不一定要這名稱)並將產生的 Personal Access
Token 複製到值的欄位。

## .travis.yml
```yml
language: node_js
node_js:
  - "4"

before_install:
  - git config --global push.default matching
  - git config --global user.name "xxxx"
  - git config --global user.email "xxx@xxx.xxx"
  - sed -i'' "/^ *repo/s~github\.com~${DEPLOY_REPO}@github.com~" _config.yml

install:
  - npm install hexo-cli -g
  - npm install

script:
  - hexo clean
  - hexo d -g --silent

cache:
  directories:
    - node_modules
```

# 自動佈署
在設定完成後，之後在寫完文章進行 git push，就會去 tigger Travis CI
來進行佈署的動作，打完收工。
