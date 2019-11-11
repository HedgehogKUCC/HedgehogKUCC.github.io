---
title: BOM DOM innerHTML createElement
date: 2019-11-09
tags: 
  - html
  - js
---

# 參考資源

- [DOM](https://ithelp.ithome.com.tw/articles/10202689)
- [BOM](https://www.happycoding.today/posts/43)

## 什麼是 DOM

全名為 `Document Object Model` 中文翻譯為 `文件物件模型`

<br>

## 什麼是 BOM

全名為 `Browser Object Model` 中文翻譯為 `瀏覽器物件模型`

`window.outerHeight` : 整個瀏覽器高度

`window.innerHeight` : 畫面顯示的高度

[print、location、open](https://codepen.io/hedgehogkucc/pen/oNNyREP)、[innerHeight](https://codepen.io/hedgehogkucc/pen/zYYaQbj)

<br>

## innerHTML

***方法*** : 組完字串後，傳進語法進行渲染

***優點*** : 效能快

***缺點*** : 資安風險 `XSS` (Cross-Site Scripting) , 要確保來源沒問題

***特性*** : 將原本內容清空 , 再傳進我們寫的資料 !!! 

<br>

**外層**使用單引號 `''`

**內層**就要使用雙引號 `""`

[EX-1](https://codepen.io/hedgehogkucc/pen/QWWxxxK) 、 [EX-2](https://codepen.io/hedgehogkucc/pen/VwwddER) 、 [EX-3](https://codepen.io/hedgehogkucc/pen/jOOKKJB)

<hr>

<br>

## createElement

***方法*** : 以 `DOM` 節點來處理

***優點*** : 安全性高

***缺點*** : 效能差

<br>

[EX-1](https://codepen.io/hedgehogkucc/pen/XWWYYwq)、[EX-2](https://codepen.io/hedgehogkucc/pen/XWWYBrY)

