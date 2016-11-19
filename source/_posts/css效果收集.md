---
title: css效果收集
date: 2016-10-21 10:56:33
tags: [css]
---
整理一些常见的css效果

## css文字扫光
来源[https://github.com/confidence68/blog/issues/211](https://github.com/confidence68/blog/issues/211)

html代码：

```
<div class="sample">
    <div class="blank_text">天行健，君子以自强不息</div>
</div>

```

<!--more-->

css代码：

```
.sample{
    background-color: #0E1326;
    padding-top:30px;
    overflow: hidden;
  }
  .blank_text{
    position: relative;
    width:200px;
    margin:20px auto;
    color: #fff;
    line-height: 1;
    font-size: 50px;
    font-size: 0.74074rem;
    text-align: center;
    overflow: hidden;
    font-family: "icomoon";
  }
.blank_text:after{
    width: 300%;
    height: 100%;
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    background: -webkit-gradient(linear, left top, right top, color-stop(0, rgba(15,20,40, 0.7)), color-stop(0.4, rgba(15,20,40, 0.7)), color-stop(0.5, rgba(15,20,40, 0)), color-stop(0.6, rgba(15,20,40, 0.7)), color-stop(1, rgba(15,20,40, 0.7)));
    -webkit-animation: slide ease-in-out 2s infinite; 
}
@-webkit-keyframes slide{
    0%{-webkit-transform:translateX(-66.666%);}
    100%{-webkit-transform:translateX(0);}


```

核心点在于background渐变和关键帧动画

{% codepen lixuejiang kkzkod 0 %}

## 图片扫光
来源[https://github.com/confidence68/blog/issues/211](https://github.com/confidence68/blog/issues/211)

html代码：

```

<div class="logo">
    <a href="http://www.haorooms.com"><img src="http://sandbox.runjs.cn/uploads/rs/216/0y89gzo2/banner03.jpg" /></a>
</div>


```

css代码

```

@keyframes aniBlink {
from {
margin-left:-440px
}
to {
    margin-left:500px
}
}
@-webkit-keyframes aniBlink {
from {
margin-left:-440px
}
to {
    margin-left:500px
}
}
.logo {
    position:relative;
    width:440px;
    height:160px;
    overflow:hidden;
}
.logo a {
    display:inline-block
}
.logo a:before {
    content:'';
    position:absolute;
    width:60px;
    height:160px;
    margin-top:0px;
    margin-left:-440px;
    overflow:hidden;
    z-index:6;
  overflow: hidden;
  background: -moz-linear-gradient(left, rgba(255, 255, 255, 0) 0, rgba(255, 255, 255, 0.2) 50%, rgba(255, 255, 255, 0) 100%);
  background: -webkit-gradient(linear, left top, right top, color-stop(0%, rgba(255, 255, 255, 0)), color-stop(50%, rgba(255, 255, 255, 0.2)), color-stop(100%, rgba(255, 255, 255, 0)));
  background: -webkit-linear-gradient(left, rgba(255, 255, 255, 0) 0, rgba(255, 255, 255, 0.2) 50%, rgba(255, 255, 255, 0) 100%);
  background: -o-linear-gradient(left, rgba(255, 255, 255, 0) 0, rgba(255, 255, 255, 0.2) 50%, rgba(255, 255, 255, 0) 100%);
  -webkit-transform: skewX(-25deg);
  -moz-transform: skewX(-25deg);
}
.logo:hover a:before {
 -webkit-animation:aniBlink .8s ease-out forwards;
 -moz-animation:aniBlink .8s ease-out forwards;
 -o-animation:aniBlink .8s ease-out forwards;
 animation:aniBlink .8s ease-out forwards
}


```

{% codepen lixuejiang wzRzyX 0 %}

其他链接：
[css常用效果总结](http://www.haorooms.com/post/css_common)
[css的不常用效果总结](https://github.com/confidence68/blog/issues/170)