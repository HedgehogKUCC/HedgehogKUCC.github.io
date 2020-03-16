---
title: landscape 添加樣式
date: 2020-03-16
tags: 
  - w3HexSchool
  - hexo
---

<img src="https://i.imgur.com/9LPdwGN.jpg" width="1024" alt="Hexo landscape add style" />

## 目錄

  - [滑鼠點擊有愛心](#click-love)
  - [GitHub Corners](#ghc)
  - [配置訪問統計](#count)
  - [Disqus](#disqus)
  - [將 tag 加上 icon](#add-icon)
  - [加上 favicon.ico](#favicon)
  - [Gitalk](#gitalk)

<!-- more -->

<br>

<h2 id="click-love">滑鼠點擊有愛心</h2>

`/themes/landscape/source/js/src` 新建 `clicklove.js`

將以下程式碼增加至 `clicklove.js`

```js
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);
```

之後打開 `/themes/landscape/layout/layout.ejs` 在 `/body` 前添加這段程式碼

```html
<!-- 頁面點擊小紅心 -->
<script type="text/javascript" src="/js/src/clicklove.js"></script>
```

<br>

參考來源：

[http://ylong.net.cn/hexo_conf.html](http://ylong.net.cn/hexo_conf.html)

[https://zhuanlan.zhihu.com/p/60424755](https://zhuanlan.zhihu.com/p/60424755)

<br>

<h2 id="ghc">GitHub Corners</h2>

[http://tholman.com/github-corners/](http://tholman.com/github-corners/)

將選好樣式的程式碼複製

貼到 `/themes/landscape/layout/_partial/header.ejs`

先將這段程式碼註解

```html
<nav id="sub-nav">
  <% if (theme.rss){ %>
    <a id="nav-rss-link" class="nav-icon" href="<%- url_for(theme.rss) %>" title="<%= __('rss_feed') %>"></a>
  <% } %>
  <a id="nav-search-btn" class="nav-icon" title="<%= __('search') %>"></a>
</nav>
<div id="search-form-wrap">
  <%- search_form({button: '&#xF002;'}) %>
</div>
```

再將程式碼貼到 `/div` 後面

```html
<div id="header-inner" class="inner">
</div>

<!-- 貼上程式碼 -->
```

**注意的是： href 後面是自己的 GitHub 的網址，記得修改哦！**

<br>

<h2 id="count">配置訪問統計</h2>

`/themes/landscape/layout/_partial/footer.ejs`

```html
<footer id="footer">
  <% if (theme.sidebar === 'bottom'){ %>
    <%- partial('_partial/sidebar') %>
  <% } %>
  <div class="outer text-center">
    <div id="footer-info" class="inner">
      &copy; <%= date(new Date(), 'YYYY') %> <%= config.author || config.title %><br>
      <span id="busuanzi_container_site_uv">
        本站訪客數&nbsp;<span id="busuanzi_value_site_uv"></span>&nbsp;人次&nbsp;&nbsp;|&nbsp;&nbsp;
      </span>
      <span id="busuanzi_container_site_pv">
        本站總訪問量&nbsp;<span id="busuanzi_value_site_pv"></span>&nbsp;次
      </span>
    </div>
  </div>
  <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js">
  </script>
</footer>
```

<br>

參考來源

[http://ibruce.info/2015/04/04/busuanzi/](http://ibruce.info/2015/04/04/busuanzi/)

<br>

<h2 id="disqus">Disqus</h2>

申請帳號 [https://disqus.com/](https://disqus.com/)

在 `/themes/landscape/_config.yml` 新增

```
# DISQUS comments
disqus_shortname: Your Disqus Website Name
comments: true
```

`shortname:` 填入你的 `Disqus Website Name`

之後照著 [步驟](http://zehuama.logdown.com/posts/2019/02/14/add-disqus-on-landscape-hexo) 修改就可以完成囉

步驟中下面這段程式碼不用加，會導致介面 `479px` 以下時多出額外空白！！！

```
<% if (page.permalink){ %>
  var disqus_url = '<%= page.permalink %>';
<% } %>
```

<br>

參考來源

[https://jim51.github.io/2018/08/14/hexo-comments-disqus/](https://jim51.github.io/2018/08/14/hexo-comments-disqus/)

<br>

<h2 id="add-icon">將 tag 加上 icon</h2>

`/themes/landscape/source/css/_partial/article.styl`

將程式碼

```css
.article-tag-list-link
  &:before
    content: "#"
```

修改為

```css
.article-tag-list-link
  &:before
    content: "\f02b"
    font-family: FontAwesome
```

<br>

參考來源

[Hexo Custom Theme - Using FontAwesome Icons](http://jr0cket.co.uk/2014/06/hexo-custom-theme---adding-navbar-icon-links-using-fontawesome.html)

[http://astronautweb.co/snippet/font-awesome/](http://astronautweb.co/snippet/font-awesome/)

<br>

<h2 id="favicon">加上 favicon.ico</h2>

將 `favicon.ico` 放在 `root/source`

修改 `themes/landscape/_config.yml`

```
favicon: favicon.ico
```

注意：前面不用在 `\`，會導致 deploy GitHub 變成 `\\`。

<br>

修改 `themes/landscape/layout/_partial/head.ejs`

```html
<!-- 找出這段程式碼 -->
<link rel="icon" href="<%- theme.favicon %>">

<!-- 修改為 -->
<link rel="icon" href="<%- config.root %><%- theme.favicon %>">
```

<br>

參考來源

[https://coffee0127.github.io/blog/2016/08/09/hexo-configuration/](https://coffee0127.github.io/blog/2016/08/09/hexo-configuration/)

[ICONFINDER](https://www.iconfinder.com/search/?q=cat&price=free)

[Favicon線上製作轉換工具](http://tw.faviconico.org/favicon)

<br>

<h2 id="gitalk">Gitalk</h2>

安裝

[官網](https://github.com/gitalk/gitalk/blob/master/readme-cn.md)有介紹兩種方式

而我是將 `CSS` 和 `JS` 下載放進裡面引入

  - https://unpkg.com/gitalk/dist/gitalk.css
  - https://unpkg.com/gitalk/dist/gitalk.min.js

<br>

這是我所放的位置

`themes/landscape/source/css/gitalk.css`

`themes/landscape/source/js/src/gitalk.min.js`

<br>

在這邊引入

`themes/landscape/layout/_partial/head.ejs`

```html
<!-- Gitalk -->
<link rel="stylesheet" type="text/css" href="/css/gitalk.css" />
<script type="text/javascript" src="/js/src/gitalk.min.js"></script>
```

<br>

在 `themes/landscape/_config.yml` 配置上 `gitalk`

```yml
# Gitalk
gitalk:
  enable: true
```

<br>

建立 GitHub OAuth APP，請參考 [https://hsiangfeng.github.io/hexo/20191206/2397475810/](https://hsiangfeng.github.io/hexo/20191206/2397475810/)

<br>

在 `themes/landscape/layout/_partial/article.ejs` 配置容器和生成插件

```ejs
<% if (!index && theme.gitalk['enable']==true){ %>
<div id="gitalk-container"></div>
<script>
  var title = location.pathname.substr(0, 50)
  var gitalk = new Gitalk({
    clientID: 'Your ID',
    clientSecret: 'Your Secret',
    repo: 'hedgehogkucc.github.io',
    owner: 'hedgehogkucc',
    admin: ['hedgehogkucc'],
    id: title,
    distractionFreeMode: true,
    labels: ['Gitalk'],
    language: 'zh-TW'
  })
  gitalk.render('gitalk-container')
</script>
<% } %>
```

<br>

會多寫這行 `var title = location.pathname.substr(0, 50)` 原因請看[這篇-6.3 Error: Validation Failed.](https://blog.csdn.net/weiwosuoai/article/details/90573929)

<br>

遇到其它問題可以參考 [https://www.codeleading.com/article/19832233755/](https://www.codeleading.com/article/19832233755/)

<br>

我有下載這個別人優化好的 主題 - [Landscape-x](https://github.com/HipHopCoderS/Landscape-x) 來研究怎麼改

<br>

在這邊分享自己遇到一個雷......

就是 `source/_posts/` 裡面檔案名稱 `2020-02-03-JavaScript考題-1.md` 千萬不要有空白.....

不然當你在該篇文章要登入 GitHub 轉頁時都會跑回主頁

原因可以開啟 **開發人員工具** 的 Network 查看訊息

會有一個 error 檔案去查看 `Headers` 資訊

<br>

圖片來源： <a href="https://pngtree.com/free-backgrounds">free background photos from pngtree.com</a>