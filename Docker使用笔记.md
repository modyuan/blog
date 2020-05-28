# Docker使用笔记

docker是一个轻量级的虚拟机。用go语言写的。



## 安装

参见 https://docs.docker.com/engine/install


## 配置国内镜像

### 对于MacOs和Windows
在图形管理界面的配置中，有一个可以添加json配置的地方，写入一下内容：
```json
{
    "registry-mirrors":
        ["https://hub-mirror.c.163.com/"]
}
```
### 对于Linux服务器
 在`/etc/docker/daemon.json`中加入相同内容就可以了。


## 基本概念

### 镜像

镜像是一个不可变的对象，就好像一个光盘镜像一样，当我们运行一个光盘镜像，就会创建一个`容器`去运行它。

镜像有很多种，常见的比如操作系统镜像，代表一个操作系统的环境。

我们可以用`docker pull 镜像名:版本号 `去获取它，其中版本号可以省略，默认是`latest`，也就是最新版。

`docker image ls` 列出本机所有镜像文件。

`docker rmi 镜像名或镜像ID` 可以删除镜像

```text
[root@xxx ~]# docker save -o centos.tar xianhu/centos:git    # 保存镜像
[root@xxx ~]# docker load -i centos.tar    # 加载镜像
```

### 仓库

类似于代码仓库，这里是镜像仓库，是Docker用来集中存放镜像文件的地方。注意与注册服务器（Registry）的区别：注册服务器是存放仓库的地方，一般会有多个仓库；而仓库是存放镜像的地方，一般每个仓库存放一类镜像，每个镜像利用tag进行区分，比如Ubuntu仓库存放有多个版本（12.04、14.04等）的Ubuntu镜像。

### 容器

类似于一个轻量级的沙盒，可以将其看作一个极简的Linux系统环境（包括root权限、进程空间、用户空间和网络空间等），以及运行在其中的应用程序。Docker引擎利用容器来运行、隔离各个应用。容器是镜像创建的应用实例，可以创建、启动、停止、删除容器，各个容器之间是是相互隔离的，互不影响。注意：镜像本身是只读的，容器从镜像启动时，Docker在镜像的上层创建一个可写层，镜像本身不变。

容器是镜像的运行环境，容器中程序对环境的修改不会影响到外界。当然如果需要的话，我们把内部的socket端口绑定到宿主机上，也可以挂载目录。

`docker run [options] 镜像名 运行命令` 

常用的选项有

- `-i` `-t` (可以写在一块`-it`)，`-i` 代表打开STDIN（这样就可以输入了），`-t`代表模拟一个终端环境，当我们需要在容器中用vim等时，就需要这个了。
- `--name`指定创建容器的名字，没有指定的话会分配一个随机的名字（很奇怪的名字……）

`docker ps -a`可以展示所有创建的容器（名字、ID、运行状态等）。

`docker stop 容器ID` 停止指定的容器。

`docker rm 容器ID` 可以指定的容器，如果容器正在运行，那么就需要先停止才能删除。

我们也可以使用组合命令删除所有容器：

`docker rm $(docker ps -a -q)` 这样就可以删除所有容器。

**注意**：fish的语法有所不同，子命令不加`$`，直接`docker rm (docker ps -a -q)`





## 新建自有镜像

### 1. 利用镜像启动一个容器后进行修改

```text
[root@xxx ~]# docker run -it centos:latest /bin/bash    # 启动一个容器
[root@72f1a8a0e394 /]#    # 这里命令行形式变了，表示已经进入了一个新环境
[root@72f1a8a0e394 /]# git --version    # 此时的容器中没有git
bash: git: command not found
[root@72f1a8a0e394 /]# yum install git    # 利用yum安装git
......
[root@72f1a8a0e394 /]# git --version   # 此时的容器中已经装有git了
git version 1.8.3.1
```

此时利用exit退出该容器，然后查看docker中运行的程序（容器）：

```text
[root@xxx ~]# docker ps -a
CONTAINER ID  IMAGE    COMMAND      CREATED   STATUS   PORTS    NAMES
72f1a8a0e394  centos:latest "/bin/bash"  9 minutes ago   Exited (0) 3 minutes ago      angry_hodgkin
```

这里将容器转化为一个镜像，即执行commit操作，完成后可使用docker images查看：

```text
[root@xxx ~]# docker commit -m "centos with git" -a "qixianhu" 72f1a8a0e394 xianhu/centos:git

[root@xxx ~]# docker images
REPOSITORY       TAG    IMAGE ID         CREATED             SIZE
xianhu/centos    git    52166e4475ed     5 seconds ago       358.1 MB
centos           latest 0584b3d2cf6d     9 days ago          196.5 MB
```

其中，-m指定说明信息；-a指定用户信息；72f1a8a0e394代表容器的id；xianhu/centos:git指定目标镜像的用户名、仓库名和 tag 信息。注意这里的用户名xianhu，后边会用到。

此时Docker引擎中就有了我们新建的镜像xianhu/centos:git，此镜像和原有的CentOS镜像区别在于多了个Git工具。此时我们利用新镜像创建的容器，本身就自带git了。

```text
[root@xxx ~]# docker run -it xianhu/centos:git /bin/bash
[root@520afc596c51 /]# git --version
git version 1.8.3.1
```

利用exit退出容器。注意此时Docker引擎中就有了两个容器，可使用docker ps -a查看。

### 2. 利用Dockerfile创建镜像

Dockerfile可以理解为一种配置文件，用来告诉docker build命令应该执行哪些操作。一个简易的Dockerfile文件如下所示，官方说明：[Dockerfile reference](https://link.zhihu.com/?target=https%3A//docs.docker.com/engine/reference/builder/)：

```bash
# 说明该镜像以哪个镜像为基础
FROM centos:latest

# 构建者的基本信息
MAINTAINER xianhu

# 在build这个镜像时执行的操作
RUN yum update
RUN yum install -y git

# 拷贝本地文件到镜像中
COPY ./* /usr/share/gitdir/
```

有了Dockerfile之后，就可以利用build命令构建镜像了：

```text
[root@xxx ~]# docker build -t="xianhu/centos:gitdir" .
```

其中-t用来指定新镜像的用户信息、tag等。最后的点表示在当前目录寻找Dockerfile。

构建完成之后，同样可以使用docker images命令查看：

```text
[root@xxx ~]# docker images
REPOSITORY        TAG       IMAGE ID      CREATED            SIZE
xianhu/centos     gitdir    0749ecbca587  34 minutes ago     359.7 MB
xianhu/centos     git       52166e4475ed  About an hour ago  358.1 MB
centos            latest    0584b3d2cf6d  9 days ago         196.5 MB
```

