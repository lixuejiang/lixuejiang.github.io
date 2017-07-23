---
title: 深入研究Virtual DOM
date: 2016-12-18 10:10:12
tags: [翻译,DOM]
---

对Virtual DOM这个名词并不陌生，但是有什么深入的理解谈不上。看到medium上rajaraodv写的[The Inner Workings Of Virtual DOM](https://medium.com/@rajaraodv/the-inner-workings-of-virtual-dom-666ee7ad47cf#.o9t0ifnaf)这篇文章，比较深入的介绍了Virtual DOM的各个方面，在此翻译一下。

这篇文章比较简单，但是图片比较多。

<!--more-->

![http://ggbond.qiniudn.com/1-TF0TZszVwpYc1Pba7Dbk7Q.png](http://ggbond.qiniudn.com/1-TF0TZszVwpYc1Pba7Dbk7Q.png)

Virtual DOM (VDOM 也叫 VNode) 很魔幻 ✨，但是也很复杂以至于让人难以理解😱。像React，Preact这些js的库都用到了Virtual DOM。不幸的是，我没有找到任何一篇深入浅出的解释VDOM文章或者文档，所以我决定自己写一篇。

> 注意：这篇文章很长，为了更通俗易懂，我加了很多图片，同时这也使这篇文章很长。

>  我用了Preact的代码，希望将来你很容易看懂，但是我觉得大多数情况下也适用于React。我希望读到这篇文章的人能更好的理解React和Preact，也为他们的发展做出一点贡献

本文会举一个简单的例子，然后介绍不同的场景，让你理解他们是怎么样运行的：

1. Babel 和 JSX
2. 创建虚拟节点-只是一个单一的虚拟DOM元素
3. 处理组件和子组件
4. 初始渲染和创建DOM元素
5. 重新渲染
6. 删掉DOM元素
7. 替换DOM元素

# App
这个App是一个简单的[可过滤搜索器](http://codepen.io/rajaraodv/pen/BQxmjj)。包含“FilteredList”和“List”两个组件。List组件渲染了一个列表（默认值是“California”和“New York”）。App还有一个搜索框，通过在搜索框里输入文字来过滤列表。

![http://ggbond.qiniudn.com/2016-12-18%2010:29:53.png](http://ggbond.qiniudn.com/2016-12-18%2010:29:53.png)

# 大图
首先，我们用JSX来写组件，然后用Babel的CLI工具转成纯JS。然后用Preact的[“h” (hyperscript)函数](https://github.com/dominictarr/hyperscript)转成VDOM树。最终Preact的Virtual DOM算法把VDOM转换成真正的DOM，这样就生成了我们的App。


![http://ggbond.qiniudn.com/1-imVzO1P2k1cIuf04_rGYGw.png](http://ggbond.qiniudn.com/1-imVzO1P2k1cIuf04_rGYGw.png)

在了解VDOM的生命周期之前，先来了解一下JSX.

## Babel和JSX
在React和Preact这些库里，没有html，只有JavaScript。所以我们需要在JavaScript里写html，但是在纯js里写DOM简直是噩梦😱

对我们的App来说，html像下面这样

![http://ggbond.qiniudn.com/1-KExHlJcp3om5h1JFA09Ahw.png](http://ggbond.qiniudn.com/1-KExHlJcp3om5h1JFA09Ahw.png)
![http://ggbond.qiniudn.com/1-uOSYO2xCEcuGWNzKNjKUnA.png](http://ggbond.qiniudn.com/1-uOSYO2xCEcuGWNzKNjKUnA.png)

这就是jsx，允许你在JavaScript里写html，然后也可以在{}里使用JavaScript。

用jsx写组件就很容易：

![http://ggbond.qiniudn.com/1-zUYkQpIdUdPp_SIOnEX8Bg.png](http://ggbond.qiniudn.com/1-zUYkQpIdUdPp_SIOnEX8Bg.png)

![http://ggbond.qiniudn.com/1-Xc92YmYZeaJ6oX0PCGYfyw.png](http://ggbond.qiniudn.com/1-Xc92YmYZeaJ6oX0PCGYfyw.png)

## 把jsx转换成JavaScript
jsx很酷，但是不是有效的JS，浏览器不支持。我们需要的是真实的DOM。JSX仅仅是在写DOM的表现层的时候有用。
所以我们需要一个函数来把jsx转换成相应的JSON对象（VDOM,也是一个树形结构），然后我们可以把这个json对象作为创建真实DOM的输入。

这个函数就叫h，和React里的[React.createElement](https://facebook.github.io/react/docs/react-api.html#createelement)是一样的功能。

> “h” 代表hyperscript


怎么样把jsx转换成h函数呢，这就是Babel干的事情。Babel遍历每一个JSX节点，把他们转换成h函数的调用

![图片](http://ggbond.qiniudn.com/1-TC2uwjc4mBrVHzOd0AeShw.png)

## Babel JSX (React Vs Preact)
Babel默认会把jsx转成React.createElement调用，因为默认是React。

但是我们也能通过添加[Babel编译宏](https://babeljs.io/docs/plugins/transform-react-jsx/)，把这个函数的名字改成任何我们想要的名字：

```

Option 1:
//.babelrc
{   "plugins": [
      ["transform-react-jsx", { "pragma": "h" }]
     ] 
}
Option 2:
//Add the below comment as the 1st line in every JSX file
/** @jsx h */


```

## 挂载到真实的DOM

starting mount和render函数都被转换到了h函数里，这是一切的开端：

```

//Mount to real DOM
render(<FilteredList/>, document.getElementById(‘app’));
//Converted to "h":
render(h(FilteredList), document.getElementById(‘app’));



```

## h函数的输出

h函数接收Babel转换后的JSX，创建一个叫“VNode”的节点（React通过“createElement”创建ReactElement）一个Preact的“VNode”（或者是React的“Element”）就是一个包含自身属性和子元素的DOM节点，看起来像这样：

```

{
   "nodeName": "",
   "attributes": {},
   "children": []
}


```

举个🌰，我们的App的DOM节点看起来像这样：


```

{
   "nodeName": "input",
   "attributes": {
    "type": "text",
    "placeholder": "Search",
    "onChange": ""
   },
   "children": []
}

```

> 注意：h函数并不会创建整个DOM树，对于指定的节点，只创建一个js的对象，但是因为render函数的参数是一个树形的DOM，最终的VNode看上去就像一棵树
> 
> 相关代码：
> h：[https://github.com/developit/preact/blob/master/src/h.js](https://github.com/developit/preact/blob/master/src/h.js)
> VNode：[https://github.com/developit/preact/blob/master/src/vnode.js](https://github.com/developit/preact/blob/master/src/vnode.js)
> render：[https://github.com/developit/preact/blob/master/src/render.js](https://github.com/developit/preact/blob/master/src/render.js)
> buildComponentFromVNode：[https://github.com/developit/preact/blob/master/src/vdom/diff.js#L102](https://github.com/developit/preact/blob/master/src/vdom/diff.js#L102)


## Preact的虚拟DOM算法流程图

下面的流程图介绍了组件和子组件是如何创建，更新和删除的。也展示了像“componentWillMount”这样的生命周期函数是何时被调用的

> 我们会一步一步的来分析每一个过程，所以不要觉得太复杂。


![生命周期](http://ggbond.qiniudn.com/1-TF0TZszVwpYc1Pba7Dbk7Q.png)

要马上理解确实很困难，让我们根据不同的场景来一步步看：

> 我会用黄色来高亮生命周期相关的部分：


### 场景1：初始创建App

#### 1.1为指定的组件创建VNode（Virutal DOM）
这张图展示了为给定组件创建VNode树的初始循环，在这个循环里没有创建子组件（创建子组件的过程略有不同）

![VNODE](http://ggbond.qiniudn.com/1-JVwKJe82S1wHFqzr17DQ4g.png)
下面这张图展示了当我们的App第一次运行的时候发生了什么，Preact最终为FilteredList组件创建了一个包含子组件和自身属性的VNode

![图](http://ggbond.qiniudn.com/1-CI75nMf-7j4ldxPJQ8nciQ.png)

目前为止，我们有了一个VNode，其中div是它的父节点，input和List是它的子节点

> 相关代码：
> 大多数生命周期函数：[https://github.com/developit/preact/blob/master/src/vdom/component.js](https://github.com/developit/preact/blob/master/src/vdom/component.js)

#### 1.2 如果不是组件则创建真实的DOM
这一步主要是创建父节点div，循环创建子节点

![http://ggbond.qiniudn.com/1-X1tcv3n47OuAExoTBu3Q5A.png](http://ggbond.qiniudn.com/1-X1tcv3n47OuAExoTBu3Q5A.png)

div显示如下：
![http://ggbond.qiniudn.com/1-l-FmgMheFRYFuhCJQA4gcQ.png](http://ggbond.qiniudn.com/1-l-FmgMheFRYFuhCJQA4gcQ.png)

> 相关代码：
> document.createElement: [https://github.com/developit/preact/blob/master/src/dom/recycler.js](https://github.com/developit/preact/blob/master/src/dom/recycler.js)


#### 1.3 重复创建子节点
这一步，要循环创建所有节点，对我们的App来说，就是input和List

![http://ggbond.qiniudn.com/1-XadPNReNXRQyIl6FJOpPlQ.png](http://ggbond.qiniudn.com/1-XadPNReNXRQyIl6FJOpPlQ.png)

#### 1.4 把子节点添加到父节点
这一步处理叶子节点，因为input有一个div的父节点，我们把input作为div的子节点，然和创建List，也就是div的第二个子节点

![http://ggbond.qiniudn.com/1-iLQQTuRdKIhlsh5Vgaox0w.png](http://ggbond.qiniudn.com/1-iLQQTuRdKIhlsh5Vgaox0w.png)

到这一步，我们的app看上去像这样：
![http://ggbond.qiniudn.com/1-uyA28eM-UBnU0SSX8vPSdg.png](http://ggbond.qiniudn.com/1-uyA28eM-UBnU0SSX8vPSdg.png)

> 注意：创建完input之后并不是立即去创建list组件，而是先把input添加到父div节点之后才继续处理List节点
> 相关代码：
> appendChild：[https://github.com/developit/preact/blob/master/src/vdom/diff.js](https://github.com/developit/preact/blob/master/src/vdom/diff.js)


#### 1.5 处理子组件
控制流又回到1.1开始处理List组件，因为List是一个组件而不是DOM元素，会调用List的render函数生成VNodes

![http://ggbond.qiniudn.com/1-XadPNReNXRQyIl6FJOpPlQ.png](http://ggbond.qiniudn.com/1-XadPNReNXRQyIl6FJOpPlQ.png)

List的虚拟节点看上去像下面这样：
![http://ggbond.qiniudn.com/1-PbgdX1qNPoHh5Ok3yY83gQ.png](http://ggbond.qiniudn.com/1-PbgdX1qNPoHh5Ok3yY83gQ.png)

> 相关代码：
> buildComponentFromVNode：[https://github.com/developit/preact/blob/master/src/vdom/diff.js#L102](https://github.com/developit/preact/blob/master/src/vdom/diff.js#L102)


#### 1.6 重复1.1到1.4处理所有的子节点
![http://ggbond.qiniudn.com/1-Pwkyw7P2bbpEL9zQB1YaMA.png](http://ggbond.qiniudn.com/1-Pwkyw7P2bbpEL9zQB1YaMA.png)

下面这张图展示了每个节点被添加的过程（深度优先）
![http://ggbond.qiniudn.com/1-e6rihM5IE3PRedd311iinA.png](http://ggbond.qiniudn.com/1-e6rihM5IE3PRedd311iinA.png)

#### 1.7 结束处理
这一步就结束了，调用所有组件的“componentDidMount”函数，然后结束

![http://ggbond.qiniudn.com/1-xRspRlx0WM-y2Au0zyegog.png](http://ggbond.qiniudn.com/1-xRspRlx0WM-y2Au0zyegog.png)

> 重要提示：当这些步骤完成以后，每个组件都有一个对真实DOM的引用，用来更新和比较，避免重新创建同样的DOM节点：


### 场景2：删除叶子节点
当我们在搜索框里输入“cal”，然后敲下回车之后，就只剩下了(New York)这个叶子节点，删除了List的第二个节点

![http://ggbond.qiniudn.com/1-AyQmTTZFnfC_VKv23D8FrA.png](http://ggbond.qiniudn.com/1-AyQmTTZFnfC_VKv23D8FrA.png)

让我们看看这个场景的流程是怎么样的：

#### 2.1 像之前一样，创建虚拟节点

在初始化渲染之后，将来的每一次更改都是update。当创建节点的时候，update的生命周期和create的生命周期很像。也会从头创建VNodes

但是因为是更新而不是创建组件，会调用每个组件和字组件的“componentWillReceiveProps”, “shouldComponentUpdate”, and “componentWillUpdate”方法

另外在update的周期里，如果VNodes对应的DOM元素已经存在，则不会重新创建

![http://ggbond.qiniudn.com/1-pCYNGbZL_zT1gbMtXYVt9A.png](http://ggbond.qiniudn.com/1-pCYNGbZL_zT1gbMtXYVt9A.png)

> 相关代码：
> removeNode：[https://github.com/developit/preact/blob/master/src/dom/index.js#L9](https://github.com/developit/preact/blob/master/src/dom/index.js#L9)
> insertBefore：[https://github.com/developit/preact/blob/master/src/vdom/diff.js#L253](https://github.com/developit/preact/blob/master/src/vdom/diff.js#L253)

#### 2.2 使用组件对真实DOM的引用，避免重新创建DOM

像先前提到的，每一个组件都有一个对其在初始化过程中创建的真实DOM的一个引用，下图展示了我们的App的引用

![http://ggbond.qiniudn.com/1-JjV4E5Ag9ogDvH9ILWDO2Q.png](http://ggbond.qiniudn.com/1-JjV4E5Ag9ogDvH9ILWDO2Q.png)
当虚拟节点创建之后，节点的每一个属性都会和真实DOM的节点属性比较，如果真实DOM是存在的，则循环跳到下一个节点

![http://ggbond.qiniudn.com/1-fHYrhlaGOJKnWTzn0YrQ8g.png](http://ggbond.qiniudn.com/1-fHYrhlaGOJKnWTzn0YrQ8g.png)

> 相关代码：
> innerDiffNode：[https://github.com/developit/preact/blob/master/src/vdom/diff.js#L185](https://github.com/developit/preact/blob/master/src/vdom/diff.js#L185)

#### 2.3 如果真实DOM里还有其他节点则删除

![http://ggbond.qiniudn.com/1-ALjitFLcW_lIHvaswCp0GA.png](http://ggbond.qiniudn.com/1-ALjitFLcW_lIHvaswCp0GA.png)

因为有差异，“New York”节点在真实DOM里被下面的流程展示的算法删除了，该算法还会调用“componentDidUpdate”生命周期函数


![http://ggbond.qiniudn.com/1-m74VaQdTuuonA_R-LkT3xw.png](http://ggbond.qiniudn.com/1-m74VaQdTuuonA_R-LkT3xw.png)

### 场景3：卸载整个组件
考虑这样一种用户场景：我们在过滤器了输入blabla，因为它既不匹配“California”也不匹配“New York”,所以不需要渲染List这个子组件。这也就意味着我们需要卸载整个组件

![http://ggbond.qiniudn.com/1-Ki5tKGKiRI4P_ma_fz3chQ.png](http://ggbond.qiniudn.com/1-Ki5tKGKiRI4P_ma_fz3chQ.png)

![http://ggbond.qiniudn.com/1-5GheZk3dZ7st_mvBcWG4nw.png](http://ggbond.qiniudn.com/1-5GheZk3dZ7st_mvBcWG4nw.png)
删除一个组件和删除一个节点类似。另外，当删除一个被组件引用的节点的时候，框架会调用“componentWillUnmount”函数，然后递归删除所有的DOM元素。
下图展示了真是DOM里ul对List组件的引用

![http://ggbond.qiniudn.com/1-yZ3TSnTRf95mGSoR-HiwHw.png](http://ggbond.qiniudn.com/1-yZ3TSnTRf95mGSoR-HiwHw.png)

下图中高亮的部分展示了删除/卸载组件是如何工作的
The below picture highlights the section in the flowchart to show how deleting/unmounting a component works.

![http://ggbond.qiniudn.com/1-QGGL_W7zEr3FtR1VODXt_w.png](http://ggbond.qiniudn.com/1-QGGL_W7zEr3FtR1VODXt_w.png)

> 相关代码：
> unmountComponent：[https://github.com/developit/preact/blob/master/src/vdom/component.js#L250](https://github.com/developit/preact/blob/master/src/vdom/component.js#L250)

《完》

