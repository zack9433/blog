---
title: state-in-angular-applications-notes
sitemap: false
date: 2017-07-13 11:10:03
tags: 
  - angular
  - ngrx
  - redux
description: 主要筆記觀看 State in Angular Applications 的影片
---

> 前一陣子在 Angular 的 Community 有舉辦一個在船上的 [Conference](https://ngcruise.com/)，最近在 YouTube 可以看到 Conference 的內容了，此篇主要筆記一下在 Application 裡會有那些 State 需要處理。

# Type of State

* Server State
    * 存在 Server 端的資料
* Persistent State
    * Server State 的子集，透過網路存取而來
* Client State
    * State 不存在 Server 端，例如：透過 Input 來作 filter 的功能，在 Input 輸入的值就是一種  Client State
    * Recommendation: It’s good practice to reflect both the persistent and client state in the URL. 這表示你可以透過分享網址給其他人並且保留同一個 State
* URL or Router State
    * 透過 URL 紀錄當前的 State，當頁面重整後可依據目前的 URL 還原 State
* Transient Client State
    * 不會透過 URL 來紀錄，但如果使用者換頁後在切回來，還是會回到先前的 State，但是如果透過網址分享出去給另一個使用者，是無法還原 State，例如：看 YouTube 影片看到一半，切換到另一部影片，然後在切回來看，是可以從上次觀看的地方繼續觀看，但如果分享此影片給其他人，其他人都是會從頭開始觀看
* Local UI State
    * 獨立的 Component 或是 Directive 有時會有自己的 State，例如：紀錄當前 Component Active 的顏色。Local UI State 是可以做成 mutable，不一定要 immutable。
    
# State Synchronization
![State Synchronization](/images/state-synchronization.png)

## Syncing Server and Persistent State
![Fail Case](/images/fail-case.png)

前端狀態的 talk 會被 rate 過，但後端狀態是沒有 rate，呈現 Server State 和 Persistent State不同步的狀態。Workaround 的解法如下：

![Ad-hoc 1](/images/ad-hoc-1.png)

但這解法對於在多個 Request 並且沒辦法保證回應順序下還是會遇到狀態不同步的問題。

![Race Condition](/images/race-condition-1.png)

## Syncing URL and Client State

假設要作一個 filter 的功能，一開始設計如下：

![Feature Filter](/images/filter-1.png)

但當我們重整頁面後，我們先前所 filter 出來的資料就消失了，我們也沒辦法透過網址分享給其他人我們所 filter 出來的結果，所以我們加上了 Workaround 的解法，如下：

![Ad-hoc 2](/images/ad-hoc-2.png)

但這解法在遇到很快的切換頁面時還是會遇到問題，如下：

![Race Condition](/images/race-condition-2.png)

以上這些因為由 Response 回應順序的問題，是很難從 Unit Test 得知的。我們可以歸納犯了以下的錯誤：

## Mistakes

* State management and side effects aren’t separated.
* No clear sync strategy of the persistent state and the server.
* No clear sync strategy of the client state and the URL
* Our model is mutable

# Separating State Management

> Rule: Separate services/computation from state management.

![Separating State](/images/state-separate-1.png)

Store 負責管理 State，當 component 分派 action 來，Store 會複製目前 State 並產生新的 State。Effect 也可以去監聽那些 Action 並針對此 Action 去執行 Side Effect 的動作，例如向後端 server 傳送資料等等，讓後端與前端作一次 Sync。

![Backend Service](/images/service-1.png)
![Reducer](/images/reducer-1.png)

> Rule: Use immutable data for persistent and client state

使用 immutable data 的好處是我們可以保證 State 在系統內的一致性，也能做到 undo 的動作。通常資料是會分享到其他 Component 或者是 Server 端的時候，最好是保持 immutable 的特性。

![Effect](/images/effect-1.png)

透過 Effect 來作 state sync，另外用 switchmap 來避免回應順序的問題

![Reducer](/images/reducer-2.png)
![Optimistic Update Flow](/images/optimistic-update-flow.png)

> Rule: Optimistic updates require separate actions to deal with errors.

# Synchronizing URL and Client State

當有 URL 改變的時候會去觸發 Reducer 或者 State change

> Rule: Always treat Router as the source of truth.

![Component with Store](/images/component-with-store.png)
![URL and Client State Change Flow](/images/url-and-client-state-chnage-flow.png)
![Effect](/images/effect-2.png)

Store updates its state

![Reducer](/images/reducer-3.png)

Overall Interaction:

![Overall Interaction Flow](/images/overall-interaction-flow.png)

# Analysis:

* Updating via UI or via Location is the same.
* Components dispatch actions and react to state changes
* No race conditions (switchmap)

# Summary

* Ad-hoc solutions do not fully work
* To fix the issues we:
    * Switched to using NgRx and immutable data
    * Made Router the source of truth
* Main takeaway: be deliberate about how you manage state
