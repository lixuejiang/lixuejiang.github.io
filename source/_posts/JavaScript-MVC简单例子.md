---
title: JavaScript MVC简单例子
date: 2016-10-03 20:18:38
tags: [JavaScript,MVC,翻译]
---
[原文](https://alexatnet.com/articles/model-view-controller-mvc-javascript)
我很喜欢JavaScript（我也是），因为它是世界上最具有扩展性的语言之一。它支持很多编程方式是编码技术，但是这种灵活性是很危险的--如果实践或者设计模式错误的或者不一致的，那很容易就把JavaScript的项目搞的一团糟。
这篇文章的目的是向大家展示如何在一个简单的JavaScript开发组件过程中应用MVC模式。这个组件类似HTML里的listbox（“select”标签），有一些可编辑的列表：用户可以选中或者删除、新增一个列表项。该组件包含三个类，对于MVC设计模式的三个部分。

<!--more-->

我希望这篇文章是一篇很好的读物，如果你考虑把这个例子应用到你的项目里那就再好不过了。我相信你已经准备好了开发JavaScript程序需要的一切：脑子，手，文本编辑器，还有一个浏览器（例如chrome）（开玩笑。。）
在这里要先介绍一下MVC模式。你也许知道，这个模式是以它的主要部分命名的：模型，存储应用程序的数据模型；视图：把模型渲染到适当的表现层；控制器：更新模型，维基百科里对MVC的组件定义如下：

* Model - 应用程序操作的信息在特定领域的表现（有点别扭）。领域逻辑也就是意味着原始数据（举个栗子：计算今天是不是用户的生日，或者是用户购物车里的商品总数，税款，应付多少钱等等）
* View - 将model渲染成一种可交互的模式，典型的就是用户接口。MVC经常用在web应用程序里，web程序里的view就是HTML页面还有为页面收集动态数据的代码。
* Controller - 处理和响应事件，特别是用户行为，触发模型或者视图的变化，也就是更新模型或者视图，是连接模型和视图的纽带

这个组件的数据就是一些列表项，其中的每一项都可以被选中和删除。所以，组件的model很简单-它包含一个数组和一个被选中的item的索引：

```
/**
 * The Model. Model stores items and notifies
 * observers about changes.
 */
function ListModel(items) {
    this._items = items;
    this._selectedIndex = -1;

    this.itemAdded = new Event(this);
    this.itemRemoved = new Event(this);
    this.selectedIndexChanged = new Event(this);
}

ListModel.prototype = {
    getItems : function () {
        return [].concat(this._items);
    },

    addItem : function (item) {
        this._items.push(item);
        this.itemAdded.notify({ item : item });
    },

    removeItemAt : function (index) {
        var item;

        item = this._items[index];
        this._items.splice(index, 1);
        this.itemRemoved.notify({ item : item });
        if (index === this._selectedIndex) {
            this.setSelectedIndex(-1);
        }
    },

    getSelectedIndex : function () {
        return this._selectedIndex;
    },

    setSelectedIndex : function (index) {
        var previousIndex;

        previousIndex = this._selectedIndex;
        this._selectedIndex = index;
        this.selectedIndexChanged.notify({ previous : previousIndex });
    }
};

```

事件是一个实现观察者模式的简单类：

```
function Event(sender) {
    this._sender = sender;
    this._listeners = [];
}

Event.prototype = {
    attach : function (listener) {
        this._listeners.push(listener);
    },
    notify : function (args) {
        var index;

        for (index = 0; index < this._listeners.length; index += 1) {
            this._listeners[index](this._sender, args);
        }
    }
};

```

视图需要定义交互的空间，这个任务有很多可以替代的接口，但是我推荐最简单的一个。我希望我的列表项在一个ListBox控件里，下面有两个按钮，”+“用来添加列表项，”-“用来删除选中的项目。Listbox的原生方法支持选中一个项目。一个视图类和一个通过注册handler或者回调来处理用户接口的输入时间的控制器紧紧的绑定在一起（来自[维基百科](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)）。

下面是视图和控制器的代码：

```
/**
 * The View. View presents the model and provides
 * the UI events. The controller is attached to these
 * events to handle the user interraction.
 */
function ListView(model, elements) {
    this._model = model;
    this._elements = elements;

    this.listModified = new Event(this);
    this.addButtonClicked = new Event(this);
    this.delButtonClicked = new Event(this);

    var _this = this;

    // attach model listeners
    this._model.itemAdded.attach(function () {
        _this.rebuildList();
    });
    this._model.itemRemoved.attach(function () {
        _this.rebuildList();
    });

    // attach listeners to HTML controls
    this._elements.list.change(function (e) {
        _this.listModified.notify({ index : e.target.selectedIndex });
    });
    this._elements.addButton.click(function () {
        _this.addButtonClicked.notify();
    });
    this._elements.delButton.click(function () {
        _this.delButtonClicked.notify();
    });
}

ListView.prototype = {
    show : function () {
        this.rebuildList();
    },

    rebuildList : function () {
        var list, items, key;

        list = this._elements.list;
        list.html('');

        items = this._model.getItems();
        for (key in items) {
            if (items.hasOwnProperty(key)) {
                list.append($('<option>' + items[key] + '</option>'));
            }
        }
        this._model.setSelectedIndex(-1);
    }
};

/**
 * The Controller. Controller responds to user actions and
 * invokes changes on the model.
 */
function ListController(model, view) {
    this._model = model;
    this._view = view;

    var _this = this;

    this._view.listModified.attach(function (sender, args) {
        _this.updateSelected(args.index);
    });

    this._view.addButtonClicked.attach(function () {
        _this.addItem();
    });

    this._view.delButtonClicked.attach(function () {
        _this.delItem();
    });
}

ListController.prototype = {
    addItem : function () {
        var item = window.prompt('Add item:', '');
        if (item) {
            this._model.addItem(item);
        }
    },

    delItem : function () {
        var index;

        index = this._model.getSelectedIndex();
        if (index !== -1) {
            this._model.removeItemAt(this._model.getSelectedIndex());
        }
    },

    updateSelected : function (index) {
        this._model.setSelectedIndex(index);
    }
};

```

当然了，模型视图和控制器都需要被实例化：

```
$(function () {
    var model = new ListModel(['PHP', 'JavaScript']),
        view = new ListView(model, {
            'list' : $('#list'), 
            'addButton' : $('#plusBtn'), 
            'delButton' : $('#minusBtn')
        }),
        controller = new ListController(model, view);
    
    view.show();
});​

```

HTML如下：

```
<select id="list" size="10"></select>
<button id="plusBtn">  +  </button>
<button id="minusBtn">  -  </button>

```

UML类图如下：
![http://ggbond.qiniudn.com/mvc.png](http://ggbond.qiniudn.com/mvc.png)

{% codepen lixuejiang JROjbV 0 %}

几点总结：

* 1.controller把模型和视图关联起来
* 2.controller响应视图的用户事件，更新模型
* 3.模型在变化的时候需要更新视图，例子里用了观察者模式，视图往模型的更新事件里注册了回调函数
* 4.模型的更新事件里只是更新了视图，在实际生产中需要设计到持久化或者和后端的交互
* 5.定义了Event对象。

且看backbone是如何处理。
