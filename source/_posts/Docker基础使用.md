---
title: Docker基础使用
tags:
  - - Docker基础
categories:
  - - 学习
    - Docker
date: 2020-03-23 23:39:52
copyright: true
description: Docker的一些基本命令总结
---

### Docker容器使用
> 创建并启动运行一个新容器
```
docker run -itd --name hineUbuntu --network test-net ubuntu /bin/bash

-it:交互式终端方式运行
-d:后台运行
--name:给容器命名
--network:加入哪个Docker网络中
/bin/bash:放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bas
————————————————————————————————————————————————————————————————————————
docker run -d -P training/webapp python app.py

-P:将容器内部使用的网络端口映射到我们使用的主机上的随机端口
————————————————————————————————————————————————————————————————————————
docker run -d -p 5000:5000 training/webapp python app.py
docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py

-p:将容器内部使用的网络端口映射到我们使用的主机上的指定端口
```
> 查看指定容器使用的端口映射到宿主机的端口号
```
docker port d6403a52907a[容器ID]
```
> 查看容器内部标准日志输出
```
docker logs -f d6403a52907a[容器ID]
docker logs -f --since="2020-03-26T09:46:31" --tail=10 4eac9eda8950[容器ID]

-f: 让 docker logs 像使用 tail -f 一样来一直实时输出容器内部的标准输出。
```
> 查看容器内部运行的进程
```
docker top d6403a52907a[容器ID]
```
> -d命令启动的容器在后台，此时想要进入运行中的容器
```
docker attach 37199d8fe729[容器ID]

docker exec -it e61409a69155 /bin/bash

attach:使用exit退出时会停止当前进入的容器
exec:使用exit退出不会影响当前进入的容器
```
> 查看本地容器
```
docker ps -a

-a:查看所有容器
-l:查看最后一次创建的容器
```
> 开启/重启已经存在的某个容器
```
docker start e61409a69155[容器ID]

docker restart e61409a69155[容器ID]
```
> 停止某个容器
```
docker stop e61409a69155[容器ID]
```
> 将容器导出到本地
```
docker export e61409a69155[容器ID] > /Users/hine/docker/mysql.tar[本地路径]
```
> 将本地容器导入为docker镜像
```
docker import /Users/hine/docker/mysql.tar[本地路径/网上url] test/ubuntu:v1[镜像仓库repository:tag版本号]
```
> 删除指定容器
```
docker rm -f 37199d8fe729[容器ID]

-f:强制删除运行中的容器
```
> 删除清理所有终止状态的容器
```
docker container prune  
```
> 查看容器底层配置及状态信息
```
docker inspect d6403a52907a

返回JSON格式的具体容器底层信息
```

### Docker镜像使用
> 列出镜像列表
```
docker images
```
> 查找需要使用的镜像
```
可以到镜像网址搜索，如：https://hub.docker.com
或
docker search kafka
```
> 拉取载入镜像
```
docker pull training/webapp
```
> 删除镜像
```
docker rmi 6fae60ef3446[镜像ID/name]
```
> 基于已有容器创建新镜像
```
docker commit -m="提交描述信息" -a="镜像作者" c69b9eabedec hine/ubuntu:v1

-m:提交的描述信息
-a:指定的镜像作者
c69b9eabedec:容器ID
hine/ubuntu:v1:指定要创建的目标镜像名
```
> 基于Dockerfile创建新镜像
```
docker build -t hine/centos:6.7 .

-t:指定创建的目标镜像名
.:Dockerfile文件所在目录，可指定Dockerfile绝对路径，会将此目录打包上传给docker
```
> 为指定镜像添加标签
```
docker tag bdd580ced7ff hine/centos:dev
```
> 推送镜像
```
https://hub.docker.com

docker push username/ubuntu:18.04
```

### Docker容器互联
> 查看docker所有网络
```
docker network ls
```
> 创建一个新的Docker网络
```
docker network create -d bridge test-net

-d:指定网络类型 bridge/overlay
```
> 容器加入网络
```
docker run -itd --name test1 --network test-net ubuntu /bin/bash
```