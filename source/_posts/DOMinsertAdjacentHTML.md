---
title: DOM insertAdjacentHTML
date: 2016-09-25 21:57:33
tags: [翻译,DOM]
---
[原文](http://ejohn.org/blog/dom-insertadjacenthtml/)
当我在寻找一个更好的往document里注入html fragments（我在[这篇](http://ejohn.org/blog/dom-documentfragments/)文章里提到过的）的方法的时候，我决定花点时间研究一下IE浏览器的[insertAdjacentHTML](https://msdn.microsoft.com/en-us/library/ms536452.aspx)方法。

这个方法在IE4里就开始使用了，当前版的Opera也支持。它允许你往document的不同位置插入一段格式正确的html片段。位置如下：

* .insertAdjacentHTML("beforeBegin", ...)	before
* .insertAdjacentHTML("afterBegin", ...)	prepend
* .insertAdjacentHTML("beforeEnd", ...)	append
* .insertAdjacentHTML("afterEnd", ...)	after

这个方法只对html元素生效，而且使用方便。

<!--more-->

{% codepen lixuejiang ozkmNN 0 %}

第一眼看到这个方法能正常运行而且似乎速度也还比较快的时候，我想到了两个问题：和之前提到的[Document Fragment technique ](http://ejohn.org/blog/dom-documentfragments/)比起来，速度到底怎么样，对于所有的奇怪的测试用例能正常工作吗？

* 我建了一个测试用例来比较三种不同的注入方式：jQuery里的注入方式，Document Fragment technique，和用insertAdjacentHTML。结果证明insertAdjacentHTML和Document Fragment technique都比jQuery1.3的要快，在IE6里
* insertAdjacentHTML有一个很大的问题：在IE6里不是所有的html元素都支持这个方法（table, tbody, thead, 和tr 元素不支持）
* insertAdjacentHTML不支持xml

所以为什么还要花时间来讨论一个在主流浏览器里都半生不熟的方法呢，因为他即将是[html5规范](https://w3c.github.io/html/#dom-insertadjacenthtml-html)的一部分，这就意味着我们会看到大量的浏览器会开始实现这个方法（它也会鼓励现在的厂商更加完善和有效的实现这个方法）
浏览器实现这个方法将会显著降低实现一个伟大的JavaScript库所需要的代码量，我很期待这个方法被广泛使用的那一天（和querySelectorAll一样）的到来，到时候我们就可以坐下来精简我们的代码了。

这篇文章写于2008年11月24日，离下载已经过去了8年。[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentHTML)记载insertAdjacentHTML() 将指定的文本解析为 HTML 或 XML，然后将结果节点插入到 DOM 树中的指定位置处。该方法不会重新解析调用该方法的元素，因此不会影响到元素内已存在的元素节点。从而可以避免额外的解析操作，比直接使用 innerHTML 方法要快。