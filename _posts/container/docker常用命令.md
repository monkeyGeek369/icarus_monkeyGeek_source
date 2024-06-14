---
title: docker常用命令
category: container
date: 2024-06-13 15:30:06
updated: 2024-06-13 15:30:06
enname: docker_command
categories: container
tags: docker
keywords:
	- docker
	- 容器
permalink:
thumbnail:
---



docker常用命令介绍

说明:应用容器化大体上分为三个步骤:

1、配置DockerFile或dock er-compose.yaml等文件

2、基于DockerFile创建镜像

3、基于镜像来创建容器

<!--more-->

参考资料:

https://yeasy.gitbook.io/docker_practice#/

</br>

# 基本概念

1. 镜像:一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）
2. 分层存储(UnionFS):镜像只是一个虚拟的概念，其实际体现并非由一个文件组成，而是由一组文件系统组成，或者说，由多层文件系统联合组成。镜像构建时，会一层层构建，前一层是后一层的基础。每一层构建完就不会再发生改变，后一层上的任何改变只发生在自己这一层
3. 容器:镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等.有自己独立的命名空间、文件系统、网络环境,是环境隔离的.
4. 容器存储层:每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为 **容器存储层**。容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡
5. 仓库:存储镜像的库,可以使用公共库https://hub.docker.com/,也可以自建库.公共库一般网络联通性不好,需要配置国内镜像,此内容不再此详细介绍
6. docker-compose:是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速编排.它允许用户通过一个单独的 `docker-compose.yml` 模板文件（YAML 格式）来定义一组相关联的应用容器为一个项目（project）。
7. 基本架构:Docker 采用了 `C/S` 架构，包括客户端和服务端。Docker 守护进程 （`Daemon`）作为服务端接受来自客户端的请求，并处理这些请求（创建、运行、分发容器）。



</br>

# 镜像命令

```python
# 通过dockerfile构建镜像
docker build [OPTIONS] PATH | URL | -
# -f用来指定具体的Dockerfile
# 最后的.表示上下文,即相对于当前路径的路径,次路径下的文件将被统一打包进入docker服务端,并作为初始路径
# -t 或 --tag 选项给镜像指定一个标签（名称和可选的标签）
docker build -f /path/to/Dockerfile -t myimage:latest .
# 可以通过--build-arg增加多个构建参数key=value
docker build --build-arg http_proxy=http://proxy:port --build-arg https_proxy=http://proxy:port .

        
# 下载镜像
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
docker pull dockurr/windows:2012
  
# 列出镜像
docker image ls
# 查询所有镜像包括中层镜像(相当于临时镜像)
docker image ls -a

# 查看镜像容量
docker system df


# 运行镜像
#  -i：交互式操作,-t:终端,-d:守护态运行,不会输出信息到终端,--rm：这个参数是说容器退出后随之将其删除
# /bin/sh:放在镜像名后的是“命令”,/bin/sh表示使用交互式shell
docker run -itd --rm python:3.8.18 /bin/sh
  
# 删除镜像
docker image rm [选项] <镜像1> [<镜像2> ...]

```

</br>

# 多阶段构建

为什么要有多阶段构建?

一个项目的构建过程如果全部写在一个dockerfile中会导致命令过多,包括项目及其依赖库的编译、测试、打包等流程.构建过程会非常长,后期维护也不方便.因此把构建分为多个阶段,每一个阶段构建完成后再进行下一阶段构建



多阶段构建与普通构建在文档内容上的区别?

```python
# 普通构建
FROM golang:alpine
RUN apk --no-cache add git ca-certificates
WORKDIR /go/src/github.com/go/helloworld/
COPY app.go .
CMD ["./app"]

# 多阶段构建(写在一个dockerfile文件中)
FROM golang:alpine as builder
RUN apk --no-cache add git
WORKDIR /go/src/github.com/go/helloworld/
RUN go get -d -v github.com/go-sql-driver/mysql
COPY app.go .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM alpine:latest as prod
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=0 /go/src/github.com/go/helloworld/app .
CMD ["./app"]

```



只构建某一个阶段?

利用as来命名别名FROM golang:alpine as builder

例如当我们只想构建 `builder` 阶段的镜像时，增加 `--target=builder` 参数即可

docker build --target builder -t username/imagename:tag .





</br>

# 容器命令

```python
# docker-compose命令控制容器

# 运行容器-通过docker-compose编排方式(指定具体的文件路径)
# 该命令十分强大，它将尝试自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作
docker-compose -f win-server-2012.yaml up


```

```python
# docker命令控制容器

# 查看容器列表
docker ps

# 启动容器
docker start container_name_or_id
# 启动容器后立即看到其输出
docker attach container_name_or_id
# 重启容器
docker restart container_name_or_id
# 设定端口映射
docker start -p 8080:80 container_name_or_id
  
# 终止容器
docker stop container_name_or_id
# 设定超时时间:单位秒
docker stop -t 30 container_name_or_id

# 删除容器
docker rm container_name_or_id

# 进入容器
# -i:交互式
# -t:命令窗口
# bash:指定linux交互方式,根据操作系统不同可以自由改变,例如针对unix等/bin/sh
docker exec -it 69d1 bash
```

</br>

# 数据卷

`数据卷` 是一个可供一个或多个容器使用的特殊目录，它绕过 UnionFS，可以提供很多有用的特性：

- `数据卷` 可以在容器之间共享和重用
- 对 `数据卷` 的修改会立马生效
- 对 `数据卷` 的更新，不会影响镜像
- `数据卷` 默认会一直存在，即使容器被删除



```python
# 创建数据卷
docker volume create my-vol

# 列出数据卷
docker volume ls

# 挂载数据卷
# 命名一个web的容器并以nginx:alpine镜像启动,d守护运行
# mount挂载数据卷,将数据卷my-vol挂在到容器目录/usr/share/nginx/html
docker run -d --name web --mount source=my-vol,target=/usr/share/nginx/html nginx:alpine
# 挂载一个主机目录到容器成为数据卷
docker run -d --name web --mount type=bind,source=my-vol,target=/usr/share/nginx/html nginx:alpine
  
# 删除数据卷
docker volume rm my-vol
```







</br>

</br>

</br>

<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?2f798e6b269c8a40f12bef25d7f1876d";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>

<div style="height:260px; background-color:rgb(238,240,244); padding:10px;border-radius:10px;">
    <p style="color:#f36c21;font:bold 16px/20px 'kaiTi';">
      版权声明：本文为博主原创文章，欢迎转载，转载请注明作者、原文超链接，感谢各位看官!!!
    </p>
    <p>
      <span style="font:bold 16px/20px 'kaiTi';">本文出自：</span><a href="https://monkeyGeek369.github.io">monkeyGeek</a> 
    </p>
    <p>
      <span style="font:bold 16px/20px 'kaiTi';">座右铭：</span><span>生于忧患，死于安乐</span> 
    </p>
    <p>
      <span style="font:16px/20px 'kaiTi';">欢迎志同道合的朋友一起交流、探讨！</span> 
    </p>
    <img style="height:auto; width:auto;flot:left;" src="../../../../image/monkey64.png" /><span style="font:16px/20px 'kaiTi';flot:left;">   monkeyGeek</span>


