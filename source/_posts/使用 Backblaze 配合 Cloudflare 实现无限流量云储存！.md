---
cover: /images/a528598b998663a940b35ffe94f9bec1.png
categories: 白嫖
tags:
  - CF
  - 反代
  - 网盘
title: 使用 Backblaze 配合 Cloudflare 实现无限流量云储存！
date: '2023-11-28 21:51:00'
updated: '2023-12-26 18:46:00'
---

Backblaze（下文中简称 B2）. 是一家美国云存储和数据备份公司，它的两个主要产品是针对企业和个人市场的 B2 云存储和计算机备份服务。

- 存储容量：10GB
- 网络流量：1GB / 天
- 上传流量：无限
- 下载请求数：2500 次 / 天
- 上传请求数：2500 次 / 天
- BUCKET(桶)：100 个
- BUCKET(桶) 文件数：无限

由于 B2 加入了 CloudFlare 的 带宽联盟（ Bandwidth Alliance）（如下图红圈部分所示） ，所以 B2 与 CloudFlare 之间的流量直接免费，也就是每天无限量下行流量。


![6ae67a603b2c55ed3e717e7751e69b834e972c55e31fc706476e310f305186e6.png](/images/a4885fcb910ec9bb6e944061c7874229.png)

1. 点击 [https://www.backblaze.com/b2/sign-up.html](https://www.backblaze.com/b2/sign-up.html)
2. 填写邮箱，密码即可注册。
3. 登陆平台 – My Account – 我的设置 – 验证 Email

提醒一下，界面右下角可以切换到简体中文


![4da867cd0082ad8ceaf74819240fcdd91c247d260e869421590599c2edc457fb.png](/images/16154fed9b045ec626551653510fd14b.png)

1. 登陆平台 – 创作一个桶

名称随意，桶里面的档案选择公众，其他保持默认即可


![6f27e3bcb2c482c4317054238657f2f374e5a414983381d9876a6731dfc5edf2.png](/images/959bde64221867714d07b35e41df1985.png)

1. 创建成功后 点击 【上载 / 下载】可以去上传一个文件

![2f0c5f9ee7527ff23bd2b5a55a4754c68882d8894c9208f0c0bde2a8f7eb5b78.png](/images/e5e3581d29debafaa54a4eb3f6421a7c.png)

1. 上传成功后，单机文件可以看见文件详情内容

要记住友好 URL 中的域名，如图是 f004.backblazeb2.com ，接下来套 Cloudflare 时会用到。


![2e49aa62e842547e4c70831435b067dc9a8966730b81ed26b27ba617c0d52ad0.png](/images/72c2a3c1e08698abebfa57f955a9e9a9.png)

1. 在 Cloudflare 上解析一个二级域名：

![7f90c0bb5862509929b21080aed2a241078031f8c86ae78c1c4efa2dfd01f8b2.png](/images/3dd26507dd457a631b7cbf7806c7a9cf.png)


类型选 CNAME，名称随意，目标填你在友好 URL 中看到的域名，代理状态要开启。

1. 更改 SSL/TLS 加密模式
在 Cloudflare 的左侧面板中点击 SSL/TLS:

![89cf535adb86bc55319c2c259a7b6cdc7ab16b08a9faf262896767d0bb89442d.png](/images/c7076caedbe39c41bb06b8fcd4d74ae6.png)


选择完全（严格）模式：


![b40a9a022aa8b711e3b6b3bea303c5a5b36c2a22e78b9f58999cda26a97b23bb.png](/images/5d5ec6144016527d58f4e3c4d428ec21.png)

1. 设置缓存
点击左侧面版中的 “规则”：

![cae157b0619339e0bff11c5ee27b5bb1422e5d068f971afa547590fe968feb87.png](/images/2f860277f76f5b9abfb3650c02481381.png)


点击 “创建规则”：


![e580b5c661fc2806d31c4b1da2e54e3b51d979ee050ffb4c59a2918e6010fb89.png](/images/146e079b6abe2b110f10a9bb61ee9d45.png)


URL 输入上一步中设置的域名，选择： 缓存级别 – 标准，边缘缓存 TTL – 1 个月。


打开 [https://secure.backblaze.com/b2_buckets.htm](https://secure.backblaze.com/b2_buckets.htm), 点击 ‘’桶设定‘’：


![9bdc7f77a4f5868d01e1fd13830161343c193ef17a875f24f47d73a984a57e5f.png](/images/c29bf60123fda1a758d270dbb692b4dd.png)


在桶信息中填写：`{"cache-control":"max-age=720000"}` :


![96772363096b2a190858d6aef293a20b48cabd816f8574b6d75bc486346005ae.png](/images/b2c44dc13690c38eb855c49444ce290b.png)


最后点击‘’更新桶‘’即可。


![549ced8997164b11e088d6bbdbefc13293a9f00d05de49e7908213195ec02821.png](/images/e165faaefed8349cd1e6972eb13047a9.png)


在友好 url 中将 B2 的域名改为你设置的域名即可。

- [https://www.backblaze.com/zh_CN/cloud-storage.html](https://www.backblaze.com/zh_CN/cloud-storage.html)
- [https://51.ruyo.net/17895.html](https://51.ruyo.net/17895.html)

---


▎本文由 [简悦 SimpRead](https://simpread.pro/) 转码。

