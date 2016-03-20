# HOW TO LOSE WEIGHT in the browser

原文地址：[HOW TO LOSE WEIGHT in the browser](https://browserdiet.com/en/)

And what if we got together a bunch of experts who work on large sites to create a definitive front-end performance guide?

要是说我们聚集了一大帮专注于创建大型网站的专家来制定一份终极前端性能指南呢？

And not just one of those boring guides made for robots, what if we did something fun? What about getting together Briza Bueno (Americanas.com), Davidson Fellipe (Globo.com), Giovanni Keppelen (ex-Peixe Urbano), Jaydson Gomes (Terra), Marcel Duran (Twitter), Mike Taylor (Opera), Renato Mangini (Google), and Sérgio Lopes (Caelum) to make the best reference possible?

而且不是那种给机器人看的无趣指南，假如说我们做了一件很有趣的事情？找到了Briza Bueno (Americanas.com), Davidson Fellipe (Globo.com), Giovanni Keppelen (ex-Peixe Urbano), Jaydson Gomes (Terra), Marcel Duran (Twitter), Mike Taylor (Opera), Renato Mangini (Google),还有 Sérgio Lopes (Caelum)来一起制定一份最佳指南。

That's exactly what we've done! And we'll guide you in this battle to create even faster sites.

这正是我们所做的！并且我们将在这场（网站性能）战役中指引大家编写运行更加快速的网站。

— Zeno Rocha, project lead.

## ？？does performance really matter?
## ？？性能真的那么重要吗？
Of course it matters and you know it. So why do we keep making slow sites that lead to a bad user experience? This is a community-driven practical guide that will show you how to make websites faster. Let's not waste time showing millionaire performance cases, let's get straight to the point!

当然重要而且您也心知肚明。那为什么我们还要继续编写运行缓慢用户体验糟糕的网站呢？下面是一份社区驱动的实战指南，将会向您展示如何编写响应快速的网站。我们就不浪费时间列举[大量需要优化性能的场景](https://github.com/zenorocha/browser-diet/wiki/Impact-of-performance)了，直奔主题吧！

## HTML
### 25 avoid inline/embedded code
### 25 避免内联/内嵌代码
There are three basic ways for you to include CSS or JavaScript on your page:

在页面中引入CSS或JavaScript有三种基本方法：

**1) Inline:** the CSS is defined inside a `style` attribute and the JavaScript inside an `onclick` attribute for example, in any HTML tag;

**1) 内联:** 在任意HTML标签中，CSS定义在一个`style`属性里，JavaScript定义在一个`onclick`属性中；

**2) Embedded:** the CSS is defined inside a `<style>` tag and the JavaScript inside a `<script>` tag;

**2) 内嵌:** CSS定义在一个 `<style>` 标签中，JavaScript定义在一个`<script>` 标签中;

**3) External:** the CSS is loaded from a `<link>` and the JavaScript from the `src` attribute of the `<script>` tag.

**3) 外部链接:** CSS通过一个 `<link>` 标签加载，JavaScript 通过  `<script>` 标签里的 `src` 属性加载。

The first two options, despite reducing the number of HTTP requests, actually increase the size of your HTML document. But this can be useful when you have small assets and the cost of making a request is greater. In this case, run tests to evaluate if there's any actual gains in speed. Also be sure to evaluate the purpose of your page and its audience: if you expect people to only visit it once, for example for some temporary campaign where you never expect return visitors, inlining/embedded code will help in reducing the number of HTTP requests.

前两种选择，尽管减少了 HTTP 请求，却增加了 HTML 文档的大小。这在您的页面资源需求少并且请求代价大的情况下很有好处的。要是是这种情况的话，进行测试看是否网页加载速度有实际性的提升。同时要评估页面的目标和受众群体：如果您的用户只需访问这个页面一次，例如一些短期的活动页面，并不需要用户回访，内联/内嵌代码就有助于减少 HTTP 请求数量。

*Avoid manually authoring CSS/JS in the middle of your HTML (automating this process with tools is preferred).*

*避免手动在 HTML 中编写 CSS/JS 代码（使用自动化工具执行这项操作更好）。*

The third option not only improves the organization of your code, but also makes it possible for the browser to cache it. This option should be preferred for the majority of cases, especially when working with large files and lots of pages.

第三种选择不仅可以提高代码的组织性，还能让浏览器对其进行缓存。大多数情况的网站都应该选择这种方式，尤其是文件较大和页面较多时。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-avoid-inlineembedded-code) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#avoid-inlineembedded-code)*

### 24 styles up top, scripts down bottom
### 24 样式放上面，脚本放下面
When we put stylesheets in the `<head>` we allow the page to render progressively, which gives the impression to our users that the page is loading quickly.

当我们把样式表放在 `<head>` 标签里时，我们允许页面逐步渲染，这会使我们的用户觉得页面加载很快。

```html
<head>
  <meta charset="UTF-8">
  <title>Browser Diet</title>

  <!-- CSS -->
  <link rel="stylesheet" href="style.css" media="all">
</head>
```

But if we put them at the end of the page, the page will be rendered without styles until the CSS is downloaded and applied.

如果我们把样式表放在页面底部的话，页面将会在没有样式的情况下被渲染出来，直到 CSS 文件下载好并被运用。

On the other hand, when dealing with JavaScript, it's important to place scripts at the bottom of the page as they block rendering while they're being loaded and executed.

另一方面，对于 JavaScript，把它们放于页面底部是很重要的，因为它们在加载和执行时会阻断页面渲染进程。

```html
<body>
  <p>Lorem ipsum dolor sit amet.</p>

  <!-- JS -->
  <script src="script.js"></script>
</body>
```

*[References-参考](https://github.com/zenorocha/browser-diet/wiki/References#styles-up-top-scripts-down-bottom)*

### 23 try out async
### 23 尝试异步
To explain how this attribute is useful for better performance, it's better to understand what happens when we don't use it.

要解释为何这个属性有助于提高性能，最好先了解一下我们不使用它时会怎样。

```html
<script src="example.js"></script>
```

In this form, the page has to wait for the script to be fully downloaded, parsed and executed before being able to parse and render any following HTML. This can significantly add to the load time of your page. Sometimes this behavior might be desired, but generally not.

在这种情况下，页面要等到脚本下载、解析和执行完毕后才会开始解析和渲染后面的 HTML 代码。这会显著增加页面的加载时间。有时候这种行为正是您需要的，但通常情况下不是。

```html
<script async src="example.js"></script>
```

The script is downloaded asynchronously while the rest of the page continues to get parsed. The script is guaranteed to be executed as soon as the download is complete. Keep in mind that multiple async scripts will be executed in no specific order.

脚本是异步下载页面的其余部分继续得到解析。脚本执行保证一旦下载完成。请记住,多个异步脚本将在任何特定的顺序执行。
脚本将会异步下载而页面的其他部分则会被继续解析。脚本一旦下载完成就会被执行。请记住，多个异步脚本不会按照指定顺序执行。

*[References-参考](https://github.com/zenorocha/browser-diet/wiki/References#try-out-async)*

## CSS
### 22 minify your stylesheets
### 22 精简样式表
To maintain readable code, it's a good idea to write comments and use indentation:

为了维持代码的可读性，编写注释和使用缩进是很好的方式：

```css
.center {
  width: 960px;
  margin: 0 auto;
}

/* --- Structure --- */

.intro {
  margin: 100px;
  position: relative;
}
```

But to the browser, none of this actually matters. For this reason, always remember to minify your CSS through automated tools.

但是对浏览器来说，这些都没有意义。因此，请牢记使用自动化工具精简您的 CSS 文件。

```css
.center{width:960px;margin:0 auto}.intro{margin:100px;position:relative}
```

This will shave bytes from the filesize, which results in faster downloads, parsing and execution.

这将会从文件大小中减去不少字节，从而获得更快的下载、解析和执行速度。

For those that use pre-processors like Sass, Less, and Stylus, it's possible to configure your compiled CSS output to be minified.

对于那些使用预处理器例如 [Sass](http://sass-lang.com/),[Less](http://lesscss.org/) 还有 [Stylus](http://learnboost.github.com/stylus/) 的人，您可以进行设置使编译输出后的 CSS 文件为已精简版本。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-minify-your-stylesheets) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#minify-your-stylesheets)*

### 23 combining multiple css files
### 23 合并多个 CSS 文件
Another best practice for organization and maintenance of styles is to separate them into modular components.

组织和维护样式的另外一个最佳实践就是将其分为一个个的模块化组件。

```html
<link rel="stylesheet" href="structure.css" media="all">
<link rel="stylesheet" href="banner.css" media="all">
<link rel="stylesheet" href="layout.css" media="all">
<link rel="stylesheet" href="component.css" media="all">
<link rel="stylesheet" href="plugin.css" media="all">
```

However, an HTTP request is required for each one of these files (and we know that browsers can only download a limited number resources in parallel).

然而，这些文件每一个都需要一个 HTTP 请求（我们知道浏览器并行下载文件的数量是有限的）。

```html
<link rel="stylesheet" href="main.css" media="all">
```

So combine your CSS. Having a smaller number of files will result in a smaller number of requests and a faster loading page.

所以，合并您的 CSS 文件。文件数量少，请求也就少，页面也会加载得更快。

Want to have the best of both worlds? Automate this process through a build tool.

想要两全其美?通过构建工具自动化实现这个过程。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-combining-multiple-css-files) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#combining-multiple-css-files)*

### 20 prefer `<link>` over `@import`
### 20 使用 `<link>` 而不是 `@import`
There's two ways to include an external stylesheet in your page: either via the `<link>` tag:

有两种方法可以在页面中引入样式表：要么是通过 `<link>` 标签：

```html
<link rel="stylesheet" href="style.css">
```

Or through the `@import` directive (inside an external stylesheet or in an inline `<style>` tag):

或者通过 `@import` 指令（写在外部样式表中或者内联 `<style>` 标签中）:

```html
import url('style.css');
```

When you use the second option through an external stylesheet, the browser is incapable of downloading the asset in parallel, which can block the download of other assets.

当您在一个外部样式表中使用第二种方式引入文件时，浏览器将无法并行下载该资源，因此下载时会阻塞其他资源的下载。

*[References-参考](https://github.com/zenorocha/browser-diet/wiki/References#prefer--over-import)*

## JAVASCRIPT
### 19 load 3rd party content asynchronously
### 19 异步加载第三方内容
Who has never loaded a third-party content to embed a Youtube video or like/tweet button?

谁不曾为了内嵌一个 Youtube 视频或者 like/tweet 按钮而加载一些第三方内容？

The big problem is that these codes aren't always delivered efficiently, either by user's connection, or the connection to the server where they are hosted. Or this service might be temporarily down or even be blocked by the user's or his company's firewall.

最大的问题是，这些代码并不总是高效传输的，无论是通过用户的连接，亦或是通过到服务器托管的地方的连接。又或者这项服务可能暂时停滞，甚至是被用户或其公司的防火墙阻断。

To avoid it becoming a critical issue when loading a site or worse, lock the entire page load, always load these codes asynchronously (or use [Friendly iFrames](https://www.facebook.com/note.php?note_id=10151176218703920)).

为了避免它在加载网页的时候成为一个关键问题，严重些的，甚至会阻断整个页面的加载，请记住总是异步加载这些代码（或者使用 [Friendly iFrames](https://www.facebook.com/note.php?note_id=10151176218703920) 方案）。

```javascript
var script = document.createElement('script'),
    scripts = document.getElementsByTagName('script')[0];
script.async = true;
script.src = url;
scripts.parentNode.insertBefore(script, scripts);
```

Alternatively, if you want to load multiple 3rd party widgets, you can asynchronously load them with using [this script](https://gist.github.com/zenorocha/5161860).

或者，如果您想要加载多个第三方小部件，您可以使用[此脚本](https://gist.github.com/zenorocha/5161860)来实现异步加载它们。

*[Video-视频](http://www.webpagetest.org/video/view.php?id=111011_4e0708d3caa23b21a798cc01d0fdb7882a735a7d) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#load-3rd-party-content-asynchronously)*

### 18 cache array lengths
### 18 缓存数组长度
The loop is undoubtedly one of the most important parts related to JavaScript performance. Try to optimize the logic inside a loop so that each iteration is done efficiently.

循环无疑是与 JavaScript 性能相关的最重要的部分之一。尽量去优化一个循环里的逻辑，使每次迭代都能够高效完成。

One way to do this is to store the size of the array that will be covered, so it doesn't need to be recalculated every time the loop is iterated.

其中一种优化做法就是存储将会使用到的数组的大小，这样每次循环迭代时就不需要重新计算值。

```javascript
var arr = new Array(1000),
    len, i;

for (i = 0; i < arr.length; i++) {
  // Bad - size needs to be recalculated 1000 times
  // 糟糕 - 数组长度需要计算 1000 次
}

for (i = 0, len = arr.length; i < len; i++) {
  // Good - size is calculated only 1 time and then stored
  // 正确 - 数组长度只要计算一次然后存起来用
}
```

*[Results on JSPerf-JSPerf上的测试结果 ](http://www.webpagetest.org/video/view.php?id=111011_4e0708d3caa23b21a798cc01d0fdb7882a735a7d)*

> **What is [jsPerf](https://jsperf.com/)?**
jsPerf aims to provide an easy way to create and share test cases, comparing the performance of different JavaScript snippets by running benchmarks. 
>[利用jsPerf优化Web应用的性能](https://software.intel.com/zh-cn/articles/optimize-web-app-using-jsperf)
>[Best of jsperf (2000-2013)](http://www.sitepoint.com/jsperf1/)

**Note:** *Although modern browsers engines automatically optimize this process, remains a good practice to suit the legacy browsers that still linger.*

**须知：** *尽管如今的浏览器引擎都会自动优化这个过程，为了适应仍旧挥之不去的老旧浏览器引擎，这仍然不失为一个良好实践。*

In iterations over collections in HTML as a list of Nodes (*NodeList*) generated for example by `document.getElementsByTagName('a')` this is particularly critical. These collections are considered "live", i.e. they are automatically updated when there are changes in the element to which they belong.

在那些以 HTML 里的一系列节点（节点列表）如由 `document.getElementsByTagName('a')` 生成的节点为集合做循环操作的例子里，这一点尤其重要。这些集合被认为是“活”的，也就是说，它们在它们所属的元素发生变化时会自动更新。

```javascript
var links = document.getElementsByTagName('a'),
    len, i;

for (i = 0; i < links.length; i++) {
  // Bad - each iteration the list of links will be recalculated to see if there was a change
  // 糟糕 - 每次迭代，链接列表都需要重新计算长度以确保没有发生变化
}

for (i = 0, len = links.length; i < len; i++) {
  // Good - the list size is first obtained and stored, then compared each iteration
  // 正确 - 列表长度先计算获取然后就存储起来，每次迭代时再用来做比较
}

// Terrible: infinite loop example
// 可怕：无限循环的例子
for (i = 0; i < links.length; i++) {
  document.body.appendChild(document.createElement('a'));
  // each iteration the list of links increases, never satisfying the termination condition of the loop
  // 每次迭代，链接列表长度都会增长，永远无法满足循环的终止条件
  // this would not happen if the size of the list was stored and used as a condition
  // 如果列表长度一开始就被储存起来用作比较条件，这种情况就不会发生
}
```

*[References-参考 ](https://github.com/zenorocha/browser-diet/wiki/References#cache-array-lengths)*

### 17 avoid document.write
### 17 避免使用 `document.write`
The use of `document.write` causes a dependency to the page on its return to be fully loaded.

`document.write` 语句需要页面完全加载完才能执行。

This (bad) practice has been abolished for years by developers, but there are still cases where its use is still required, as in synchronous fallback for some JavaScript file.

这项（糟糕的）实践早已被开发者摒弃多年，但仍有需要用到它的情况，例如作为一些 JavaScript 文件同步操作的回退机制。

[HTML5 Boilerplate](https://github.com/h5bp/html5-boilerplate/), for example, uses this technique to load jQuery locally if Google's CDN doesn't respond.

例如，[HTML5 Boilerplate](https://github.com/h5bp/html5-boilerplate/) 就使用这项技术，用来在 Google 的 *CDN* 没有响应时加载本地 jQuery 文件。

```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
<script>window.jQuery || document.write('<script src="js/vendor/jquery-1.9.0.min.js"><\/script>')</script>
```

**Attention:** *`document.write` performed during or after `window.onload` event replaces the entire content of the current page.*

**注意：** *在 `window.onload` 事件执行期间或执行完毕后执行 `document.write` 将会替换当前页面的所有内容。*

```html 
<span>foo</span>
<script>
  window.onload = function() {
    document.write('<span>bar</span>');
  };
</script>
```

The result of the final page will be only *bar* and not *foobar* as expected. The same occurs when it runs after `window.onload` event.

上面代码执行后，页面的最终呈现效果中，只会有 *bar*，而不是您以为的 *foobar*。当 `document.write` 在 `window.onload` 事件触发完后再执行也是如此。

```html
<span>foo</span>
<script>
  setTimeout(function() {
    document.write('<span>bar</span>');
  }, 1000);
  window.onload = function() {
    // ...
  };
</script>
```

The result will be the same as the previous example if the function scheduled by `setTimeout` is executed after `window.onload` event.

如果由 `setTimeout` 预定执行的函数在 `window.onload` 事件后执行，那么结果就会和上一个例子一样。

*[References-参考 ](https://github.com/zenorocha/browser-diet/wiki/References#avoid-documentwrite)*

### 16 minimize repaints and reflows
### 16 最小化 repaint(重绘)和 reflow(回流)
Repaints and reflows are caused when there's any process of re-rendering the DOM when certain property or element is changed.

由于某些属性或元素改变而引起的 DOM 重渲染就会导致 repaint 和 reflow。

Repaints are triggered when the appearance of an element is changed without changing its layout. Nicole Sullivan describes this as a change of styles like changing a `background-color`.

当一个元素外观改变而布局不变时就会触发 repaint 。Nicole Sullivan 将它描述为就是改变样式，例如改变一个 `background-color` 的值。

Reflows are the most costly, since they are caused by changing the page layout, such as change the width of an element.

Reflow 消耗最大，因为它是由改变页面布局触发的，如改变一个元素的宽度。

> **What is Reflow?**
> Reflow is the name of the **web browser process** for re-calculating the positions and geometries of elements in the document, for the purpose of re-rendering part or all of the document. Because reflow is a **user-blocking operation** in the browser, it is useful for developers to understand how to improve reflow time and also to understand the effects of various document properties (DOM depth, CSS rule efficiency, different types of style changes) on reflow time.
>[Minimizing browser reflow](https://developers.google.com/speed/articles/reflow)

There is no doubt that excessive reflows and repaints should be avoided, so instead of doing this:

毫无疑问，应该避免过多的 reflow 和 repaint，所以不要这样做:

```javascript
var div = document.getElementById("to-measure"),
    lis = document.getElementsByTagName('li'),
    i, len;

for (i = 0, len = lis.length; i < len; i++) {
  lis[i].style.width = div.offsetWidth + 'px';
}
```

Do this:

应该这样做：

```javascript
var div = document.getElementById("to-measure"),
    lis = document.getElementsByTagName('li'),
    widthToSet = div.offsetWidth,
    i, len;

for (i = 0, len = lis.length; i < len; i++) {
  lis[i].style.width = widthToSet + 'px';
}
```

When you set `style.width`, the browser needs to recalculate layout. Usually, changing styles of many elements only results in one reflow, as the browser doesn't need to think about it until it needs to update the screen. However, in the first example, we ask for `offsetWidth`, which is the layout-width of the element, so the browser needs to recalculate layout.

当您设置 `style.width` 的值时，浏览器需要重新计算布局。通常，改变多个元素的样式只会导致一次 reflow，浏览器在最终更新屏幕显示前都不需要进行 reflow。但是，在第一个例子中，我们需要得到 `offsetWidth` 的值，也就是元素的布局宽度，因此浏览器就需要重新计算布局。

If you need to read layout data from the page, do it all together before setting anything that changes layout, as in the second example.

如果您需要得到页面的布局数据，就在对布局进行任何更改前一次性获取，像第二个例子一样。

*[Demo-示例](http://jsbin.com/aqavin/2/quiet) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#minimize-repaints-and-reflows)*

### 15 avoid unnecessary dom manipulations
### 15 避免不必要的 dom 操作

Everytime you touch the DOM without needing to do so, a Panda dies.

每次您在没必要的情况下操作了 DOM，一只熊猫就会死去。

Seriously, browsing by DOM elements is costly. Although JavaScript engines are becoming increasingly powerful and fast, always prefer to optimize queries of the DOM tree.

说真的，浏览 DOM 元素消耗很大。尽管 JavaScript 引擎正变得越来越强大、快速，优化对 DOM 树的查询操作总是好的。

One of the simplest optimizations is the caching of frequently accessed DOM elements. For example, instead of querying the DOM every iteration of a loop, query it once and save the result in a variable, using that every iteration of the loop instead.

最简单的优化方式之一就是缓存使用频繁的 DOM 元素。例如，不要在一个循环的每次迭代中都查询 DOM，而是查询一次然后把值存在一个变量中，每次迭代中使用该变量。

```javascript
// really bad!
for (var i = 0; i < 100; i++) {
  document.getElementById("myList").innerHTML += "<span>" + i + "</span>";
}
```

```javascript
// much better :)
var myList = "";

for (var i = 0; i < 100; i++) {
  myList += "<span>" + i + "</span>";
}

document.getElementById("myList").innerHTML = myList;
```

```javascript
// much *much* better :)
var myListHTML = document.getElementById("myList").innerHTML;

for (var i = 0; i < 100; i++) {
  myListHTML += "<span>" + i + "</span>";
}
```

*[Results on JSPerf-JSPerf上的测试结果 ](http://jsperf.com/browser-diet-dom-manipulation/11)*

### 14 minify your script
### 14 精简脚本
Just like CSS, to maintain readable code, it's a good idea to write comments and use indentation:

就像 CSS 一样，为了维护具有可读性的代码，写注释和使用缩进是很好的做法。

```javascript
BrowserDiet.app = function() {

  var foo = true;

  return {
    bar: function() {
      // do something
    }
  };

};
```

But to the browser, none of this actually matters. For this reason, always remember to minify your JavaScript through automated tools.

但对浏览器来说，这些都不具备意义。因此，请记住总是使用自动化工具精简您的 JavaScript 代码。

```javascript
BrowserDiet.app=function(){var a=!0;return{bar:function(){}}}
```

This will shave bytes from the filesize, which results in faster downloads, parsing and execution.

这将会从文件大小中减去不少字节，从而获得更快的下载、解析和执行速度。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-minify-your-script) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#minify-your-script)*

### 13 combine multiple js files into one
### 13 将多个 js 文件合并成一个
Another best practice for organization and maintenance of scripts is to separate them into modular components.

组织和维护脚本的另一个最佳实践就是将其分离成模块化的组件。

```html
<script src="navbar.js"></script>
<script src="component.js"></script>
<script src="page.js"></script>
<script src="framework.js"></script>
<script src="plugin.js"></script>
```

However, an HTTP request is required for each one of these files (and we know that browsers can only download a limited number resources in parallel).

然而，这些文件每一个都需要一个 HTTP 请求（我们知道浏览器并行下载文件的数量是有限的）。

```html
<script src="main.js"></script>
```

So combine your JS. Having a smaller number of files will result in a smaller number of requests and a faster loading page.

所以，合并您的 JS 文件。文件数量少，请求也就少，页面也会加载得更快。

Want to have the best of both worlds? Automate this process through a build tool.

想要两全其美?通过构建工具自动化实现这个过程。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-combine-multiple-js-files-into-one) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#combine-multiple-js-files-into-one)*

## JQUERY
### 12 always use the latest version of jquery
### 12 总是使用最新版本的 jquery
The core jQuery team is always looking to bring improvements to the library, through better code readability, new functionality and optimization of existing algorithms.

jQuery 核心团队总是希望能够通过更好的代码可读性，新的功能和对现有算法的优化来给库带来改进。

For this reason, always use the latest version of jQuery, which is always available here, if you want to copy it to a local file:

为此，请总是使用最新版本的 jQuery，如果您想将其拷贝到本地的话，可从这里获取：

> http://code.jquery.com/jquery-latest.js

But *never* reference that URL in a `<script>` tag, it may create problems in the future as newer versions are automatically served to your site before you've had a chance to test them. Instead, link to the latest version of jQuery that you need specifically.

但是*永远不要*在一个 `<script>` 标签里引用那个 URL 地址，那样在未来可能会产生问题，因为更新版本的代码库会自动提供给您的网站使用而您还没来得及去测试它。相反，链接到您需要的特定的 jQuery 的最新版本。

```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
```

Just like the wise [Barney Stinson](https://browserdiet.com/img/new-is-always-better.gif) says, *"New is always better"* :P

就像明智的 [Barney Stinson](https://browserdiet.com/img/new-is-always-better.gif) 说的，*“新的总是更好 ”* :P

*[References-参考](https://github.com/zenorocha/browser-diet/wiki/References#always-use-the-latest-version-of-jquery)*

### 11 selectors
### 11 选择器
Selectors is one of the most important issues in the use of jQuery. There are many different ways to select an element from the DOM, but that doesn't mean they have the same performance, you can select an element using classes, IDs or methods like `find()`, `children()`.

使用 jQuery 最重要的问题之一就是选择器。从 DOM 中选择一个元素有很多不同的方式，但这并不意味着它们有相同的性能，您可以使用类，ID 或者像 `find()`、`children()` 这样的方法来选择一个元素。

Among all of them, select an ID is the fastest one, because its based on a native DOM operation:

在它们之中，选择 ID 是最快的一种，因为它是基于原生的 DOM 操作的：

```javascript
$("#foo");
```

*[Results on JSPerf-JSPerf上的测试结果 ](http://jsperf.com/browser-diet-jquery-selectors)*  

### 10 use for instead of each
### 10 使用 `for` 而不是 `each`
The use of native JavaScript functions nearly always results in faster execution than the ones in jQuery. For this reason, instead of using the `jQuery.each` method, use JavaScript's own `for` loop.

使用原生的 JavaScript 函数几乎总是比 jQuery 里的那些有更快的执行速度。因此，使用 JavaScript 自己的 `for` 循环而不是 `jQuery.each` 方法。

But pay attention, even though `for in` is native, in many cases it performs worse than `jQuery.each`.

但是注意，即使 `for in` 是原生方法，在许多情况下，它的执行效率比 `jQuery.each` 更糟糕。

And the tried and tested `for` loop gives us another opportunity to speed things up by caching the length of collections we're iterating over.

`for` 循环的尝试和测试给了我们另一个加快运行速度的机会，也就是将需要遍历的集合的长度缓存起来。

```javascript
for ( var i = 0, len = a.length; i < len; i++ ) {
    e = a[i];
}
```

The use of reverse `while` and reverse `for` loops is a hot topic in the community and are frequently cited as the fastest form of iteration. However it's typically avoided for being less legible.

使用反向 `while` 和反向 `for` 循环是社区里的一个热门话题，经常被援引为是最快的迭代形式。不过因为它可读性较差，人们通常避免使用它。

```javascript
// reverse while
while ( i-- ) {
  // ...
}

// reverse for
for ( var i = array.length; i--; ) {
  // ...
}
```

*[Results on JSPerf-JSPerf上的测试结果 ](http://jsperf.com/browser-diet-jquery-each-vs-for-loop) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#use-for-instead-of-each)*  

### 9 don't always use jquery...
### 9 别总使用 jquery
Sometimes vanilla JavaScript can be easier to use and have better performance than jQuery.

有时候原生 Javascript 比 jQuery 更易于使用并且有更好的性能表现。

> **[What is VanillaJS?](http://stackoverflow.com/questions/20435653/what-is-vanillajs)**
VanillaJS is a name to refer to using plain JavaScript without any additional libraries like jQuery. People use it as a joke to remind other developers that many things can be done nowadays without the need for additional JavaScript libraries.

Consider the following HTML:

基于下面的 HTML 代码：

```html
<div id="text">Let's change this content</div>
```

Instead of doing this:

不这么写：

```javascript
$('#text').html('The content has changed').css({
  backgroundColor: 'red',
  color: 'yellow'
});
```

Use plain JavaScript:

而是使用原生 Javascript：

```javascript
var text = document.getElementById('text');
text.innerHTML = 'The content has changed';
text.style.backgroundColor = 'red';
text.style.color = 'yellow';
```

It's simple and **much** faster.

这样简单并且运行速度**更**快。

*[Results on JSPerf-JSPerf上的测试结果 ](http://jsperf.com/jquery-vs-javascript-performance-text) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#dont-use-jquery)*

## IMAGES
### 8 use css sprites
### 8 使用 css 精灵图
This technique is all about grouping various images into a single file.

这种技术是将多个图片组成一个图片文件。

![示例图片](https://browserdiet.com/assets/img/sprite-example.jpg)

And then positioning them with CSS.

然后使用 CSS 给它们定位。

```css
.icon-foo {
  background-image: url('mySprite.png');
  background-position: -10px -10px;
}

.icon-bar {
  background-image: url('mySprite.png');
  background-position: -5px -5px;
}
```

As a result, you've dramatically reduced the number of HTTP requests and avoided any potential delay of other resources on your page.

由此，您成功的使 HTTP 请求的数量大幅减少了并且避免了页面上其他资源潜在的延迟问题。

When using your *sprite*, avoid leaving too much white space between images. This won't affect the size of the file, but will have an effect on memory consumption.

当使用 *精灵图* 的时候，避免在图片之间留有太多空白。这不会影响文件的大小，但会影响内存消耗。

And despite nearly everyone knowing about sprites, this technique isn't widely used—perhaps due to developers not using automated tools to generate sprites. We've highlighted a few that can help you out with this.

尽管几乎每个人都知道精灵图，这种技术却没有被广泛使用，可能是由于开发人员没有使用自动化的工具来生成它们。我们着重介绍了一些能帮到您的工具。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-use-css-sprites) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#use-css-sprites)*

### 7 data uri
### 7 数据 uri
This technique is an alternative to using CSS sprites.

这项技术是除了使用 CSS 精灵图之外的另一种方法。

A [Data-URI](http://en.wikipedia.org/wiki/Data_URI_scheme) is a way to inline the content of the URI you would normally point to. In this example we are using it to inline the content of the CSS background images in order to reduce the number of HTTP requests required to load a page.

Data-URI 是一个用来内联您通常指向的内容的方法。在下面这个例子里，我们使用 Data-URI 来内联 CSS 背景图片的内容，以此来减少加载页面所需的 HTTP 请求数。

Before:

以前：

```css
.icon-foo {
  background-image: url('foo.png');
}
```

After:

现在：

```css
.icon-foo {
  background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAAA1BMVEUAAACnej3aAAAAAXRSTlMAQObYZgAAAApJREFUCNdjYAAAAAIAAeIhvDMAAAAASUVORK5CYII%3D');
}
```

All browsers from IE8 and above support this base64-encoded technique.

所有浏览器包括 IE8 及以上都支持这种 base64 编码的技术。

Both this method and the CSS spriting method require build time tools to be maintainable. This method has the advantage of not requiring manual spriting placement as it keeps your images in individual files during development.

这种方法和 CSS 精灵图方法都需要构建工具来使其具有可维护性。它的优点是不需要您手动放置精灵图上各个图片的位置，使您在开发过程中仍旧可以使用独立的图片文件。

However has the disadvantage of growing the size of your HTML/CSS considerably if you have large images. It is not recommended to use this method if you aren't gzipping your HTML/CSS during HTTP requests as the size overhead might negate the speed gains you get from minimizing the number of HTTP requests.

它的不足之处在于如果您的图片很大，您的 HTML/CSS 文件大小将大幅增长。如果您不打算在进行 HTTP 请求时压缩 HTML/CSS 文件，那就不推荐您使用这种方法，因为请求大小的开销有可能会抵掉您精简 HTTP 请求后所获得的速度增长。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-data-uri) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#data-uri)*

### ６ don't rescale images in markup
### ６ 不要在标签中重新调节图像大小
Always define the `width` and `height` attributes of an image. This will help avoid unnecessary repaints and reflows during rendering.

总是定义图片的 `width` 和 `height` 属性。这可以避免渲染时有不必要的重绘和回流。

```html
<img width="100" height="100" src="logo.jpg" alt="Logo">
```

Knowing this, John Q. Developer who has a *700x700px* image decides to use this technique to display the image as *50x50px*.

知道了这一点，John Q. Developer 有一个 *700x700px* 的图片，他决定使用这项技术来使图片显示为 *50x50px*。

What Mr. Developer doesn't realize is that dozens of unnecessary *kilobytes* will be sent over the wire.

Developer 先生没有意识到的是，许多不必要的字节将通过网络发送出去。

Always keep in mind: just because you can define with width and height of an image in HTML doesn't mean you should do this to rescale down large images.

永远记住：仅仅因为您可以在 HTML 中定义图像的宽度和高度，并不意味着您应该使用这种方法来将大图片调小。

*[References-参考](https://github.com/zenorocha/browser-diet/wiki/References#dont-rescale-images-in-markup)*

### 5 optimize your images
### 5 优化图片
Image files contain a lot of information that is useless on the Web. For example, a JPEG photo can have *Exif* metadata from the camera (date, camera model, location, etc.). A PNG contains information about colors, metadata, and sometimes even a miniature embedded thumbnail. None of this is used by the browser and contributes to filesize bloat.

图像文件包含大量对于互联网来说无用的信息。比如，一张 JPEG 照片会带有从相机里获取的 *Exif* 元数据(日期、相机型号、地理位置，等等)。一张 PNG 图片会带有关于颜色、元数据的信息，有时候甚至是一张微型的嵌入式缩略图。这些对浏览器来说没有任何用处，并且会使文件大小膨胀。

There are tools that exist for image optimization that will remove all this unnecessary data and give you a slimmer file without degrading quality. We say they perform *lossless* compression.

存在一些用于优化图片的工具，可以删除所有不必要的数据并且不会降低文件质量，给您一个精简的文件。我们说它们是执行了*无损* 压缩。

Another way to optimize images is to compress them at the cost of visual quality. We call these *lossy* optimizations. When you export a JPEG, for example, you can choose the quality level (a number between 0 and 100). Thinking about performance, always choose the lowest number possible where the visual quality is still acceptable. Another common lossy technique is to reduce the color palette in a PNG or to convert PNG-24 files into PNG-8.

优化图片的另一种方法是以牺牲视觉质量为代价进行压缩。我们称其为*有损* 优化。比如，当您导出一张 JPEG 图片时，您可以选择质量等级(0到100之间的数字)。考虑到显示问题，总是选择视觉质量仍可接受的最小的数字。另一个常见的有损压缩技术是减少 PNG 文件中的颜色调色板或将 PNG-24 文件转换成 PNG-8。

To improve user perceived performance, you should also transform all your JPEG files in progressive ones. Progressive JPEGs have great browser support, are very simple to create and have no significant performance penalty. The upside is that the image will appear sooner on the page ([see demo](http://www.patrickmeenan.com/progressive/view.php?img=http://farm2.staticflickr.com/1434/1002257937_021cb46a33_o.jpg)).

为了改善用户可感知的性能表现，您也应该将所有的 JPEG 文件转换成渐进式的。渐进式 JPEG 技术为大多数浏览器所支持，非常易于创建并且没有什么显著的性能损失。它有利的一面是，页面上的图像能更早显示([见演示](http://www.patrickmeenan.com/progressive/view.php?img=http://farm2.staticflickr.com/1434/1002257937_021cb46a33_o.jpg))。

*[Useful tools-有用的工具](https://github.com/zenorocha/browser-diet/wiki/Tools#wiki-optimize-your-images) / [References-参考](https://github.com/zenorocha/browser-diet/wiki/References#optimize-your-images)*

## BONUS
### 4 diagnosis tools: your best friend
### 4 诊断工具：您最好的朋友
If you'd like to venture into this world of web performance, it's crucial to install the [YSlow](http://yslow.org/) extension in your browser—they'll be your best friends from now on.

如果您想在网页性能的世界里展开冒险，在您的浏览器里安装 [YSlow](http://yslow.org/) 扩展是至关重要的——从现在开始，它们将会是您最好的朋友。

Or, if you prefer online tools, visit the [WebPageTest](http://www.webpagetest.org/), [HTTP Archive](http://httparchive.org/), or [PageSpeed](https://developers.google.com/speed/pagespeed/insights/) sites.

又或者，如果您更喜欢在线工具的话，可以访问 [WebPageTest](http://www.webpagetest.org/), [HTTP Archive](http://httparchive.org/), 或者 [PageSpeed](https://developers.google.com/speed/pagespeed/insights/) 网站。

In general each will analyse your site's performance and create a report that gives your site a grade coupled with invaluable advice to help you resolve the potential problems.

一般来说，这些工具都将分析您的网站的性能，然后生成一个报告，给您的网站打分，并提供宝贵的建议来帮助您解决潜在的问题。

### 3 that's it for today!
### 3 今天就到这里了！
We hope that after reading this guide you can get your site in shape. :)

我们希望阅读完这篇指南能帮助您完成您的网站。:)

And remember, like all things in life, [there's no such thing as a silver bullet](http://www.cs.nott.ac.uk/~cah/G51ISS/Documents/NoSilverBullet.html). Performance tuning of your application is a worthwhile venture, but should not be the sole basis of all your development decisions—at times you'll need to weigh out the costs and benefits.

记住，就像生活中的所有事情，[没有所谓的良方](http://www.cs.nott.ac.uk/~cah/G51ISS/Documents/NoSilverBullet.html)。应用程序的性能调优是一件值得投入的事情，但不应该是您所有开发决策的唯一考虑依据——有时候您需要权衡利弊。

Want to learn more? Check out the [References](https://github.com/zenorocha/browser-diet/wiki/References) that we used to write this guide.

想要学习更多？查看我们用来写这篇指南的[参考资料](https://github.com/zenorocha/browser-diet/wiki/References)。

Have suggestions? Send a tweet to [@BrowserDiet](http://twitter.com/browserdiet/) or a [pull request](https://github.com/zenorocha/browser-diet) on Github.

有建议？向 [@BrowserDiet](http://twitter.com/browserdiet/) 发送一条推特，或者在 Github 上发送一条 [pull request](https://github.com/zenorocha/browser-diet)。

Don't forget to share with your friends, let's make a faster web for everyone. \o/

别忘了和您的朋友分享，让我们一起为每个人创建一个更快的网站。\o/
