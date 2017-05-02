---
title: 一个前端工程师的Python历险记
date: 2017-03-18 16:00:35
tags: [python]
---

人生苦短，我用Python
自从入坑Python以来，发现Python的灵活和强大，简直就是瑞士军刀，想要的功能都有对应的实现。在此记录这两个月以来学习的历程，还有踩过的坑。先来首Python之歌：

```
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!

```

<!--more-->

[中文翻译和解释](http://blog.csdn.net/gzlaiyonghao/article/details/2151918)：

```
Python之禅 by Tim Peters
 
优美胜于丑陋（Python 以编写优美的代码为目标）
明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
可读性很重要（优美的代码是可读的）
即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
当存在多种可能，不要尝试去猜测
而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）

```

## 开发环境搭建
> 本文所指的开发环境都是指在mac上的搭建的，其他环境可能略有不同。

### 多版本共存

很多人都知道有Python2和Python3两种主流的版本。而mac seirra os内置了Python2.7版本，并且mac os的很多功能都依赖于Python2.7。所以：

请不要删除系统自带的Python2.7！

请不要删除系统自带的Python2.7！

请不要删除系统自带的Python2.7！

重要的事情说三遍，删除之后系统自带的很多功能将不能正常使用。

但是我们在开发过程中又需要使用Python3的版本来进行开发（新版本对字符编码等功能处理较友好），那应该怎么办呢？

首先，Python可以多版本共存，所以可以直接使用`brew install python` 来安装Python3.

安装完以后如果在终端输入`python`命令则使用的是Python2，如果输入`python3`则使用Python3.是不是很科学！

但是总不能每次运行我们的代码的时候都手动指定Python的版本吧？显然我们能想到的问题别人肯定已经遇到过而且有了解决方案，这里介绍几种Python多版本共存的方案：

- `pyenv`,如果用过node.那应该都知道nvm,pyenv就是Python界的nvm,可以安装多个版本的Python，指定当前shell会话使用哪个版本
- `virtualenv`，可以为简历虚拟目录，类似node的node_modules,在创建目录的时候指定Python的版本，只要进入了当前的env,那么运行Python命令使用的就是简历该环境时指定的Python版本
- `virtualenvwrapper`,顾名思义就是`virtualenv`的wrapper，因为virtualenv的命令比较繁琐。所以有人包装了一个比较方便的版本

> 每个工具具体怎么用还请大家自行百度或者Google.

我自己使用的就是`virtualenvwrapper`，一般创建两个环境，一个`env27`代表Python2.7，一个`env34`,代表Python3.4

有时候也会一个Python项目创建一个env,因为可能多个项目都依赖同一个第三方的包，但是这个包的版本相互不一样，所以用不同的虚拟环境隔离开来，这个时候就能体会到虚拟环境的好处了。

更重要的是我们在项目开发完成之后需要导出依赖包部署到测试环境或者线上，这个时候一般用`pip freeze > requirements.txt`导出依赖包，类似node的package.json，然后在其他的环境执行`pip install -r requirements.txt`即可安装依赖

### 包管理工具

- easy_install
- [pip](http://blog.csdn.net/olanlanxiari/article/details/48086917)


### IDE

- 推荐pycharm
- sublime
- vs code

### interpretor

- python 自带
- [ipython](http://ipython.org/)
- [bpython](https://bpython-interpreter.org/)

### 基本语法

#### 数据类型

- 可变
    + dict
    + object
    + array
    + ==
- 不可变
    + number
    + string
    + truple

#### 数学运算

#### 逻辑运算


#### 内置方法

#### 内置模块



### 其他资源

- [一些技巧](http://blog.jobbole.com/32748/)
- [听说你会Python](http://python.jobbole.com/86869/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)

## Django
### 搭建

### Django app,model,url,admin

### django 自定义命令

## sqlalchemy

### sqlalchemy映射Django model

## pyexcel