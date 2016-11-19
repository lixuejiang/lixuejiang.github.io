---
title: 简单实现JavaScript的继承
date: 2016-09-30 16:11:04
tags: [翻译,JavaScript]
---
[原文](http://ejohn.org/blog/simple-javascript-inheritance/),作者是jQuery之父John Resig
最近，关于JavaScript的继承，也是为了我正在写的JavaScript相关的书籍，我做了许多工作。在做这些事情的过程中，我尝试了很多种不同的模拟JavaScript继承的方式，在这些方式里面，我最喜欢的还是[base2](https://code.google.com/archive/p/base2/)和[Prototype](http://prototypejs.org/)的实现方式

我想把这些技术的核心提取成一种简单的可以的，方便理解的，没有其他依赖的方式。另外，我希望结果是简单的而且高可用的，下面是一个例子：

<!--more-->

```
var Person = Class.extend({
	init: function(isDancing){
		this.dancing = isDancing;
	},
	dance: function(){
		return this.dancing;
	}
});

var Ninja = Person.extend({
	init: function(){
		this._super( false );
	},
	dance: function(){
		// Call the inherited version of dance()
		return this._super();
	},
	swingSword: function(){
		return true;
	}
});

var p = new Person(true);
p.dance(); // => true

var n = new Ninja();
n.dance(); // => false
n.swingSword(); // => true

// Should all be true
p instanceof Person && p instanceof Class &&
n instanceof Ninja && n instanceof Person && n instanceof Class


```

关于这种实现方式，有几点事情需要注意：

* 创建构造函数的方式应该尽量简单（在这个例子里，提供了一个init方法）
* 为了创建一个新的类，你需要扩展一个已经存在的类
* 所有的类都继承自同一个祖先：Class，因此如果你想创建一个全新的类，他必须是Class类的子类
* 最有挑战性的事情是：必须提供覆写方法的权限（正确设置上下文），在上面的代码里通过使用`this._super()`方法来调用Person超类的`init()` 和` dance()`方法。

对这个结果我很满意：它有助于加强“类”的概念作为一种结构，保持简单的继承，允许调用超类的方法。

# 简单的类创建和继承
下面是简单的实现（注释完善）：
```
/* Simple JavaScript Inheritance
* By John Resig http://ejohn.org/
* MIT Licensed.
*/
// Inspired by base2 and Prototype
(function(){
	var initializing = false, fnTest = /xyz/.test(function(){xyz;}) ? /\b_super\b/ : /.*/;

	// The base Class implementation (does nothing)
	this.Class = function(){};

	// Create a new Class that inherits from this class
	Class.extend = function(prop) {
		var _super = this.prototype;

		// Instantiate a base class (but only create the instance,
		// don’t run the init constructor)
		initializing = true;
		var prototype = new this();
		initializing = false;

		// Copy the properties over onto the new prototype
		for (var name in prop) {
		// Check if we’re overwriting an existing function
			prototype[name] = typeof prop[name] == “function” &&
			typeof _super[name] == “function” && fnTest.test(prop[name]) ?
			(function(name, fn){
				return function() {
					var tmp = this._super;

					// Add a new ._super() method that is the same method
					// but on the super-class
					this._super = _super[name];

					// The method only need to be bound temporarily, so we
					// remove it when we’re done executing
					var ret = fn.apply(this, arguments);
					this._super = tmp;

					return ret;
				};
			})(name, prop[name]) :
			prop[name];
		}

		// The dummy class constructor
		function Class() {
			// All construction is actually done in the init method
			if ( !initializing && this.init )
				this.init.apply(this, arguments);
		}

		// Populate our constructed prototype object
		Class.prototype = prototype;

		// Enforce the constructor to be what we expect
		Class.prototype.constructor = Class;

		// And make this class extendable
		Class.extend = arguments.callee;

		return Class;
	};
})();


```

在我看来，这两个最棘手的部分是“初始化/不叫的init”和“创建_super方法”部分。我想捂住那些简单，这样你将有什么在这个方法的实现了很好的理解。

<b>初始化</b>
为了用函数原型来模拟继承，我们使用了一种传统的方法，创建一个超类的实例，并把这个实例赋值给新对象的原型，如果不使用上面的方法，代码如下：
```

function Person(){}
function Ninja(){}
Ninja.prototype = new Person();
// Allows for instanceof to work:
(new Ninja()) instanceof Person

```

这种实现有什么不好的地方呢？我们只是想利用‘instanceof’带来的好处，而不是实例化一个Person对象和运行他的构造函数带来的开销，为了解决这个问题，我们在代码里定义了一个叫initializing的变量，不管什么时候，只要我们想实例化一个类，然后用它的原型的时候，就把这个变量设置为true。

因此，在实际构建这个函数的时候我们需要确保没有在初始化模式，并相应的调用init方法：

```

if ( !initializing )
this.init.apply(this, arguments);

```

特别重要的是，init方式可以运行各种开销比较大的启动代码(连接到服务器，创建DOM元素，等等)。因为init方法只会运行一次。

<b>父类的方法</b>
在实现继承的，创建一个类继承父类的功能时候，一个很强烈的愿望就是有权限访问一个你覆写掉的方法。这本文讲的实现方法里，引入了一个新的临时方法._super，这是子类里唯一能调用父类同名方法的地方。
举个例子，如果你想调用父类的构造函数，你应该这样做：

```
var Person = Class.extend({
	init: function(isDancing){
		this.dancing = isDancing;
	}
});

var Ninja = Person.extend({
	init: function(){
		this._super( false );
	}
});

var p = new Person(true);
p.dancing; // => true

var n = new Ninja();
n.dancing; // => false

```

实现这个给你需要以下几个步骤，首先，被用来扩展已经存在的类的对象字面量会被merge到Person的实例上。在megre的过程中，需要检查一下，我们要merge的属性是否是个方法，替换的值是不是也是个方法，如果是，那么我们就需要做一些处理来实现super方法。

请注意，我们实现了一个匿名的闭包来封装新的加强版的super方法。一开始我们需要保存对老的this._super方法的引用（不管他存在不存在），然后做完我们想做的事情之后恢复它。这对于已经存在同名遍历的属性是很有用的。

接下去，我们创建一个_super方法，这个方法仅仅是对已经存在的父类方法的原型的引用。当他是对象的一个属性的时候，我们就不需要额外设置，或者是更新作用域，这里的上下文会自动设置。


注：看原文并不是很理解作者说的具体是什么，翻译比较晦涩，需要再根据代码，完成更易懂的翻译。