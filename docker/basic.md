## 基础操作

```bash
# 添加用户docker执行权限
$ sudo usermod -aG docker $USER

# 启动一个容器绑定本地8000到容器5000,要求8000端口和name没有被占用
$ docker run -d -t -p 8000:5000 --name demo ubuntu:18.04

# 删除运行中的容器
$ docker rm -f test1

# 在运行中的容器内安装依赖库
$ docker exec demo apt update
$ docker exec demo apt install python3 python3-pip -y
$ docker exec demo pip3 install flask

# 在容器中创建目录
$ docker exec demo mkdir /code

# 复制当前目录下文件到容器中,文件首先需要存在
$ docker cp a.py "demo:/code/a.py"

# 在容器中运行程序
$ docker exec demo python3 /code/a.py

# 执行命令后CTRL+C不会终止程序,程序会在后台执行,需要用stop停止运行
$ docker stop demo

# 使用脚本配置容器依赖

############################
# run.sh
cd /code
python3 a.py
############################

############################
# install.sh
apt update
apt -y install python3 python3-pip
pip3 install flask
############################

$ docker exec demo1 mkdir /code
$ docker cp install.sh "demo1:/code/install.sh"
$ docker cp run.sh "demo1:/code/run.sh"
$ docker cp a.py "demo1:/code/a.py"
$ docker exec demo1 bash /code/install.sh
$ docker exec demo1 bash /code/run.sh

# 启动停止运行的容器
$ docker start demo

# 停止运行的容器
$ docker stop demo

# 查看所有容器
$ docker ps -a

# 删除容器,强制删除运行的容器需要-f
$ docker rm [-f] demo

# 执行bash命令,sh -c 'commands'
$ docker exec demo sh -c 'date > /b.txt'
```

## Dockerfile

```bash
# 自行编写配置文件来创建镜像
########################################
# 在 Dockerfile 文件中 # 是注释
# FROM 用于指定构建镜像使用的基础镜像
FROM ubuntu:18.04
 

# RUN 用于在构建镜像的时候在镜像中执行命令
# 这里我们安装 python3 和 flask web 框架
RUN apt update
RUN apt -y install python3 python3-pip
RUN pip3 install flask


# COPY 相当于命令的 docker cp
# 把本机当前目录下的 app.py 文件拷贝到镜像的 /code/app.py
# 和 docker cp 不同的是，COPY 会自动创建镜像中不存在的目录，比如 /code
COPY app.py /code/app.py


# WORKDIR 用于指定从镜像启动的容器内的工作目录
WORKDIR /code


# CMD 用于指定容器运行后要执行的命令和参数列表
# 这样从本镜像启动容器后会自动执行 python3 app.py 这个命令
#
# 由于我们已经用 WORKDIR 指定了容器的工作目录
# 所以下面的命令都是在 /code 下执行的
CMD ["python3", "app.py"]

# 你可能会看到有资料介绍一个 ENTRYPOINT 参数用于指定容器运行后的入口程序
# 但是这个参数在现在的意义已经很小了，请忽略之
########################################

# Dockerfile构建镜像
# .指明构建镜像时工作目录为当前目录,Dockerfile cp时从.中复制文件到容器
$ docker build -t webimage .

# 查看所有镜像
$ docker images

# 从自定义镜像运行容器
# CTRL+C就退出应用
$ docker run -p 8001:5000 --name demo2 webimage
```

## 服务器安装

```bash
#########################################
# install-docker.sh
# ubuntu1804
# https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce
#########################################
```

## Volumn

```bash
# volume独立于container,用于数据持久化

# 创建volume
$ docker volume create testvolume

# 查看volume
$ docker volume ls

# 删除volume
$ docker volume rm testvolume

# 容器中使用volume
$ docker volume create web

# 数据卷挂载到container /volume
$ docker run -d --name demovolume --mount source=web,target=/volume webimage

# 挂载共享目录,type=bind代表挂载共享目录
$ docker run -p 8080:80 \
    --name nginx1 \
    --mount type=bind,source="${PWD}",target=/usr/share/nginx/html/ \
    nginx

# 单/多文件挂载,文件必须存在,否则认为挂载目录,!挂载必须使用绝对路径!
$ docker run -p 8081:80 \
    --name nginx2 \
    --mount type=bind,source="${PWD}"/index.html,target=/usr/share/nginx/html/test.html \
    --mount type=bind,source="${PWD}"/test.html,target=/usr/share/nginx/html/test1.html \
    nginx
```

## Compose

```bash
# 默认container之间隔离,compose配置虚拟网络以及文件互相访问,一组容器作为项目,容器称为service

# 安装
# https://docs.docker.com/compose/install/
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

$ sudo chmod +x /usr/local/bin/docker-compose

$ docker-compose --version

#############################################################
# docker-compose.yml
# 表示这是 compose 配置文件第三版
version: '3'

# 每个服务都是一个 Docker 容器
# 所以必须用 image 指定服务的镜像名或者从 Dockerfile 中 build 镜像
services:
  pyweb:
    # build 指定了 Dockerfile 所在的路径
    build: .
    # ports 指定暴露的端口，9000 是宿主机，5000 是容器
    # 可以指定多个暴露端口
    ports:
      - "9000:5000"
    # depends_on 设定了依赖，这里 redisdemo 会先于 pyweb 启动
    # 但是如果 redisdemo 启动时间长于 pyweb
    # 那么 pyweb 运行的时候 redisdemo 未必可用
    depends_on:
      - redisdemo

  redisdemo:
    # 每个服务必须用 image 指定镜像名或者从 Dockerfile 中 build
    # 这里用 image 指定镜像，redis:alpine 是 redis 项目的官方 Docker 镜像
    image: "redis:alpine"
#############################################################

#############################################################
# app.py
from flask import Flask
from redis import Redis


app = Flask(__name__)
# redisdemo 是 compose 中创建的主机名，由 docker-compose.yml 中指定
# compose 会给每个容器提供 DNS 服务保证容器间互相访问
redis = Redis(host='redisdemo', port=6379)


@app.route('/')
def index():
    count = redis.incr('hits')
    return 'views {}'.format(count)


if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
#############################################################

# 创建compose项目,-d后台运行
$ docker-compose up [-d]

# 暂停容器运行
$ docker-compose stop

# 关闭并删除项目所有容器
$ docker-compose down

# 修改app.py不会重新构建镜像,因为Dockerfile没有改变
# 强制build
$ docker-compose up --build

# 开发期间通过挂载目录避免多次build

#############################################################
services:
  pyweb:
    build: .
    ports:
      - "9000:5000"
    depends_on:
      - redisdemo
    # volumes 参数把当前目录挂载到容器的 /code
    # docker-compose 的配置中才支持相对路径的挂载
    volumes:
      - .:/code
#############################################################

# 服务器部署

# run.sh
# 重启项目   
cd /tmp/compose33
sudo docker-compose down
sudo docker-compose up -d

# upload-and-run.sh
# 本地同步脚本
# 上传项目到服务器
scp -r . ubuntu@111.231.98.114:/tmp/compose33

# 在服务器重启项目
ssh ubuntu@111.231.98.114 'sh /tmp/compose33/run.sh'
```

## 其他

```bash
# 更换docker源
# https://lug.ustc.edu.cn/wiki/mirrors/help/docker
$ systemctl restart docker

# 容器内的源
# apt -> 帮助 -> 选择对应版本
https://opsx.alibaba.com/mirror

# pip
/root/.pip/pip.conf

#######################################################
# pip.conf
[global]
trusted-host=mirrors.aliyun.com
index-url=https://mirrors.aliyun.com/pypi/simple
#######################################################

#######################################################
# sources.list
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
#######################################################

# npm
https://npm.taobao.org/

#######################################################
# Dockerfile
# 加速镜像创建
FROM ubuntu:18.04

# 使用阿里云的 apt 源
COPY sources.list /etc/apt/sources.list
RUN apt update
RUN apt -y install python3 python3-pip

# 使用阿里云的 pip 源
COPY pip.conf /root/.pip/pip.conf
RUN pip3 install flask


CMD ["python3"]
#######################################################

# 可选镜像
alpine和普通Linux为基础的镜像

# tag
# image:tag
python:3.7-alpine3.8
python = python:latest

# 1. 使用专用镜像例如python镜像而不是在ubuntu上安装python
# 2. 使用基于alpine的镜像

# 分层和缓存
# Docker对Dockerfile中命令生成缓存,没有改动就使用缓存,指令顺序重要,如果前面命令有改变就会全部重新build
# 把嘴不容易改变的命令放在前面
# COPY中的文件的内容改变缓存也会失效

# 指定构建镜像的文件

#######################################################
# docker.txt
FROM ubuntu:18.04
COPY sources.list /etc/apt/sources.list
RUN apt update
#######################################################

$ docker build -t imagecache -f docker.txt .

# 端口绑定
8000:3000 = 0.0.0.0:8000:3000

# 日志
# compose日志
$ docker-compose logs

# 服务日志
$ docker-compose logs pyweb

# compose配置
$ docker-compose up = docker-compose -f docker-compose.yml up

$ docker-compose -f init.yml up
$ docker-compose -f debug.yml up

#######################################################
# docker-compose
version: '3'

services:
  pyweb:
    build: .
    depends_on:
      - redisdemo
    volumes:
      - .:/code

  redisdemo:
    image: "redis:alpine"
#######################################################

#######################################################
# debug.yml 
version: '3'

services:
  pyweb:
    ports:
      # 只有本机能访问，其他机器无法访问
      - "127.0.0.1:9000:5000"
#######################################################

# 先读入docker-compose后读入debug
$ docker-compose -f docker-compose.yml -f debug.yml up

# compose配置环境变量

#######################################################
# debug.yml
# 文件中读取环境变量
version: '3'

services:
  bash:
    # 可以使用 env_file 的形式从文件中读取环境变量
    env_file:
      - a.env
      - b.env
#######################################################

#######################################################
# debug.yml
# 参数读取环境变量
version: '3'

services:
  bash:
    # environment 参数用于指定环境变量
    environment:
      - PRODUCTION
      - NAME=prod.yml
      - "USER=nike"
#######################################################

# gogs
$ docker run -p 3000:3000 gogs/gogs:latest

# compose中互相访问注意需要使用!DNS!

# drone
# http://readme.drone.io/
```

## CI

```bash
# 生成秘钥
$ ssh-keygen -t rsa -f /tmp/id_rsa
$ cat /tmp/id_rsa.pub >> ~/.ssh/authorized_keys

# 配置gogs和drone需要使用DNS代替IP

# gogs安装
#######################################################
# docker-compose.yml
version: '3'

services:
    gogs:
      image: gogs/gogs:0.11.53
      restart: always
      volumes:
        # 将 gogs 的数据文件存储在本机
        - "./data/gogs:/data"
      ports:
        - "3000:3000"
      environment:
        - "RUN_CROND=true"
      depends_on:
        - postgres
    postgres:
      image: postgres:9.5
      restart: always
      volumes:
        # 将数据库文件存储到本机，以免丢失
        - "./data/postgresql:/var/lib/postgresql"
      ports:
        - "127.0.0.1:5432:5432"
      environment:
        # 数据库的连接信息
        - "POSTGRES_USER=admin"
        - "POSTGRES_PASSWORD=123456"
        - "POSTGRES_DB=gogs"
#######################################################

# drone安装

#######################################################
# docker-compose.yml
version: '3'

services:
  server:
    image: drone/drone:0.8.6
    ports:
      - 8000:8000
    volumes:
      - ./data/drone:/var/lib/drone/
    restart: always
    environment:
      # false 表示禁止注册
      - DRONE_OPEN=false
      # DRONE_ADMIN 配置的用户作为管理员
      - DRONE_ADMIN=kuaibiancheng.com
      # 本机主机名
      - DRONE_HOST=http://111.231.98.114
      # 随机输入一个字符串
      - DRONE_SECRET=random_string_123
      # 使用 gogs 服务
      - DRONE_GOGS=true
      # gogs 的地址
      - DRONE_GOGS_URL=http://111.231.98.114:3000
      # gogs 的 git 用户名
      - DRONE_GOGS_GIT_USERNAME=kuaibiancheng.com
      # 密码
      - DRONE_GOGS_GIT_PASSWORD=123
      # 私有模式
      - DRONE_GOGS_PRIVATE_MODE=true
      # 关闭 ssl 验证（我们没有配置 https 访问）
      - DRONE_GOGS_SKIP_VERIFY=true
  agent:
    image: drone/agent:0.8.6
    command: agent
    restart: always
    depends_on:
      - server
    volumes:
      # 这样才可以在容器中使用宿主机的 Docker 服务
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      # secret 和上面的 DRONE_SECRET 配置一致
      - DRONE_SECRET=random_string_123
      # 上面的 server 服务的 9000 端口
      - DRONE_SERVER=server:9000
#######################################################

# 先在生产服务器git clone

#######################################################
# git 仓库目录下的.drone.yml
pipeline:
  run:
    image: python:3.7-alpine3.8
    commands:
      - python3 test.py
  deploy:
    image: appleboy/drone-ssh
    host: 192.168.63.128
    username: nike
    secrets: [ ssh_key ]
    port: 22
    script:
      - cd /home/nike/todo
      - git pull
      - sh update.sh
#######################################################

#######################################################
# update.sh
# restruct build image
docker build -t pywebimage .

# del container
docker rm -f pyweb

# start new container
docker run -d -p 5000:5000 --name pyweb pywebimage
#######################################################

# plugins.drone.io
```
