---
title: 透過 npm script 執行相依任務，並透過 npm config 傳值的小技巧
sitemap: false
date: 2016-06-20 10:58:35
description: 筆記透過 npm 定義 script task 順序與傳在 task 間傳值得技巧。
tags:
- nodejs
- npm
---

> 會有這篇文章是因為在定義 e2e 測試任務的時候，碰到執行任務上的問題。
> 心得：npm script task 已經足夠應付大部分的狀況囉！不過在比較複雜的狀況，例如：task 有先後順序或者是並行執行就必須多定義一些 task 來完成。

# 並行執行
假設有兩個任務 A 和 B 要並行執行的話

```json
{
  "scripts": {
    "parallel": "npm run watch & npm run start",
    "watch": "mocha -w app.spec.js",
    "start": "node app.js"
  }
}
```

# 循序執行
要作 e2e 測試所以，先起 phantomjs 之後再起 protractor。

```json
{
  "scripts": {
    "test:e2e": "npm run phantom && npm run protractor",
    "phantom": "phantomjs --webdriver=4444",
    "protractor": "protractor protractor.conf.js"
  }
}
```

以上會發現當 protractor 跑完測試後，phantomjs 並沒有停止執行，所以必須在執行完 protractor 後要停止 phantomjs。作法大致上是紀錄 phantomjs pid 然後在 protractor 執行完後 kill phantomjs pid。

```json
{
  "name": "myapp",
  "scripts": {
    "test:e2e": "npm run phantom && npm run protractor",
    "phantom": "phantomjs --webdriver=4444 & npm config set config myapp:phantomjspid $!",
    "protractor": "protractor protractor.conf.js && kill $npm_package_config_phantomjspid"
  }
}
```
