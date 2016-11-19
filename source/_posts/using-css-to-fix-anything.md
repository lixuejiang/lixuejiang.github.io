---
title: using css to fix anything
date: 2016-09-17 16:14:03
tags: [css,翻译]
---
译自[Using CSS to Fix Anything: 20+ Common Bugs and Fixes](http://www.noupe.com/design/using-css-to-fix-anything-20-common-bugs-and-fixes.html)
毫无疑问，一个有逻辑和结构化的布局是最可行的。不仅仅是因为你的布局会在不同的浏览器之间变化，还因为css有很多种方式来定位一个元素。今天我们要分享一些技巧来避免创建css布局的陷阱。
这是这一序列文章的第一部分。外面有那么多的好的技巧，如果你发现有更简单或者更好的方法，请你在文章的末尾发表评论或者给我发邮件。我会尽我所能在这一系列的下一篇文章中包含进来。

<hr>
你也许对该系列的其他文章感兴趣：
* [Using CSS to Do Anything: 50+ Creative Examples and Tutorials](http://www.noupe.com/design/using-css-to-do-anything-50-creative-examples-and-tutorials.html)
* [101 CSS Techniques Of All Time- Part 1](http://www.noupe.com/design/101-css-techniques-of-all-time-part-1.html)
* [101 CSS Techniques Of All Time- Part 2](http://www.noupe.com/design/101-css-techniques-of-all-time-part2.html)

<!--more-->
<hr>
# 修复IE的bug
## 1.[双边距bug](http://www.cssnewbie.com/double-margin-float-bug/)
这是IE独家的bug。当一个元素浮动时，如果设置了和浮动方向一致的外边距，则元素最终的外边距是指定的两倍。要解决这个问题很简单，你只需要设置浮动元素为`display：inline`.代码如下：
```
#content {
    float: left;
    width: 500px;
    padding: 10px 15px;
    margin-left: 20px; }
To something like this:

#content {
    float: left;
    width: 500px;
    padding: 10px 15px;
    margin-left: 20px; 
    display:inline;
}
```
![http://noupe.com/img/cb11.gif](http://noupe.com/img/cb11.gif)

## 2.Overcoming the Box Model Hack
如果你想指定任何div的宽度，不要设置padding和marin。而是创建一个未设置宽度的子div，然后指定其padding和margins，代码如下：
```
#main-div{
	width: 150px;
}
#main-div div{
	border: 5px;
	padding: 20px;
}
```
而不是：
```
#main-div{
	width: 150px;
	border: 5px;
	padding: 20px;
}
```
## 3.[IE6不识别min-height](http://www.cssplay.co.uk/boxes/minheight.html)
“min-height” 在火狐里能正常运行，但是IE6会忽略它，IE6的height和火狐的min-height功能一样。
> IE7解决了这个问题。

```
/* for understanding browsers */
.container {
	width:20em;
	padding:0.5em;
	border:1px solid #000;
	min-height:8em; 
	height:auto;
}
/* for Internet Explorer */
/*\*/
* html .container {
	height: 8em;
}
/**/
```
## 4.[IE6的最小宽度](http://www.cssplay.co.uk/boxes/minwidth.html)
IE6中最小宽度的一种实现
<hr>
# 居中块元素
## 5.水平居中块元素
有很多种方式来水平居中一个块元素，选择哪种方式取决于元素的尺寸是设置的百分百还是绝对值。水平居中整个页面的方法如下：
```
body{
	text-align: center;
}
#container
{
	text-align: left;
	width: 960px;
	margin: 0 auto;
}
```
## 6.垂直居中
原文写的不太对，这里就不翻译了。有兴趣的可以看[https://css-tricks.com/centering-css-complete-guide/](https://css-tricks.com/centering-css-complete-guide/)这篇文章。或者用[http://howtocenterincss.com/](http://howtocenterincss.com/)这个网页提供的代码，居中你的元素。

# 表格列问题
## 7.[你的css表格混乱不堪的原因](http://warpspire.com/posts/css-column-tricks/)
通过一些有用的图表和代码段，告诉你怎么修复css的列问题。通俗易懂
## 8.[扩大盒子的bug](http://www.positioniseverything.net/explorer/expandingboxbug.html)
当你想创建一个两列的浮动布局时，IE会创建一个“float drop”。这是因为在一个固定宽带的div里，内容宽带超过了div的宽度的时候，div后面的内容会被往后挤。

![http://noupe.com/img/cb12.gif](http://noupe.com/img/cb12.gif)

# CSS定位
# CSS浮动
# 更简单的圆角实现
# CSS表单问题
# 其他

