---
title: JS 爬坑之路
date: 2016-12-26 19:46:58
tags:
categories: front-end
---

## 共享原型数据

思考下面代码的输出：

```JavaScript
var foo = {count: 1};
var bar = Object.create(foo);
console.log(bar.count++); // 1
console.log(foo.count); // ?
```

最后一行的输出是 **1**，仔细思考一下其实就明白了。定义 `bar` 的时候将 `foo` 对象添加到了 `bar` 原型链中，因此在 `bar` 的原型链中可以找到 `count` 属性，而 `bar.count++` 等价于 `bar.count = bar.count + 1`。在这个赋值语句的左边，进行 **LHS** 查询，`bar` 自身没有 `count` 属性，所以这个时候，`bar` 会添加一个新属性 `count`；在赋值语句的右边，进行 **RHS** 查询，`bar` 自身也没有 `count` 属性，但是在其原型链中可以找到 `count` 属性，并获取其值。所以这行代码最终给 `bar` 对象添加了一个 `count`  属性，并将其赋值为 '2'，而 `foo` 的 `count` 值依然为 '1'。


## Event loop

```JavaScript
console.log('A');
setTimeout(() => {console.log('B');}, 0);
console.log('C');
```

你是不是认为上面的代码输出是 `A B C`？而实际输出是 `A C B`，这是为什么呢？

JavaScript 是单线程的，因此任务需要排队执行。JS 引擎永远都只能执行一个任务，但是 JS 引擎通常都是在一个宿主环境中运行的，最常见的事浏览器和 Node.js。像 Ajax 和 Timer 函数都是由宿主提供的，当你发起了一个 Ajax 请求时，你会给 Ajax 一个回调函数，这时候 JS 引擎会告诉浏览器，“这个函数我先不执行，你帮我保存一下，等你接收完数据之后，告诉我我该执行这个函数了。”然后浏览器就会监听网络请求，等到接收完数据后，浏览器通知 JS 引擎，“数据接收完了，你该执行这段代码了”。等到 JS 引擎执行完当前任务后，就会执行 Ajax 请求的回调函数了。浏览器和 JS 引擎之间交互的中介叫做 `Event loop`。

`setTimeout` 和 `setInterval` 也是一样的，需要注意的一点是，JS 引擎会保证回调函数不会在你设置的时间前被调用，也就是说你设置了 1000ms，实际有可能在 1200ms 后才执行回调函数。 `setTimeout(..0)` 可以 hack 异步调用，就像上面那样，只有在第三行执行完之后，才会调用 `setTimeout` 的回调函数。
