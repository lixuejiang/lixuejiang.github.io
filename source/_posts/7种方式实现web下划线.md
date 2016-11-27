---
title: 7种方式实现web下划线
date: 2016-11-27 10:38:05
tags: [翻译,前端]
---

本文来自[css-tricks](https://css-tricks.com/styling-underlines-web/)，介绍了在不同的场景下实现下划线的7种方式。

# 简介
有许多种不同的方式来实现下划线，你也许还记得[Crafting link underlines on Medium](https://medium.com/design/7c03a9274f9)这篇文章。Medium也不是想做什么疯狂的[事情](http://tympanus.net/Development/InlineAnchorStyles/)，他们只是想让他们的文字下面好看一点。

<!--more-->

![http://ggbond.qiniudn.com/2016-11-27%2010:46:33.png](http://ggbond.qiniudn.com/2016-11-27%2010:46:33.png)
这是一条基本的下划线，它大小合适。绝对比浏览器的默认样式好看的多。所以，看上去Medium为了实现这个效果也经历了许多困难。两年后，实现一个好看的下划线依然一样困难

# 目标
为什么不能直接用`text-decoration: underline`呢？如果我们谈论的是理想场景，underline需要满足下面这些场景：

* 在基线下面定位
* 跳过[descenders](https://en.wikipedia.org/wiki/Descender)
* 改变颜色，厚度（thickness）和样式
* 重复包裹文字
* 在任何背景下都生效

我认为这些需求都是很合理的，但是到目前为止，没有一种直观的方式用css来实现上面的所有目标

# 方法
因此，能实现下划线的方法都有哪些？

* `text-decoration`
* `border-bottom`
* `box-shadow`
* `background-image`
* `SVG filters`
* `Underline.js (canvas)`
* `text-decoration-*`

让我们来一个个的讨论这些方法的好处和坏处

## text-decoration
`text-decoration`是最直接的实现方式，只用一个属性就能实现给文字添加下划线。当字体比较小的时候，看上去没什么问题，但是当字体变大的时候，看上去就有点笨拙了。

{% codepen lixuejiang aByLLO 0 %}

`text-decoration`最大的问题是缺少定制化，它直接使用文字的颜色和字体，不能跨浏览器修改样式，更多内容后面会介绍

### 好处

* 使用方便
* 定位在基线下面
* 在Safari和iOS里跳过了descenders
* 多行也没问题
* 所有的背景也ok

### 坏处

* 在其他浏览器里不能跳过descenders
* 不能修改颜色，厚度和样式

## border-bottom
`border-bottom`在快速和定制化之间提供了一个很好的平衡。这个方法用了css边框，你可以修改颜色，厚度和样式。
`inline`元素的`border-bottom`长这样

{% codepen lixuejiang XNaeVW 0 %}

最大的问题是下划线离文本有多远--下划线就在descenders下面。你可以通过让元素变成`inline-block`然后减少`line-height`来改变这个距离。但是你就失去了包裹文本的能力。单行元素没问题，其他情况就不行了。

{% codepen lixuejiang xRLXYZ 0 %}

另外，你可以用`text-shadow`来盖住descenders附近的部分，但是你需要用和背景色一样的颜色。这一位置他只能在固定颜色的背景上生效，渐变的背景和背景图没用。

### 好处

* 用`text-shadow`可以跳过descenders
* 可以改变颜色，厚度和样式
* 可以对颜色和厚度实施渐变和动画
* `inline-block`自动包裹
* 用`text-shadow`可以支持任意背景

### 坏处

* 定位很远，而且很难重定位
* 需要使用一系列不相干的属性
* Janky text selection when using text-shadow ？？

## box-shadow
`box-shadow`需要两个`inset box shadows`来实现下划线：其中一个创建一个矩形，另外一个盖在上面。这种方法要能生效的前提是使用固定颜色的背景

{% codepen lixuejiang eBEeOp 0 %}
你也可以用`text-shadow`来模拟文字的descenders和underline之间的空白。但是如果下划线和文字的颜色不一样的时候，或者空白最够薄的时候，他就不能像`text-decoration`一样很好的工作了。

### 好处

* 可以定位在基线下面
* 用`text-shadow`可以跳过descenders
* 可以改变颜色，厚度
* 多行也能生效

### 坏处

* 不能修改样式
* 不能在渐变和背景图的情况下生效

## 背景图
`background-image`是最接近我们想要的而且问题最少的实现方式。想法是用线性渐变和`background-position`来创建一个在水平方向重复的背景图。然后设置元素`display: inline`

{% codepen lixuejiang LbjOEj 0 %}

下面这个方法没有用线性渐变，而是用的背景图，你可以用你自己的图片来实现一些很酷的效果：
{% codepen lixuejiang RoZjNy 0 %}

### 好处

* 可以定位在基线下面
* 用`text-shadow`可以跳过descenders
* 可以改变颜色，厚度（允许0.5像素）和样式
* 可以使用自定义图片
* 多行也能生效
* 用`text-shadow`可以支持任意背景

### 坏处
* 在不同的分辨率，不同的浏览器和不同的缩放情况下，图片的尺寸不一样


## SVG filters
这种方法我一直在研究。你可以创建一个画了一条线的SVG filter行内元素，把他该在文本上面。然后给这个元素设置一个ID，之后就可以在css里引用这个元素：`url(‘#svg-underline’)`
其中一个优势就是在不依赖`text-shadow`的情况下支持透明。这也就意味着在任何背景上都可以跳过descenders，包括渐变和背景图。但是他只支持单行的文本

{% codepen lixuejiang bBrYEJ 0 %}

在 Chrome 和 Firefox里看起来像这样：
![http://ggbond.qiniudn.com/2016-11-27%2013:27:08.png](http://ggbond.qiniudn.com/2016-11-27%2013:27:08.png)
IE, Edge, 和 Safari的浏览器支持有问题，很难在css里测试浏览器对SVG filter的支持。可以用`@supports`属性。但是这只能测试引用是否生效。而不是filter自己。这个方法不支持跨浏览器。


### 好处

* 可以定位在基线下面
* 跳过descenders
* 可以改变颜色，厚度和样式
* 支持任意背景

### 坏处

* 不支持多行
* 在 IE, Edge, 或者 Safari里不生效，在这几个浏览器里可以用`text-decoration`


## Underline.js (Canvas)
Underline.js很屌，如果你还没看过他的[tech demo](http://underlinejs.org/),赶紧停下来，去看一看。有一个很酷炫的时长9分钟的介绍它是如何工作的视频。在这里我会简短的介绍一下：它用`<canvas>`标签来画下划线。这是一种很新颖的方法，而且能效果出奇的好。

尽管有名气。Underline.js只是个demo。你不能在不修改他们的代码的情况下直接在你的项目里引用它。
在这里把它作为一种实现方式来讨论还是很值得的。`<canvas>`可以实现漂亮的可交互的下划线。但是你必须得写一些JavaScript代码。

## text-decoration-* 属性
还记得`text-decoration`里提到的后面会详细介绍吗？
`text-decoration`自身也能很好的工作，你也可以添加一些实验性的属性来定义样式：

Remember the “more on that later” part? Well, here we are.

* text-decoration-color
* text-decoration-skip
* text-decoration-style

也不要太激动。需要考虑浏览器兼容性

### TEXT-DECORATION-COLOR
`text-decoration-color`允许你改变下划线的颜色，可以和文字颜色不一样。它的跨浏览器支持比想象中的要好。在Firefox和Safari里都能正常运行。
但是有一个问题：如果没有清除descenders，Safari会把下划线放在文字上面🙃
Firefox：
![http://ggbond.qiniudn.com/2016-11-27%2013:47:03.png](http://ggbond.qiniudn.com/2016-11-27%2013:47:03.png)

Safari：
![http://ggbond.qiniudn.com/2016-11-27%2013:47:57.png](http://ggbond.qiniudn.com/2016-11-27%2013:47:57.png)

### TEXT-DECORATION-SKIP
`text-decoration-skip`可以跳过descenders
![http://ggbond.qiniudn.com/2016-11-27%2013:49:05.png](http://ggbond.qiniudn.com/2016-11-27%2013:49:05.png)

这个属性是非标准的，现在只有Safari支持。所以需要加-webkit-前缀。Safari默认启用这个属性，这也就是为什么在没有指定这个值的时候，下划线也跳过了descenders
如果你在用Normalize，你应该知道最近的版本禁止了这个属性，来保证各个浏览器的显示一直。如果你想要一些梦幻般的下划线，你可以打开这个属性

### TEXT-DECORATION-STYLE
`text-decoration-style`提供了像`border-style`一样的属性。另外还添加了`wavy lines`:
 
* dashed
* dotted
* double
* solid
* wavy

目前只有FireFox支持这个属性：
![http://ggbond.qiniudn.com/2016-11-27%2013:55:21.png](http://ggbond.qiniudn.com/2016-11-27%2013:55:21.png)

是不是看上去很眼熟？


### 少了什么？
`text-decoration-*`这一系列属性确实比用其他的css属性来实现下划线要直观。但是看一看我们一开始的需求。这些属性不支持定义厚度和位置。

经过一番研究，发现了下面两个属性：

* text-underline-width
* text-underline-position

他们出现在早起的css草案里，但是因为缺少利润而没被实现。不用怪他们（指浏览器厂商）。

# 结论

所以实现下划线最好的方式是什么？

[这就要看实际情况了](https://codepen.io/rachsmith/details/YweZbG/)
对于字体小的问题，我推荐`text-decoration`，然后用`text-decoration-skip`。在大多数浏览器里看起来有点平淡。但是下划线就那样，人们也不会介意。但是如果你足够耐心，总是有机会的。总有一天，下划线在不需要更改任何东西的情况下变得很帅气。

对于body text，可以用`background-image`。它能生效，而且很不错，还有Sass mixins。如果下划线很细或者需要和文本不一样 的颜色的时候，还可以用`text-shadow`

对于单行的文本，可以用`border-bottom`.
如果想跳过渐变和背景图上的descenders，用SVG filters。
将来的浏览器支持足够好了，答案是`text-decoration-* `属性
