---
cover: /images/cd06cc1f3140050aa3747a717d0b7eca.png
categories: notion
tags: []
title: Rclone 系列教程 4 挂载 Onedrive | GAKIYUKR'S Blog
date: '2023-11-15 15:23:00'
updated: '2023-12-26 18:46:00'
---

> 本文讲述 Rclone 及其衍生产品挂载 OneDrive/SharePoint 教程。 创建应用程序（可选） 也可以使用 Rclone 官方的公用 API ，但是出于速度和稳定性的考虑，不…


本文讲述 Rclone 及其衍生产品挂载 OneDrive/SharePoint 教程。


## 创建应用程序（可选）


也可以使用 Rclone 官方的公用 API ，但是出于速度和稳定性的考虑，不建议这样做。


去 AAD 中自建一个 API 能解决后期很多莫名其妙的不必要的问题。


如果你的子账号并没有 AAD 访问权限，那么也就没办法了，只能 WebDev 曲线救国或者使用公用 API 。


不做仔细的文字介绍了，看图即可。


![image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)


![image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)


![image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)


![image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)


![image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)


![image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)


![image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)


记得需要代表组织同意。


![977955f15382ce2c6039ca08c69545d31258294862.png](/images/e8d21c686f5b5c385ea53906f1395ed4.png)


## 配置文件


需要一台 Windows、或者是带 浏览器 + GUI 的 Linux 设备，才能在线授权获取令牌，微软不支持手动授权获取令牌。


可以在 Windows 配置完成后，直接将配置文件内容复制到 Linux 设备。


Windows 整体操作上与 Linux 一致，可以直接照着以前的经验使用。


```text
 D:\rclone-v1.62.2>rclone config
 Current remotes:
 ​
 Name                 Type
 ====                 ====
 ​
 e) Edit existing remote
 n) New remote
 d) Delete remote
 r) Rename remote
 c) Copy remote
 s) Set configuration password
 q) Quit config
 e/n/d/r/c/s/q> n
 ​
 Enter name for new remote.
 name> 配置文件名称
 ​
 Option Storage.
 Type of storage to configure.
 Choose a number from below, or type in your own value.
  1 / 1Fichier
    \ (fichier)
  2 / Akamai NetStorage
    \ (netstorage)
  3 / Alias for an existing remote
    \ (alias)
  4 / Amazon Drive
    \ (amazon cloud drive)
  5 / Amazon S3 Compliant Storage Providers including AWS, Alibaba, Ceph, China Mobile, Cloudflare, ArvanCloud, DigitalOcean, Dreamhost, Huawei OBS, IBM COS, IDrive e2, IONOS Cloud, Liara, Lyve Cloud, Minio, Netease, RackCorp, Scaleway, SeaweedFS, StackPath, Storj, Tencent COS, Qiniu and Wasabi
    \ (s3)
  6 / Backblaze B2
    \ (b2)
  7 / Better checksums for other remotes
    \ (hasher)
  8 / Box
    \ (box)
  9 / Cache a remote
    \ (cache)
 10 / Citrix Sharefile
    \ (sharefile)
 11 / Combine several remotes into one
    \ (combine)
 12 / Compress a remote
    \ (compress)
 13 / Dropbox
    \ (dropbox)
 14 / Encrypt/Decrypt a remote
    \ (crypt)
 15 / Enterprise File Fabric
    \ (filefabric)
 16 / FTP
    \ (ftp)
 17 / Google Cloud Storage (this is not Google Drive)
    \ (google cloud storage)
 18 / Google Drive
    \ (drive)
 19 / Google Photos
    \ (google photos)
 20 / HTTP
    \ (http)
 21 / Hadoop distributed file system
    \ (hdfs)
 22 / HiDrive
    \ (hidrive)
 23 / In memory object storage system.
    \ (memory)
 24 / Internet Archive
    \ (internetarchive)
 25 / Jottacloud
    \ (jottacloud)
 26 / Koofr, Digi Storage and other Koofr-compatible storage providers
    \ (koofr)
 27 / Local Disk
    \ (local)
 28 / Mail.ru Cloud
    \ (mailru)
 29 / Mega
    \ (mega)
 30 / Microsoft Azure Blob Storage
    \ (azureblob)
 31 / Microsoft OneDrive
    \ (onedrive)
 32 / OpenDrive
    \ (opendrive)
 33 / OpenStack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
    \ (swift)
 34 / Oracle Cloud Infrastructure Object Storage
    \ (oracleobjectstorage)
 35 / Pcloud
    \ (pcloud)
 36 / Put.io
    \ (putio)
 37 / QingCloud Object Storage
    \ (qingstor)
 38 / SMB / CIFS
    \ (smb)
 39 / SSH/SFTP
    \ (sftp)
 40 / Sia Decentralized Cloud
    \ (sia)
 41 / Storj Decentralized Cloud Storage
    \ (storj)
 42 / Sugarsync
    \ (sugarsync)
 43 / Transparently chunk/split large files
    \ (chunker)
 44 / Union merges the contents of several upstream fs
    \ (union)
 45 / Uptobox
    \ (uptobox)
 46 / WebDAV
    \ (webdav)
 47 / Yandex Disk
    \ (yandex)
 48 / Zoho
    \ (zoho)
 49 / premiumize.me
    \ (premiumizeme)
 50 / seafile
    \ (seafile)
 Storage> 31
 ​
 Option client_id.
 OAuth Client Id.
 Leave blank normally.
 Enter a value. Press Enter to leave empty.
 client_id> xxxxxx    （这里是应用程序 ID）
 ​
 Option client_secret.
 OAuth Client Secret.
 Leave blank normally.
 Enter a value. Press Enter to leave empty.
 client_secret> xxxxxx    （这里是客户端密码）
 ​
 Option region.
 Choose national cloud region for OneDrive.
 Choose a number from below, or type in your own string value.
 Press Enter for the default (global).（这里选择你的 OneDrive 版本 1是普通的国际版 4是世纪互联，美国政府和德国版和我们没啥关系）
  1 / Microsoft Cloud Global
    \ (global)
  2 / Microsoft Cloud for US Government
    \ (us)
  3 / Microsoft Cloud Germany
    \ (de)
  4 / Azure and Office 365 operated by Vnet Group in China
    \ (cn)
 region> 1
 ​
 Edit advanced config?
 y) Yes
 n) No (default)
 y/n>
 ​
 Use web browser to automatically authenticate rclone with remote?
  * Say Y if the machine running rclone has a web browser you can use
  * Say N if running rclone on a (remote) machine without web browser access
 If not sure try Y. If Y failed, try N.
 ​
 y) Yes (default)
 n) No
 y/n>
 ​
 2023/04/16 17:39:57 NOTICE: Make sure your Redirect URL is set to "http://localhost:53682/" in your custom config.
 2023/04/16 17:39:57 NOTICE: If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=xkQEcXCi_IkqAey6usr2Qg
 2023/04/16 17:39:57 NOTICE: Log in and authorize rclone for access
 2023/04/16 17:39:57 NOTICE: Waiting for code...
 2023/04/16 17:40:38 NOTICE: Got code
 Option config_type.
 Type of connection
 Choose a number from below, or type in an existing string value.
 Press Enter for the default (onedrive).（选择你要挂载的内容，1是该账户的 OneDrive，2是该全局的 Sharepoint 主站点，3是选择一个该全局 Sharepoint 下的站点，4是手动输入你有权访问的 Sharepoint 站点）
  1 / OneDrive Personal or Business
    \ (onedrive)
  2 / Root Sharepoint site
    \ (sharepoint)
    / Sharepoint site name or URL
  3 | E.g. mysite or https://contoso.sharepoint.com/sites/mysite
    \ (url)
  4 / Search for a Sharepoint site
    \ (search)
  5 / Type in driveID (advanced)
    \ (driveid)
  6 / Type in SiteID (advanced)
    \ (siteid)
    / Sharepoint server-relative path (advanced)
  7 | E.g. /teams/hr
    \ (path)
 config_type> 1
 ​
 Option config_driveid.
 Select drive you want to use
 Choose a number from below, or type in your own string value.
 Press Enter for the default (xxxxxx).
  1 / OneDrive (business)
    \ (xxxxxx)
 config_driveid> 1
 ​
 Drive OK?
 ​
 Found drive "root" of type "business"
 URL: https://xxxxxx-my.sharepoint.com/personal/xxxxxx/Documents
 ​
 y) Yes (default)
 n) No
 y/n> y
 ​
 Configuration complete.
 Options:
 - type: onedrive
 - client_id: xxxxxx
 - client_secret: xxxxxx
 - token: {"access_token":"xxxxxx"}
 - drive_id: xxxxxx
 - drive_type: business
 Keep this "odmjjembyadmin" remote?
 y) Yes this is OK (default)
 e) Edit this remote
 d) Delete this remote
 y/e/d> y
 ​
 Current remotes:
 ​
 Name                 Type
 ====                 ====
 配置文件名称           onedrive
 ​
 e) Edit existing remote
 n) New remote
 d) Delete remote
 r) Rename remote
 c) Copy remote
 s) Set configuration password
 q) Quit config
 e/n/d/r/c/s/q> q
```


## 将配置文件迁移到其他设备


使用命令：


```text
 rclone config file
```


即可看到当前配置文件位置，将你在 Windows 配置好的文件，直接复制到 Linux 设备即可使用。 > 本文由简悦 SimpRead 转码

