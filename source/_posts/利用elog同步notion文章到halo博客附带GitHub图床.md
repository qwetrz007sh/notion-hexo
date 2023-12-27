---
cover: /images/f3217a53375fa77f12925bfbc2f46bb1.png
categories: 博客折腾手册
tags: []
title: 利用elog同步notion文章到halo博客附带GitHub图床
date: '2023-12-26 19:00:00'
updated: '2023-12-27 10:13:00'
---

# **前言**


 我是在此文章基础上改成了我自己的习惯（其实你参考elog官网文档也可以）：


[http://lexur.cc/archives/notion-elog-halo-doc](http://lexur.cc/archives/notion-elog-halo-doc)


使用他的配置我发现，如果你想在halo里面编辑，会得到错误提示：


**“未找到符合的html格式的编辑器”**


如果你有强迫症，请看我最下面的常见问题合集


### 虽然官方也建议不要在halo后台编辑notion同步的文章


![Untitled.png](/images/84c3311312ef6a11f0856d507a9d7470.png)


> 我写的文章主要是给大家展示：

1. 用图片标注：配置文件中重要的问题
2. 在他的基础上增加了GitHub图床
3. 可能国内可以访问的GitHub图床配置

# **简介**


> 🍭 notion


Notion是一款功能强大的协作工具，结合了笔记、项目管理、数据库和文档编辑等多种功能。用户可以在Notion中创建各种类型的内容，如文本、表格、嵌入式多媒体等，并进行组织和共享。Notion的灵活性和强大的自定义功能使得团队可以根据自己的工作流程和需求来创建和管理项目、文档和知识库，成为许多团队和个人喜爱使用的协作工具。


Notion API：[developers.notion.com](https://developers.notion.com/)


> 🍭 halo


Halo是一款开源的内容管理系统（CMS），专注于创建个人博客和小型网站。它提供了简洁的用户界面和易于使用的编辑工具，使用户能够快速搭建和管理自己的网站内容。Halo支持主题定制和插件扩展，用户可以根据自己的需求进行个性化定制。Halo还注重安全性和性能优化，为用户提供稳定可靠的网站运行环境。由于其简洁易用和灵活性，Halo受到许多个人博客作者和小型网站运营者的青睐。


Halo Docs：[docs.halo.run](https://docs.halo.run/)


> 🍭 elog


开放式跨端博客解决方案，随意组合写作平台（语雀/飞书/Notion/FlowUs）和博客平台（Hexo/Vitepress/Confluence/WordPress）等。解决博客的编写，管理，分享问题。实现一端编写，多端同步的功能。


Elog Docs：[elog.1874.cool/notion/start](https://elog.1874.cool/notion/start)


# 安装**Elog**


### **环境准备安装nodejs**

- 国内：[https://nodejs.cn/learn/](https://nodejs.cn/learn/)
- 国外：[https://nodejs.org/en](https://nodejs.org/en)

**Node 环境（>=12.0.0）**,尽量使用stable版本，即稳定版。


### **全局安装elog**


使用一下一种方法安装可以


> 若出现网络问题可以上网寻求帮助，改用pnpm。


```shell
# 使用 npm 安装 CLI
npm install @elog/cli -g

# 使用 yarn 安装 CLI
yarn global add @elog/cli

# 使用 pnpm 安装 CLI
pnpm install @elog/cli -g

# 安装指定版本
npm install @elog/cli@0.9.0 -g

```


# 配置elog文件


## **初始化**


在本地创建一个用来同步的文件夹，进入文件夹，打开终端或cmd，执行以下命令，初始化工作区。


```shell
elog init
```


初始化过后会增加以下两个文件

- `elog.config.js` ：配置文件，通过配置此文件指定elog的博客编辑站点与发布站点。
- `.elog.env`：环境变量，`elog.config.js` 文件可以读取此文件中的信息。如果使用github等同步时需要将此文件加入`.gitignore`，防止信息泄露！！！

![Untitled.png](/images/e0be82a82d624a3ca4ae43e53903d5e2.png)


## **.elog.env 环境变量**


这里填写我们之前获取的 Notion 和 Halo 和 GitHub 信息（最终效果），配置教程往下看


```shell
# Notion
NOTION_TOKEN=secret_******  
NOTION_DATABASE_ID=a0c*****

# Halo
HALO_ENDPOINT=https://dboke.com  #解释：填你自己的域名
HALO_TOKEN=pat_******
#HALO_POLICY_NAME=atta******-AxbCU #解释：经过反复测试，这个可以不用填

# Github
GITHUB_USER=qwe****  #解释：填你的github用户名
GITHUB_TOKEN=github_pat_***********  #解释：下面后获取教程
GITHUB_REPO=halotuchuang  #解释：GitHub仓库名
GITHUB_BRANCH=main #解释：以前是master现在GitHub改为了main
GITHUB_HOST=https://jsd.cdn.zzko.cn #解释：自定义cdn加速源域名
GITHUB_PREFIXKEY=blog/halo   #解释：为了避免根目录有一堆图片，还是创建下文件夹把
```


# **第一步：Notion**


首先来说获取这两个


```javascript
# Notion
NOTION_TOKEN=secret_******
NOTION_DATABASE_ID=a0c*****
```


notion申请网站：[https://www.notion.so/my-integrations](https://www.notion.so/my-integrations)


## 1.1 **Token**


1 点击`New Integration`创建Integration


![Untitled.png](/images/0af1fabd7e3a8d1cc8563f45c88afd7e.png)


2 填写必要信息


> **注意：选择工作区之后这个秘钥就只能使用选择工作区的页面！！！**


![Untitled.png](/images/92d2492ef6014ec37194b23bc4c04a62.png)


3 复制`Secrets`（点击Show即可查看秘钥）


> **秘钥前面一部分也是需要使用的！**


![Untitled.png](/images/985f38f5a6e9652bf733339f394a42cb.png)


### 2.1 获取数据库模板


1 获取数据库模板 [https://1874.notion.site/b061632fc7d644eaa12f3e0f095938fb?v=7491fbb2415c43cf9752a7e4ad9f41a5](https://1874.notion.site/b061632fc7d644eaa12f3e0f095938fb?v=7491fbb2415c43cf9752a7e4ad9f41a5)


	> 点击 `Duplicate` 将数据库模板复制到上面秘钥选择的工作区中，**工作区必须保持一致！！！**

	- 模板中的数据库字段请不要随意删除，会造成无法恢复！！！（增减字段是没有问题的）

![Untitled.png](/images/d4cf7ece2d92ba9f5f11a5c65f64e3b2.png)


找到已经复制好的模板，点开


![Untitled.png](/images/6cc98a9b6c93b173509d222d52b693fd.png)


### 2 授权数据库给秘钥！！！


> 有些教程**省略了这一步，缺少这一步创建的秘钥任然无法访问数据库！！！**


![5bc23a1eb65fff0f8286f3d6e8df-hfvn.gif](/images/ba5f1d22da4686703ce62691d1afb322.gif)


按照图片添加你新建的token


![Untitled.png](/images/1b96e1a0844fab0e5ad706d7ef60278a.png)


### 3 获取数据库ID

- 复制浏览器地址栏数据库的id即可
- 数据库ID为最有一个斜杠之后到？之前的部分，完全由字母和数字组成的uuid

![Untitled.png](/images/22ed778c2b78ebca8582fc8aaebda0df.png)


![Untitled.png](/images/240ec934821c2cbdf456c26e3a132f46.png)


# **第二步：**Halo配置


```javascript

# Halo
HALO_ENDPOINT=https://dboke.com  #解释：填你自己的域名
HALO_TOKEN=pat_******
HALO_POLICY_NAME=atta******-AxbCU #解释：halo的储存源，必填，否则无法使用GitHub图床


```


### 1.**endpoint**


halo网站的地址，例如 `http://halo.1874.orb.local`，区分 `http/https` 。


### 2.**token**


打开halo控制台，找到 用户→点击管理员用户→个人令牌→创建令牌。


	所需权限：

	- 附件管理
	- 文章管理

![Untitled.png](/images/1f110328f70f367c7d75ed81142ff46a.png)


### 3.**policyName**


> 可以将图片上传到halo设定的存储中去，默认本地，也可存储到OSS


> 由于我们要使用GitHub图床，所以你需要在halo里配置GitHub储存源


**请在halo安装、配置此插件：**[**https://github.com/guicaiyue/plugin-githuboss**](https://github.com/guicaiyue/plugin-githuboss)


配置教程：这个插件作者GitHub里面有教程，如果有不懂的也可以问我


Halo 的存储策略，可前往**附件管理**中，F12 打开浏览器开发者控制台，刷新后查看`/apis/storage.halo.run/v1alpha1/policies`接口，找到返回的`metadata.name`字段值，例如我的是：attachment-policy-AxbCU


PS：如果不填，则使用默认存储到本地，会占用本地硬盘空间


# **第三步：GitHub图床**配置


```javascript

# Github
GITHUB_USER=qwe****  #解释：填你的github用户名
GITHUB_TOKEN=github_pat_***********  
GITHUB_REPO=halotuchuang  #解释：GitHub仓库名
GITHUB_BRANCH=main #解释：以前是master现在GitHub改为了main
GITHUB_HOST=https://jsd.cdn.zzko.cn #解释：自定义cdn加速源域名
GITHUB_PREFIXKEY=blog/halo   #解释：为了避免根目录有一堆图片，还是创建下文件夹把
```


GITHUB_TOKEN：获取方法


![Untitled.png](/images/60e2ff9406b56418cd9eb78983d228ce.png)


![Untitled.png](/images/33bd0eadc17c031aa023cf6d1cf56f6d.png)


![Untitled.png](/images/7e5d9e673e0b64818bb9c9a5ed17d4a5.png)


PS：HOST如果不填，则使用默认源，可能会导致图片上传失败：


![Untitled.png](/images/bf01616afa98cc5e457597c8a76b1b5e.png)


### .elog.env配置的Notion 和 Halo 和 GitHub 信息（最终效果）


![Untitled.png](/images/88a8c8d89a951329c1e670b84247b963.png)


## 接下来配置**elog.config.js**


cmd里面输入**elog.config.js**


![Untitled.png](/images/628fb4c7696bb063aeeffd1c9f69a16c.png)


默认配置文件是使用的语雀，你替换为我修改的即可


```javascript
module.exports = {
  write: {
    platform: 'notion',
    notion: {
      token: process.env.NOTION_TOKEN,
      databaseId: process.env.NOTION_DATABASE_ID,
      filter:  {property: 'status', select: {equals: '待发布'}}
    }
  },
  deploy: {
    platform: 'halo',
    local: {
      outputDir: './docs',
      filename: 'title',
      format: 'markdown',
    },
    halo: {
      endpoint: process.env.HALO_ENDPOINT,
      token: process.env.HALO_TOKEN,
      policyName: process.env.HALO_POLICY_NAME,
      rowType: 'html',
      needUploadImage: false,
    }
  },
  image: {
    enable: true,
    platform: 'github',
    local: {
      outputDir: './docs/images',
      prefixKey: '/images',
      pathFollowDoc: false,
    },
    github: {
      user: process.env.GITHUB_USER,
      token: process.env.GITHUB_TOKEN,
      repo: process.env.GITHUB_REPO,
      branch: process.env.GITHUB_BRANCH, 
      host: process.env.GITHUB_HOST,
      prefixKey: process.env.GITHUB_PREFIXKEY,
    }
  }
}
```


**下面是解释，你可以参考着调整**


```javascript
module.exports = {
  write: {
    platform: 'notion', #解释：使用notion同步
    notion: {
      token: process.env.NOTION_TOKEN,
      databaseId: process.env.NOTION_DATABASE_ID,
      filter: {property: 'status', select: {equals: '待发布'}} #解释：状态为待发布的文章，才会同步
    }
  },
  deploy: {
    platform: 'halo', #解释：同步到halo博客
    local: {#解释：默认备用，没删
      outputDir: './docs',
      filename: 'title',
      format: 'markdown',
    },
    halo: {
      endpoint: process.env.HALO_ENDPOINT,
      token: process.env.HALO_TOKEN,
      policyName: process.env.HALO_POLICY_NAME,
      rowType: 'html', #解释：这是elog官方建议的html，如果你想在halo后台修改文章（我也不建议），你可以修改为markdown，同时在应用市场安装markdown编辑器
      needUploadImage: false, #解释：不需要halo来上传图片，elog已经配置了github图床来上传了，否则会重复上传！
    }
  },
  image: {
    enable: true,  #解释：开启elog上传图片
    platform: 'github',  #解释：使用GitHub图床
    local: {   #解释：默认备用，没删
      outputDir: './docs/images',
      prefixKey: '/images',
      pathFollowDoc: false,
    },
    github: {  #解释：这下面的内容需要跟.elog.env对应
      user: process.env.GITHUB_USER,
      token: process.env.GITHUB_TOKEN,
      repo: process.env.GITHUB_REPO,
      branch: process.env.GITHUB_BRANCH, #解释：增加自定义分支
      host: process.env.GITHUB_HOST,   #解释：增加自定义加速源
      prefixKey: process.env.GITHUB_PREFIXKEY,  #解释：增加自定义仓库路径
    }
  }
}
```


# **第四步：编写文章**


第一行模板不要动


点击新增行，输入文章名


![Untitled.png](/images/b6ba6a7dab02c91829d81285d5d5b0b2.png)


打开编写内容后


![Untitled.png](/images/db67ec0f69734319c61326f2790e18f9.png)


修改状态为：待发布


# **第五步：同步**


执行以下命令进行同步


_同步文章/发布文章_


```javascript
elog sync -e .elog.env
```


_清除本地缓存,如果想要重新发布则清除缓存后重新同步即可。_


```javascript
elog clean
```


然后本片文章就发布成功了


# 常见问题


### 选择本地存储后，文章内部不显示图片？


![Untitled.png](/images/593560fc07795c6ff29ff5575df91ed0.png)


**原因1：**有可能是，你使用的域名访问博客，但是docker配置文件写的还是ip


**解决方法：**修改docker配置文件，把ip改为域名后，重新运行docker，compose搭建的需要进入文件夹运行：docker-compose down && docker-compose up -d


**原因2：**插件问题，把所以的插件都先禁用，看看能不能显示


## 进阶教程：使用GitHub Action自动同步notion文章到halo


官方教程：[https://elog.1874.cool/notion/vy55q9xwlqlsfrvk#notion示例](https://elog.1874.cool/notion/vy55q9xwlqlsfrvk#notion%E7%A4%BA%E4%BE%8B)

