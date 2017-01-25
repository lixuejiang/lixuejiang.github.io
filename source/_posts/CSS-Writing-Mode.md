---
title: CSS Writing Mode
date: 2017-01-25 09:46:00
tags: [翻译,CSS]
---

最近需要实现一个文字竖排的效果，类似下图这种效果，所以研究了一下css的writing mode，顺便翻译了一篇[文章](https://24ways.org/2016/css-writing-modes/)
![css write mode](https://24ways.org/2016/css-writing-modes/)

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
- 2.

to be continued
