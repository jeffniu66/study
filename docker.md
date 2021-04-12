---
typora-root-url: ../study
typora-copy-images-to: ./docker_img
---

# 1. 简介

虚拟化容器技术。Docker基于镜像，可以秒级启动各种容器。每一种容器都是一个完整的运行环境，容器之间互相隔离。

![image-20210412222909892](/docker_img/image-20210412222909892.png)

<font color=red>相当于windows安装系统时的ghost镜像</font>

# 2. Docker安装

参考官方文档

```java
https://docs.docker.com/engine/install/centos/
```

查看docker版本

```shell
docker -v
```

查看docker镜像

```shell
docker images
```

设置开机启动docker

```shell
systemctl enable docker
```

配置阿里云镜像加速服务

![image-20210412233208687](/docker_img/image-20210412233208687.png)

# 3. Docker安装redis

下载redis最新镜像

```shell
docekr pull redis
```

官方镜像下提供了一些简单启动命令

![image-20210412234537258](/docker_img/image-20210412234537258.png)

<font color=gree>完整启动命令</font>

-v: 目录挂载（外部目录:docker内部目录）

-d: 后台运行

mkdir -p /mydata/redis/conf

touch /mydata/redis/conf/redis.conf

```shell
docker run -p 6379:6379 --name redis -v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```

有坑，/etc/redis/下的redis.conf并不存在，需要预先创建好

