---
cover: /images/957bb3bec66723aab49d69ec5bd821b1.webp
categories: linux
tags:
  - linux命令
title: Linux rsync 命令同步文件与目录文件夹
date: '2023-11-16 10:40:00'
updated: '2023-12-27 10:21:00'
---

> Rsync 用于在两个远程计算机之间同步文件和文件夹。它仅通过传输源和目标之间的差异来提供快速的增量文件传输


Rsync 用于在两个远程计算机之间同步文件和文件夹。它仅通过传输源和目标之间的差异来提供快速的增量文件传输。


Rsync 可用于镜像数据，增量备份，在系统之间复制文件，可替代 [`scp`](https://www.myfreax.com/how-to-use-scp-command-to-securely-transfer-files/)，[`sftp`](https://www.myfreax.com/how-to-use-linux-sftp-command-to-transfer-files/)和 [`cp`](https://www.myfreax.com/cp-command-in-linux/)日常等使用的命令。


`rsync`命令已预安装在大多数 Linux 发行版和 macOS。可以运行命令`rsync --version`检查是否已安装 rysnc，命令将会打印 rysnc 的版本号`rsync  version 3.01`。


## 安装 rsync


如果终端提示你 bash: command not found: rysnc，说明你的系统没有安装 rsync。可以使用发行版的软件包管理器安装 rysnc。


如果你的计算机运行的系统是基于 Debian 的 Linux 发行版，例如 Ubuntu，Linux mint 请运行命令`sudo apt install rsync`安装 Rsync。


如果你的计算机运行的系统是基于 RedHat 的 Linux 发行版。例如 CentOS，Fedora。请运行命令`sudo yum install rsync`安装 Rsync。


## Rsync 命令


`rsync`语法有三种，分别是本地到本地形式 Local to Local，本地到远程 Local to Remote，远程到本地 Remote to Local。


其中`OPTION`是 rsync 选项。`SRC`是源目录。`DEST`是目标目录。`USER`是远程用户名。`HOST`是远程主机名名称，可以是 IP 地址或者可解释的域名。


```text
Local to Local:  rsync [OPTION]... [SRC]... DEST
Local to Remote: rsync [OPTION]... [SRC]... [USER@]HOST:DEST
Remote to Local: rsync [OPTION]... [USER@]HOST:SRC... [DEST]
```


`rsync`提供了许多控制其行为的选项。以下是最经常使用的选项。

- `a`/`-archive`存档模式，等效于`rlptgoD`。此选项指示`rsync`递归同步目录，传输特殊设备和块设备，保留符号链接，组，所有权和权限等。
- `z`/`-compress`，此选项将强制`rsync`在数据发送给目标计算机之前对数据进行压缩。
- `P`等效于`-partial --progress`。使用此选项时，`rsync`将在传输过程中显示进度条并保留部分传输的文件。在慢速或不稳定的网络连接传输大文件时非常有用。
- `-delete`使用此选项时，`rsync`将从目标位置删除相同的文件。适合用于镜像文件。
- `q`/`-quiet`此选项禁止显示非错误消息。`e`此选项使您可以选择其他远程 shell 程序。默认使用 ssh。

## Rsync 基础


rysnc 最简单的用法就是在本地的目录之间复制文件。运行 rysnc 命令的用户必须对源目录或者文件具有读取权限，并且对目标目录具有写入权限。


如果目标参数未指定文件名，rsync 将会保留原始文件名称。要使用其它文件名称保复制文件，请在目标参数指定文件名。


值得一提的是`rsync`命令会根据源目录是否使用斜杠`/`，而又不同的处理方式。


如果在源目录尾部添加斜杠，`rsync`会将目录的内容复制到目标目录。在省略斜杠，`rsync`则会将源目录复制到目标目录。


```text
rsync -a /opt/filename.zip /tmp/newfilename.zip

rsync -a /var/www/domain.com/public_html/ /var/www/domain.com/public_html_backup/
```


## Rsync 远程同步数据


当使用`rsync`进行远程传输时，rsync 必须安装在源计算机和目标计算机。`rsync`默使用 SSH 作为远程 shell 程序。


如果您尚未为远程计算机[设置 SSH 无密码登录](https://www.myfreax.com/how-to-setup-passwordless-ssh-login/)，`rsync`会要求您输入用户名和密码。远程计算机 SSH 服务监听的端口不是默认端口 22 时，请使用`-e`选项指定端口。


当传输大量数据或者大文件时，建议在 [screen](https://www.myfreax.com/how-to-use-linux-screen/)，[nohup](https://www.myfreax.com/linux-nohup-command/)，[tmux](https://www.myfreax.com/getting-started-with-tmux/) 运行`rsync`命令或使用 rsync 命令的`-P`选项。


## Rsync 增量备份 / 更新 / 复制


在增量复制或者备份时，强烈建议使用`-t`选项，该选项用与保持文件的 mtime 属性不变。mtime 是文件的修改时间。


如果没有指定`-t`选项时，目标文件 mtime 属性会设置为系统时间，导致下次更新检测到 mtime 不同，从而导致增量更新无效。


通常你可能还需要显示 rsync 同步过程的详细信息，使用`-v`选项。确认是否正确实现增量同步。


对于同步大量的数据或者大文件，`rsync`命令的`-P`选项可以显示进度并保留部分传输的文件。


remote_user 是远程计算机的用户名，remote_host_or_ip 远程计算机的 IP 地址或者可解释的域名。


```text
rsync -avtP /opt/media/ remote_user@remote_host_or_ip:/opt/media/
```


## Rsync 同步本地目录到远程计算机


```text
rsync -a /opt/media/ remote_user@remote_host_or_ip:/opt/media/
```


## Rsync 同步远程计算机目录到本地目录


```text
rsync -a remote_user@remote_host_or_ip:/opt/media/ /opt/media/
```


## Rsync 指定 SSH 端口


```text
rsync -a -e "ssh -p 2322" /opt/media/ remote_user@remote_host_or_ip:/opt/media/
```


## Rsync 后台同步数据


```text
rsync -a -P remote_user@remote_host_or_ip:/opt/media/ /opt/media/
```


## 排除文件和目录


[当你要](https://www.myfreax.com/how-to-exclude-files-and-directories-with-rsync/)[排除文件或目录](https://www.myfreax.com/how-to-exclude-files-and-directories-with-rsync/)时，您需要使用源目录的相对路径。有两种方式可以排除文件和目录。


第一种方式是使用`rsync`命令的`--exclude`选项，在命令行指定要排除的文件和目录。`--exclude`选项可以重复使用多次排除多个文件与目录。


第二种方式是使用`rsync`命令的`--exclude-from`选项并指定一个文件，该文件包含要排除的目录与文件的路径。


在以下示例中，排除`src_directory`目录的`node_modules`和`tmp`目录，也就是目录`src_directory/node_modules`，`src_directory/tmp`。


```text
rsync -a --exclude=node_modules --exclude=tmp /src_directory/ /dst_directory/
```


```text
rsync -a --exclude-from='/exclude-file.txt' /src_directory/ /dst_directory/
```


```text
node_modules
tmp
```


/exclude-file.txt


## 结论


在本教程中，您学习了如何在 Linux 使用 Rsync 命令复制和同步文件和目录。如有任何疑问，请随时发表评论。 

