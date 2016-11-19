---
title: Full-Stack Redux Tutorial
date: 2016-08-25 12:56:05
tags: [react,redux,翻译]
---
本文是对[Full-Stack Redux Tutorial](http://teropa.info/blog/2015/09/10/full-stack-redux-tutorial.html#table-of-contents)这篇文章的翻译

Redux是当下发生在JavaScript领域最令人激动的事情之一。它通过以下优势在一堆的js的框架和库里脱颖而出：
* 一个简单的，可以预测的状态模型
* 注重函数式编程和不可变数据
* 一个微小的API

Redux是一个很小的库，要学习他的api并不困难，但是对于大多数人，纯函数式和不可变数据微小的构建块和自身的限制会让他们感到举步维艰，这到底是怎么实现的？
本教程讲引导你从头开始构建一个全栈的Redux和[Immutable-js](http://facebook.github.io/immutable-js/)应用程序。应用测试驱动开发，我们将一步步构建一个对应真是世界的应用，Node+Redux的后端和React+Redux的前端
同时还会用到ES6, [Babel](http://babeljs.io/) , [Socket.IO](http://socket.io/) , [Webpack](http://webpack.github.io/) 和 [Mocha](https://mochajs.org/) .这是一个有趣的开发栈，可以加快你的开发速度。

<!--more-->
## 准备
这篇教程对于知道如何开发JavaScript 应用程序的开发者来说将会非常有用。我们将会用到Node, ES6, React, Webpack, Babel。如果你熟悉这些工具，那么你将会很顺利的完成后面的任务，否则，在出发之前你应该先了解这些知识。
> 如果你正在寻找一篇很好的介绍通过ES6,webpack，react和Babel来开发应用程序的文章，我建议你登录[reactjsprogram](http://www.reactjsprogram.com/?asdf=36750_hrdghgnp)这个网站或者[survivejs](http://survivejs.com/)这个网站


## App

## The Architecture
## The Server Application
### Designing The Application State Tree
### Project Setup
### Getting Comfortable With Immutable
### Writing The Application Logic With Pure Functions
#### Loading Entries
#### Starting The Vote
#### Voting
#### Moving to The Next Pair
#### Ending The Vote
### Introducing Actions and Reducers
### A Taste of Reducer Composition
### Introducing The Redux Store
### Setting Up a Socket.io Server
### Broadcasting State from A Redux Listener
### Receiving Remote Redux Actions
## The Client Application
### Client Project Setup
#### Unit Testing support
### React and react-hot-loader
### Writing The UI for The Voting Screen
### Immutable Data And Pure Rendering
### Writing The UI for The Results Screen And Handling Routing
### Introducing A Client-Side Redux Store
### Getting Data In from Redux to React
### Setting Up The Socket.io Client
### Receiving Actions From The Server
### Dispatching Actions From React Components
### Sending Actions To The Server Using Redux Middleware
## Exercises
### 1. Invalid Vote Prevention
### 2. Improved Vote State Reset
### 3. Duplicate Vote Prevention
### 4. Restarting The Vote
### 5. Indicating Socket Connection State
### Bonus Challenge: Going Peer to Peer
