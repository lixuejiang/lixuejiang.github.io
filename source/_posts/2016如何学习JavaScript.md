---
title: 2016如何学习JavaScript
date: 2016-10-05 17:16:04
tags: [JavaScript,翻译]
---

虽然2016快结束了，但是这篇文章还是很有意思，值得一读，原文[在此](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f#.5dzju1fmk)。很长的一篇文章，反应了现在的前端发展变化很快，特别是现在的react还有各种预编译的出现，导致现在即使做一个很简单的功能，都需要掌握很多繁杂的事情。
做为一个前端，需要不停的了解很多新的名词，技术。而且这些名词层出不穷，也不知道什么时候会过时，疲于奔命。
回过头来想一想，每一种技术的出现都有其原因和背景，都是为了解决生产中的某些问题，或则是为了提高生产力，或者是为了提高用户体验等等。而每一种技术的实现都有其内在的原理，世界变化万千，里唯一贯。

<hr>

*这篇文章是是受Circle CI的一篇叫“[这就是未来](https://circleci.com/blog/its-the-future/)”的启发而写，就像所有的JavaScript框架一样，没必要太严肃对待。在写这篇文章的时候不会创建任何的JavaScript框架。*

有人问我说：我现在有一个项目，但是说实话，我没有写过太多的web代码，我听说前端变化很快，你是这里最跟得上时代的web开发者。

*我确实是前端工程师，我就是你要找的人。我在2016年做web开发。可视化，音乐播放器，无人机踢球，等等。我刚从jsconf和reactconf回来，所以我知道最新的开发web app的技术。 *

<!--more-->
酷毙了，我想开发一个展示用户最近的活动的页面，所以我只需要从REST endpoint获取到数据，把他们现实到一个可以排序的表格里，当服务器有变化的时候更新这个表格。我在想是不是可以用jQuery来展示获取和展示数据？

*不，不要再用jQuery了，你应该学习react，现在是2016了。*

好吧，什么是React？

*它是Facebook的一帮人开发的一个超级酷的库，你能够很容易的处理视图的变化，从而给你的application代理高的可控性和更高的性能*

听起来不错，我可以用React来显示服务端的数据吗？

*当然可以，不过首先你得往你的web页面里添加 React 和 React DOM这两个库。*

等一等，为什么是两个库？

*其中一个是处理JSX的，另外一个是操作DOM的。*

JSX?什么是JSX?

*JSX是一种有点像xml的JavaScript语法扩展，是描述DOM的另外一种方式，你可以把它理解为更好的html。*

HTML有什么问题？

*现在是2016了，没人直接书写html了。*

好的，不管怎么说，如果我添加了这两个库我是不是就能用React了？

*不完全是，你还需要添加Babel，然后你才能用React*

另外一个库？Babel是什么？

*Babel是一个翻译器，把你写的任何版本的JavaScript翻译到目标版本的JavaScript。你没必要一定要用Babel，如果你还在用ES5.但是现在是2016了，你应该学学其他同学，用ES2016+来写你的代码*

ES5? ES2016+? 我有点懵逼了，什么是ES5和ES2016+?
*ES5代表ECMAScript 5. 大多数浏览器实现了这个版本，所以用的人也最多.*

ECMAScript?

*对, 你知道吗, 现在的JavaScript是在1999的基础上发展而来的，它的第一版是在1995年发布的，那个时候JavaScript还叫livescript，只能运行在Netscape里。简直是一团糟。现在好了，现在已经有7个版本的JavaScript了*

7 个版本？. 讲真， ES5 and ES2016+ 是什么？

*第五和第七版本的JavaScript.*

等一下，为什么没有第六版本？

*你是说ES6吗？我的意思是，每个版本都是上一个版本的超集，因此，如果你用了es2016+，你也能用上一个版本的所有特性*

好，那为什么用ES2016+而不是ES6?

*当然了，你可以用ES6，但是如果你想用async和await这些特性，你需要用ES2016+.否则，你将会陷入如何正确控制ES6 generators异步回调的流程的泥潭里.*

你刚说这些我听不懂，这些名词让我很困惑。我只是想从服务器获取数据。我只是想从CDN加载jQuery，然后用ajax获取数据，为什么我不能这么做？

*现在是2016了，不再需要jQuery了，不需要再写意大利面条那样的代码了。每个人都知道*

好，所以我的选择是加载三个库，获取数据，显示一个HTML的表格。

*对，包含着三个库，然后通过一个模块管理器把他们打包成一个文件.*

我明白了，什么是模块管理器？

*具体的定义依赖运行环境，在web里，模块管理器是指那些支持AMD或者 CommonJS模块的东西*

好。。。吧！AMD 和 CommonJS又是什么鬼？

*有很多种方式来描述JavaScript的库和文件之间是如何交互的，你知道exports和requires吗？你可以写很多定义了AMD或者commonjs API的JavaScript文件，然后用Browserify把他们打包到一起*

什么是Browserify？

*它能帮助你把commonjs规范的文件打包正能再浏览器里运行的文件，之所有有这个东西是因为大多数人把他们的模块都发布到npm里*

npm？

*它是一个很大的库，大家把他们的代码或者依赖发布到上面*

就像CDN？

*不太像，更新一个中心化的数据库，每个人都可以发布或者下载库，所以你可以在本地开发的时候使用这些库，然后把他们发布到CDN*

哦，像Bower！

*对，现在是2016了，没人再用Bower了*

哦，我明白了，所以我需要从npm下载库？

*对，举个栗子，如果你想用React，你下载React，引入到你的代码里。对每一个流行的JavaScript库，都可以用这种方法*

哦，就行Angular！

*Angualar是2015年的事情了。对，angular也可以这样用，另外VueJS 和 RxJS是2016年比较库的两个库，想学一学吗？*

还是聚焦React，我今天已经学了很多东西了。所以，如果我想用React，那么我先从npm下载，然后再用Browserify吗？

*对*

这对于回去依赖然后把他们关联到一起，似乎太复杂了

*确实是这样，这就是为什么你需要一个像grunt，gulp，Broccoli这样的任务管理器来自动运行Browserify。哎呀，你甚至可以用Mimosa*

Grunt? Gulp? Broccoli? Mimosa? 我们讨论的是什么鬼?

*任务管理器，但是他们现在没有那么酷了，我们在2015年使用他们，后来用makefile，但是现在我们用webpack来打包所用的东西*

Makefiles？我以为那只是用在C或者C++里面？

*对，显然，在web里我们喜欢编译代码，然后关注基本的东西*

哦，你提到了webpack

-*它是另外一个模块管理器，也是一个任务管理器，是更好的Browserify*

哦，好吧？为什么它更好？

*额，也是不是更好。它只是对于依赖如何处理，提供了更多的可选项。你可以用不同的模块管理器，不光是Commonjs一个。举个栗子，像支持原生的ES6的模块*

对这个CommonJS/ES6，我是彻底懵逼了

*每个人都是这样，但是你没必要对SystemJS关注太多*

上帝啊，另外一个js的名词，这个是SystemJS又是什么

*不像Browserify和webpa 1.x，SystemJS是一个动态的模块加载器，它把多个模块打包的多个文件里，而不是打包成一个文件*

等一等，我一直认为我们是要把我们的文件打包到一个大文件里，然后一次加载这个大文件。

*对，但是HTTP/2就要来了，对个http请求会更好*

等一下，我们还能不能添加那三个库了？

*我的意思是你可以从cdn加载这三个外部文件，但是你还是需要babel*

哦，that is bad right（不太懂这句话的意思，所以不翻译）？

*对，你需要包含整个babel-core，在生成环境中效率不好，所以你需要一序列的预处理。不需要压缩assets，uglify，内联css，异步JavaScript，另外*

我明白了，我明白了，所以如果不直接从cdn加载文件，应该怎么做？

*我会用Webpack + SystemJS + Babel combo把typescript翻译成js*

Typescript？我以为我们是直接写JavaScript

*Typescript就是JavaScript，是JavaScript的超集，包好了很多es6的特性*

我以为ES2016+就是ES6的超集了，那为什么还需要Typescript？

*因为你可以它可以让JavaScript支持强类型，减少运行时错误，现在是2016年，你应该让你的js支持强类型*

很显然，Typescript支持这个

*Flow也一样，尽管它只做类型检查*

Flow是什么？

*他是Facebook的一帮人搞出来的一个类型检查器，用OCaml实现，因为函数式编程很帅*

OCaml? 函数式编程?

*今天那些吊炸天的人在用的东西，现在是2016了，函数式编程？高阶函数？柯里化？纯函数？*

我不知道你在说什么

*没人在一开始就知道这些，你只需要知道函数式编程比面向对象编程好，我们应该在2016年使用它*

等一下，我在大学学习了过OOP，我觉得它很好啊？

*在java被oracle收购以前，OOP确实很好，我们现在也还在用，但是现在每个人都意识到修改程序状态就像踢婴儿，所以现在每个人都转向了不可变数据和函数式编程。Haskell的那些人已经叫了好今年了，另外不要忘了Elm那些人。幸运的是，在web里，我们有像Ramda这样的库让我们可以直接用JavaScript来实现函数式编程*

你是因为他删除了名字吗？到底什么是Ramda？

*不是。Ramda，就像lamada，David Chambers发明的库*

谁？

*David Chambers，吊炸天的一个人。Ramda的一个贡献者。你还需要了解Erik Meijer，如果你真的想学函数式编程*

Erik Meijer是谁？

*一个搞函数式编程的人，也很屌。你还应该了解Tj，Jash Kenas, Sindre Sorhus, Paul Irish还有。。。*

好吧，我必须阻止你继续说下了。所有这些都很好，但是我觉得对于获取数据然后显示出来，这些都太复杂了，而且没必要。我很确定我不需要了解这些人，也不需要学习这些事情来创建一个展示动态数据的表格。所以让我们回到react，我应该怎么样用React从服务端获取数据？

*额，实际上，你不是用react从服务端获取数据，而是用react显示数据*

好吧，你杀了我吧，那你用什么获取数据？

*Fetch*

用Fetch来fetch数据，起名字的人需要一个词库

*我知道，Fetch是通过XMLHttpRequests来获取数据的原生实现的命名*

哦，那不就是AjAX吗？

*ajax仅仅是用了XMLHttpRequests，但是，很确定的是，Fetch允许你在promise的基础上使用ajax，这样你就可以避免回调地狱*

回调地狱？

*对，每次你向服务器发送一个一部请求，都需要等待服务器的响应，所以你需要再函数里嵌套函数，这就被叫做回调地狱*


哦，那么promise就是解决这个问题的吗？

*确实是，通过promise来管理回调，你就能更容易的写出可以让人理解的代码，然后模拟测试他们，也可以一次模拟一个请求，然后等他们全部返回*


然后Fetch可以完成这件事情？

*对，除非你用的是比较新的浏览器，否则你需要包含Fetch的polyfill或者使用Request, Bluebird 或者 Axios*

我到底需要了解多少库，到底有多少库？

*这可是JavaScript，有成千上万的库来做同一件事情，我们了解库，事实上，我们有最好的库，我们的库很大，有时候我们在库里还包含了Guy Fieri的画像*

你刚提到了Guy Fieri，我们先跳过，Request, Bluebird 或者 Axios这些到底是干什么的？

*他们是发起XMLHttpRequests请求然后返回promise的库*

难道jQuery的ajax方法一开始没有返回promise对象吗？

*在2016年我们不用“J”这个单词了。只用Fetch，如果浏览器不支持，就用Bluebird, Request或者Axios这些polyfill来代替。然后用内置了await方法的await来管理promise，这样你就有正确的控制流了。*


这是你第三次提到await了，但是我完全不知道这是啥？

*Await允许你阻塞一个异步调用，允许你更好的控制啥时候去获取数据，提高了代码的可读性。这很赞，你只需要确保添加了babel，或者用syntax-async-functions还有transform-async-to-generator插件*

疯了吧？

*没有，更疯狂的是你要先预编译typescript的代码，然后用babel把编译后的代码转成能使用await的*

啥？typescript里不包含这个？

*下个版本里会有，现在的1.7版本的目标是ES6.所以如果你想在浏览器里用await，首先你需要把typescript编译到ES6.然后用babel把es6编译到es5*

我不知道说什么了

*其实很简单，用typescript来写所有的代码，所有用的Fetch的模块都用babel的 stage-3 preset编译到ES6.然后用SystemJS加载。如果没有用Fetch，polyfill或者用Bluebird, Request or Axios。用await来处理所有的promise*

对于简单我们有不同的定义。所以，经过这么多仪式，我终于拿到了数据，现在可以用react来显示了吗？

*你的页面需要处理状态变化吗？*

额，我觉得没有必要，我只需要显示一些数据

*Oh，谢天谢地，不然的话我还要向你介绍Flux，还有向Flummox, Alt, Fluxible这些实现了flux的库。而且说实话，你应该用Redux。*

我想跳过这些名字，说到底，我只是想展示数据。

*哦，如果你仅仅是想展示数据，那一开始你就没必要用react，你只需要用模板引擎就好了。*

你在开玩笑吗？你觉得这很好玩吗？你就是这样对待那些爱你的人的吗？

*我只是解释你可以用的方案*

stop！

*我的意思是，即使只是用模板引擎，如果我是你的话我也会用ypescript + SystemJS + Babel combo*

我只是想显示一些数据，不是要扮演sub zero的死亡。只要告诉我要用什么模板引擎，我来用就好了。

*有很多，你熟悉哪一个*

咕~~(╯﹏╰)b，记不起名字了，这是很久以前的事情了。（大概是不知道有哪些模板引擎）

*jTemplates? jQote? PURE?*

额，没印象，还有其他的吗？

*Transparency? JSRender? MarkupJS? KnockoutJS? 有双向绑定的？*

还有其他的吗？

*PlatesJS? jQuery-tmpl? Handlebars?现在还有人在用*

也许吧，有和最后一个类似的吗？

*Mustache, underscore?说实话我觉得lodash都有自己的模板引擎了，这些都是2014年的了*

还有更新的吗？

*Jade? DustJS?*

不是

*DotJS? EJS?*

不是

*Nunjucks? ECT?*

不是

*额，应该有像typescript语法的，jade？*

不对，你刚说过了

*我的意思是Pug，也就是jade，jade现在就是pug了*

不，想不起来了，你会用哪一个？

*也许会用ES6的模板引擎*

让我想想，这需要ES6？

*完全正确*

看我使用的是什么浏览器，还需要babel

*完全正确*

另外，如果我不想添加整个核心的库，我需要从npm加载他们。

*完全正确*

需要像Browserify, or Wepback, 或者其他类似SystemJS的东西

*完全正确*

如果不用webpack，还需要一个任务运行器

*完全正确*

另外，如果我用了函数式编程还有强类型语言，我首先需要预编译typescript，或者引入流程控制的东西

*完全正确*

如果我想用await的话还需要babel

*完全正确*

因此我就可以用 Fetch, promises，控制流，还有其他神奇的东西

*不要忘了polyfill Fetch，Safari现在还不支持*

你知道吗？我觉得我们可以搞定了，事实上，我觉得我可以搞定了，我可以搞定web，我可以搞定JavaScript的一切

*挺好的，接下来的几年我们会继续编写Elm或者 WebAssembly*

我还是去写后端吧，我搞不定这么多的变化，版本，编译器，转译器。JavaScript社区简直疯了，如果它觉得每个人都能跟上脚步的话。

*我知道了，你可以去Python社区试试*

为什么？

*难道没听过Python 3吗*

--完--

