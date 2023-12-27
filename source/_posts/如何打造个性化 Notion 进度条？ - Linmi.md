---
cover: /images/539f13d4b0da857eb0b4cd6c8ac4f195.png
categories: notion
tags:
  - notion
title: 如何打造个性化 Notion 进度条？ - Linmi
date: '2023-11-17 10:10:00'
updated: '2023-12-26 18:46:00'
---

我在今年七月份发了一个关于 Notion 进度条的演示 Demo，说好了要写一个教程，但是一直拖，就拖到了现在，索性就动手写一波吧。


我在今年七月份发了一个关于 Notion 进度条的演示 Demo，说好了要写一个教程，但是一直拖，就拖到了现在，索性就动手写一波吧。


Hi~ 我是 Linmi，这一次我们来聊聊如何利用 Notion formula 来制作一个个性化进度条，本文算是 Notion 的进阶的使用教程，有一定的理解难度，但是做出来会很有成就感喔。


## 实现效果


![755596ecdb301b0c9aa826ae99ef937578f3a8abf2e326a1aa4b24cf6b620c9c.gif](/images/e4a547a6e7b4a56d1f9813688c83cf62.gif)


## 起源


当时在 Notion 群里有一位小伙伴想为他的健身计划做一个进度条，刚好我之前也实现过，索性就贴出了代码，然后就有意思了，**我飘了**，到处群发截图，就是不发教程，是不是这人很可恶。


回过头来说，第一版我用了大量的 IF，考虑到我的编程能力不及我的好友 [longyi](https://github.com/hilongjw)（一位知名开源开发者，具体可以点看他的 Github） 的 0.0000001，能实现真的是万幸。


? 虽说代码垃圾，但是实现效果还是可圈可点的。


第二版在我了解 Notion formula 的 `Slice()` 函数之后，做了相应的修改，意味着代码减少了很多，修改的属性也就少了很多。


## 思路


这一次我们来试试实现 2019 已经过去了多少的进度条演示，这里要用到计算日期已经过去多久。


从开头的 GIF 演示中，整个进度条是由十个方块■组成，一个方块■代表 10%，0 - 10% 就占一个方块■，11% - 20% 就占两个方块■■，以此类推。


那么就很简单了，比如今年已经过去 180 天，经过计算，得出来今年已经走完了 180/365 = 49%，49% 在 41% - 50% 之间，占用五个方块，显示效果为 「■■■■■ 49%」。


## 实现


这里的我选择一步步来讲解，如果已经熟悉 Database 新建使用，直接跳过看下面代码解析即可。


### 新建 Database，创建属性为 Formula


这里我们新建的 Database 选择 Table 浏览模式。这里我新建两个 Table Database，以方便分享模板供对比参考。


先建一个 Page，再创建一个 Table-inline ，选择右上角「···」将「Full Width」打开。


![23276eb18a00367c2af8dd49d0cbc527219a7361dc6c14f1224bf91773a82dd4.gif](/images/3f5f33580ef1bd2aed1863e2b0bd3e27.gif)


接着我们修改属性，这里需要设置先 2019.01.01，用现在日期减去 2019.01.01，就能得到今年过了多少天。接着设置 Formula，计算出今年已经过去多少天。


这里我直接对两个现有的属性就行修改，「Tag」 修改为「Time」，「File」修改为「formula」，适当拉长输入框。


![2c4bea1b381fdd7828707f930fff4e109a43f54a7755d68e863532b2ff29c279.gif](/images/e9e98f2dff656e8fefe178533fa99af1.gif)


然后 Time 里选择好 2019.01.01，也顺便带个技巧，Notion 默认的 Time 是不符合国人的视觉习惯，需要修改一下，操作如下图：


![37be42ca2ee9edd3e2b0ad5fa33772936efe6fc1cae0c94c894d19c9596e0195.gif](/images/f56c3ad92827424df67177f03c705ed6.gif)


### 计算 2019 已经过去多少天


具备初始时间，咱就开始吧，这里我们要获取现在的时间，手动输入？当然不是，Notion 提供了获取现在时间的函数 `now()`，接着我们来计算一下今年已经过去多久 = now() - 2019.01.01，行不行，不行。


在之前的文章中我也提到，Notion formula 中的时间是不支持直接加减，需要用到函数 `dateBetween()`，具体解析请看：[Notion 制作纪念日与倒计时 | Notion 进阶教程](https://linmi.cc/1621.html)


![e01b7e4e23f722500a78957af274f6cc640d4c93fe79cc8f12e020d570f1ebd9.gif](/images/a1f41eb6575474fdd687782fa67fa533.gif)


代码如下：


```text
dateBetween(now(), prop("Time"), "days")
```


如果不是计算日期，单纯加减即可，无需函数。


### 计算 2019 已经过去的百分比


接下来我们计算百分比，套用之前的代码，这里我们将已经过去的时间除以 365，这里为了做演示不考虑闰年之类，接着得到一个小数，但是这里我们需要展示百分比，那么就需要对这个小数乘以 100，然后在后面加上 % 得到最后 10% 的显示效果。


![7154c0de019a0eeeea1b810a60d9f1cb13ad439b9ce4199d79841c8bedf89e22.gif](/images/d47d02af85f78cf75ff0c0e9f7e731fc.gif)


代码如下：


```text
format(toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 100)) + "%"
```


这里简单对代码做一下解析，由于 `dateBetween()` 计算出来的是时间，和数字属性不一样，所以我们需要对这个时间进行转化，使用 `toNumber()` 函数来将刚刚的时间转换成数字，再除以 365。


接着就是需要在后面添加上 「%」，由于「%」是文本，与数字属性又不一样，所以这里得将数字转化成文本，这样两者才能拼合。


其实演示图中又遇到一个问题，那就是百分比后面小数太多了，有什么办法能换成整数么？


Notion formula 提供了 `Round()` 函数，你可以将它理解为四舍五入。


![43ef547f9ed69c81a2c63040ae13a3b07a5cf867d2590f773d325c0552ab490a.png](/images/0218176048f598c32045b5903e4ca9c9.png)


针对这段代码，我们进行一下四舍五入。


```text
toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 100)
```


转换后：


```text
round(toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 100))
```


最后总体代码：


```text
format(round(toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 100))) + "%"
```


![389dfba0272770e7dff4a655321bb92cf4cc650d9863d13bc03af81fef9a2ea4.png](/images/99f606717131f62eb756cb486c509119.png)


基础工作准备完成后，接下来可以开始我们的进度条制作。


### 制作进度条


现在我正在考虑要不要说明一下 IF 判断来做这件事，代码讲起来也异常消耗人的注意力。嗯，还是直接贴代码，重点讲 Slice() 函数。


IF 判断代码（开头 GIF 演示代码）


```text
if(round(prop("已完成") / prop("总量") * 100) == 100, "■■■■■■■■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 100 and round(prop("已完成") / prop("总量") * 100) >= 90, "■■■■■■■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 90 and round(prop("已完成") / prop("总量") * 100) >= 80, "■■■■■■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 80 and round(prop("已完成") / prop("总量") * 100) >= 70, "■■■■■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 70 and round(prop("已完成") / prop("总量") * 100) >= 60, "■■■■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 60 and round(prop("已完成") / prop("总量") * 100) >= 50, "■■■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 50 and round(prop("已完成") / prop("总量") * 100) >= 40, "■■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 40 and round(prop("已完成") / prop("总量") * 100) >= 30, "■■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 30 and round(prop("已完成") / prop("总量") * 100) >= 20, "■■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) < 20 and round(prop("已完成") / prop("总量") * 100) > 0, "■ " + format(round(prop("已完成") / prop("总量") * 100)) + "%", if(round(prop("已完成") / prop("总量") * 100) == 0, "⛽️ 快开始任务吧", "❌ 我猜你数字填错了")))))))))))
```


是的，看起来很难受。还是选取轻便的方法实现吧。


### Slice() 函数快捷实现进度条


让我们我们先来看看 `Slice()` 函数，


![43ae7504508a49cce380952a2b702beddee08cc7cc0afda2423fb4cf8ec7db6c.png](/images/f2830fa301b65d859d041f99c460d752.png)


`Slice()` 函数会对输入的字符进行切割，输入我们想切割的数量，就会得到切割完成后数据。


这里我们可以对进度条进行简单的切割，做一下演示。


![313c94eaa59b8ca1aa8793f6dd3d8160fc61a89763f0f15f9e43702bc1f89096.gif](/images/8dc86b24c1828573e52d225a9af912f7.gif)


代码如下：


```text
slice("■■■■■■■■■■", 9)
```


注意：这里切割十个方块，最后剩下的只有一个，即（10 - 9），后面我们会用到这个。


顺着这样的思路，9 能不能是一个可以变得量呢，比如随着日期百分比进行一个变化？也就是说可以跟着上面的百分比变化。那我们试试看。


![529150855ac6600f285700af231aec18661558396f850e1252edb72b41bb6cf5.gif](/images/c6ee029dc155ef86c366ec3c3814f773.gif)


代码如下：


```text
slice("■■■■■■■■■■", 10 - toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 10))
```


没问题，成功实现，这里对代码做一下解析。这里我们先对小数乘以十，得到一个个位数。


因为 `Slice()` 函数中的数字是切割掉前面的方块，所以我们反向操作，用 10 减去上面的个位数，那么`Slice()` 就会切割完成后的方块就是我们想要的方块啦。


再接再厉，接下来则呢么在进度条后面加上数字百分比呢？


我们直接相加即可。


![8bedfe5956cc48ae9dce225cda21d728651a8e7c03fdab67e7181cea8b95d38f.gif](/images/f83bbd73f4c3dbd902c60d848f5fdf89.gif)


代码如下：


```text
slice("■■■■■■■■■■", 10 - toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 10)) + " " + format(round(toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 100))) + "%"
```


中间的「" "」是一个空格，主要是为了美观。


? 到此，教程就结束啦！? 这里感谢一下 Maylie 小姐姐。


**“诶，别急，你先等等，开头的个性化呢？”**


**你瞧我这榆木脑袋，这就做。**


### 个性化你的进度条


在文章之前我们提到了方块■，这里的方块能不能替换成 Emoji，或者是文字呢？


当然可以！这里我将方块■替换成爱心❤️，效果如下：


![49814091f15e308595a272c114ea0fc0f64115ef7a032843dcd72a2c6401428b.gif](/images/d9c512ec1a1dd8b0277530609227b936.gif)


代码如下：


```text
slice("❤️❤️❤️❤️❤️❤️❤️❤️❤️❤️", 20 - toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 20)) + " " + format(round(toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 100))) + "%"
```


但是需要注意的是爱心❤️是两个字符，所以切割出来的数据是不准确的，需要将倍数乘以二，最终得到我们想要的效果。


```text
20 - toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 20))
```


当然按照这个思路，还可以制作月球转动的效果。


![e6ef1db7297524ce0e077c7f3ed84432f686d3bc32bb9032b26fc80d8c07540f.gif](/images/49b1e875c19ff133ea14f8753d73c51b.gif)


期待各位小伙伴们的发挥。


## 延伸


当然 Notion 进度条不仅仅可以来表示还剩多久，还可以表示工作进度、健身进度、学习进度、阅读进度、旅行进度等等……


基本操作原理都一样，按照自己需求定制即可。


说回来，开头的 GIF 其实有一些有趣的点，就是加了 emoji「❌」还有「⛽️」，「2019 已经过去了多少的进度条演示」中，孤零零的显示一个这个应该怎么制作呢？


### IF 判断看这里


这里就需要用到 IF 判断了，针对之前健身进度，这里有三个指标。

1. 当健身天数为零的时候，那么就显示「⛽️ 快开始任务吧」
2. 当健身天数不为零，且在范围内，显示进度条
3. 当健身天数不为零，但是数字超出限定范围内，比如负数，那么就显示「❌ 我猜你数字填错了」

顺着这个逻辑，我们就可以使用代码轻松实现出来。


先把「2019 已经过去了多少的进度条演示」判断代码贴一下：


```text
if(empty(prop("Time")), "", slice("❤️❤️❤️❤️❤️❤️❤️❤️❤️❤️", 20 - toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 20)) + " " + format(round(toNumber(dateBetween(now(), prop("Time"), "days") / 365 * 100))) + "%")
```


效果如下：


![9e29a15e376fff391f2c8350b74c26fe68bb814364c337fcbe36f6a3f9f26a0a.gif](/images/9c4edd8cc3586de8de8d15932353e358.gif)


? 剩下交给大家，欢迎评论留言。


## 后话


Notion formula 的能力怎么说，限制性还是比较大，基础的展示计算可以做，但是一些联动就很困难，或者说根本就做不到。


最近刚好也看到官方对 formula 的反馈，未来会做比较大的更新，我甚是期待，不过就算没有看到官方的反馈，按照创始人的构思，怎么会不做呢？


对了，目前 Notion 看起来是以优化性能为主要任务，还有就是 Notion API 正在开发中，更期待，能完成与其他应用的联动，Notion 就飞起了。


---


▎本文由 [简悦 SimpRead](https://simpread.pro/) 转码。

