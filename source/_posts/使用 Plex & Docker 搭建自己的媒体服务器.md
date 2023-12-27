---
cover: /images/ac32102899d496319072be46e481edce.png
categories: 影视库搭建
tags:
  - emby
title: 使用 Plex & Docker 搭建自己的媒体服务器
date: '2023-11-17 15:37:00'
updated: '2023-12-27 12:22:00'
---

Plex 是 Plex，Inc. 制造的全球流媒体服务和客户端服务器媒体播放器平台。PlexMedia Server 整理用户收藏和在线服务中的视频，音频和照片，并将其流式传输到播放器。尽管提供多种媒体服务， 我这里的需求只是流媒体播放我的本地音乐集~还有 R 音声作品~而已，所以关于后面的媒体配置相关都是关于**音乐** 的， 使用 **Docker** 和 [**linuxserver**](https://www.linuxserver.io/) 提供的 [**镜像**](https://hub.docker.com/r/linuxserver/plex) 搭建 Plex Server 非常简单


部分截图：


![fabda89ee1c16fd16c8bb9cfb888630e2799769371dea05af185f66d966c25ee.png](/images/c35add514ed4461c48e9fa4713661235.png)


Music Library


![7485070ceb472b3caba56247548f8d0a845acc17b3584c778e84b4dce04ff37c.png](/images/3e977d3a034dfb06285fac158da1edaf.png)


Music Album


![b8029c79d3fc2b196d7a3b972312f28bb7fd546cd1407008066ef7dd183ad99f.png](/images/d5e10c6a9ac1ab408441ff480c7c9ba0.png)


Playing Music


![3a9b2c3aa5da7d45fe2ead436f4b2031b0e4a45e54f37cea8ac221ad3ac6a258.png](/images/e26c178a4849c902c3386c85b8ba23c8.png)


Music Artist


相比与同类型的开源选择 _Jellyfin_， 界面更加漂亮，依赖于大公司的技术使用体验也更加好，接下来我们将要使用 Docker 搭建自己的服务


你需要准备的东西有

- NAS 设备 (树莓派 4B 即可) + 公网 IP / 内网穿透 / 土豪直接买云服务器
- 了解`docker` 和 `docker-compose`工具，命令配置的基本使用

## 搭建 Plex Server


新建文件夹`plex`， 创建`docker-compose.yml`


```text
mkdir plex && cd plex
touch docker-compose.yml
```


写入下面的配置


```text
version: "2.1"
services:
  plex:
    image: linuxserver/plex
    container_name: plex
    environment:
      - PUID=1000
      - PGID=1000
      - VERSION=docker
      - PLEX_CLAIM=your-claim
    volumes:
      - ./config:/config
      - /path/to/your/music/library:/musics:ro
    restart: unless-stopped
    mem_limit: 700m
    memswap_limit: 2000m
    devices:
      - /dev/dri:/dev/dri
    ports:
      - 32400:32400
```


需要注意的几点:

- `PUID`和`PGID`用于配置容器内进程的`UID`和`GID`， 全都设置为 0 表示以`root`用户运行，如果你这里不是很明白的话可以无脑设置为 0 以避免部分权限问题
- `mem_limit`和`memswap_limit`属于可选项， 如果你 (我) 使用 **2GB** 内存的树莓派你可能会担心超出内存而出现的问题 (一般出现在扫描媒体时， 日常占用并不大， **2GB** 内存足矣)，如果你的内存充足的话，完全可以忽略这两个配置项
- `PLEX_CLAIM`环境变量用于认证自己的服务器，也是可选， 你可以从 [**这里**](https://www.plex.tv/claim/) 获取 (注意需要可用的 plex 账号)， 另外 claim 的有效期一般只有 **4** 分钟 ，如果服务器网络不佳，建议先通过执行`docker-compose pull`拉取镜像之后再获取，防止过期 (虽说过期后再重新 claim 也行)
- `/path/to/your/music/library:/musics:ro` 将自己本地的音乐库映射到 plex container 的`/musics`目录， 并且只读 (`ro`即`read only`)，你可以将自己用`nextcloud`或`syncthing`同步过来的曲目映射到这里

执行`docker-compose up`然后静待服务器启动完成， 启动完成后可以访问`ip:32400/web`进入 web 界面


检查你的远程访问有没有正确开启， 成功的示例如下


![366d28e5742c47e2a1f31ede1d1262bd4be30f60f9d60dddbff2ec0a980b60ec.png](/images/09f8182314a241cbdfded80f24ab71e9.png)


由于我端口映射并非标准的`32400`端口， 所以这里手动指定了一下自己的端口


如果已经成功了的话， 你就可以在 [plex 官网](https://app.plex.tv/desktop/#!/)看到自己的 server 了， 接下来你可以按照提示设置自己的媒体库， 至于音乐库相关的高级选项可以参考下面的解释


![e528ecde5075c1ef3943a2a98f76fa34416a5d93320cd6cef5c35169e596f948.png](/images/af9c78ef7b354874e9b740c761782ad7.png)


移动端示例截图


![64396c683dd97e8d514fda8aa15df49643f2d37cbbf7b58140d5314a09cef07b.png](/images/afe94ad0719e3ee262f3267f4480f61e.png)


![150947a649f8c47b06fe5a4067fe1fb770e41572e17ceceae6f21477e7501f4b.png](/images/7206af500d5ec6b2d7ef243747d89c53.png)


## 音乐相关高级配置解释


扫描器和代理这两个选项不用考虑， 默认就行， 其中`Plex Music`是最近几个版本更新的扫描器， 性能更好，扫描速度明显快于旧的`Plex Music Scanner`


![8d21f1e9364459c56d52abe125992eb4ed4ce140af6e1f51211cdaaf4a2e46b5.png](/images/a30201dabcefc6fc29bff8207bf5dac6.png)


从网易云下载的曲子一般会自带正确的`ID3`**tag**， 我们这里可以直接选中`Prefer local metadata` ， 让扫描器优先使用自带的 **tag**.


`Artist Bios` 歌手信息。推荐保持选中， 我这里没有选中是因为我歌单里面太多冷门**优秀**歌手 / 作曲比如 `Acyanxi`， plex 拉取不到这些歌手 / 作曲的信息，导致后面只有一小部分歌手有封面而多数都没有， 强迫症的我直接把这个选项给关了。


`Album Reviews and Critic Ratings` ， `Popular Tracks` 和`Concerts` 歌曲评价相关，热门曲目和演唱会信息 我个人认为没有必要没有打开， 你可以根据自己的情况选择， 我没有打开， 毕竟热门的并不代表适合自己， 演唱会又不能去 (没钱买机票)， 我觉得好听的就是最好的， 看 xx 评价


`Genres` 注意网易云给的 **tag** 中的`Genres`都是空的， 如果你认为音乐风格对你来说有必要可以保持默认的`Plex Music`选项，虽然并不保证一定能得到


`Album Art`专辑封面， 网易云提供了， 所以我直接选择了`Local Files Only`


![f6c66d67b4121e14022e3339e3d1ff1a82db9ebce31ae466ea37edf3b408a5a2.png](/images/e99589b6ac1382f4a16e820725253a2e.png)


Scanning


## 自用脚本分享 - 转移本地音乐到 Plex


### NCM 文件批量解码


```text
import os
from ncmdump import dump
from pathlib import Path

source_dir = Path(r"F:\Musics\netease")

def output_path(input_path: Path, metadata) -> str:
    return (input_path.parent /
            (input_path.stem + "." + metadata['format'])).__str__()

if __name__ == '__main__':
    for file in source_dir.iterdir():
        if file.name.endswith('.ncm'):
            print(str(file))

            dump(file, output_path, skip=True)
            os.remove(file)
```


`ncmdump`模块来自 [https://github.com/nondanee/ncmdump](https://github.com/nondanee/ncmdump)， 你可以通过执行下面的命令安装依赖和`ncmdump`


```text
pip install pycryptodome mutagen
pip install git+https://github.com/nondanee/ncmdump.git
```


### 删除重复文件


网易云音乐下载的曲目有可能会出现重复下载的情况， 一般情况下重复下载的曲目文件名称有比较明显的特征，例如`XX - XXX (1).mp3`， 可以通过这个特征删除掉重复的下载的曲目


```text
from pathlib import Path
import re
import os

source_dir = Path(r"F:\Musics\netease")
target = re.compile(r"^.+\([1-9]\)\.(mp3|ncm)$")

if __name__ == '__main__':
    for file in source_dir.iterdir():
        if not file.name.endswith((".mp3", "ncm")):
            continue

        if target.match(file.name):
            print(file)
            os.remove(file)
```


### 目录转移（Plex 推荐格式）


plex 推荐了音乐相关的目录格式为


```text
Music/ArtistName/AlbumName/TrackNumber - TrackName.ext
```


例如


```text
/Music
   /Pink Floyd
      /Wish You Were Here
         01 - Shine On You Crazy Diamond (Parts I-V).m4a
         02 - Welcome to the Machine.mp3
         03 - Have a Cigar.mp3
   /Foo Fighters
      /One By One
      /There is Nothing Left to Lose
   /U2
      /Joshua Tree
```


如果一首曲子存在多个`artists`， plex 推荐使用 `Various Artists`作为`ArtistName`， 搞不清理由，由于我下载的曲目太多是多艺术家创作的， 都放在这样的文件夹里让我感觉很乱，因此并没有遵守这个推荐规则


为了后面新增曲目时能快速找到哪些是新增的曲目， 我这里随便使用了`sqlite`保存了源文件目录的位置


```text
from pathlib import Path
from mutagen.id3 import ID3
import shutil
import sqlite3

source_dir = Path(r"F:\Musics\netease")
target_dir = Path(r"F:\nextcloud\Musics\NETEASE")
db_path = 'music.db'

def safe_name(name: str) -> str:
    """this function is awful and if you got a better one, just tell me
    """
    return name.replace("/", "／").replace(":", "：").replace(">", "❯").replace(
        "|", " ").replace("<",
                          "❮").replace("\\", " ").replace("?", "？").replace(
                              '"', "'").replace("*", "＊").strip().strip('.')

if __name__ == '__main__':
    con = sqlite3.connect(db_path)
    cur = con.cursor()


    cur.execute('''CREATE TABLE IF NOT EXISTS musics
                (path text NOT NULL PRIMARY KEY)''')

    for source_file in source_dir.iterdir():
        if source_file.name.endswith(".ncm"):
            continue

        source_file = source_file.absolute()
        cur.execute("SELECT * FROM musics WHERE path = ?", [str(source_file)])
        if cur.fetchone():
            continue

        metadata = ID3(source_file)
        artist = source_file.name.split('-')[0].strip().split(',')[0]
        try:
            album = safe_name(metadata.get("TALB").text[0])
        except AttributeError:
            print(source_file, "has no album, just skipped")
            continue

        target_file: Path = target_dir / artist / album / source_file.name

        if target_file.exists():

            cur.execute("INSERT INTO musics VALUES (?)", [str(source_file)])
            con.commit()
            continue

        if not target_file.parent.exists():
            target_file.parent.mkdir(parents=True, exist_ok=True)

        shutil.copy(source_file, target_file)


        cur.execute("INSERT INTO musics VALUES (?)", [str(source_file)])

        con.commit()

    con.close()
```


---


▎本文由 [简悦 SimpRead](https://simpread.pro/) 转码。

