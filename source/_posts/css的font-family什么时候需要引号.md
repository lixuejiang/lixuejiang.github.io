---
title: CSS的font family什么时候需要引号
date: 2017-01-02 10:15:22
tags: [css,翻译]
---
[原文](https://mathiasbynens.be/notes/unquoted-font-family)，如果你也对font family在什么情况下要加引号和转义很困惑，这篇文章也许能帮到你。

`font-family: 'Comic Sans MS'`的引号到底需不需要？

<!--more-->

根据 [css校验器](http://jigsaw.w3.org/css-validator/)的规则，这里的引号是需要的，因为font family里含有空格：

> 字体族的名字里如果含有空格，需要用引号包含起来。如果引号被省略，名字前后的空格会被忽略，而且所有的名字里面的空格都会被合并成一个空格。

但是这是[错误的](https://www.w3.org/Bugs/Public/show_bug.cgi?id=16362).该警告消息说，所有的包含空格的font family都需要用引号，这是不对的。`font-family: Comic Sans MS`(没有引号)也是完全正确的css，也能按照期望的方式工作。

在现实中，情况也许比这个更复杂，要像完全理解css font family的规则，我们需要先搞清楚css 字符串和标识符之间的区别


# 字符串和标识符
关于[字符串](https://www.w3.org/TR/CSS2/syndata.html#strings),w3c规范是这样说的：

> 字符串可以放在单引号也可以放在双引号里

而[标识符](https://www.w3.org/TR/CSS2/syndata.html#value-def-identifier)的定义如下：

> 在CSS里，标识符(包括元素名称，类名，选择器里的ID)只能包含字符[a-zA-Z0-9]，ISO 10646里比U+00A0大的字符，还有连字符（-）和下划线（_）


[ISO 10646](https://en.wikipedia.org/wiki/Universal_Coded_Character_Set)定义了全局字符集，和Unicode标准相关。需要注意的是，这里说的是hyphen-minus，而不是hyphen（我并不知道他们到底说的是什么），hyphen的Unicode编码是U+2010，而hyphen-minus的编码是U+002D，下划线是U+005。Unicode允许的最大编码是U+10FFFF，所以所有满足正则表达式[-_a-zA-Z0-9\u00A0-\u10FFFF]的字符在标识符里都是允许的。

规范里还说：

>标识符不能以数字，两个下划线后者一个下划线后面跟一个数字开头。标识符也能包含转义字符还有ISO 10646定义的数字编码。举个例子：`B&W?`应该写成`B\&W\?`或者`B\&W\?`.

翻译成正则：所有满足`^(-?\d|--)`的都不是合法的标识符

# 空白字符
[CSS2.1](https://www.w3.org/TR/CSS21/fonts.html#font-family-prop)和[CSS3](http://dev.w3.org/csswg/css3-fonts/#font-family-prop)都指出：

>字体族的名字要么作为字符串用引号包含起来，要么作为标识符，不需要引号。这就意味着在没有引号的名称里，开头的大多数标点符号和数字都需要被转义

注意：“a sequence of one or more identifiers”暗示多个空格分隔的标识符组成了一个单一的字体族名字。因此，`font-family: Comic Sans MS`是合法的，和`font-family: 'Comic Sans MS'`等价。前者包含三个空格分隔的标识符，后者只是一个字符串。


规范里用一个新的段落来澄清：

>如果font family的名字是一系列标识符。那么计算机识别的最终值是单个空格分隔的标识符转换成字符串后的值。

关于前面提到的CSS校验器发出的警告描述了当标识符前后有空格时会发生什么：

>标识符前后的空格会被忽略，内部的其他多个空格会被转成一个空格。

规范也暗示了未转义的空白字符不能成为标识符的一部分，也不能是一序列标识符的开头

# 一般的字体族关键字

规范定义了如下的字体族关键字：`serif`, `sans-serif`, `cursive`, `fantasy`, 和 `monospace`.这些关键字可以作为普通的回退机制，以防期望的字体不可以用的时候。作者被鼓励在字体定义的最后面加上一种上面这些字体之一作为最后的可选项，来提高代码的鲁棒性。作为关键字，他们不需要引号。

换句话说，`font-family: sans-serif`代表的是一个普通的sans-serif字体族。而`font-family: 'sans-serif'`则代表了一个名字叫sans-serif的字体。这个区别很重要。

# 其他的关键字

还有其他的一些关键字：

>当字体族的名字和关键字的值一样时（`inherit`, `serif`, `sans-serif`, `monospace`, `fantasy`, 和 `cursive`），需要加引号以防和关键字混淆。`initial`和`default`作为保留字以备将来的使用，如果作为字体名的时候需要用引号括起来。浏览器不需要考虑`<family-name>`这种情况

注意：所有的关键字都是[大小写不敏感的](https://www.w3.org/TR/CSS21/syndata.html#characters)。举个栗子：`Monospace`和`monospace`，`mOnOsPaCe`都代表同一个关键字。如果你想用和关键字一样名字的字体时，需要加引号


# 总结
目前为止，在约定的合法的标识符了唯一不允许的字符是[U+0020 space characters](https://codepoints.net/U+0020)，所有的空格分隔非关键字的部分都是合格的标识符，标识符可以作为一个没有引号的字体族名字。

如果一个字体族的名字和关键字一样，那么需要加引号、

如果你想用非法的css标识符作为字体族的名字或者其中的一部分，那么你需要用字符串的形式代替，或者[转义](https://mathiasbynens.be/notes/css-escapes)剩下的没有引号的标识符。

一些例子：

```

/* 非法的，因为标识符里没有/ */
font-family: Red/Black;
/* 合法的 — `/`被转义了 */
font-family: Red\/Black;
/* 非法的，字符串和标识符不能混用: */
font-family: 'Lucida' Grande;
/* 合法的 — it’s a single string: */
font-family: 'Lucida Grande';
/* 合法的 — 空格分隔的标识符序列: */
font-family: Lucida Grande;
/* 合法的 — 空格分隔的两个标识符: */
font-family: Lucida     Grande;
/* 非法的 标识符里不允许出现`!`*/
font-family: Ahem!;
/* 合法的 — 字符串: */
font-family: 'Ahem!';
/* 合法的 — `!`转义了: */
font-family: Ahem\!;
/* 非法的 标识符不能以数字开头: */
font-family: Hawaii 5-0;
/* 合法的 — 字符串: */
font-family: 'Hawaii 5-0';
/* 合法的 — `\35 `是 `5`的转义: */
font-family: Hawaii \35 -0;
/* 合法的 — `\ ` 是空格的转义 ` `: */
font-family: Hawaii\ 5-0;
/* 合法的 — `$` : */
font-family: $42;
/* 合法的 —  `$` 转义了: */
font-family: \$42;
/* 合法的 — `€` 允许出现在标识符里: */
font-family: €42;

```


# 字体族名字合法性检查器

我做了一个[工具](https://mothereff.in/font-family)来检查一个给定的字符串需不需加引号。

![https://mothereff.in/font-family](http://ggbond.qiniudn.com/validator@2x.png)

《完》