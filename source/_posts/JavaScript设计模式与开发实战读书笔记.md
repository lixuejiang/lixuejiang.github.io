---
title: JavaScript设计模式与开发实战读书笔记
date: 2016-10-02 10:07:29
tags: [JavaScript,读书笔记]
---
# 基础知识
## 面向对象的JavaScript

> 如果它走起 路来像鸭子,叫起来也是鸭子,那么它就是鸭子。

* 多态背后的思想是将“做什么”和“谁去做以及怎样去做”分离开来,也就是将“不变的事 物”与 “可能改变的事物”分离开来。
* 多态最根本的作用就是通过把过程化的条件分支语句转化为对象的多态性,从而 消除这些条件分支语句。
* 在 JavaScript 语言中不存在类的概念,对象也并非从类中创建出来的,所有的 JavaScript 对象都是从某个对象上克隆而来的。
* 所有的数据都是对象。
* 要得到一个对象,不是通过实例化类,而是找到一个对象作为原型并克隆它。  对象会记住它的原型。
* 如果对象无法响应某个请求,它会把这个请求委托给它自己的原型。

<!--more-->

## this、call、apply
### this的指向大致可以分为4类：

* 作为对象的方法调用。

```
var obj = { 
	a: 1,
	getA: function(){
		alert ( this === obj ); // 输出:true 
		alert ( this.a ); // 输出: 1
	} 
};

 obj.getA();

```

* 作为普通函数调用。

```

window.name = 'globalName';
var getName = function(){ 
	return this.name;
};
console.log( getName() );

```

* 构造器调用。

```
var MyClass = function(){ 
	this.name = 'sven';
};
var obj = new MyClass();
alert ( obj.name ); // 输出:sven

```
* Function.prototype.call 或 Function.prototype.apply 调用。

### call和apply的用途

* 改变 this 指向
* Function.prototype.bind

```
Function.prototype.bind = function( context ){ 
	var self = this; // 保存原函数
	return function(){ // 返回一个新的函数
		return self.apply( context, arguments );// 执行新的函数的时候,会把之前传入的 context 
		// 当作新函数体内的 this
	}
};


```

* 借用其他对象的方法

`[].prototype.slice.call(arguments)`

## 闭包和高阶函数
跟闭包和内存泄露有关系的地方是,使用闭包的同时比较容易形成循环引用,如果闭包的作 用域链中保存着一些 DOM 节点,这时候就有可能造成内存泄露。但这本身并非闭包的问题,也
并非 JavaScript 的问题。在 IE 浏览器中,由于 BOM 和 DOM 中的对象是使用 C++以 COM 对象 的方式实现的,而 COM 对象的垃圾收集机制采用的是引用计数策略。在基于引用计数策略的垃圾回收机制中,如果两个对象之间形成了循环引用,那么这两个对象都无法被回收,但循环引用造成的内存泄露在本质上也不是闭包造成的。同样,如果要解决循环引用带来的内存泄露问题,我们只需要把循环引用中的变量设为 null 即可。将变量设置为 null 意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运 行时,就会删除这些值并回收它们占用的内存。

### 高阶函数
高阶函数是指至少满足下列条件之一的函数。

* 函数可以作为参数被传递;
* 函数可以作为返回值输出。

# 设计模式
## 单例模式

```
var getSingle = function( fn ){
	var result;
	return function(){
		return result || ( result = fn .apply(this, arguments ) );
	} 
};

```
## 策略模式
定义一系列的算法,把它们一个个封装起来,并且使它们可以相互替换。
## 代理模式
## 迭代器模式
## 发布-订阅模式
## 命令模式
## 组合模式
## 模板方法模式
## 享元模式
## 职责链模式
## 中介者模式
## 装饰者模式
## 状态模式
## 迭代器模式
# 设计原则和编程技巧
## 最少知识原则
## 开放封闭原则
## 面向接口编程
## 代码重构

