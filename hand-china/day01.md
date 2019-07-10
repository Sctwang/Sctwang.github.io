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

- 方法：`yum -y install docker`

#### MySQL5.7

- 方法：开启 `docker(systemctl start docker)` 之后，执行 `docker pull mysql:5.7`

- 在 docker 中打开 mysql 服务：

`docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=xxxx -d mysql:5.7`

其中 xxxx为以 root 权限连接数据库时的密码，mysql 为对此服务起的别名。

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

### Navicat

- 版本：`navicat for mysql10.0.11`

### IDEA

- 版本：`ideaIU-2018.1.4`

### Git

- 版本：`Git-2.9.2-64-bit`