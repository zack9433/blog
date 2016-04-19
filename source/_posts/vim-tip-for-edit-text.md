---
title: VIM 修改文字的小技巧
sitemap: false
date: 2016-04-19 14:40:44
tags: vim
description: 筆記 VIM 編輯器修改文字的小技巧
---
> 天下武功，為快不破。筆記一下最近學到 VIM 修改的小技巧

# 修改 HTML Tag 中的內容
在 normal mode 下，想要修改某個 tag 內容
範例：
```html
<div>Hello wolrd<div>
```
游標移動到 `div` 上，按下 `c + i + t`。

# 修改 function 中的內容
在 normal mode 下，想要修改某個 function 內容
範例：
```js
function greet() {
  return 'hello';
}
```
游標移動到 function block 中，按下 `c + i + {`。

# 修改單引號中的字串內容
在 normal mode 下，想要修改某個字串內容
範例：
```js
function greet() {
  return 'hello';
}
```
游標移動到單引號中，按下 `c + i + '`。
