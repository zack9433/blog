---
title: 在 Neovim 中快速切換 Terminal 視窗的小技巧
sitemap: false
date: 2016-04-30 21:29:30
description: 主要介紹利用新版 Neovim 的 Terminal 來打造 IDE 風格的編輯器並且能在編輯區與 Terminal 間快速切換。
tags:
- neovim
- vim
---

> 主要介紹利用新版 Neovim 的 Terminal 來打造 IDE 風格的編輯器並且能在編輯區與 Terminal 間快速切換。

最近心血來潮，想要重整一下編輯器的工作流程，已經從 VIM 切換成 Neovim 一段時間了，在開發 Web 的過程中是利用 [vim-dispatch](https://github.com/tpope/vim-dispatch) 搭配 tmux 來做成 IDE like 的感覺，我只要在 Neovim 中下例如：`Dispatch npm start` 就會開啟一個 tmux pane 在下方，然後在搭配 [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator) 來作一個編輯器與 tmux 之間的切換，但看到 Neovim 有支援在編輯中開啟 Terminal 就來試試看吧！

# Demo
![Demo](/images/slfLrCclC5.gif)

# 安裝
在 Neovim 安裝以下 Plugins
- [vim-dispatch](https://github.com/tpope/vim-dispatch) 
- [vim-dispatch-neovim](https://github.com/radenling/vim-dispatch-neovim) 

# 切換視窗快捷設定
```
" Make ctrl-h/j/k/l move between windows and auto-insert in terminals
func! s:mapMoveToWindowInDirection(direction)
    func! s:maybeInsertMode(direction)
        stopinsert
        execute "wincmd" a:direction

        if &buftype == 'terminal'
            startinsert!
        endif
    endfunc

    execute "tnoremap" "<silent>" "<C-" . a:direction . ">"
                \ "<C-\\><C-n>"
                \ ":call <SID>maybeInsertMode(\"" . a:direction . "\")<CR>"
    execute "nnoremap" "<silent>" "<C-" . a:direction . ">"
                \ ":call <SID>maybeInsertMode(\"" . a:direction . "\")<CR>"
endfunc
for dir in ["h", "j", "l", "k"]
    call s:mapMoveToWindowInDirection(dir)
endfor
```
