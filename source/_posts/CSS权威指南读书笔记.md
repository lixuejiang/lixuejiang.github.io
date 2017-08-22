---
title: CSS权威指南读书笔记
date: 2015-08-20 12:24:53
tags: [css,读书笔记]
TOC: true
---
# CSS权威指南

## 一、候选表达式

### 候选样式表可以实现主题切换

- 有title的link样式表为首选样式表


## 二、选择器

## 三、结构和层叠
## 四、值和单位
## 五、字体
### font-family
* 一个font-family下有很多字体，浏览器会根据用户安装的字体进行渲染
* 可以指定多个font-family，在操作系统不支持第一个字体系列的情况下，使用第二个，以此类推
* 如果font-family的名称是关键字或者含有特殊字符，需要用引号括起来
* 不能把系统支持的非关键字的名称用引号括起来

<!--more-->

### font-weight
* 值：`normal`|`bold`|`bolder`|`lighter`|`100`|`200`|`300`|`400`|`500`|`600`|`700`|`800`|`900`|`inherit`
* 计算值：数字值(如100等等),或者某个数字值加上某个相对数
* 400对应normal，700对应bold
* 加粗度上移或者下移？？？

### font-size
* 绝对值：`xx-small`|`x-small`|`small`|`medium`|`large`|`x-large`|`xx-large`|`<length>`|`<percentage>`|`inherit`
* 相对值：`smaller`|`larger`，元素的大小相对于其父元素的大小在绝对梯度上上移或下移，缩放因子为1.2
* 初始值：medium
* 百分数：根据父元素的字体大小来计算
* 计算值：绝对长度
### font-style
* 值:`italic`|`oblique`|`normal`|`inherit`
### font-variant
* 值：`small-caps`|`normal`|`inherit`

### font
* 把所有的font相关的属性合在一起，去掉关键字，只用font表示
* font-size相对于父元素来计算，line-height相对于元素的font-size来计算
* 各个属性是顺序无关的
* normal值可以忽略不写
* font-size必须在line-height之后

### 系统字体
* caption：用于有标题的控件，如按钮
* icon：用于对图片加标签
* menu：用于菜单
* message-box：用于对话框
* small-caption：用于对小控件加标签
* status-bar：用于窗口状态条

> 利用这些值，可以创建一个基于web的应用，使之看上去非常类似于用户操场系统自带的应用。

### 字体匹配
1.访问机器上安装的所有字体，还有浏览器内置的字体，找到指定的字体
2.根据找到的字体，列出字体属性列表。基于这个列表，浏览器会先对显示元素时使用的字体系列做第一个选择，如果匹配，那么浏览器就使用这个字体，否则，需要多做一些工作



## 六、文本属性
### 文本缩进text-indent
* 值：<length>|<百分比>|inherit
* 应用于：块级元素
* 百分数：相对于包含块的宽度
* 计算值：对于百分数值，要根据指定确定
> 如果想把一个行内元素的第一行缩进，可以用左内边距或外边距创造这种效果

### 文本对齐text-align
* 值：left|center|right|justify|inherit

### line-height
* 值： <length>|<百分比>|<number>|normal|inherit
* 行间距=line-height减去font-size
* 行间距除2是元素的行内框
> line-height定义了元素中文本基线之间的最小距离，文本基线拉开的距离可能比line-height值更大

### vertical-align
* 值：baseline|sub|super|top|text-top|Middle|bottom|text-bottom|百分比|length|inherit
* 应用与行内元素和表单元格
* 百分比：相对于元素的line-height值
* 长度：相对于对齐前，就是没有设置vertical-align之前上升或者下降指定距离
> vertical-align不影响块级元素中内容的对齐
> 垂直对齐的文本不会成为另一行的一部分，也不会覆盖其他行中的文本

### word-spacing
用于修改字间间隔
* 值：<length>|normal|inherit
* 对于normal，为绝对长度0，不影响间隔，否则为绝对长度

### text-transform
* 值：uppercase|lowercase|capitalize|none|inherit

### text-decoration 
* 值：none|[underline||overline||line-through||blink]|inherit
* text-decoration 的值会替换而不是累积
* 不能继承，也不能去掉父元素的text-decoration 

### text-shadow
* 值：none|[<clolor>||<length><length><length>?,]*[<clolor>||<length><length><length>?]|inherit

### white-space
* 值：normal|nowrap|pre|pre-wrap|pre-line|inherit

|值         |空白符 |换行符|自动换行
|---------- | -----|-----|------|
| pre-line  | 合并  |保留 |允许
| mormal    | 合并  |忽略 |允许
| nowrap    | 合并  |忽略 |不允许
| pre       | 保留  |保留 |不允许
| pre-wrap  | 保留  |保留 |允许

### direction
* 值：ltr|rtl|inherit

## 七、基本视觉格式化
### 水平格式化
正常流中块级元素框的水平部分总和就等于父元素的width
* 水平属性：margin，border，padding，width
* 7个属性值加起来是父元素的width值
* 只有margin-left，margin-right，width可以设置为auto，其余属性必须设置为特定的值
* margin-left，margin-right，width中的某个值为auto，而余下两个属性指定为特定的值，那么设置为auto的属性为父元素的width减去不为auto的值
* 两个外边距都为auto时，平分两个外边距，使元素居中
* 某个外边距以及width设置为auto，设置为auto的外边距会变为0
* 三个属性值都设置为auto的时候，外边距会变为0，width会尽量宽，与默认情况相同
* 负外边距会使元素超出父元素的内部
* 百分比：相对于父元素的width来计算
> 边框不能使用百分数，只能是长度，所以使用百分数将无法创建灵活的元素布局
> 非替换块级元素的所有规则同样适用于替换块元素，如果width为auto，元素的宽度则是内容的固有宽度，如果不设置高度，则高度按照比例变化，反之亦然

2015年3月31日08`:`54`:`53
### 垂直格式化
* 相关属性：margin-top，margin-bottom，border-top，border-bottom，padding-top，padding-bottom，height
* 百分数：相对于父元素的height计算
* 垂直外边距：垂直相邻外边距会合并，去较大的值作为边界
* 在包含块上设置边框或内边距时，会使其子元素的外边距包含在包含快内。外边距合并时，只会合并父元素和相邻元素。
* 没有边框或外边距的情况下，子元素和父元素跟相邻元素一起计算边界合并
* 负外边距：都为负时，取绝对值大的，一正一负则相加
2015年4月3日08`:`52`:`53

### 行内布局
* 匿名文本：所有未包含在行内元素中的字符串
* 行间距：line-height减去font-size
* 行内框：对于非替换元素，行内框的高度刚好等于line-height的值，对于替换元素，行内框的高度恰好等于内容区的高度。
* 行内元素的背景应用与内容区以及所有内边距

### 行内格式化
* line-height只影响行内元素和其他行内内容，不影响块级元素
* 在块级元素上声明line-height会为该块级元素的内容设置一个最小行框高度
* 行内框的顶端必须与该行行框的顶端对齐
* 负边距会使替换元素的行内框小于正常大小，这是使替换元素挤入其他行的唯一办法
2015年4月14日08`:`31`:`06

### 改变元素显示(display)
* 值：none|inline|block|inlie-block|list-item|run-in|table|inline-table|table-row-group|table-header-group|table-footer-group|table-row|table-column-group|table-column|table-cell|table-caption|inherit
* inline-block元素作为额替换元素放在行中，在元素内部，会像块级元素一样设置内容的格式，也有width和height，
2015年4月14日08`:`31`:`06

## 八、内边距、边框和外边距
* 内边距包括背景，外边距不包括

### marging

* margin： top right bottom left（顺时针）
* margin的百分数相对于父元素的width计算，包括上下外边距
* 值复制：左复制右，下复制上，右复制上
> 对于只包含文本的行，能改变行间距离的属性只有line-height、font-size和vertical-align

### 边框（border）
没有样式就没有边框
* border-style：[none|hidden|dotted|dashed|solid|double|groove|ridge|iinset|outset]{1,4}|inherit
* border-width:[thin|medium|thick|<length>]{1,4}|inherit
> 如果希望边框出现，就必须先声明一个边框样式
* border-color:[<color>|transparent]{1,4}|inherit
* border:[<border-width>||<border-style>||<border-color>]|inherit
> 不论为行内元素的边框指定怎么样的宽度，元素的行高都不会改变

### (内边距)padding
2015年4月20日19`:`08`:`21

## 九、颜色和背景
### 前景色
* color：<color>|inherit

### 背景色
* background-color：<color>|transparent|inherit

### 背景图像
* background-image：<uri>|none|inherit
> 背景图像不能继承
* background-repeat：repeat|repeat-x|repeat-y|no-repeat|inherit
* background-position：<`center`>|百分数|长度
* background-attachment：scroll|fixed|inherit
> 如果background-position有两个值，他们必须一起出现，而且如果这两个值是长度或百分数，则必须按水平值在前垂直值在后的顺序

2015年4月21日08`:`37:12

## 十、浮动和定位
### 浮动
* 值：left|right|none|inherit
* 浮动元素周围的外边距不会合并
* 如果确实要浮动一个非替换元素，则必须为该元素声明一个width
* 浮动元素会生成一个块级框，而不论这个元素本身是什么
> 1、浮动元素的左（或者右）外边界不能超出其包含块的左（或者右）内边界
> 2、浮动元素的左（或者右）外边界必须是源文档中之前出现的左（或者右）浮动元素的左（或者右）外边界
> 3、左浮动元素的右外边界不会在其右边右浮动元素的左外边界的右边，反之亦然。
> 4、一个浮动元素的顶端不能比其父元素的内顶端更高，如果一个浮动元素在两个合并外边距之间，放置这个浮动元素时就好像在两个元素之间有一个块级父元素，意思就是三个元素，如果中间一个浮动，不会浮动到第一个上面，还是在中间的位置
> 5、浮动元素的顶端不能比之前所有浮动元素或者块级元素的顶端更高
> 6、如果源文档中一个浮动元素之前出现另外一个元素，浮动元素的顶端不能比包含该元素所生产框的任何行框的顶端更高
> 8、浮动元素必须尽可能高的放置
> 9、左浮动元素必须向左尽可能远，右浮动元素必须向右尽可能远。
* 浮动元素会延伸，从而包含其所有后代浮动元素。
* 行内框与一个浮动元素重叠时，其边框、背景和内容都在该浮动元素之上显示
* 块级元素与一个浮动元素重叠时，其边框和背景在该浮动元素之下显示，而内容在浮动元素之上显示
* 清除浮动

### 定位(position)
* 值：static|relative|absolute|fixed|inherit
* static：元素框正常生成，块级元素生成一个矩形框，作为文档流的一部分，行内元素则会创建一个或多个行框。
* relative：元素框偏移某个距离，元素仍保持其未定位前的形状，她原本所占的空间仍保留
* absolute：元素框从文档流中完全删除，并相对于其包含快定位，元素原来在文档流里的空间会消失，定位后生成一个块级框
* fixed：固定
#### 包含快
* 根元素的包含快由用户代理建立，跟元素一般是html元素，有的浏览器会使用body作为根元素
* 非根元素，如果其position值是relative或static，包含快则由最近的块级框、表单元格或行内块祖先框的内容边界构成
* 对于一个非根元素，如果其position值是absolute，包含快设置为最近的值不是static的祖先元素，这个过程如下
> 如果这个祖先是块级元素，包含快则设置为该元素的内边距边界
> 如果祖先是行内元素，包含块则设置为该祖先元素的内容边界
> 如果没有祖先，包含块设置为初始包含块

#### 偏移属性(top，right，bottom，left)
* 值：<length>|<percentage>|auto|inherit
* 这些属性描述的是距离包含块最近边的偏移

#### 内容溢出和裁剪
overflow：内容在元素中放不下时，该如何处理
* 值：visible|hidden|scroll|auto|inherit
* 应用于：块级元素和替换元素

clip：绝对定位元素的内容溢出其内容框
#### 绝对定位
* 元素绝对定位时，会从文档流中删除，然后相对于包含块定位，
* 绝对定位元素的包含块是最近的position值不为static的祖先元素
* 元素绝对定位时，会为其后代元素建立一个包含块
* 如果文档可以滚动，定位元素会随着它滚动，只要绝对定位元素不是固定定位元素的后代
#### 固定定位
#### 相对定位
2015年4月30日08`:`44`:`24

## 十一、表布局
暂时不看
## 十二、列表与生成内容
### 列表
#### 列表类型(list-style-type)
|值|类型|
|--|--|
|disc|使用一个实心圆作为列表项标志|
|circle|使用一个空心圆作为列表项标志|
|square|使用一个空心或者实心方块作为列表项标志|
|decimal|1,2,3,4,5，。。 。。。|
|decimal-loading-zero|01,02,03,04.。。。。。|
|upper-alpha|A,B,C,D,E|

* list-style-type只能用于display值为list-item的元素


#### list-style-image
* 值：<uri>|none|inherit

#### list-style-position
* 值：inside|outside|inherit
### 生成内容
由浏览器创建的内容，而不是由标志或内容来表示
* 禁止浮动或定位：before和：after内容
* 禁止使用列表样式属性以及表属性
* 如果：before和：after选择器的主体是块级元素，则display属性只接受值none、inline、block和marker。其他值都处理为block
* 如果：before和：after选择器的主体是一个行内元素，属性display只能接受值none和inline，所有其他值都处理为inline
 2015年5月5日08`:`29`:`14

## 十三、用户界面样式
### 系统字体
|关键字|描述|
|------|---|
|caption|标题控件使用的样式，如按钮和下拉控件
|icon|操作系统图标标签所用的字体样式，如硬盘驱动器
|menu|下拉菜单和菜单列表中文本使用的字体样式
|message-box|对话框中文本使用的字体样式
|small-caption|标题小控件的标签使用的字体样式
|status-bar|窗口状态条中文本使用的字体样式
* 这些值只能用于font属性，而且是简写

### 系统颜色
|关键字|描述|
|--|--|
|ActiveBorder|活动窗口的外边框
|ActiveCaption|活动窗口标题的背景色
|AppWorkspace|应用程序中的背景色
|Background|桌面的背景色
|ButtonFace|三维按钮面上使用的颜色
|ButtonHighlight|按钮高亮颜色
|ButtonShadow|阴影色
|ButtonText|按钮文本的颜色
|CaptionText|。。。|

### 光标
### 轮廓(outline-style)
* 值：none|dotted|dashed|solid|double|groove|ridge|inset|outset|inherit
* 一个元素周围必然有相同的轮廓样式

### 轮廓宽度(outline-width)
* 值：thin|medium|thick|<length>|inherit

### 轮廓颜色(outline-color)
* 值：<color>|invert|inherit

## 十四、非屏幕媒体