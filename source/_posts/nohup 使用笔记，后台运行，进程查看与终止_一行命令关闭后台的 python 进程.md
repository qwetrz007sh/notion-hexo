---
cover: /images/515fa3b824f29f7666e1931557029d1f.png
categories: linux
tags:
  - linux命令
title: nohup 使用笔记，后台运行，进程查看与终止_一行命令关闭后台的 python 进程
date: '2023-12-26 11:10:00'
updated: '2023-12-26 17:10:00'
---

1.[nohup](https://so.csdn.net/so/search?q=nohup&spm=1001.2101.3001.7020)，用途：不挂断地运行命令。


语法：nohup Command [ARG…][]


```text
nohup python a.py
nohup bash start.sh
```


无论是否将 [nohup 命令](https://so.csdn.net/so/search?q=nohup%E5%91%BD%E4%BB%A4&spm=1001.2101.3001.7020)的输出重新定向到终端，输出都将附加到当前目录的 nohup.out 文件中；


如果当前目录中的 nohup.out 文件不可写，输出重定向到 $HOME/nohup.out 文件当中。


如果没有文件能创建或打开以用于追加，那么 Command 参数指定的命令不可调用。


退出状态：该命令返回下列出口值：


126 可以查找但不能调用 Command 参数指定的命令。


127 nohup 命令发生错误或不能查找由 command 参数指定的命令


否则，nohup 命令的退出状态是 command 参数指定命令的退出状态。

1. &

用途：在后台运行


一般两个一起用


```text
nohup command &
nohup /usr/local/node/bin/node /www/im/chat.js >> /usr/local/node/output.log 2>&1 &
```


查看运行的后台进程

1. 

```text
ps -ef
ps -aux|grep python
```


jobs 命令只看当前终端生效的，关闭终端后，在另一个终端 jobs 已经无法看到后台跑的程序了，此时利用 ps(进程查看命令）


2）


```text
ps -ef
ps -aux|grep python
```


![cd54a329a31640a4b3e123e543aea54b.png](/images/515fa3b824f29f7666e1931557029d1f.png)


a: 显示所有程序


u: 以用户为主的格式来显示


x: 显示所有程序，不以终端机来区分


注意：


用 ps -def | grep 查找进程很方便，最后一行总是会 grep 自己


用 grep -v 参数可以将 grep 命令排除掉


![5518f894f0914978acd26fe70088d354.png](/images/bf639702c58e3abaf91eb596ba7693ea.png)


再用 awk 提取一下进程 ID


![4bccea218d194d7a99dcd5126b585de8.png](/images/0ba4fef58e70f1e041ea4e3f7b1761e3.png)


查看到进程 id 后， 使用 kill 杀掉进程


```text
kill -9 9488
```


杀掉所有 python 进程


```text
killall python
```


> 本文由简悦 SimpRead 转码

