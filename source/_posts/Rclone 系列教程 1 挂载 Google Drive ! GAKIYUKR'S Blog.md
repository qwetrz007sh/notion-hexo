---
cover: /images/e5bfedf688992135673fb3a1adb00980.png
categories: ''
tags: []
title: Rclone 系列教程 1 挂载 Google Drive | GAKIYUKR'S Blog
date: '2023-11-15 16:58:00'
updated: '2023-12-26 18:46:00'
---

> 前言 因为网上 Rclone 教程鱼龙混杂，有很多都瞎鸡儿写的，很多细节没有兼顾到，这次我正式打算开坑写一个 Rclone 系列教程（从入门到入土），力求讲到每一个细节…


## 前言


因为网上 Rclone 教程鱼龙混杂，有很多都瞎鸡儿写的，很多细节没有兼顾到，这次我正式打算开坑写一个 Rclone 系列教程（从入门到入土），力求讲到每一个细节。


鉴于这是第一期，这里简单介绍一下 Rclone：Rclone 是一个开源的网盘挂载程序，支持全平台，可以把网盘 / 网络驱动器挂载为本地文件夹。可以用来上传下载文件、或者与云端服务备份同步。


官网：[Rclone](https://rclone.org/)


GitHub：[https://github.com/ncw/rclone](https://github.com/ncw/rclone)


服务器建议配置：

1. 1C1G 服务器，可以装一个宝塔或者其他图形化文件管理系统。不需要 GUI 的话 512m 甚至更低也能用。
2. 境外服务器，Rclone 支持的储存大多数是境外，国内机器都连不上，没啥用。
3. 硬盘 io 起码看得过去，比 U 盘还低那就没得玩了。

本文是 Rclone 系列教程第一期，这一期讲解对 Google Drive 的挂载。


## 安装 Rclone


首先，使用 `sudo -i` 切换至 root 用户。


官方给出了一键脚本，无需任何多余的操作。


```text
 curl https://rclone.org/install.sh | sudo bash
```


## 设置 Google Drive API


这是 Google 家的服务，那么需要什么网络环境我相信你也非常明白，Google 都打不开那么你也没有必要继续看了。


首先，打开 [Google API Console](https://console.cloud.google.com/) ，第一次打开的时候是下图的界面：


同意使用协议，然后进行下一步。


![Snipaste_2022-03-16_19-27-04.png](/images/c71f9cf004ecdeb821f7558812230cbd.png)


在最上面的搜索框搜索”Google Drive API“


![Snipaste_2022-03-16_19-28-18.png](/images/6a08b3eee382b57714c5066361731dac.png)


点击 启用，然后点击左边的”OAuth 同意屏幕 “，选择” 外部“


![Snipaste_2022-03-16_19-34-17.png](/images/26b21d18415314361def0ef30a6f70cf.png)


![Snipaste_2022-03-16_19-49-39.png](/images/e8163521ae0c1a316e8cbd6acb4bfd36.png)


必填的应用名称，邮箱自己填好即可。


![Snipaste_2022-03-16_19-50-40.png](/images/3d0433d04cb6541991a409284c2536ce.png)


![Snipaste_2022-03-16_19-51-26.png](/images/19818b359198f00ad811d3ce5821ac31.png)


到了这里，就可以点击左边的 “凭据” 了，然后点击上方的”创建凭据“，选择“OAuth 客户端 ID”。


![Snipaste_2022-03-16_19-56-49.png](/images/2c1913c7f90399e38bccb1d0e511e447.png)


![Snipaste_2022-03-16_19-57-23.png](/images/2747c9ab12f5e7b42a0f797bf5427ce2.png)


![Snipaste_2022-03-16_19-57-35.png](/images/3ebba0b8aa1ee61a4126772ef803f58a.png)


这里的类型可以随便选，名称也可以随便写。


![Snipaste_2022-03-16_19-57-55.png](/images/236fd4b68841e95bd7b15a263e18f3b6.png)


![Snipaste_2022-03-16_20-41-13.png](/images/70f94a3d26a974807e5f532aaf6c35e7.png)


生成的 ID 和密钥保留备用。


![Snipaste_2022-03-16_20-42-17.png](/images/1a964847156dec5cdd7a53bb4ede882e.png)


## 编辑 Rclone 配置文件


安装完成 Rclone 后，使用`rclone config` 命令设置配置文件，按 N 新建一个配置文件，然后设置配置文件名称（这里很重要，不要瞎吉儿写）


这里选择你的储存，Google Drive 这里是 16，我们就输入 16 按回车。（这个数字会变的，不要直接照抄）


![Snipaste_2022-03-16_21-08-11.png](/images/63762f7ea6f8f0d3d196fc0513d9b6d0.png)


client_id 写你前面在 Google API Console 网页上获取的 ID。


![Snipaste_2022-03-16_21-25-22.png](/images/2302f5e3f4309d6830e93a4a6a51d24f.png)


client_secret 同理。


![Snipaste_2022-03-16_21-42-05.png](/images/798689c1908f4a3d9f26f4f83eff0d8d.png)


这个地方输入 1，给予全部权限。


)


![Snipaste_2022-03-16_21-59-24.png](/images/293accbb93ec87f912e65a8e5b3de6bd.png)


root_folder_id 填写文件夹的唯一 ID，留空挂载根目录。


唯一 ID 的格式为该文件夹网页链接`folders`后的部分。


service_account_file 啥都不写，直接回车。


![Snipaste_2022-03-16_22-06-53.png](/images/f0e7e5b8a0033a787b49d6e693e8df2f.png)


![Snipaste_2022-03-16_22-23-45.png](/images/b267825315f15822588e5a90045d74e4.png)


这里重点来了：**两个选项一定都要选 N，手动配置**，因为你的命令行 Linux 没有浏览器，自动配置是用不了的。（GUI 用户当我放屁）


然后打开授权网址，登录账号。将网站返回的密钥填回 ssh 中，回车确认。


![Snipaste_2022-03-16_22-34-25.png](/images/6cc911964263352f4c48e15c380d434f.png)


![Snipaste_2022-03-16_22-37-52.png](/images/19512398f2d6860ee166801ec6fdfa0a.png)


![Snipaste_2022-03-16_22-38-07.png](/images/ae0d52aa190d94250fcb1bdca3877c0c.png)


这里询问你是否为团队盘，根据自己情况选择。


![Snipaste_2022-03-16_22-38-21.png](/images/663d60a0f7e378e48fb6c06191b611b1.png)


![Snipaste_2022-03-16_22-45-55.png](/images/738999a347561850bfc87bddcb541cde.png)


然后会询问你是否保存，按 Y 保存配置。


到这里，就可以按 Q 退出编辑配置文件了。


![Snipaste_2022-03-16_22-46-21.png](/images/c86bebec15f78a199df890e1e894b175.png)


![Snipaste_2022-03-16_22-47-06.png](/images/1191524f0042051770e3547468bf7eea.png)


## 挂载至文件夹


首先，你需要创建一个空文件夹，用于挂载的目录。（这里就是我为什么推荐装个宝塔等 GUI 的文件管理了，当然你用 SFTP 等也完全没问题）


使用命令：**（除了两个括号和里面的内容，一个空格和符号也不能删）**


```text
 rclone mount 配置文件名称（按照我演示的话就是gd）: /home/gdrive \ （这里的这个文件夹需要提前创建好，并保持为空）
  --umask 0000 \
  --default-permissions \
  --allow-non-empty \
  --allow-other \
  --buffer-size 32M \
  --dir-cache-time 12h \
  --vfs-read-chunk-size 64M \
  --vfs-read-chunk-size-limit 1G &
```


看到下面的提示则配置成功。


![Snipaste_2022-03-16_22-55-15.png](/images/8d0e27952e371c2d1459a5fb85d06859.png)


然后去目标文件夹看看，可以发现正常显示 Google Drive 的文件了。


> 本文由简悦 SimpRead 转码


![Snipaste_2022-03-16_22-59-27.png](/images/b27051852a8247cff0975ee7467bb84a.png)

