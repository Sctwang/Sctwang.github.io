转载自[**macrozheng**](https://mp.weixin.qq.com/s/d_CuljDTJq680NTndAay8g)

## 开发者必备 Docker 命令

> 本文主要讲解 Docker 环境的安装以及 Docker 常用命令的使用，掌握这些对 Docker 环境下应用的部署具有很大帮助。

## Docker 简介

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux 或 Windows 机器上。使用 Docker 可以更方便低打包、测试以及部署应用程序。

## Docker 环境安装

1. 安装 yum-utils：

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

2. 为 yum 源添加 docker 仓库位置：

```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

3. 安装 docker:

```
yum install docker-ce
```

4. 启动 docker:

```
systemctl start docker
```

## Docker 镜像常用命令

### 搜索镜像

```
docker search java
```

### 下载镜像

```
docker pull java:8
```

### 如何查找镜像支持的版本

> 由于 docker search 命令只能查找出是否有该镜像，不能找到该镜像支持的版本，所以我们需要通过 docker hub 来搜索支持的版本。

- 进入 docker hub 的官网，地址：https://hub.docker.com
- 然后搜索需要的镜像
- 查看镜像支持的版本
- 进行镜像的下载操作：

```
docker pull nginx:1.17.0
```

### 列出镜像

```
docker images
```

### 删除镜像

- 指定名称删除镜像

```
docker rmi java:8
```

- 指定名称删除镜像（强制）

```
docker rmi -f java:8
```

- 强制删除所有镜像

```
docker rmi -f $(docker images)
```

## Docker 容器常用命令

### 新建并启动容器

```
docker run -p 80:80 --name nginx -d nginx:1.17.0
```

- -d 选项：表示后台运行
- --name 选项：指定运行后容器的名字为 nginx, 之后可以通过名字来操作容器
- -p 选项：指定端口映射，格式为：hostPort:containerPort

### 列出容器

- 列出运行中的容器：

```
docker ps
```

- 列出所有容器

```
docker ps -a
```

### 停止容器

```
# $ContainerName及$ContainerId可以用docker ps命令查询出来docker stop $ContainerName(或者$ContainerId)
```

比如：

```
docker stop nginx#或者docker stop c5f5d5125587
```

### 强制停止容器

```
docker kill $ContainerName(或者$ContainerId)
```

### 启动已停止的容器

```
docker start $ContainerName(或者$ContainerId)
```

### 进入容器

- 先查询出容器的 pid：

```
docker inspect --format "{{.State.Pid}}" $ContainerName(或者$ContainerId)
```

- 根据容器的 pid 进入容器：

```
nsenter --target "$pid" --mount --uts --ipc --net --pid
```

### 删除容器

- 删除指定容器：

```
docker rm $ContainerName(或者$ContainerId)
```

- 强制删除所有容器；

```
docker rm -f $(docker ps -a -q)
```

### 查看容器的日志

```
docker logs $ContainerName(或者$ContainerId)
```

### 查看容器的 IP 地址

```
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $ContainerName(或者$ContainerId)
```

### 同步宿主机时间到容器

```
docker cp /etc/localtime $ContainerName(或者$ContainerId):/etc/
```

### 在宿主机查看 docker 使用 cpu、内存、网络、io 情况

- 查看指定容器情况：

```
docker stats $ContainerName(或者$ContainerId)
```

- 查看所有容器情况：

```
docker stats -a
```

### 进入 Docker 容器内部的 bash

```
docker exec -it $ContainerName /bin/bash
```



## 修改 Docker 镜像的存放位置

- 查看 Docker 镜像的存放位置：

```
docker info | grep "Docker Root Dir"
```

- 关闭 Docker 服务：

```
systemctl stop docker
```

- 移动目录到目标路径：

```
mv /var/lib/docker /mydata/docker
```

- 建立软连接：

```
ln -s /mydata/docker /var/lib/docker
```
