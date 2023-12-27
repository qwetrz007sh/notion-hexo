
# 已弃用更换alist了


# zfile文件列表


## 2. 项目展示


### 2.1 [前台首页](https://docs.zfile.vip/#/README?id=%E5%89%8D%E5%8F%B0%E9%A6%96%E9%A1%B5)


### 2.2[图片预览](https://docs.zfile.vip/#/README?id=%E5%9B%BE%E7%89%87%E9%A2%84%E8%A7%88)


### 2.3 [视频预览](https://docs.zfile.vip/#/README?id=%E8%A7%86%E9%A2%91%E9%A2%84%E8%A7%88)


### 2.4 [文本预览](https://docs.zfile.vip/#/README?id=%E6%96%87%E6%9C%AC%E9%A2%84%E8%A7%88)


### 2.5 [音频预览](https://docs.zfile.vip/#/README?id=%E9%9F%B3%E9%A2%91%E9%A2%84%E8%A7%88)


### 2.6 [后台设置-驱动器列表](https://docs.zfile.vip/#/README?id=%E5%90%8E%E5%8F%B0%E8%AE%BE%E7%BD%AE-%E9%A9%B1%E5%8A%A8%E5%99%A8%E5%88%97%E8%A1%A8)


### 2.7 [后台设置-新增驱动器](https://docs.zfile.vip/#/README?id=%E5%90%8E%E5%8F%B0%E8%AE%BE%E7%BD%AE-%E6%96%B0%E5%A2%9E%E9%A9%B1%E5%8A%A8%E5%99%A8)


### 2.8 [后台设置-站点设置](https://docs.zfile.vip/#/README?id=%E5%90%8E%E5%8F%B0%E8%AE%BE%E7%BD%AE-%E7%AB%99%E7%82%B9%E8%AE%BE%E7%BD%AE)


mkdir -p /root/docker_data/zfile && cd /root/docker_data/zfile


创建 docker-compose.yml 编辑输入：


```text
version: '3.3'
services:
    zfile:
        container_name: zfile
        restart: always
        ports:
            - '18080:8080'    # 左边的端口可以修改，右边的端口不要修改
        volumes:
            - './db:/root/.zfile-v4/db'      # 数据库映射到当前文件夹下的db目录
            - './logs:/root/.zfile-v4/logs'  # 日志文件映射到当前文件夹下的logs目录
            - './data:/root/.zfile-v4/data'  # 额外映射了一个data目录，等下本地存储我们就可以填/root/.zfile-v4/data目录啦
            - './application.properties:/root/application.properties'
        image: zhaojun1998/zfile
```


保存后运行


```text
curl -o /root/docker_data/zfile/application.properties *https://c.jun6.net/ZFILE/application.properties*
```


再运行


```text
docker-compose up -d
```


### 5.2 更新


```shell

cp -r /root/docker_data/zfile /root/data/docker_data/zfile.archive  # 万事先备份，以防万一

cd /root/docker_data/zfile  # 进入docker-compose所在的文件夹

docker-compose pull    # 拉取最新的镜像

docker-compose up -d   # 重新更新当前镜像
```


利用Docker-compose搭建的应用，更新非常容易～


### 5.3 卸载


```shell
cd /root/data/docker_data/zfile  # 进入docker-compose所在的文件夹

docker-compose down    # 停止容器，此时不会删除映射到本地的数据

rm -rf /root/data/docker_data/zfile  # 完全删除映射到本地的数据
```

