
## 免费图片托管解决方案，Flickr/imgur替代品。使用Cloudflare Pages和Telegraph。


## 感谢


本次使用的程序是来自github上的二位大佬，我把拼合了一些


Telegraph-Image：https://github.com/cf-pages/Telegraph-Image


telegraph-Image：https://github.com/x-dr/telegraph-Image


本次使用的仓库地址：


https://github.com/gwcx/imgdalao


# 如何部署


## 提前准备


你唯一需要提前准备的就是一个Cloudflare账户 （如果需要在自己的服务器上部署，不依赖Cloudflare ）


手把手教程


简单3步，即可部署本项目，拥有自己的图床


1.下载或Fork本仓库 (注意：目前请使用fork，在使用下载部署存在问题)


2.打开Cloudflare Dashboard，进入Pages管理页面，选择创建项目，如果在第一步中选择的是fork本仓库，则选择连接到 Git 提供程序，如果第一步中选择的是下载本仓库则选择直接上传


![Untitled.png](/images/ebe339be333668b45722534e3cf263cd.png)


![Untitled.png](/images/2a290975ed1b2311cb8eb75d9ab253a3.png)


![Untitled.png](/images/681c1d0f26582edba6ecff3dba816285.png)


按照页面提示输入项目名称，选择需要连接的git仓库（第一步选择的是fork）或是上传刚刚下载的仓库文件（第一步选择的是下载本仓库），点击部署站点即可完成部署


# 特性


1.无限图片储存数量，你可以上传不限数量的图片


2.无需购买服务器，托管于Cloudflare的网络上，当使用量不超过Cloudflare的免费额度时，完全免费


3.无需购买域名，可以使用Cloudflare Pages提供的*.pages.dev的免费二级域名，同时也支持绑定自定义域名


4.支持图片审查API，可根据需要开启，开启后不良图片将自动屏蔽，不再加载


5.支持后台图片管理，可以对上传的图片进行在线预览，添加白名单，黑名单等操作


# 绑定自定义域名


在pages的自定义域里面，绑定cloudflare中存在的域名，在cloudflare托管的域名，自动会修改dns记录


![Untitled.png](/images/fa2a37d727347c536123b7f571fb86b2.png)


在文件的asset/js/upload.js文件中的第218和220行建议修改为自己的加速域名！


# 开启图片审查


1.请前往https://moderatecontent.com/ 注册并获得一个免费的用于审查图像内容的API key


2.打开Cloudflare Pages的管理页面，依次点击设置，环境变量，添加环境变量


3.添加一个变量名称为ModerateContentApiKey，值为你刚刚第一步获得的API key，点击保存即可


注意：由于所做的更改将在下次部署时生效，你或许还需要进入部署页面，重新部署一下该本项目


开启图片审查后，因为审查需要时间，首次的图片加载将会变得缓慢，之后的图片加载由于存在缓存，并不会受到影响


![Untitled.png](/images/8a342876300e2a6352339ea91e959830.png)


# 限制


1.由于图片文件实际存储于Telegraph，Telegraph限制上传的图片大小最大为5MB


2.由于使用Cloudflare的网络，图片的加载速度在某些地区可能得不到保证


3.Cloudflare Function免费版每日限制100,000个请求（即上传或是加载图片的总次数不能超过100,000次）如超过可能需要选择购买Cloudflare Function的付费套餐，如开启图片管理功能还会存在KV操作数量的限制，如超过需购买付费套餐


# 图片管理功能


1、支持图片管理功能，默认是关闭的，如需开启请部署完成后前往后台依次点击设置->函数->KV 命名空间绑定->编辑绑定->变量名称填写：img_url KV 命名空间 选择你提前创建好的KV储存空间，开启后访问http(s)://你的域名/admin 即可打开后台管理页面


变量名称 | KV命名空间


img_url | 选择提前创建好的KV储存空间


![Untitled.png](/images/ba617f0a20b23213504a90c0be52f828.png)


![Untitled.png](/images/a34294545d6185be628bde92070961e4.png)


2、后台管理页面新增登录验证功能，默认也是关闭的，如需开启请部署完成后前往后台依次点击设置->环境变量->为生产环境定义变量->编辑变量 添加如下表格所示的变量即可开启登录验证


变量名称 值


BASIC_USER <后台管理页面登录用户名称>


BASIC_PASS <后台管理页面登录用户密码>


![Untitled.png](/images/fad804b7a3fe1a62b5b2e98d8f5d9e98.png)


![Untitled.png](/images/e00895bab8e98a41c03c18669be0518e.png)

