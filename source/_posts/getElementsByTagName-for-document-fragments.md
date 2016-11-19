---
title: getElementsByTagName for document fragments
date: 2016-09-26 09:55:33
tags: [翻译,DOM]
---
[原文链接](http://dean.edwards.name/weblog/2009/12/getelementsbytagname/)
[document fragments](http://ejohn.org/blog/dom-documentfragments/)没有实现[getElementsByTagName](https://www.w3.org/TR/DOM-Level-2-Core/core.html#ID-1938918D)方法，但是实现了其他的[选择器api]():[querySelector/querySelectorAll](https://www.w3.org/TR/selectors-api/#nodeselector),除了那些写[选择器引擎](http://www.paulirish.com/2008/javascript-css-selector-engine-timeline/)的人，没人关心这个，如果你也是关心这个，那么请继续阅读。
在没有很多dom方法的情况下，我依然可以写我的代码，但是没有getElementsByTagName方法不行，所以我决定实现它。

# 关于性能
JavaScript是一门很好的脚本语言，但是仍然是脚本语言。
要实现高性能的循环应该尽量避免下面的这些做法：

* 1.try/catch块
* 2.方法调用，即使是原生的方法
* 3.获取dom属性（或者任何会调用复杂的getter/setter的方法）

请注意这是一个有序列表！

<!--more-->

代码如下：
```
function getElementsByTagName(node, tagName) {
  var elements = [], i = 0, anyTag = tagName === "*", next = node.firstChild;
  while ((node = next)) {
    if (anyTag ? node.nodeType === 1 : node.nodeName === tagName) elements[i++] = node;
    next = node.firstChild || node.nextSibling;
    while (!next && (node = node.parentNode)) next = node.nextSibling;
  }
  return elements;
}
```

> 一些需要注意的事情
* Document fragments 不能有父节点
* 这个方法只能查询document fragments，所以需要确保上下文节点不是一个普通的html元素

一个例子
```
a=document.createDocumentFragment();
b=document.createElement("div");
b.innerHTML="<div></div><div></div>"
a.appendChild(b);
alert(getElementsByTagName(a,"DIV").length)//3
```
