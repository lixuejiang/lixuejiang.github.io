---
title: emmet
date: 2015-03-15 13:06:22
tags: emmet
---
## html
### 后代：>
nav>ul>li
```
<nav>
<ul>
<li></li>
</ul>
</nav>
```
### 兄弟：+
div+p+bq
```
<div></div>
<p></p>
<blockquote></blockquote>
```
### 上级：^
div+div>p>span+em^bq
```
<div></div>
<div>
<p><span></span><em></em></p>
<blockquote></blockquote>
</div>
```

<!--more-->

### 分组：()
div>(header>ul>li*2>a)+footer>p
```
<div>
<header>
<ul>
<li><a href=""></a></li>
<li><a href=""></a></li>
</ul>
</header>
<footer>
<p></p>
</footer>
</div>
```
### 乘法：*
ul>li*5
```
<ul>
<li></li>
<li></li>
<li></li>
<li></li>
<li></li>
</ul>
```
### 自增符号：$
ul>li.item$*5
```
<ul>
<li class="item1"></li>
<li class="item2"></li>
<li class="item3"></li>
<li class="item4"></li>
<li class="item5"></li>
</ul>
```
### 占位自增$$
ul>li.item$$$*5
```
<ul>
<li class="item001"></li>
<li class="item002"></li>
<li class="item003"></li>
<li class="item004"></li>
<li class="item005"></li>
</ul>
```
### 自减符合：$@
ul>li.item$@-*5
```
<ul>
<li class="item5"></li>
<li class="item4"></li>
<li class="item3"></li>
<li class="item2"></li>
<li class="item1"></li>
</ul>
```
### 从某个数字开始自增：$@3
ul>li.item$@3*5
```
<ul>
<li class="item3"></li>
<li class="item4"></li>
<li class="item5"></li>
<li class="item6"></li>
<li class="item7"></li>
</ul>
```
### 文本：{}
a{Click me}
```
<a href="">Click me</a>
```
### ID和类属性
* `#header`
```
<div id="header"></div>
```
* .title
```
<div class="title"></div>
```
* form#search.wide
```
<form id="search" class="wide"></form>
```
* p.class1.class2.class3
```
<p class="class1 class2 class3"></p>
```

### 自定义属性
* p[title="Hello world"]
```
<p title="Hello world"></p>
```
*td[rowspan=2 colspan=3 title]
```
<td rowspan="2" colspan="3" title=""></td>
```

* [a='value1' b="value2"]
```
<div a="value1" b="value2"></div>
```

### 缩写：
* !


```
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Document</title>
</head>
<body>

</body>
</html>
```
### 所有的html标签的缩写
input:hidden
```
<input name="" type="hidden"/>
```
冒号后指定类型

### 支持自定义标签

文章内容来自[segmentfault](http://segmentfault.com/blog/trigkit4/1190000000661601)和[官网]()

## CSS