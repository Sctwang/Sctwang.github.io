# 配置环境

## Maven

- 版本：3.5.0

- 方法：将 maven 加入系统 Path 环境；

  `F:\apache\apache-maven-3.5.0\apache-maven-3.5.0\bin`

## 虚拟机

### VMware

- 版本：`VMware-workstation-full-14.1.2-8497320`

### CentOS

- 版本：`CentOS-7-x86_64-DVD-1708`

**注意事项**：在 VMware 中安装 CentOS 时，在选择语言界面开启网络连接。

### Docker

- 方法：`yum install -y docker`

- 镜像加速器：来自于[阿里云教程](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)，注册登录后，会有一个专属加速通道

   - **CentOS 版**

     #### 1.安装／升级 Docker 客户端

     推荐安装 1.10.0 以上版本的 Docker 客户端，参考文档 [docker-ce](https://yq.aliyun.com/articles/110806)

     #### 2. 配置镜像加速器

     针对 Docker 客户端版本大于 1.10.0 的用户

     您可以通过修改 daemon 配置文件 `/etc/docker/daemon.json` 来使用加速器

     ```CentOS
     sudo mkdir -p /etc/docker
     sudo tee /etc/docker/daemon.json <<-'EOF'
     {
       "registry-mirrors": ["加速器地址"]
     }
     EOF
     sudo systemctl daemon-reload
     sudo systemctl restart docker
     ```

  - **Windows 版本**

    #### 1. 安装／升级 Docker 客户端

    对于 Windows 10 以下的用户，推荐使用 Docker Toolbox

    Windows 安装文件：[下载地址](http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/)

    对于 Windows 10 及以上的用户 推荐使用 Docker for Windows

    Windows 安装文件：[下载地址](http://mirrors.aliyun.com/docker-toolbox/windows/docker-for-windows/)

    #### 2. 配置镜像加速器

    针对安装了 Docker Toolbox 的用户，您可以参考以下配置步骤：

    创建一台安装有 Docker 环境的 Linux 虚拟机，指定机器名称为 default，同时配置 Docker 加速器地址。

    ```CentOS
    docker-machine create --engine-registry-mirror=https://加速器地址 -d virtualbox default
    ```

    查看机器的环境配置，并配置到本地，并通过 Docker 客户端访问 Docker 服务。

    ```CentOS
    docker-machine env defaulteval "$(docker-machine env default)"docker info
    ```

    针对安装了 Docker for Windows 的用户，您可以参考以下配置步骤：

    在系统右下角托盘图标内右键菜单选择 Settings，打开配置窗口后左侧导航菜单选择 Docker Daemon。编辑窗口内的 JSON 串，填写下方加速器地址：

    ```CentOS
    {
      "registry-mirrors": ["加速器地址"]
    }
    ```

    编辑完成后点击 Apply 保存按钮，等待 Docker 重启并应用配置的镜像加速器。

    #### 注意

    Docker for Windows 和 Docker Toolbox 互不兼容，如果同时安装两者的话，需要使用 hyperv 的参数启动。

    ```CentOS
    docker-machine create --engine-registry-mirror=https://加速器地址 -d hyperv default
    ```

    Docker for Windows 有两种运行模式，一种运行 Windows 相关容器，一种运行传统的 Linux 容器。同一时间只能选择一种模式运行。

    #### 3. 相关文档

    [Docker 命令参考文档](https://docs.docker.com/engine/reference/commandline/cli/)

    [Dockerfile 镜像构建参考文档](https://docs.docker.com/engine/reference/builder/)

  - **Ubuntu 版**

    #### 1. 安装／升级 Docker 客户端

    推荐安装 1.10.0 以上版本的 Docker 客户端，参考文档 [docker-ce](https://yq.aliyun.com/articles/110806)

    #### 2. 配置镜像加速器

    针对 Docker 客户端版本大于 1.10.0 的用户

    您可以通过修改 daemon 配置文件 `/etc/docker/daemon.json` 来使用加速器

    ```CentOS
    sudo mkdir -p /etc/docker
    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
      "registry-mirrors": ["加速器地址"]
    }
    EOF
    sudo systemctl daemon-reload
    sudo systemctl restart docker
    ```

    



#### MySQL5.7

- 方法：开启 `docker(systemctl start docker)` 之后，执行 `docker pull mysql:5.7`

- 在 docker 中打开 mysql 服务：

`docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=xxxx -d mysql:5.7`

其中 xxxx为以 root 权限连接数据库时的密码，mysql 为对此服务起的别名。

- Dokcer 进入 MySQL 容器命令：

  - `docker exec -it <mysql_name> bash` 
- `mysql -uroot -p`
  - 输入 root 密码并回车
  
- Docker 容器中的 MySQL 镜像数据持久化：

  - 问题：为做持久化前，机器重启之后，MySQL 镜像中的数据会出现丢失
  - 解决：对镜像产生的数据进行持久化，保存到本地磁盘，每次重启不会发生丢失数据现象
  - 方法：

  ~~~sql
  
  
  
  
  
  ~~~

  

####  Redis

- 方法：在 Docker 中执行 `docker pull redis`

- 在 docker 中打开 redis 服务：

  `docker run --name redis -p 6379:6379 -d redis:latest redis-server`

**备注：**

问题：在第二天对数据库和 redis 进行连接时，提示连接失败；

解决方法：对 CentOS7 的网络和 docker 进行重启，命令为`systemctl restart network && systemctl restart docker`

查看 docker 已安装的镜像 `docker images`

查看 docker 目前正在运行的服务 `docker ps -a`

移除 docker 中的服务 `docker rm <对应服务名或唯一 ID>`

删除镜像：`docker rmi -f [镜像 ID]`

更多有关 Docker 使用教程：[阮一峰博客](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

### JDK

- 版本：`jdk-8u191-windows-x64`

- 方法：将 jdk 加入系统 Path 环境；

  `C:\Program Files\Java\jdk1.8.0_191\bin`

  `C:\Program Files\Java\jre1.8.0_191\bin`

  新建变量，变量名为 `JAVA_HOME` ，变量值为`C:\Program Files\Java\jdk1.8.0_191`

## Navicat

- 版本：`navicat for mysql10.0.11`

## IDEA

- 版本：`ideaIU-2018.1.4`

## Git

- 版本：`Git-2.9.2-64-bit`