---
title: CSS Writing Mode
date: 2017-01-25 09:46:00
tags: [翻译,CSS]
---

最近需要实现一个文字竖排的效果，类似下图这种效果，所以研究了一下css的writing mode，顺便翻译了一篇[文章](https://24ways.org/2016/css-writing-modes/)
![css write mode](http://ggbond.qiniudn.com/2017-01-25%2009:48:57.png)

<!--more-->
如果你没有太多时间，我就从结果开始，来点餐前小菜。你可以用一些鲜为人知的，但是很重要的而且很强大的css属性来让文本竖着排列。就像下面这样：

![](http://ggbond.qiniudn.com/2017-01-25%2009:54:21.png)

除了竖排文本，还可以用同样的方法来设置icon和其他的按钮，当然了也可以设置任何其他的元素。

我用的css属性改变了浏览器思考元素方向的方式，把元素相当于原来正常的方向旋转了90°。看一下下面的codepen，高亮H1,看看鼠标的显示：

{% codepen lixuejiang apypQr  1 %}

代码很简单：

```

h1 {
  writing-mode: vertical-rl;
}

```

这个属性改变了h1的属性模式，从默认的水平从上到下变成了垂直从左到右。如果把这个属性运用到了html跟元素，则整个页面的书写方式都会改变。甚至会影响滚动条的方向。

在上面的例子里，只是改变了h1的书写方式，其他元素并不受影响。

现在餐前小菜已经结束，下面进入正餐，解释一下[css 书写模式规范](https://drafts.csswg.org/css-writing-modes/)

# 为什么需要学习wtiting modes

有三个理由需要学习这个属性，包括西方的听众，我会解释整个系统，而不是快速告诉你结果：

- 1.我们生活的世界是很大，学习其他语言很好玩，你们当中的许多人也许用中文，日文或者韩文来写你们的页面。
- 2.用writing-mode来改变字节的侧面很酷。这个css属性可以用在所有你能想到的地方，即使你只使用英语
- 3.最重要的是，理解书写模式对理解Flexbox和 CSS Grid很有帮助。在学习Writing Mode之前，我觉得我的知识体系很有一个很大的坑，我并没有完全理解Grid和Flexbox的工作方式。当我接触到Writing Mode之后，Grid和Flexbox都变得容易起来。也能理解align-*和justify-*这类对齐属性。

不管你是否知道，writing mode是每一个布局首先创建的块。你可以使用默认的LTR方向的，垂直的TTB方向，也可以指定特定的值：

# CSS 属性

writing-mode的5个值：

```

writing-mode: horizontal-tb;
writing-mode: vertical-rl;
writing-mode: vertical-lr;
writing-mode: sideways-rl;
writing-mode: sideways-lr;

```

[css 书写规范](https://drafts.csswg.org/css-writing-modes/)支持了一个很宽范围的书写语言。这很复杂，但是全球的书写语言的演变确是很简单的。

首先我会介绍一下现有的书写系统的基本布局方式。然后再看看上面这些属性做了什么：

# 行方向，块方向，字符方向

在web的世界里有‘block’ 和 ‘inline’两种布局方式，如果你曾经写过`display: block`和`display: inline`,那么你已经了解了这方面的知识。

block在垂直方向是默认从上往下排列的。

![](http://ggbond.qiniudn.com/2017-02-19%2009:54:12.png)

Inline是按照每一行的方式排列的。在水平方向的默认方式是从左到右，想象一下你正在读的这段文字，是用一个打字机一个个字符打出来的，这就是inline的方向。

![](http://ggbond.qiniudn.com/2017-02-19%2009:54:39.png)

而字符的方向是字符点的方向，如果你输入一个大写字母“A”，哪一边才是字符的顶部呢？不同的语言有不同的方向。大多数语言是指向页面的顶部，但是不是所有的语言都这样


![](http://ggbond.qiniudn.com/2017-02-19%2009:55:09.png)

综合起来看

![](http://ggbond.qiniudn.com/2017-02-19%2009:55:39.png)

>默认的样式看起来像这样

现在我们知道了block, inline, 和 character的不同，来看看在不同的书面语言系统里是怎么样的

# CSS Writing Mode的四种书写系统

css规范包含了主要的四种书写系统：拉丁语系，阿拉伯语系，汉语和蒙古语

## 拉丁语系

拉丁语系覆盖了时间70%的人口

![](http://ggbond.qiniudn.com/2017-02-19%2009:57:59.png)

文本是水平的，从左到右，块方向是从上到下的。

之所以叫拉丁语系，是它包含了所有使用拉丁字母的语言，包括英语，西班牙语，德语，法语还有许多其他的语言。另外还有一些不使用拉丁字母的语言也属于这个语系，比如希腊，西里尔字母（俄罗斯，乌克兰，保加利亚，塞尔维亚等等），还有Brahmic（梵文，泰文，藏文）等等。

css默认是拉丁语系。
然而最佳实践是在`<html> `标签里指定语言和书写方向，那本文举例，`<html lang='en-gb' dir='ltr'>`告诉浏览器本网站是用英式英语出版，从左到右书写。

## 阿拉伯语系

阿拉伯，希伯来和其他一些语言的行是从右到左的，也就是所谓的RTL。
注意：行的方向依然是水平的，而快的方向是从上到下，字符是竖直方向的。

![](http://ggbond.qiniudn.com/arabicsystem.jpg)

不仅仅是文本是从右到左，网站上所有的元素都是如此。右上角是起始位置，重要的东西在右边。眼睛是从右往左读。所以典型的RTL网站的布局和LTR是一样的，只不过是水平方向反过来了。

![](http://ggbond.qiniudn.com/UNenar.jpg)

对于大多数web开发者来说，我们的国际化的经验主要是支持阿拉伯和希伯来语。

css通过提供对LTR的hack来支持国际化和RTL，所以开发者不得不写一堆的hack。举个栗子：Drupal社区提出了一个公约：给每一个margin-left或者-right，每一个padding-left 或者 -right，每一个float: left 或者 float: right添加注释`/* LTR */`,后面的开发者可以搜索每一个注释，然后把每一个left改成right,反之亦然。这是如此的单调而且容易出错。css本身需要更好的方式来支持开发者只写一次布局的代码，然后方便的切换不同的布局方向。

新版的css布局就是这样做的。Flexbox，Grid还有对齐用的是开始和结束而不是左和右。这样的话要切换就很容易（相当于和左右解耦了）。只要写`justify-content: flex-start`,`justify-items: end`甚至是`margin-inline-start: 1rem`也能正常工作而不需要修改任何代码

这种方式就好多了。虽然用开始和结束来代替左右会让人困惑，但是这对多语言的项目是很友好的。

怎么样声明书写方向呢？不要在css里写而是在html里写，这样即使css没有加载，浏览器也知道该怎么布局页面

比较明智的做法是在html跟元素，还需要声明你的语言。就像上面提到的一样，本文用的是`<html lang='en-gb' dir='ltr'>`。在阿拉伯联合王国，需要用`<html lang='ar' dir='rtl'>`

如果需要支持混合语言的，那就比较复杂了，但是本文不打算深入介绍。这篇文章的主要目的是介绍css和布局，而不是国际化

在拉丁语系和阿拉伯语系里，不过是从左到右还是从右到左都使用一个css属性：`writing-mode: horizontal-tb`。因为在这两个语系里行的方向都是水平的，块的方向都是从上到下：

horizontal-tb是浏览器的默认方式，所以不需要特别指定，除非你需要用其他的方式来覆盖，你可以认为浏览器内置了下面的代码：

```

html {
  writing-mode: horizontal-tb;
}

```

下面我们看看垂直语系：

## Han-based 语系

事情开始变得有趣起来了。
Han-based书写系统包括CJK语言，即中文，日文，韩文还其他的。布局页面的时候有两种方式，有时两种一起用。

大多数CJL的文字和拉丁语系的一样，都是水平从上到下，行是从左到右。这是从20世纪开始的现代化方式。然后再计算机和web领域得到推广

```

section {
  writing-mode: horizontal-tb;
}

```

你也知道，什么也写的话就能得到默认值。

另外一种Han-based的语言可以在垂直方向布局。inline是垂直的。而block是从右到左的。

![](http://ggbond.qiniudn.com/hansystem.jpg)

注意到水平方向的文字是从左到右的，而垂直方向的文字是从右到左的。
这个日本的时尚杂志使用了混合的书写模式，封面是从左边打开，和英文的杂志正好相反

![](http://ggbond.qiniudn.com/voguejpcover.jpg)

![](http://ggbond.qiniudn.com/voguejppage.jpg)

你也可以单独设置每一个元素的书写模式：

```

div.articletext {
  writing-mode: vertical-rl;
}

```

也可以改变页面的垂直方向的默认布局，然后设置某个元素为horizontal-tb

```

html { 
  writing-mode: vertical-rl;
}
h2, .photocaptions, section {
  writing-mode: horizontal-tb;
}

```

如果你的页面有滚动条，那么书写模式决定了是把左上角作为七点，滚动到右边（如果我们用了horizontal-tb）还是把右上角作为其他，滚动到左边。这里有个例子![CSS Writing Mode demo by Chen Hui Jing](https://www.chenhuijing.com/zh-type/)，你可以切换水平和垂直的书写模式，然后看看有什么区别

![](http://ggbond.qiniudn.com/chenhuijingdemo.jpg)

## 蒙古语系

蒙古语也是垂直的语系。文本垂直排练。像Han-based语系一样。但是有两个不同的地方：第一，block的布局是从左到右的。下面这幅图展示了蒙古语的维基百科长什么样：

![](http://ggbond.qiniudn.com/mongolianwikipedia.jpg)

你也许会想，这看上去也不是很奇怪。把你的头向左倾斜，这就很熟悉了。block从屏幕的左边开始往右。inline从页面的顶部开始往下（和RTL很像，只不过是逆时针旋转了90°）。但是还有另外一个很大的不同。字符的方向是上下颠倒的。蒙古语字符的顶部不是指向左边，而是指向block块的方向。指向右边：

![](http://ggbond.qiniudn.com/mongoliansystem.jpg)
现在你也许会想忽略这些细节。大概你也不好直接去写蒙古文。但是我告诉你这为什么很重要--蒙古语定义了书写模式：vertical-lr，也就意味着我们不能用vertical-lr按照我们期望的方式来写其他的语言：

我们想象一下vertical-rl和vertical-lr的工作方式，大概像下面这样：


![](http://ggbond.qiniudn.com/verticalexpected.png)

但是这是错误的。实际上是像下面这样：

![](http://ggbond.qiniudn.com/verticalactual.png)

是不是和想象的不一样。`writing-mode: vertical-rl`和`writing-mode: vertical-lr`都是顺时针旋转的，并不是逆时针。

如果你在写蒙古文，你可以用下面的css来应用Han-based的书写模式。对整个的html或者指定的元素


```

section {
  writing-mode: vertical-lr;
}

```

如果你想在设计图形界面的时候不用水平方向的书写模式，我不觉得`writing-mode: vertical-lr`有用，当文本有两行的时候，他的布局方式是不可控的，我用的较多的是`writing-mode: vertical-rl`而不是`-lr`


# 图形设计的书写模式

我们如何把英语的H标签变成竖着的呢？可以用 ` transform: rotate()`

这里有两个例子，每个例子一个方向（这些例子都用CSS Gri做布局，所以需要使用支持这个属性的浏览器来测试，比如Firefox Nightly或者chrome）。

![](http://ggbond.qiniudn.com/wm4A.jpg)

在 [this demo 4A](http://labs.jensimmons.com/workshop/writingmode-4A.html), 文本顺时针旋转:

```

h1 {
  writing-mode: vertical-rl;
}

```

![](http://ggbond.qiniudn.com/wm4B.jpg)

在这个例子里，文本逆时针旋转:

```

h1 {
  writing-mode: vertical-rl;
  transform: rotate(180deg);
  text-align: right;
}

```

我用vertical-rl来旋转文本，然后旋转180°，再用`text-align: right`让文字跑到页面的顶部。这是一种hack的做法，但是有用。

现在可以直接用CSS属性来实现:

```

h1 {
  writing-mode: sideways-rl;
}

```

另一个例子

```
h1 {
  writing-mode: sideways-lr;
}
```

唯一的问题是只要Firefox浏览器支持这两个属性，其他浏览器都识别不了sideways-*，这就意味着我们还不能使用它。

正常来说，writing-mode的[浏览器兼容性](http://caniuse.com/#feat=css-writing-mode)没问题。所以我用了`writing-mode: vertical-rl`和` transform: rotate(180deg)`来hack.

如果你还想做其他的尝试，可以加上`text-orientation: upright`试试看，


![](http://ggbond.qiniudn.com/wm4C.jpg)

在[ demo 4C](http://labs.jensimmons.com/workshop/writingmode-4C.html)里：

```

h1 {
  writing-mode: vertical-rl;
  text-orientation: upright;
  text-transform: uppercase;
  letter-spacing: -25px;
}

```

你可以去看看我所有的Writing Modes的demo[http://labs.jensimmons.com/#writing-modes](http://labs.jensimmons.com/#writing-modes)

剩下两个Demo在这里：

{% codepen lixuejiang dNExjm  1 %}


