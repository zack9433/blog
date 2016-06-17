---
title: 抓取 pull request 到 local 測試
sitemap: false
date: 2016-06-17 14:15:11
description: 在工作上遇到要在 local 端測試 pull request，所以筆記一下指令
tags:
- github
- gitlab
---

> 心得：跟 Gitlab 相比，Github 方便多了。

# Github 指令
```sh
git fetch origin pull/<pull req ID>/head:<branch name>

git checkout <branch name>
```
大致上就是這樣子

更新 pull request branch

```sh
git pull origin pull/<pull req ID>/head --no-ff
```

# Gitlab 指令

出處：[http://doc.gitlab.com/ce/workflow/merge_requests.html](http://doc.gitlab.com/ce/workflow/merge_requests.html)

1. 打開 **.git/config**  
```
[remote "origin"]
  url = https://gitlab.com/gitlab-org/gitlab-ce.git
  fetch = +refs/heads/*:refs/remotes/origin/*
```
2. 加入 `fetch = +refs/merge-requests/*/head:refs/remotes/origin/merge-requests/*`  
```
[remote "origin"]
  url = https://gitlab.com/gitlab-org/gitlab-ce.git
  fetch = +refs/heads/*:refs/remotes/origin/*
  fetch = +refs/merge-requests/*/head:refs/remotes/origin/merge-requests/*
```
3. `git fetch origin`
4. `git checkout <branchname>`
