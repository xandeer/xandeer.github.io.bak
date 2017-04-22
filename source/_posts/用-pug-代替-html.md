---
title: 用 pug 代替 html
date: 2017-04-20 19:14:03
tags: front-end
categories:
---

## pug 是什么？

![sad pug](http://oag78t4c2.bkt.clouddn.com/xandeer/sad-pug.jpg)

来自东北的忧郁小狗。

认识了真实世界的 pug，下面介绍前端领域的 `pug`：

一个普通的模板引擎，只用来渲染 `HTML`，不像 `mustache` 等用途那么广。如果你为成天写 `<>` 而烦，那 `pug` 绝对是很好的解决方案。如果你学过 `Python`，或者平常写代码有良好的缩进风格，那么写 `pug` 不会让你感到烦恼；平常不注意代码缩进，那么从 `pug` 开始吧。

## pug 语法简介

`pug` 提供了编程语言的多数基本语法，变量、条件分支、循环等。因此用 `pug` 生成 `HTML` 也就实现了 `HTML` 结构的逻辑复用，如有时好几个页面都有类似甚至一样的结构时，可以用 `block` 与 `extends` 或者 `mixin` 复用代码，代替之前的在不同 `*.html` 文件间复制粘贴。当然，这些不是要介绍的重点，我现在的项目基本都会使用 `vue`，因此逻辑相关的部分基本都是用 `vue` 的指令实现。

重点是**使用 `pug` 之后，不用再写 `HTML`，代码既好看又变少了**。

```pug
#pug.hello(data-msg="hello")
  p.hello-msg Hello World
```

上面的代码转换成 `HTML`，是下面这个样子：

```html
<div id="pug" class="hello msg" data-msg="hello">
  <p class="hello-msg">Hello Wrold</p>
</div>
```

是不是写起来比 `HTML` 更舒服？如果答案是肯定的话，欢迎加入 `pug`。学习成本估计也就看文档一两个小时，之后需要用什么再查看几次文档就足够了。

更多的语法就不说了，我学的时候还没有中文文档，现在已经有完整的[中文文档](https://pugjs.org/zh-cn/api/getting-started.html)了，欢迎去看。Dash 中也可以找到文档，不过好像只有英文版，后期查看时会挺方便。

## 怎么将 pug 转换为 HTML？

如果使用 `vscode` 或者 `atom`，都有插件可以将 `pug` 文件转换成 `HTML` 的插件，其它编辑器不太清楚。

如果已经在使用 `webpack`，那么有个 `loader` 叫 `pug-loader`。没有使用 `webpack` 的前端同学，赶紧去用吧。

## 在 vue 中使用 pug

这个实在是 so easy，`vue` 的 `template` 支持 `lang` 属性，所以直接像下面这样写就搞定了：

```html
<template lang="pug">
#pug.hello(data-msg="hello")
  p.hello-msg Hello World
</template>
```

不要忘了添加 `pug-loader`。

## 最后

做一个有追求的开发，能自动化的要想办法自动化，可以简化的要简化。将节省的时间与精力用来学习更多，或者与喜欢的人一起去做喜欢的事。

> 将时间浪费在美好的事物上。

与诸君共勉。