---
title: 用ES6生成器解决node回调地狱
date: 2016-12-04 10:45:02
tags: [ES6, 翻译]
---

原文[A Study on Solving Callbacks with JavaScript Generators](http://jlongster.com/A-Study-on-Solving-Callbacks-with-JavaScript-Generators)

当我开始写nodejs的时候，非常讨厌两件事情：1.所有流行的模板引擎，2.回调的扩散(回调地狱)。我愿意忍受回调，因为我理解基于事件的服务端是多么的强大。但是自从我看到JavaScript支持生成器以后，我知道我等了很久的这一天终于到来了。

<!--more-->

今天，他们在[V8](http://wingolog.org/archives/2013/05/08/generators-in-v8)和油猴子里实现了这个规范，新时代到来了(海贼王要诞生了。。)

尽管在V8（node）的命令行需要加harmony参数才能使用生成器，而且在所有的浏览器里实现还需要一段时间（Firefox已经全部支持了）。我们仍然可以学习怎么样用生成器来写异步代码。我们应该尽早掌握这种方法。

你可以下载0.11版本以后的node，然后给node传递--harmony 或者--harmony-generators参数来启用node对生成器的支持。我相信在不久的将来，当es6成为最终的标准的时候，这两个参数就不再需要了。

所以，到底生成器是如何解决回调地狱的呢？生成器函数可以通过yield关键字暂停执行，返回一个值。下次调用的时候从上次暂停的地方继续执行。这就意味着我们可以暂停一个函数的执行，等这个函数需要等待另外一个函数的执行结果的时候，而不是传递一个回调函数给它。

用英语来解释一种编程语言的结构是不是不好玩？那就直接上代码吧。

> 为了确保读者能跟上我的脚步，可以阅读[A Closer Look at Generators Without Promises](http://jlongster.com/A-Closer-Look-at-Generators-Without-Promises)

## 基础知识

在我们深入了解异步之前，我们先看看原生的生成器函数。生成器是通过function* 关键字定义的

```

function* foo(x) {
    yield x + 1;

    var y = yield null;
    return x + y;
}

```

我不想过多的关注这段代码，而想聚焦于怎么使用这个异步结构：

```

var gen = foo(5);
gen.next(); // { value: 6, done: false }
gen.next(); // { value: null, done: false }
gen.send(8); // { value: 13, done: true }

```

如果我想记笔记，我会这样写：

* yield后面可以跟任何的表达式，这对于在任何结构中暂停一个函数的执行非常有用，例如`foo(yield x, yield y)`或者循环
* 可以像调用函数一样调用生成器。但是他只返回一个生成器对象。你需要调用next方法唤起生成器的继续执行。当需要给生成器传值的时候，调用send方法。`gen.next()`和`gen.next()`等价。`gen.throw`从生成器内部抛出一个异常
* 生成器不好返回原生值，而是返回一个有两个属性的对象：value和done。这样就很容易判断一个生成器是否执行结束。而不是像老的api一样抛出停止迭代的异常

## 异步解决方案1：suspend
你也许会问上面的代码到底是怎么解决node的回调地狱的。其实，如果我们可以暂停一个函数的执行。我们就可以通过一些语法糖，让异步的回调看上去像同步的一样。

问题来了：什么是语法糖？
第一个解决方案是[suspend库](https://github.com/jmar777/suspend)。他是我们能想到的最简单的方式了。只有16行[代码](https://github.com/jmar777/suspend/blob/master/lib/suspend.js)

用这个库的代码像这样：

```

var suspend = require('suspend'),
    fs = require('fs');

suspend(function*(resume) {
    var data = yield fs.readFile(__filename, 'utf8', resume);
    if(data[0]) {
        throw data[0];
    }
    console.log(data[1]);
})();

```
suspend函数把生成器转换成了一个一般的函数。它给生成传递一个resume函数，作为异步函数的回调。另外它会通过一个含有error和value两个值的数组唤醒生成器。

生成器和resume之间的关系很有意思，但是它也有弊端。首先，返回一个两个元素的数组很烦人，即使用解构（var [err, res] = yield foo(resume)）。我更倾向于返回一个值，如果有错误就抛出异常。看上去这个库支持这个选项，但是我认为应该把它作为默认值。

另外，需要显示的传递resume参数有点尴尬，而且也不好组合，因为有时候我想等到上面的函数执行完毕，我仍然需要传递一个callback参数进去，就像以前的node一样。这将导致更多的混乱和错误处理。因为error需要继续传递而不是抛出异常，所以你需要在每个异步调用里都手动检查，然后继续传递error对象。

最后，你也没办法做一些像并行处理一些复杂的控制流这种事情。[README](https://github.com/jmar777/suspend#what-about-parallel-execution-mapping-etc)里说其他的控制流库解决了这个问题。你需要结合suspend和这些库中的其中一个。但是我更想看到生成器原生支持这些库。

> ps:[kriskowal](https://twitter.com/kriskowal) 提到了[creationix](https://twitter.com/creationix)写的[gist](https://gist.github.com/creationix/5544019).更好的实现了单独的生成器来处理基于回调的代码。它很酷，默认抛出error。而且更简洁

## 异步解决方案2：Promises
更好的处理异步的方案是[promises](http://promises-aplus.github.io/promises-spec/).promise是一个代表了将来值的一个对象。你可以组合promise来代表异步调用的控制流行为。

我不打算在这里过多的介绍异步控制流，这需要费好多时间，另外已经有了很好的[解释](http://domenic.me/2012/10/14/youre-missing-the-point-of-promises/),最近有很多人强调要定义promise的API和行为，允许在不同的库之间交互，其实想法很简单。

我用的是[Q](https://github.com/kriskowal/q),因为他已经支持生成器，而且比较成熟了。它的早起实现叫[taskjs](http://taskjs.org/)，但是不是标准的promise实现。

让我们看一个真实世界的例子。我们经常用一些简单愚蠢的例子。下面这段代码创建一个博客对象，然后返回它：

```

client.hmset('blog::post', {
    date: '20130605',
    title: 'g3n3rat0rs r0ck',
    tags: 'js,node'
}, function(err, res) {
    if(err) throw err;

    client.hgetall('blog::post', function(err, post) {
        if(err) throw err;

        var tags = post.tags.split(',');
        var posts = [];

        tags.forEach(function(tag) {
            client.hgetall('post::tag::' + tag, function(err, taggedPost) {
                if(err) throw err;
                posts.push(taggedPost);

                if(posts.length == tags.length) {
                    // do something with post and taggedPosts

                    client.quit();
                }
            });
        });

    });
});


```

这甚至不是一个复杂的例子，你看看它有多丑。回调的很快把代码挤到了屏幕的右边。而且，为了查询所有的标签。我们需要手动管理每一个查询条件，确保他们已经准备好。

让我们用Q来重新实现：

```

var db = {
    get: Q.nbind(client.get, client),
    set: Q.nbind(client.set, client),
    hmset: Q.nbind(client.hmset, client),
    hgetall: Q.nbind(client.hgetall, client)
};

db.hmset('blog::post', {
    date: '20130605',
    title: 'g3n3rat0rs r0ck',
    tags: 'js,node'
}).then(function() {
    return db.hgetall('blog::post');
}).then(function(post) {
    var tags = post.tags.split(',');

    return Q.all(tags.map(function(tag) {
        return db.hgetall('blog::tag::' + tag);
    })).then(function(taggedPosts) {
        // do something with post and taggedPosts

        client.quit();
    });
}).done();

```
我们把这些基于回调的代码改成了基于promise的。但是这个只是个简单的例子，只要我们有了promise，就可以调用then方法来等等异步操作的执行结果。更多的细节请参考promises/A+[规范](http://promises-aplus.github.io/promises-spec/)。

Q还实现了其他的一些方法，比如all：一个promise的数组作为参数，等待他们都执行完毕。当执行完的时候，也就意味着所有的异步流程都执行结束了，而且所有未被处理的错误都被抛出。根据promises/A+规范。所有的异常都会被转换为error，然后传递给错误处理函数。所以需要却被没被处理的错误被重新抛出（如果你不太理解，请阅读这篇[文章](https://blog.domenic.me/youre-missing-the-point-of-promises/)）。

注意到我们需要嵌套最终的promise handler。因为我们需要访问post和taggedPosts，不幸的是，这看起来有点像callback。

是时候见识一下生成器的力量了：

```

Q.async(function*() {
    yield db.hmset('blog::post', {
        date: '20130605',
        title: 'g3n3rat0rs r0ck',
        tags: 'js,node'
    });

    var post = yield db.hgetall('blog::post');
    var tags = post.tags.split(',');

    var taggedPosts = yield Q.all(tags.map(function(tag) {
        return db.hgetall('blog::tag::' + tag);
    }));

    // do something with post and taggedPosts

    client.quit();
})().done();


```

这是不是很神奇？让我们看看到底发生了什么
Q.async传入一个生成器，返回一个调用生成器的函数，有点像suspend库。然而，有一个关键的区别：生成器yields promise。Q把promise和每一个生成器绑定到一起，当promisefullfill的时候唤醒生成器，返回结果。

我们不需要处理笨拙的resume函数，promise隐式处理了。我们从promise中获益良多。

其中一个好处是我们可以在需要的时候使用Q，比如Q.all，Q.all可以并行的运行一系列异步操作，用这种方法，可以很容易的把显式的Q promise和隐式的promise结合在一起来创建看上去很简介的复杂的控制流。
另外，还没有嵌套的问题。因为post和taggedPosts在相同的作用域里，所以我们不需要关心then链会打破作用域。这太帅了。

错误处理有点棘手。在生成器里使用promise之前，你需要确实理解promise是怎么样工作的。在promise里错误和异常会被传递给错误处理函数，而不是被抛出。你可以通过错误回调来处理抛出的错误。`someGenerator().then(null, function(err) { ... }).`


另外，需要注意的是可以使用gen.throw方法抛出生成器内部的promise错误。这也就意味着可以用try/catch来处理生成器内部的错误

```

Q.async(function*() {
    try {
        var post = yield db.hgetall('blog::post');
        var tags = post.tags.split(',');

        var taggedPosts = yield Q.all(tags.map(function(tag) {
            return db.hgetall('blog::tag::' + tag);
        }));

        // do something with post and taggedPosts
    }
    catch(e) {
        console.log(e);
    }

    client.quit();
})();



```

它会按照你期望的方式工作。db.hgetall产生的错误都会被catch捕获，即使是嵌套的Q.all产生的错误。当然了，如果没有try/catch的话，异常会被转换为错误，然后传递给调用promise的错误处理函数。

思考一下：我们可以用try/catch来给异步代码添加错误处理函数。错误处理的动态作用域也能正常工作。所有在try里发生的未被处理的异常都会扔给catch。还可以用finally。

另外，当你调用promise的done方法的时候，默认会返回all方法的异步代码发生的错误。

```

var getTaggedPosts = Q.async(function*() {
    var post = yield db.hgetall('blog::post');
    var tags = post.tags.split(',');

    return Q.all(tags.map(function(tag) {
        return db.hget('blog::tag::' + tag);
    }));
});

```
上面的代码是标准库代码，创建promise但是不关心错误处理，你可以这样调用：

```

Q.async(function*() {
    var tagged = yield getTaggedPosts();
    // do something with the tagged array
})().done();

```
这是最上层的代码。就像前面说的，done方法确保抛出未被处理的异常。我相信上面这种模式很普遍，但是还有一种新的方法。getTaggedPosts是库方法，也就是一种promise-generating方法。

我提了一个Q.spawn的[PR](https://github.com/kriskowal/q/pull/306),现在已经merge到Q里面了，现在可以更简洁的写上面的代码

```

Q.spawn(function*() {
    var tagged = yield getTaggedPosts();
    // do something with the tagged array
});

```
spawn需要一个生成器对象作为参数，然后立即调用它，自动抛出未处理的错误。它其实等价于`Q.done(Q.async(function*() { ... })())`

## 其他方法
我们的基于promise的生成器代码越来越屌了，用一些语法糖，我们可以甩掉很多异步流带来的包袱

研究了这么久，这里有一些模式需要注意：

## 什么时候用
如果只有一个简单的函数等待一个promise，那没必要用生成器，比较下面的代码


```

var getKey = Q.async(function*(key) {
    var x = yield r.get(dbkey(key));
    return x && parseInt(x, 10);
});

```

和这个代码:

```

function getKey(key) {
    return r.get(dbkey(key)).then(function(x) {
        return x && parseInt(x, 10);
    });
}

```
我觉得上一个更简洁


## spawnMap

我发现我自己做了很多事情:

```
yield Q.all(keys.map(Q.async(function*(dateKey) {
    var date  = yield lookupDate(dateKey);
    obj[date] = yield getPosts(date);
})));

```
用spawnMap还是很有帮助的，它帮你实现了`Q.all(arr.map(Q.async(...)))`.


```

yield spawnMap(keys, function*(dateKey) {
    var date  = yield lookupDate(dateKey);
    obj[date] = yield getPosts(date);
})));

```


## asyncCallback

我发现的最后一件事是有好几次我想创建一个Q.async函数，我需要确保所有的错误都会被重新抛出，但是我不能把上面的callback转成 Q.async，因为所有的错误多被静默处理了。而且我也不能用Q.spawn，因为它不是立即执行的。这种情况发生在来自于不同库的常规回调上，比如express的`app.get('/url', function() { ... })`

也许有时候确确实实需要异步回调：

```

function asyncCallback(gen) {
    return function() {
        return Q.async(gen).apply(null, arguments).done();
    };
}

app.get('/project/:name', asyncCallback(function*(req, res) {
    var counts = yield db.getCounts(req.params.name);
    var post = yield db.recentPost();

    res.render('project.html', { counts: counts,
                                 post: post });
}));


```

## 最后的想法

当我发现生成器的时候，我希望它确实能够改善异步代码。事实证明确实可以，尽管你需要确实理解promise结合生成器是怎么工作的。把promise隐式话会把他们弄的有点神秘，所以在你理解promise之前，我不会用async和spawn。

尽管我希望js的魔法能过延续，所有的这些都会被js隐式处理，这种解决方案还是很赞。现在我们有了一种简洁的处理异步行为的方式，这种方式很有用，不仅仅用来处理文件操作。甚至可以写一些分布式的代码，比如跨CPU的或者跨机器的。

《完》
另附上两个链接：
1.[es6katas](http://es6katas.org/),一个通过tdd练习es6的网站
2.[深入浅出ES6](http://www.infoq.com/cn/es6-in-depth/)
