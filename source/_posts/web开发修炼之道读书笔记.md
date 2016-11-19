---
title: web开发修炼之道读书笔记
date: 2016-08-20 12:48:43
tags: [web,css,读书笔记]
---

## 团队合作
### 揭秘前端工程师
1.css是基本功
2.javascript，包括原生的和类库，框架
3.后端语言
### 欲精一行，必先通10行

### 提高重用性
### 冗余和精简的矛盾
### 磨刀不误砍柴工
### 制订规范
### 团队合作的最大难点

## 高质量的HTML

<!--more-->

### 标签的语义
| 标签名         | 英文全拼       | 中文翻译  |
| ------------- |:-------------:| -----:|
| div           | division      | 分割 |
|span |span|范围|
|ol|ordered list |有序列表|
|ul|unordered list |无序列表|
|li|list item|列表项目|
|dl|definition list|定义列表|
|dt|definition term|定义术语|
|dd|definition description|定义描述|
|del|deleted|删除|
|ins|inserted|插入|
|h1~h6|header |标题|
|p|paragraph|段落|
|hr|horizonal rule|水平尺|
|a|anchor|锚|
|abbr|abbreviation|缩写词|
|acronym|acronym|取首字母的缩写词|
|address|address|地址|
|var |variable|变量|
|pre|preformatted|预定义格式|
|blockquote|block quoting|区块引用语|
|strong|strong|加重|
|em|emphasized|加重|
|b|bold|粗体|
|i|italic|斜体|
|big|big|变大|
|small|small|变小|
|sup|superscripted|上标|
|br|break|换行|
|center|center|居中|
|font|font|字体|
|u|underlined|下划线|
|s|strikethrough|删除线|
|fieldset|filedset|域集|
|legend|legend|图标|
|caption|caption|标题|
**div 和span的设计目的是啥**todo
### 为什么要使用语义化标签
对搜索引擎友好。
先确定html，确定语义的标签，再来选用合适的css
### 如何确定你的语义化标签是否语义良好
去掉样式，看网页结构是否组织良好有序，是否仍然有很好的可读性
### 常见模块你真的很了解吗
需要注意几点：
* 尽可能少的使用无语义标签div和span
* 在语义不明显，既可以用p也可以用div的地方，尽量用p，因为p默认情况下有上下间距，去样式后的可读性更好，对兼容特殊终端有利
* 不要使用纯样式标签，例如b、font和u等，改用css设置。语义上需要强调的文本可以包在strong或者em标签里，strong和em有强调的寓意，其中strong的默认样式是**加粗**，em的样式*斜体*

## 高质量的css

### 怪异模式和DTD
标准模式：浏览器根据规范表现页面,元素的宽度由`padding+border+width`
怪异模式：模拟老师浏览器的行为以防止老站点无法工作。`width=padding+border`
4种DTD
1.`html 4.01 strict`
2.`html 4.01 loose`
3.`xhtml 1.0 xhtml11-transitional`
4.`xhtml 1.0 xhtml11-strict`
如果漏写DTD声明，firefox和chrome仍然会按照标准模式来解析网页，但在IE中(包括IE6,7,8)就会触发怪异模式

### 如何组织css
应用css的能力应该分成两部分：
1.CSS的API，重点是如何用css控制页面内元素的样式
2.CSS框架，重点是如何对css进行组织
### 推荐的base.css
一般网站的css分为三层
1.base，如css reset
2.common，模块化
3.pages
需要特别注意几点：
* `.fl{float：left;display：inline}`和`.fr{float：right;display：inline}`。除了设置float之外，还设置了`display：inline`.其作用是解决IE6双外边距bug。在IE6下，如果设置了元素浮动，同时又设置了margin-left或者margin-right，margin值会加倍。解决办法是设置display：inline。
* 元素居中时，需要指定宽度
* 让浮动元素的父容器能够根据浮动元素的高度而自适应高度，有三种做法：
>1.让父元素同时浮动，这种方法会让父元素也浮动起来
>2.浮动元素后紧跟一个用于清除浮动的空标签，这种方法会增加一个空标签，破坏了语义化。
>3.给父容器挂一个特殊的class，直接从父容器清除元素的浮动，例如`<div> class="clearfix"><div class="fl"></div></div>`

*`.zoom{zoom:1}`触发IE6的`hasLayout`

### 模块化css
* 如何划分模块---单一职责
> 模块与模块之间尽量不要包含相同的部分，如果有相同的部分，应该将他们提取出来，拆分成一个独立的模块。
> 模块应在保证数量尽可能少的原则下，做到尽可能简单，以提高重用性。
* 命名空间
>不要滥用子选择符
* 多用组合，少用继承。方便扩展
* 处理上下`margin`

### 低权重原则
* 当不同的选择符的样式设置有冲突时，会采用权重高的选择符设置的样式
>元素和伪元素选择器的权重：1
>class的权重：10
>id的权重：100
>行内样式的权重最高：1000

* 如果选择符权重相同，那么样式会遵循就近原则，选择最后**定义**的选择符
* 为了保证样式容易被覆盖，提供可维护性，CSS选择符需保证权重尽可能低
### css sprite
* 它只能合并用于背景的图片，对于`<img src=""/>`设置的图片，不能合并到`css sprite`里面
* 对于横向和纵向都平铺的图片，也不能使用css sprite。

> 使用css sprite技术时，需要考虑付出的代价

### css的常见问题
#### IE条件注释
* 只在IE下生效
```
<!--[if IE]>
<link type="text/css" href="test.css" rel="stylesheet"/>
<![endif] -->
```
* 在IE的特定版本
```
<!--[if IE 6]>
<link type="text/css" href="test.css" rel="stylesheet"/>
<![endif] -->
```
* 大于等于某个版本
```
<!--[if gt IE 6]>
<link type="text/css" href="test.css" rel="stylesheet"/>
<![endif] -->
```
> lte:小于等于
> lt：小于
> gte：大于等于
> gt：大于
* 在某个版本不生效
```
<!--[if ！ IE 6]>
<link type="text/css" href="test.css" rel="stylesheet"/>
<![endif] -->
```

#### 选择符前缀
* `*html`只针对IE6
* `+html`只针对IE7
>向后兼容性存在问题
>不能用于内联样式

#### 样式属性前缀
* `*`在IE6和IE7下生效
* `_`只针对IE6
```
<style type="text/css">
    .test{width: 80px;*width: 70px;_width:60px;}
</style>
```
>向后兼容性存在问题
>可以用于内联样式

#### 触发hasLayout
* `zoom：1`
* `position:relative`

### 块级元素和行内元素
* 块级元素
```
div -最常用的块级元素
dl - 和dt dd搭配使用的块级元素
form - 交互表单
h1 - 大标题
hr - 水平分隔线
ol - 排序表单
p - 段落
ul - 非排序列表
```
* 内联元素
```
a - 锚点
b - 粗体(不推荐)
br - 换行
em - 强调
font - 字体设定(不推荐)
i - 斜体
img - 图片
input - 输入框
label - 表格标签
select - 项目选择
small - 小字体文本
span - 常用内联容器，定义文本内区块
strike - 中划线
strong - 粗体强调
sub - 下标
sup - 上标
textarea - 多行文本输入框
tt - 电传文本
u - 下划线
```
块级元素和行内元素的区别：
* 块级元素独占一行，其宽度自动填满其父元素高度
* 相邻的行内元素会排列在同一行，直到一行排不下，才会换行，其宽度随元素的内容变化
* 块级元素可以设置width、height属性，行内元素设置无效

> display:inline-block:可以设置长宽，可以设置margin和padding值，但是不独占一行，宽度不占满父元素。

### relative、absolute和float
* 设置position:relative或position:absolute都可以让元素激活left、top、right、bottom和z-index属性（默认情况下这些属性未激活，设置了也无效）
* position:relative不会脱离文档流
* position:absolute完全脱离了文档流，不再在z-index:0保留占位符，其left、top、right、bottom值是相对于自己最近的一个设置了position:relative或position:absolute的祖先元素，如果祖先元素都没设置这两个属性，那么就相对于body元素
* float控制元素在同层里坐浮动或者右浮，float会改变正常的文档流排列，影响到周围元素
* position:absolute和float会隐身的改变display属性为display:inline-block。

### 居中
### 水平居中
1.行内元素水平居中
>给父元素设置text-align:center

2.确定宽度的块级元素的水平居中
> margin:0 auto;

3.不确定宽度的块级元素的居中
三种方法
* 用table实现
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>水平居中1</title>
	<style text="text/css">
	ul{list-style: none;margin: 0;padding: 0;}
	table{margin-left: auto;margin-right: auto;}
	.warp{background: #000;width: 500px;height: 100px;}
	.test li{float: left;display: inline;margin-right: 5px;}
	.test a{float: left;width: 20px;height: 20px;text-align: center;line-height: 20px;background: #316ac5;color: #fff;border: 1px solid #316ac5;text-decoration: none;}
	.test a:hover{background: #fff;color: #316ac5;}
	</style>
</head>
<body>
	<div class="wrap">
		<table>
			<tbody>
				<tr>
					<td>
						<ul class="test">
							<li><a href="#">1</a></li>
						</ul>
					</td>
				</tr>
			</tbody>
		</table>
		<table>
			<tbody>
				<tr>
					<td>
						<ul class="test">
							<li><a href="#">1</a></li>
							<li><a href="#">2</a></li>
							<li><a href="#">3</a></li>
						</ul>
					</td>
				</tr>
			</tbody>
		</table>
		<table>
			<tbody>
				<tr>
					<td>
						<ul class="test">
							<li><a href="#">1</a></li>
							<li><a href="#">2</a></li>
							<li><a href="#">3</a></li>
							<li><a href="#">4</a></li>
							<li><a href="#">5</a></li>
						</ul>
					</td>
				</tr>
			</tbody>
		</table>
	</div>
</body>
</html>
```
* 设置块级元素的display为inline，然后使用text-align:center来实现居中
```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>水平居中1</title>
		<style text="text/css">
		ul{list-style: none;margin: 0;padding: 0;}
		.warp{background: #000;width: 500px;height: 100px;}
		.test{text-align: center;padding: 5px;}
		.test li{display: inline-block;}
		.test a{float: left;width: 20px;height: 20px;text-align: center;line-height: 20px;background: #316ac5;color: #fff;border: 1px solid #316ac5;text-decoration: none;}
		.test a:hover{background: #fff;color: #316ac5;}
		</style>
	</head>
	<body>
		<div class="wrap">
			<ul class="test">
				<li><a href="#">1</a></li>
			</ul>
			<ul class="test">
				<li><a href="#">1</a></li>
				<li><a href="#">2</a></li>
				<li><a href="#">3</a></li>
			</ul>
			<ul class="test">
				<li><a href="#">1</a></li>
				<li><a href="#">2</a></li>
				<li><a href="#">3</a></li>
				<li><a href="#">4</a></li>
				<li><a href="#">5</a></li>
			</ul>
		</div>
	</body>
</html>
```
* 父元素设置float，position：relative和left：50%，子元素设置position：relative和left：-50%

### 垂直居中
1.父元素高度不确定的文本、图片、块级元素的垂直居中
通过给父元素设置相同的上下边距实现

2.父元素高度确定的单行文件的垂直居中
给父元素设置line-height来实现，line-hight值和父元素的高度值相同

3.父元素高度确定的文本、图片、块级元素的垂直居中
采用vertical-align+display:table-cell实现

### 网格布局
利用组合类

## 高质量的javascript
### 养成良好的编程习惯
#### 避免javascript冲突
* 用匿名函数将脚本包起来，可以有效的控制全局变量，避免冲突隐患
* 命名空间
```
<script type="text/javascript">
		var GLOBAL={};
		GLOBAL.namespace=function(str){
			var arr=str.split("."),o=GLOBAL;
			for (var i = (arr[0]=="GLOBAL"?1:0); i arr.length; i++) {
				o[arr[i]]=o[arr[i]]||{};
				o=o[arr[i]];
			}
}
```

#### 给程序提供统一的入口
* window.load
* DOMReady函数，`$(document).ready()`

#### 引入编译的概念
文件压缩
>目前很多前端的解决方案都引入了编译的概念，包括javascript和css，例如fis，angularjs等等，需要深入学习todo

### 分层和库
#### 层
一般分为base，common，page层
典型的javascript兼容问题
1.透明度
```
function setOpacity(node,level){
			node=typeof node=="string" ?document.getElementById(node):node;
			if (document.all) {
				node.style.filter='alpha(opacity='+level+')';
			}else{
				node.style.opacity=level/100;
			}
}
```
2.event对象
在IE下event对象是作为全局变量挂在window下面的，而其他浏览器会把event对象作为事件处理函数的参数传进去
3.冒泡
cancleBubble和stopPropaion
4.注册事件处理函数
* onxxx，不能叠加，会覆盖
* attachEvent(IE)和addEventListener

#### 库
jquery，yui等等
[框架比较](http://www.ibm.com/developerworks/cn/web/1404_wangfx_jsframeworks/)

>预留回调接口

#### 改变DOM样式的三种方式
* 设置DOMNODE的style属性

```
<span id="test">hello world</span>
<script type="text/javascript">
	var node=document.getElementById("test");
	node.style.color="red";
</script>
```
> css属性和javascript变量之间的对应关系


* 设置classname

```
<style type="text/css">
	.testStyle{color:red;background-color:black;
		font-size: 40px;font-weight: bold;}
</style>
<span id="test">hello world</span>
<script type="text/javascript">
	var node=document.getElementById("test");
	node.className="testStyle";
</script>
```

* 动态添加`<style>`标签

```
<script type="text/javascript">
	function addStyleNode(str){
		var styleNode=document.createElement("style");
		styleNode.type="text/css";
		if (styleNode.styleSheet) {
			styleNode.styleSheet.cssText=str;
		}else{
			styleNode.innerHTML=str;
		}
		document.getElementsByTagName("head")[0].appendChild(styleNode);
	}
</script>
```