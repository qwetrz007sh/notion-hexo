---
cover: /images/f3217a53375fa77f12925bfbc2f46bb1.png
categories: 博客折腾手册
tags: []
title: 利用github action自动同步notion文章到halo博客
date: '2023-12-26 19:00:00'
updated: '2023-12-27 10:13:00'
---

## 如果notion你想改为中文：[https://greasyfork.org/zh-CN/scripts/430116-notion-zh-cn-notion的汉化脚本](https://greasyfork.org/zh-CN/scripts/430116-notion-zh-cn-notion%E7%9A%84%E6%B1%89%E5%8C%96%E8%84%9A%E6%9C%AC)


---


由于elog需要手动打开cmd输入命令才能同步notion文章到halo博客，这实在是太程序员了。


为了更智能化，只需要动动notion标签就可以自动同步文章或备份到github，可以说是一劳永逸！！


> 本篇文章也是用于记录，方便我以后查看，算一个超级小白教程（毕竟我不是程序员）。实践的过程并不顺利，甚至还有些小艰难，作为纯技术小白，以下记录我的一些踩坑经历。我参考了各位大佬的文章和代码，自己研究了2天终于搞好了。


用到了哪些工具
完整的工具链如下，除了elog和halo剩下的工具都需要注册


notion→slack→pipedream→elog→halo→github


---


# 一、halo博客+elog同步 搭建


# 二、注册slack


[https://slack.com/](https://slack.com/)（注册教程就不用了吧）


### 添加notion应用


![Untitled.png](/images/802565b6c70a3ff0353c6674bb0f2dad.png)


### 与notion进行关联


![Untitled.png](/images/4c9bfbc3f703bd432d339f7c138c0912.png)


关联后，在notion配置小闪电（自动程序）用于监听文档状态（例如当有文档status设置为已发布，则给slack发送消息）


![Untitled.png](/images/6af64086bc8c05faabc3167445e34643.png)


当你把文章改为待发布时，等几分钟，会自动发消息到slack



![Untitled.png](/images/64f3dab5db174af9a5dc4bfcca061d3f.png)


# 二、注册pipedream


> 添加自动化脚本trigger用于监听slack的消息，并通知GitHub Workflow Action进行打包发布，创建trigger和http调用通知。需要提前在Github创建存储仓库并生成**Github Token**


先注册：[https://pipedream.com/](https://pipedream.com/)


### 注册后创建项目


![Untitled.png](/images/5ee5b5bd02deec700b626449219d3b6f.png)


### 创建项目后，就添加工作流


![Untitled.png](/images/b2422cd67a8f3f15c7e0d0cb7e417836.png)


### 开始设置工作流


![Untitled.png](/images/6d31aac219380c2aaab4d2b2c1e58241.png)


![Untitled.png](/images/0f2808a27a636cba0ac65b9b97994d0d.png)


![Untitled.png](/images/ce909cb074e7c1314f881c70d6399439.png)


### 然后 设置这个http图标


填写的http api教程看下面


![Untitled.png](/images/3622a0eef1188c823902e8cf588e9487.png)


### 在配置http api连接前，建议直接fock我的仓库，方便直接修改


[https://github.com/qwetrz007sh/haloaction](https://github.com/qwetrz007sh/haloaction)


![Untitled.png](/images/9db64b8af9538268895e1682bd847b30.png)


![Untitled.png](/images/2b0992aaaf8c34816cd4f42984dd8f7a.png)


### 配置http链接（这个有点麻烦，仔细看）


参数说明

- user: Github用户名
- repo: Github仓库名
- token: Github Token

### 获取Github Token


![Untitled.png](/images/ba0fe0a955cfdf25c45f4b22f5a02047.png)


![Untitled.png](/images/984496ac71485474270e95d935bf309d.png)


![Untitled.png](/images/d944e4fbc08523574a74261e08fa5e1d.png)


![Untitled.png](/images/7ea0cbcd4884f993970fdaf103a25b22.png)


![Untitled.png](/images/b3b8305a481103beec62199041ef046a.png)


![Untitled.png](/images/e24130d4a6cf486367686779928b3625.png)


得到的Github Token为：后面填的环境变量命名为：GH_TOKEN


```javascript
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
```


**自行替换调用GitHub的Deploy API的地址：**
https://serverless-api-elog.vercel.app/api/github?user=Github用户名&repo=Github仓库名&event_type=deploy&token=Github Token



打开替换好的http连接，如果显示`success!`    就说明正确的


## 配置环境变量


在本地运行时，用的是`.elog.env`文件中定义的 Notion 账号信息，而在 Github Actions 时，需要提前配置环境变量。


在 Github 仓库的设置中找到 `Secrets  and variables`，


新增仓库的环境变量：和`.elog.env`保持一致即可


```text
      
# Notion相关环境变量

NOTION_TOKEN
NOTION_DATABASE_ID

# Halo变量

HALO_ENDPOINT
HALO_TOKENK

# 图床相关环境变量

GH_USER
GHTC_TOKEN
GH_REPO
GH_HOST
GH_PREFIXKEY

# 为了把github图床的token和github action的token区别，还加一个

GH_TOKEN
```


![Untitled.png](/images/d066181df3ecbd9217796caa13f4b03a.png)


PS：GHTC_TOKEN的图床token获取方法在上篇文章（**第三步：GitHub图床**配置）已经介绍了


![Untitled.png](/images/e37f279ebc6f89006d5535609c7b5187.png)


![Untitled.png](/images/f887b30750924728340e33071b93c1a3.png)


![Untitled.png](/images/066b6bd35a9112cfb035314684f821e0.png)


# 最后，修改一下我提供的 GitHub 的工作流：


`.github/workflows/main.yaml`


我已经帮大家配置好了，大家只需要替换自己的账号就行了


![Untitled.png](/images/91f8ad5256fe94aac57b0c8d4538f741.png)


![Untitled.png](/images/a3e00abf4200f884d5dfca1e815ba124.png)


![Untitled.png](/images/21d385688705f85e4bab6093a042ecb5.png)


### 脚本触发


---


如果一切顺利，当您在 notion 中将文章的状态变为 “待发布” 时候，将会自动触发工作流，进行备份，如果遇到错误，请排查工作流的报错信息。


![6526a765b9ded.png](/images/7326ff475ec1a5b9099151c081557007.png)


最后当您二次修改文章之后再次触发，文章会进行增量更新。


## 总结


---


目前只有elog+hexo的教程，没有elog+halo的完整教程


---


参考文章：


[跨域协同]  elog+notion 实现 md 优雅备份 | MatrixCore [https://matrixcore.top/article/elog](https://matrixcore.top/article/elog)


Notion 博客折腾指南 | Think Blog[https://blog.ithuo.net/post/2023-11-07/Notion博客折腾指南](https://blog.ithuo.net/post/2023-11-07/Notion博客折腾指南)

