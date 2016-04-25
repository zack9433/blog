---
title: Sell 小技巧
sitemap: false
date: 2016-04-25 09:37:57
tags: bash, mac
description: 最近在加強自己工作的流程，希望在操作的速度上能有所提升，所以紀錄一下有用的 shell command。
---

> 此篇主要筆記一下 shell command，主要是想加快操作的速度，天下武功，為快不破啊～

# Bash
- `!!`: 執行上一次執行的命令
- `sudo !!`: 用 sudo 去執行上一次執行的命令
- `!<word>`: 執行最後一次出現某個特定字串的命令，例如：`!wget`
- `!<word>`: 顯示但不執行最後一次出現某個特定字串的命令，例如：`!wget:p`
- `ls -lhS`: 以檔案大小排序
- `top -o vsize`: 以使用 virtual memory 大小排序，可判斷那支程式導致系統變慢
- `echo "<command>" | at <time>`: 在某個特定時間執行某個命令

# MAC only
- `caffeinate -u -t 3600`: 一小時後才休眠
- `qlmanage -p <file>`: 預覽檔案
