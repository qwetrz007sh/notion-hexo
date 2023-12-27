---
cover: /images/3253eb087e7d9c93829d20b090c40f6a.png
categories: 白嫖
tags:
  - 图床
  - CF
  - 博客运维
title: 使用 PicGo+CF(Cloudflare)+B2(Backblaze) 作为博客图床
date: '2023-12-26 11:11:00'
updated: '2023-12-26 17:07:00'
---

## 前言


我对于 B2(Backblaze) 的了解一开始并不是云储存业务，而是它家的季度硬盘故障率报告


过程中由于插件部分设置在最后一步卡了一天左右，不过最后还是通过曲线方式想通了。


---


## 前期准备

- 没有阻断的网络环境
- 一个付费二级域名
- Cloudflare 账号
- B2(Backblaze) 账号

---


## 所需要的轮子


[PicGo](https://picgo.github.io/PicGo-Doc/zh/guide/)


[picgo-plugin-s3](https://github.com/wayjam/picgo-plugin-s3)


---


## 为什么要用 B2(Backblaze)

- **低审核风险；**
- **足够大的免 (bai) 费(piao)额度**[
](/images/3253eb087e7d9c93829d20b090c40f6a.png)

	![b2_free_plan.png](https://images.ostdb.info/blog/20221123/b2_free_plan.png)

- **即便以后不白嫖，收费也是大型 IDC 里最低的**[

	![b2_price.png](/images/c47035139f4dc0dc97619e3cf0ed526e.png)


	](https://images.ostdb.info/blog/20221123/b2_price.png)


B2 已经加入了 Cloudflare 的带宽联盟，完全可以 hold 住我这小博客那点流量和访问了。


---


## B2(Backblaze) 配置


### 注册账号


![step01_register.png](/images/b6738f9b18f2478c9498051f3184ed69.png)


---


### 新建 bucket


![step02_01.png](/images/dd3df2fdc61e80570cde879ce1b62a63.png)


[


![step02_02.png](/images/13eac374f4992428f793d1260d10e9f3.png)


](https://images.ostdb.info/blog/20221123/step02_02.png)[


![step03_01.png](/images/7cfe7ececbd9581abba19b1f7dbf8e52.png)


](https://images.ostdb.info/blog/20221123/step03_01.png)


这样第一个 bucket 就建立好啦，记下上 Endpoint 和 Bucket Name：


| 字段名         | 值                                                                                 |
| ----------- | --------------------------------------------------------------------------------- |
| Bucket Name | test-ostdb                                                                        |
| Endpoint    | [https://s3.us-west-004.backblazeb2.com](https://s3.us-west-004.backblazeb2.com/) |


**Endpoint 地址一定要前面加****`https://`****，这将影响后续 PicGo 插件设置**


---


### 设置 bucket


以下均为可选项目，是否配置按个人需求决定，是否配置均不影响 b2 作为图床本身的上传及使用。


### 设置缓存回源时间


将`Bucket Info`设为如下值


| 字段          | 值                                   |
| ----------- | ----------------------------------- |
| Bucket Info | `{“cache-control”:“max-age=86400”}` |


意味着 86400 秒 (即 24 小时) 内，CF 将不返回该 bucket 内重新拉取信息，如需让 CF 更新信息需手动进入 CF dashboard 清理缓存。


![step03_02.png](/images/363220a7ce308b43d7b948afd2fa3b2a.png)


---


### 存活时间设置 (Lifecycle Settings)


将其设置为`Keep only the last version of the file`，只保留 bucket 内最新版本的文件。


![step03_03.png](/images/ca45820c6c103d8120f67bf0d6e7ac87.png)


---


### 跨域规则设置 (CORS Rules)


直接引用 CF 绑定的域名，作为博客自身图床使用无跨域需求。


---


### B2 API 配置


### 新建 API 密钥


![step04_01.png](/images/657f541d1769a54494144c20153fcf17.png)


[


![step04_02.png](/images/8963f6d87993657ddd55432cb8f66c32.png)


](https://images.ostdb.info/blog/20221123/step04_02.png)


---


### 获取 API 的 keyID 及 applicationKey


点击 Create New Key 后，记下红框中的 keyID 还有 applicationKey 记下来：


![step04_03.png](/images/7fa624cb14b8fb8ee7b7512e64ffa3ac.png)


| 字段名            | 值                               |
| -------------- | ------------------------------- |
| keyID          | 004678d60f5a3240000000007       |
| applicationKey | K004OLCnqn/lzxm9Q9TH6O59zbzY+7o |


---


### 最后一点关键信息

- 随便上传点图片到 bucket 内：[

	![step05_01.png](/images/4264914265263db3588922deeb8f5cbd.png)


	](https://images.ostdb.info/blog/20221123/step05_01.png)


---

- 刹那酱真可爱， 点击打开刚刚上传的图片，记下 Friendly URL 的值:

	| 字段           | 值                                                                              |
	| ------------ | ------------------------------------------------------------------------------ |
	| Friendly URL | https://_f004.backblazeb2.com_/file/test-ostdb/20190531_Line_stamp_image_2.png |


	[


	![step05_02.png](/images/9144e9286b9360f941b2fdc973bbe6c7.png)


	](https://images.ostdb.info/blog/20221123/step05_02.png)


---


### B2 部分复盘


以上就是 B2 端所需要的所有设置了，核对下以下内容是否全部记下：


| 字段             | 值                                                                                                                                                            |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Bucket Name    | test-ostdb                                                                                                                                                   |
| Endpoint       | [https://s3.us-west-004.backblazeb2.com](https://s3.us-west-004.backblazeb2.com/)                                                                            |
| keyID          | 004678d60f5a3240000000007                                                                                                                                    |
| applicationKey | K004OLCnqn/lzxm9Q9TH6O59zbzY+7o                                                                                                                              |
| Friendly URL   | [https://f004.backblazeb2.com/file/test-ostdb/20190531_Line_stamp_image_2.png](https://f004.backblazeb2.com/file/test-ostdb/20190531_Line_stamp_image_2.png) |


---


## CF(Cloudflare) 配置


### CNAME 配置


进入 CF 的 DNS 设置，新增一条 DNS 记录，类型为 CNAME，名称可自己选择，我这里用了`test`作为前缀；


| 类型    | 名称   | 目标                   | 代理状态 |
| ----- | ---- | -------------------- | ---- |
| CNAME | test | f004.backblazeb2.com | YES  |


**目标地址为上面记事本中 Friendly URL 的主域名地址**，即`f004.backblazeb2.com`，代理状态务必开启 (黄色云朵点亮)，保证域名套上 CF 的 CDN。


![step06_02.png](/images/187aa4ae3e4932c23d053e341aadbadf.png)


记下这个 CNAME 记录：


| 字段  | 值               |
| --- | --------------- |
| URL | test.ostdb.info |


---


### 使用 CF 的转换规则隐藏 bucket 名称


**随便暴露 bucket 名称是个非常愚蠢的行为**，当然了，当你看到本文时，这个作为范例暴露出来的 bucket 已经不复存在了~


![step06_03.png](/images/9ae130d1062b5dcc86c71dd5b7f882a4.png)


---


### 传入配置


| 字段名    | 运算符 | 值                                                                                                                                                                                    | 备注                       |
| ------ | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------ |
| 主机名    | 等于  | test.ostdb.info                                                                                                                                                                      | 值为上一步设置的 CNAME 记录        |
| AND    |     |                                                                                                                                                                                      |                          |
| URI 完整 | 不包含 | [https://test.ostdb.info/file/](https://test.ostdb.info/file/*test-ostdb*/)[_test-ostdb_](https://test.ostdb.info/file/*test-ostdb*/)[/](https://test.ostdb.info/file/*test-ostdb*/) | _test-ostdb_ 为 bucket 名称 |


---


### 重写配置


| 路径  | 重写类型    | 值                                                  | 备注                       |
| --- | ------- | -------------------------------------------------- | ------------------------ |
| 重写到 | Dynamic | concat(“/file/_test-ostdb_“,http.request.uri.path) | _test-ostdb_ 为 bucket 名称 |


---


### 测试重写是否生效


将 Friendly URL 中的`f004.backblazeb2.com/file/test-ostdb/`替换为 URL`test.ostdb.info`后，看是否能正常打开图片


| 原有值                                                                                                                                                          | 替换后                                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| [https://f004.backblazeb2.com/file/test-ostdb/20190531_Line_stamp_image_2.png](https://f004.backblazeb2.com/file/test-ostdb/20190531_Line_stamp_image_2.png) | [https://test.ostdb.info/20190531_Line_stamp_image_2.png](https://test.ostdb.info/20190531_Line_stamp_image_2.png) |


![step07.png](/images/c908ee12645fa816ef12f4d6ab47da68.png)


---


## PicGo 配置


### 安装 PicGo 及 PicGo S3 Plugin


安装过程一路 next，略过不表；


插件在`插件设置`里搜`s3`即可，记得本机要安装 nodejs 环境。


本文所使用 PicGo 及插件版本如下：


| 软件包             | 版本号   |
| --------------- | ----- |
| PicGo           | 2.3.1 |
| PicGo S3 Plugin | 1.2.2 |


---


### 配置 S3 插件


看下之前记录下的那些值，该让它们派上用场了：


| 字段             | 值                                                                                                                                                            |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Endpoint       | [https://s3.us-west-004.backblazeb2.com](https://s3.us-west-004.backblazeb2.com/)                                                                            |
| keyID          | 004678d60f5a3240000000007                                                                                                                                    |
| applicationKey | K004OLCnqn/lzxm9Q9TH6O59zbzY+7o                                                                                                                              |
| Bucket Name    | test-ostdb                                                                                                                                                   |
| Friendly URL   | [https://f004.backblazeb2.com/file/test-ostdb/20190531_Line_stamp_image_2.png](https://f004.backblazeb2.com/file/test-ostdb/20190531_Line_stamp_image_2.png) |
| URL            | test.ostdb.info                                                                                                                                              |


![step08.png](/images/51a9a963c9bafb2a56af04e14c49b50c.png)


| 字段                 | 值                                                                                 | 对应项                          |
| ------------------ | --------------------------------------------------------------------------------- | ---------------------------- |
| 应用密钥 ID            | 004678d60f5a3240000000007                                                         | keyID                        |
| 应用密钥               | K004OLCnqn/lzxm9Q9TH6O59zbzY+7o                                                   | applicationKey               |
| 桶                  | test-ostdb                                                                        | Bucket Name                  |
| 文件路径               | /                                                                                 | 留空或者只填写’/‘，则上传图片全部会存放到根目录    |
| 地区                 | Null                                                                              | 可选                           |
| 自定义节点              | [https://s3.us-west-004.backblazeb2.com](https://s3.us-west-004.backblazeb2.com/) | Endpoint                     |
| 自定义域名              | [https://test.ostdb.info](https://test.ostdb.info/)                               | URL                          |
| PathStyleAccess    | **YES**                                                                           | **B2 使用的是 MinIO API，必须勾选该项** |
| rejectUnauthorized | NO                                                                                | 保持默认关闭即可，除非在 log 里出现证书错误     |
| bucketEndpoint     | **YES**                                                                           | **必须打开，否则无法正确调用自定义节点选项**     |


之后就是一边写博客，一边把图片往支持额外命令行的编辑器 (比如 [typora](https://typora.io/)) 里拖放或者黏贴就行了。记得在`文件路径`下设定好上传路径，防止所有文件全部被上传到根目录下产生混乱。


---


## 后记

- **PicGo 目前只能作为上传客户端连接 B2，无法实现管理功能；**
- B2 对大陆地区访问依然不友好。

---


## 参考链接


[使用 Backblaze B2 + Cloudflare CDN + PicGo 实现可自定义域名的 10G 免费图床解决方案](https://blog.winer.website/archives/use_blackblaze_b2_and_cloudflare_cdn_to_bulid_a_free_oss.html)


[启发](https://github.com/lsky-org/lsky-pro/discussions/448)


---


▎本文由 [简悦 SimpRead](https://simpread.pro/) 转码。

