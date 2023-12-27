
# 【Docker系列】一个反向代理神器——Nginx Proxy Manager


【Docker系列】一个反向代理神器——Nginx Proxy Manager


```text
sudo -i # 切换到 root 用户
apt update -y  # 升级 packages
apt install wget curl sudo vim git -y  # Debian 系统比较干净，安装常用的软件
```


# 安装 Docker 环境


## 安装 Docker（非大陆服务器）


curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh wget -qO- get.docker.com | bash curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh docker -v


 #查看 docker 版本 


systemctl enable docker 


# 设置开机自动启动


## 安装 Docker-compose（非大陆服务器）


### amd


```javascript
sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-uname -s-uname -m > /usr/local/bin/docker-compose
```


国内用户可以使用以下方式加快下载


```javascript
sudo curl -L https://download.fastgit.org/docker/compose/releases/download/1.27.4/docker-compose-uname -s-uname -m > /usr/local/bin/docker-compose
```


### arm


```text
sudo curl -L "https://github.com/docker/compose/releases/download/v2.1.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```


---


```text
sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version  #查看 docker-compose 版本
```


如果报错：/usr/local/bin/docker-compose: line 1: Not: command not found 运行：


```text
sudo rm /usr/local/bin/docker-compose
sudo pip uninstall docker-compose

then
sudo pip install docker-compose

then
docker-compose --version

```


docker安装


---


```text
sudo docker run --rm -v /usr/local/bin:/dist gists/docker-compose-bin:latest && sudo docker rmi gists/docker-compose-bin:latest
sudo chmod +x /usr/local/bin/docker-compose
```


### 要注明的文本


修改 Docker 配置（可选） 内容参考：烧饼博客 以下配置会增加一段自定义内网 IPv6 地址，开启容器的 IPv6 功能，以及限制日志文件大小，防止 Docker 日志塞满硬盘（泪的教训）： ​


```text
cat > /etc/docker/daemon.json <<EOF
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
EOF
```


​ ## 然后重启 Docker 服务： systemctl restart docker ​ # 安装 Nginx Proxy Manager


创建安装目录 创建一下安装的目录：


```text
mkdir -p /root/docker_data/npm && cd /root/docker_data/npm
```


来到 dockercompose 文件所在的文件夹下


这边我们直接用 docker 的方式安装。


```text
cat > /root/docker_data/npm/docker-compose.yml <<EOF
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '808:80'  # 冒号左边可以改成自己服务器未被占用的端口
      - '818:81'  # 冒号左边可以改成自己服务器未被占用的端口
      - '4438:443' # 冒号左边可以改成自己服务器未被占用的端口
    volumes:
      - ./data:/data # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 data 文件夹中
      - ./letsencrypt:/etc/letsencrypt  # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 letsencrypt 文件夹中
EOF
```


之后，


```text
docker-compose up -d
```


理论上我们就可以输入 http://ip:818 访问了。  默认登陆名和密码： 

