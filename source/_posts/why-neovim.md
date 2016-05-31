---
title: 從 VIM 到 Neovim 的原因
sitemap: false
date: 2016-05-30 22:15:44
description: 紀錄自己為什麼要用 Neovim 替換 VIM 編輯器
tags:
  - vim
  - neovim
---

> 直接說結論，請大家支持 [Neovim](https://salt.bountysource.com/teams/neovim)

# 緣由
會紀錄這篇是因為，同事問我 Neovim 跟 VIM 有差別嘛，當下只知道 Neovim 支援 async 的動作，但這似乎還是不足以讓他從 VIM 改成用 Neovim，所以我突然想了解 Neovim 的作者為什麼想要再造一個 VIM。後來上網找了一些資料，可以參考[此篇](http://geoff.greer.fm/2015/01/15/why-neovim-is-better-than-vim/)，總結出以下幾點:

## 不易維護
因為 VIM 的 Code Base 很老了，有很多很難維護的 Copy Paste Code

## Plugin API 不易使用 
Plugin API 對開發者不友善，另外開發出來的 Plugin 執行是 synchronously 所以使用者在編輯文字的過程中有可能會頓頓的

# 結論
我覺得一個軟體有好幾十年沒有重構過，另外對開發 Plugin 的人不友善，應該是要淘汰了，有人跳出來基於 VIM 重構，有更好的測試，更友善的 Plugin API，相信未來 Neovim 的社群會越來越多人加入。
