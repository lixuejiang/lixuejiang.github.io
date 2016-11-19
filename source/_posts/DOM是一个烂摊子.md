---
title: DOM是一个烂摊子
date: 2016-09-25 20:12:17
tags: [翻译]
---
本文翻译自[DOM is a mess](http://www.slideshare.net/jeresig/the-dom-is-a-mess-yahoo),主要讲了dom方法在不同的浏览器里存在的一些bug

# 混乱的DOM
几乎每一个DOM方法在某个浏览器里都存在某种问题
## 一些老方法如：
### getElementById
这似乎是最常用的dom方法，但是也有一些很奇怪的地方：

* IE和老版本的Opera会返回name == id 的元素
* 在xml里不能很好的工作

### getElementByTagName
似乎是比较常用的dom方法，在ie里也是漏洞百出

* ‘*’在IE 5.5返回空元素
* ‘*’在IE7里如果是`object`则返回空元素
* 在IE里，如果一个元素的ID是`length`,则`.length`会被重写

<!--more-->

## 一些新的方法如：
### getElementByClassName
这个方法在FF 3，Safari 3 Opera 9.6里实现，存在一些已知的bug：

* 在火狐里不能复写`HTMLElemet.prototype.getElementByClassName`方法
* Opera不能匹配第二个类（e.g. class='a b',不能匹配b）

### querySelectorAll
该方法用css选择器查找dom元素，FF 3.1,Safari 3.1 opera 10，IE8里实现实现了这个方法，存在的bug：

* IE8的混合模式不支持该方法
* Safari 3.1存在内存泄露
* Safari 3.2在混合模式下不能匹配大写字母的选择器
* 在xml里不能匹配`#id`选择器

# 编写跨浏览器的代码
策略：
## 选择你的浏览器
需要了解各个浏览器的市场占有率，操作系统情况
## 了解你的敌人
![http://ggbond.qiniudn.com/kye.png](http://ggbond.qiniudn.com/kye.png)

### 浏览器bugs

这是你主要的关注点，你的防守策略是要有一个很好的测试套件来阻止库的回退和应对浏览器的新版本的发布，而进攻策略应该是特性模拟
什么是bug：是那些没有指定，没有文档，表现怪异的行为吗？
针对浏览器bug，你只有不停的测试。

### 外部代码
使你的代码能经受任何环境的考验：
* 通过反复试验
* 继承其他的库或者奇怪的代码到你的测试套件

### 环境测试
需要100%通过的测试项：

* 标准模式
* 怪异模式
* 和prototype以及scriptaculous的集成
* 和Mootools的集成

### 污染
#### 确保你的代码不会打断外部代码

* 使用严格模式的命名空间
* 不要扩展外部的对象和元素

#### 不好的做法

* 使用全局变量
* 扩展原生对象（Array，Object）
* 扩展原生DOM

### 特性缺失

* 典型的例子就是老实浏览器缺失一些指定的特性
* 最佳方案是优雅降级，回退到一个简单的页面
* 不要对不支持的浏览器做出假设，对于不能测试的浏览器，需要提供一个平滑的回滚
* 对象检测能很好的工作

### 对象检测

* 检测对象或者属性是否存在
* 检测api是否存在
* 不要测试API的兼容性，bug可能会依然存在，需要通过特性模拟来分开测试这些api

### 事件绑定

```
function attachEvent(elem,type, handle){
	if(elem.addEventListener){
		elem.addEventListener(type,handle,false);
	}else if(elem.attachEvent){
		elem.attachEvent('on'+ type,handle);
	}else{
		elem['on'+type] = handle;
	}
}
```

### 特性模拟

* 比对象检测更先进
* 确保API能像宣传的那样工作
* 能平滑的解决bug

### 校验API

```
//运行一次，在程序开始的地方
var ELEMENTS_ONLY = (function(){
	var div = document.createElement('div');
	div.appendChild(document.createComment('test'));
	return div.getElementsByTagName('*').length === 0;
})();

var all = document.getElementsByTagName('*');

if(ELEMENTS_ONLY){
	for (var i = 0; i < all.length; i++) {
		action(all[i]);
	};
}else{
	for (var i = all.length - 1; i >= 0; i--) {
		if(all[i].nodeType === 1){
			action(all[i]);
		}
	};
}
```

### 弄清楚命名
![弄清楚命名](http://ggbond.qiniudn.com/fon.png)
### 回归

* 删除或者修改未指定的api
* 对象检测也有用
* 模拟积极发布的浏览器，所有的厂商都会提供beta的版本

### DOM遍历
有很多种DOM遍历的方法

#### 传统的DOM遍历方法

* getElementsByTagName
* getElementById
* getElementsByClassName
* .children
* getElementsByName
* .all[id]

### 自上而下的css选择器

* 传统的样式遍历，被大多数库使用
* 从左到右匹配
* `div p` :查找所有的div，找到里面的段落
* 需要对结果进行merge
* 需要删掉重复的元素

![元素查找](http://ggbond.qiniudn.com/find.png)
![http://ggbond.qiniudn.com/unique.png](http://ggbond.qiniudn.com/unique.png)

### 自下而上的选择器

* 从右到左匹配
* `div p`:找到所有的段落，看是否有div的祖先
* `div #foo`查询更快
* 比较而言，`#foo div`查询更慢

一些很好的特性
* 只有一次DOM查询
* 不需要merge
* 每次处理都是一次过滤
![http://ggbond.qiniudn.com/find1.png](http://ggbond.qiniudn.com/find1.png)
