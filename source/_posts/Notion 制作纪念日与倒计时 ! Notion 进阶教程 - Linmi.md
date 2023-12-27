---
cover: /images/fff039bd41cf0e40dc31c6934af504b3.png
categories: notion
tags:
  - notion
title: Notion 制作纪念日与倒计时 | Notion 进阶教程 - Linmi
date: '2023-11-17 10:00:00'
updated: '2023-12-26 18:46:00'
---

Notion 制作纪念日，倒数日的好处是不仅仅记录时间，还记录图片，视频，文档……


生活中我们或多或少有些值得纪念的日子，升学升迁，婚庆嫁娶，除了看时间，还可以存档我们个人的一些照片，便于我们回忆观看。


## 先看看效果？


![7789294fcbe44a08f3cdafb0660904ff8a7354dda9356eb35b75dcc34b1177c6.gif](/images/fb850cf07c24fcde81d60370d2990461.gif)


## 实现简单的倒计时


### 第一步：新建 Database


新建一个 Notion Database，接下来对属性进行一个简单修改。


![999fd1fe7277a3a3821e31f10fd9bd13919ac9f4b20c8cac15f63c969c14e60b.gif](/images/20a7adf80b250d2535947fbf4ec65b57.gif)


### 第二步：创建截止日期


开始创建我们的倒数日，比如我想知道 2019 年结束还剩多少天。


这里我将数值调到 2019.12.31


![b24e20a8a152ae6599cc8f98ef3b59d679740e3a41264a9a53a5c0a5b715457f.gif](/images/d36e0c42417db497c1a516278c7b5cbb.gif)


### 第三步：计算倒计时


计算倒计时，其实就是一个简单的减法，用截止时间减去现在时间，就能得出还剩余多少天。


简单归简单，在 Notion 中我们应该怎么实现呢？细心的小伙伴如果注意到第一步的时候其实我改了两个属性，一个是时间，另外一个就是辅助我们计算倒计时的 Notion formula。（这里就不对 Notion formula 做过多介绍，未来会单独出一篇介绍。）


Notion formula 中提供一个计算两个时间差的函数 `dateBetween()`，在 Notion formula 中我们找到这个函数，看看介绍。


![6370a1ed539a74be8dd017a18dcfa4126a8ed301aff00a05c8f1219a1be849f9.png](/images/758bfacf0c3303d59fd8fdcb81c3ddf0.png)


我们拎出使用方法


```text
dateBetween(date, date, text)
```


接下来我们对方法做一下简单的改造与介绍


```text
----
```


在 `dateBetween()` 函数中，date - date2 就是分别对应刚刚新建的时间减去现在的时间，text 就是展示这个时间的形式，可以选择年、季度、月、日、小时、分钟、秒、毫秒进行展示。


对了，Notion 也提供了当天日期的函数 `now()`，通过 `now()` 函数，我们就能很快得到当前日期。


![2c13ab01a1547771092d2abf378a177a2ac55c2f1191ae9acde2594adbd3a58a.gif](/images/b41e4e3068b74dca58ec3b8f387a2da4.gif)


有了截止时间与现在的时间，万事俱备，让我们来尝试一下使用最终时间减去现在的时间。代码如下：


```text
dateBetween(date, date2, "years")
```


![66f709d787a1d87180bc681cbb959c415f263598f6cb6dd04a01632e7b146b3b.gif](/images/12387df964f289a68d569b0b22280cc6.gif)


### 第四步：美化展示


这样我们就得到一个简单的倒数日，似乎并不美观，那我们尝试加一些 emoji 显的更有人情味些。代码如下：


```text
dateBetween(date, date2, "quarters")
```


![d58211156c9127d04c103947d5bcf2cbe1460cbee0b0d45d566a0a2d267db6eb.gif](/images/310ca5180b59181fc7ee350e75cf0cfb.gif)


诶，竟然不对？Notion formula 报错了。


哈哈，不逗大家了，这里需要对刚刚数值进行一个转格式，`format()`函数就是帮助我们转成与 “? 还剩” 相同的格式。为什么要转？


不转化前，`dateBetween(prop("Time"), now(), "days")` 就是实打实的钢铁，`"? 还剩 "`是融化的铁，我们用 `format()`函数加热实打实的铁，这样才能融合更彻底些。（这是我编的，事实上是程序规定。）


更改后的代码如下：


```text
dateBetween(date, date2, "months")
```


![71d6e43fe9e9700d3540f3271cd0cc5df2b3a02b3f8ac053f11fa9d6fdb62981.gif](/images/0d99fcc3064af2d1cac57fdb2bee149f.gif)


那么，我们的倒数日就算制作完成了！


## 如何计算并展示纪念日


其实计算纪念日与倒数日的公式形式一样，只是对 `prop("Time")` 与 `now()`做了一下调换。代码如下：


```text
dateBetween(date, date2, "weeks")
```


![ac2fe8b829f780e98daa4e91de9b994a4ea55a54e90f0d286b370f2a58acb444.gif](/images/3559f42dd947617a14b360f316863bba.gif)


我们把现有代码粘贴到之前写好的公式，发现之前的倒数日变成负数了，有点头疼。


其实 Notion formula 并不支持每一行一个公式。那么为了计算日子，岂不是要建两个 Database？


那么，有什么办法可以支持两个一起用呢？


### Notion formula if()


还好 Notion formula 是支持 if() 判断的，具体使用方法如图：


![e0f64494f329a3787c600fa81bdcfe3254bfa18e8f149e1290d7ac55b2ef6f8a.png](/images/2929c16d2ec730654fd4fc0d006f9801.png)


把实例拿出来遛一遛：


```text
dateBetween(date, date2, "days")
```


上述代码写的还是有歧义，我再转换一下


```text
dateBetween(date, date2, "hours")
```


1 从基础数学来讲，不会大于 2 ，所以得出判断就是 `"no"`，如果是小于 2 ，那么就是`"yes"`


### 使用 Notion if() 来判断展示纪念日与倒数日


其实时间可以拿来进行判断，我做一下解释

- 我们设置的时间 < 现在的时间，那么就是纪念日；
- 我们设置的时间 > 现在的时间，那么就是倒数日；
- 我们设置的时间 = 现在的时间，那么就是现在时间。

那么代码可以这么写：


```text
dateBetween(date, date2, "minutes")
```


![cb5600d6a49dce7a723e9a3c9fcfc3e0758f1827d96c9a08aaa111d0af2f7a71.gif](/images/303b8f40fe8314ec8111abc9b5e819da.gif)


再将之前的写好的代码分别粘贴到对应的倒数日与纪念日，这里我对纪念日进行了美化，具体与倒数日执行步骤一样。代码如下：


```text
dateBetween(date, date2, "seconds")
```


![42b9d43285d88cf342768f8eb9c468ad32ee5c5c6f6bdbaa958cbed8fea22687.gif](/images/5b0e88b03d83db5befd9f034dc1b6bb3.gif)


整体代码如下：


```text
dateBetween(date, date2, "milliseconds")
```


如果你是时间命名不是 Time，请将代码中所有 Time 进行一次替换，替换成你命名的名字。


我们没有制作`我们设置的时间 = 现在的时间，那么就是现在时间`这段，这个我有时间再写。（已经很晚了。


再补充一下，Notion 制作纪念日，倒数日的好处其实在这里，不仅仅记录时间，还记录图片，视频，文档等等等…


![e91b3b88706c6e216e0cf132c94c49db1449c853b6cab731016587d06077896c.gif](/images/9621a9ea1ea017811dcf9f94c8558862.gif)


## 结语


Notion 的能力远不止如此，还有更丰富的场景等着我们挖掘，下一篇我会回归到基础，教大家如何更快上手 Notion。


如果你想本篇内容能够出视频讲解，那么请点赞，到达 30 个赞我就制作视频讲解。


如果你有 Notion 相关问题，欢迎添加我的微信 `Linmiv` 交流。


---


▎本文由 [简悦 SimpRead](https://simpread.pro/) 转码。

