---
title: 'cookie,session,localstorage,sessionStorage区别和介绍'
date: 2016-09-11 19:38:20
tags: [cookie,localStorage,sessionStorage,session]
---
首先，从存储位置来看，可以分为服务端存储和客户端存储两种。
* 服务端存储：session
* 浏览器端存储：cookie，localStorage，sessionStorage

> 作为浏览器端存储的cookie，也可以在服务端对其进行操作

# cookie

cookie的内容主要包括：名字，值，过期时间，路径和域。路径与域一起构成cookie的作用范围。若不设置过期时间，则表示这个cookie的生命周期为浏览器会话期间，关闭浏览器窗口，cookie就消失。这种生命期为浏览器会话期的cookie被称为会话cookie。会话cookie一般不存储在硬盘上而是保存在内存里，当然这种行为并不是规范规定的。若设置了过期时间，浏览器就会把cookie保存到硬盘上，关闭后再次打开浏览器，这些cookie仍然有效直到超过设定的过期时间。存储在硬盘上的cookie可以在不同的浏览器进程间共享，比如两个IE窗口。而对于保存在内存里的cookie，不同的浏览器有不同的处理方式。以下是cookie的操作方式：

<!--more-->

```
//设置cookie
function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays*24*60*60*1000));
    var expires = "expires="+d.toUTCString();
    document.cookie = cname + "=" + cvalue + "; " + expires;
}
//获取cookie
function getCookie(cname) {
    var name = cname + "=";
    var ca = document.cookie.split(';');
    for(var i=0; i<ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1);
        if (c.indexOf(name) != -1) return c.substring(name.length, c.length);
    }
    return "";
}
//清除cookie  
function clearCookie(name) {  
    setCookie(name, "", -1);  
}  

```
由于cookie是存储在客户端，如果cookie被窃取，会造成安全性问题，如跨站请求伪造。另外由于每次客户端像服务端发起请求的时候都会带上cookie，所以会增加每次http请求的传输量。因此提出了cookie隔离的概念。
## cookie隔离
把js，css，图片等静态资源放在非主域名下，这样在请求这些资源的时候就不会带上主域名的cookie。从而降低传输成本和服务端的压力

# session
* session存放在服务端，类似散列表的形势，每一个session有一个sessionId。
* 客户端发起请求的时候会带上sessionID
* 如果没有sessionID，在服务端会新建一个sessionId，然后返回给客户端

# localStorage
localStorage是html5提出的，在此之前，IE已经有userData这种实现方式.localStorage是windows的一个对象：
![http://ggbond.qiniudn.com/WechatIMG13.jpeg](http://ggbond.qiniudn.com/WechatIMG13.jpeg)
## 基本操作
### set
```
localStorage.name = 'ggbond';
localStorage["name"] = "ggbond";
localStorage.setItem("name","ggbond");
```
### get
```
var name = localStorage["name"];
var name = localStorage.name;
var name = localStorage.getItem("name");
```
### foreach
localStorage提供了key方法，来获取里面所有的key
```
var storage = window.localStorage;
function showStorage(){
 for(var i=0;i<storage.length;i++){
  //key(i)获得相应的键，再用getItem()方法获得对应的值
  document.write(storage.key(i)+ " : " + storage.getItem(storage.key(i)) + "<br>");
 }
}
```

### delete
`localStorage.removeItem("name");`

### clearAll
`localStorage.clear()`

## 相关事件
HTML5的本地存储，还提供了一个storage事件，可以对键值对的改变进行监听，使用方法如下：
```
if(window.addEventListener){
 	window.addEventListener("storage",handle_storage,false);
}else if(window.attachEvent){
 	window.attachEvent("onstorage",handle_storage);
}
function handle_storage(e){
 if(!e){e=window.event;}
 //showStorage();
}
```
对于事件变量e，是一个StorageEvent对象，提供了一些实用的属性，可以很好的观察键值对的变化，如下表：

| Property | Type | Description |
| -------- |---- | ----------- |
|key|String|The named key that was added, removed, or moddified
|oldValue|Any|The previous value(now overwritten), or null if a new item was added
|newValue|Any|The new value, or null if an item was added
|url/uri|String|The page that called the method that triggered this change

# sessionStorage
sessionStorage 方法针对一个 session 进行数据存储。当用户关闭浏览器窗口后，数据会被删除。操作方法和localStorage类似
在此不再赘述

# 其他
另外还有HTML5还提供了一种客户端存储数据的方式叫websql，有兴趣的同学可以参考[基于 HTML5 中的 Web SQL Database 来构建应用程序](http://www.ibm.com/developerworks/cn/web/1108_zhaifeng_websqldb/)这篇文章