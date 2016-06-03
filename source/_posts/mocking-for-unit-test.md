---
title: 筆記單元測試中的 Mocking
sitemap: false
date: 2016-06-03 15:35:49
description: 筆記一下在做 Test 時對 Mocking 的瞭解
tags:
- testing
- unit test
---
# SUT 和 DOC
在瞭解 Mocking 前要先大概知道 SUT 和 DOC。
- SUT(System Under Test) 指的是待測程式，可能是個 function 或 object。
- DOC(Depended-on Component)，指的是 SUT 依賴的程式。

因為要測試的是 SUT 的部份而不是 DOC，所以我們希望對 DOC 進行 Mocking。

# Mocking DOC 的動機
- Isolation
指的是隔離 DOC
- Easy flow control
```javascript
// 透過 Mocking 可以指定 component.isNew() 回傳的值
if (component.isNew()) {
    insert();
}
else {
    update();
}
```
- Remove interaction with complex or external systems
例如：DOC 是要去存取 DB 或者網路等等較複雜的動作
- Testing interactions
```javascript
// 透過 Mocking 可以知道 component.insert() 或者 component.update() 有沒有被呼叫
{
...

    save: function(component) {
        if (this.isNew()) {
            component.insert();
        }
        else {
            component.update();
        }
    }
...
}
```

# Types of Mock
大多數的開發者都直æ¥將 Mock Objects 稱為 Mocks，但事實上 Mock Objects 有不同的 Type，根據 [Martin Fowler](http://www.martinfowler.com/bliki/TestDouble.html)的說法，是將 Mock Objects 稱作 Test Doubles（測試替身），並且有以下分類：
- Dummy
- Fake
- Stub
- Spy
- Mock

詳細的分別可以參考[搞笑談軟工此篇](http://teddy-chen-tw.blogspot.tw/2014/09/test-double2.html)

# Mock 技巧說明
- Mocking functions
這是最基本的 Mock，直接用 Stub 或 Spy 來作一個測試替身
- Mocking Objects
針對一個物件來進行 Mock 比較複雜一點，大概有以下方式：
    - 手工打造測試替身  
    自行 Coding 打造 Test Double  物件來替代真實的物件，但這個方式有一個麻煩的點，假如真實的物件他的方法或介面有改變，同樣 Test Double 也要修改。
    - 透過物件的定義來打造
    因為手工比較麻煩，通常直接用真實物件的 Class 來產生 Test Double。

JavaScript 範例：  

```javascript
// 假設有一個 Class A，有兩個 function
function A() {}
A.prototype.method1 = function() {};
A.prototype.method2 = function(obj, bool) {};

// 手工打造 Test Double 物件
// 此方法的麻煩就在於必須在測試開始前先額外 codding 來模擬 A
var testdouble = {
  method1: function() {},
  method2: function(obj, bool) {}
};
```

```javascript
// 假設有一個 Class A，有兩個 function
function A() {}
A.prototype.method1 = function() {};
A.prototype.method2 = function(obj, bool) {};

// 透過物件的定義來打造
// 直接將真實物件視為 Test Double 物件
// 但假如 A 的建構子有作一些額外的事情的話，有可能會產生一些副作用
var testdouble = new A();
```

# 結論
整個 Mocking 的過程，就是從原本定義好的物件
![Mocking Objects](/images/mocking-objects.png)
在透過任何 Mocking 的手段，可能是使用 `Sinon` 之類的 Mocking Library，來對 function 進行  spy(Test Double) 或其它動作，此時若直接呼叫 function 的話是不會執行原本的動作，而是執行定義 spy function 要作的事情，例如：回傳 true or false 或者字串並追蹤呼叫此 function 次數等等。
![Replace Method with Spy](/images/replace-method-with-spy.png)
另一種方式是在執行的 function 前插入 spy function，這樣除了可以追蹤 function 被呼叫幾次並且執行原本所定義的動作。
![Intercept Method with Spy](/images/intercept-method-with-spy.png)
最後要記得的是我們不是針對 SUT 來進行 Mocking 而是針對 DOC 來進行 Mocking。
