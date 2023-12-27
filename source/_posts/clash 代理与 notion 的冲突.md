---
cover: /images/683c438893f564f101e0669732b41cf5.png
categories: notion
tags:
  - notion
title: clash 代理与 notion 的冲突
date: '2023-11-15 16:51:00'
updated: '2023-12-26 18:46:00'
---

> 当打开 clash 代理之后，notion 就无法上传图片


当打开 clash 代理之后，notion 就无法上传图片


![image-1656073091111.png](/images/683c438893f564f101e0669732b41cf5.png)


## 方法


**这里是描述我解决问题的过程，如果只需要解决方法，可以直接看到最后面的部分**


### 初步尝试


第一时间想到的当然是去设置转发规则，设置 parser，添加 DIRECT 规则：


```text
parsers: # array
  - url: ----- subscribe url --------
    yaml:
      prepend-rules:
        - DOMAIN-SUFFIX, notion.so,DIRECT
```


然后更新订阅 profile


此时再去查看 notion 相关的链接：


![image-1656073252115.png](/images/ed18913855370b37c97242cfed460302.png)


### 正解


然而问题是，即使这里采用 DIRECT 规则，依旧无法上传图片，只有把 clash 的 System Proxy 关闭才能上传图片，就很奇怪。


将代理改成 DIRECT 之后，发现也是可以成功的，说明是加入的规则不正确，没有涵盖上传图片的 api。


![image-1656073478536.png](/images/fb1ec73954b1a7e681016c4474dfd940.png)


经过测试，才发现，原来 notion 的文件管理使用的是亚马逊的云服务


![image-1656073672219.png](/images/be7068d1d09543a2c33b1af21846261a.png)


通过选中 notion 中图片，并 View Original，就可以发现图片在亚马逊云上的链接：“[https://s3.us-west-2.amazonaws.com/secure.notion-static.com/xxxxxxxxxxxxxxxxxxxxx](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/xxxxxxxxxxxxxxxxxxxxx)”， 而且这个链接是没有被墙的，所以我们就可以在 parser 中添加这条规则：


```text
parsers: # array
  - url: ----- subscribe url --------
    yaml:
      prepend-rules:
        - DOMAIN-SUFFIX, notion.so,DIRECT
        - DOMAIN-SUFFIX, amazonaws.com,DIRECT
```


至此，问题解决。 > 本文由简悦 SimpRead 转码

