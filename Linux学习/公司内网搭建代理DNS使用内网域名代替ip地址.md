# 公司内网搭建代理DNS使用内网域名代替ip地址

一般在企业内部，开发、测试以及预生产都会有一套供开发以及测试人员使用的网络环境。运维人员会为每套环境的相关项目配置单独的Tomcat，然后开放一个端口，以 IP+Port 的形式访问。然而随着项目的增多，对于开发和测试人员记住如此多的内网地址，无疑是一件头疼的事情(当然你也可以使用浏览器书签管理器或者记录在某个地方)。但是你不永远不会确定，那天由于升级突然改了IP，我们可能又要重新撸一遍配置，所以内网域名还是非常有必要的。



内网域名具体有哪些优点：



- 方便记忆

- 变更IP，只需要修改DNS即可

- 服务器环境





192.168.1.170(开发)

192.168.1.180(测试)

192.168.1.190(预生产)

192.168.1.125(DNS+Nginx)



## DNS安装



### 安装容器

为了方便，我们使用docker环境手动搭建一个DNS服务器。



选择andyshinn/dnsmasq的docker镜像，2.75版本，执行命令：



docker run -d -p 53:53/tcp -p 53:53/udp --cap-add=NET_ADMIN --name dns-server andyshinn/dnsmasq:2.75

执行完毕以后，通过命令查看是否创建并运行成功：



[root@test125 ~]# docker ps

CONTAINER ID        IMAGE                    COMMAND             CREATED             STATUS              PORTS                                    NAMES

38ae71377ef1        andyshinn/dnsmasq:2.75   "dnsmasq -k"        22 hours ago        Up About an hour    0.0.0.0:53->53/tcp, 0.0.0.0:53->53/udp   dns-server



### 配置DNS

进入容器：



`docker exec -it dns-server /bin/sh`

创建代理文件：



`vi /etc/resolv.dnsmasq`

添加内容：

```

nameserver 114.114.114.114

nameserver 8.8.8.8

```

新建本地解析规则配置：



`vi /etc/dnsmasqhosts`

添加解析规则：



```

192.168.1.125  dev.52itstyle.com test.52itstyle.com sit.52itstyle.com

```

修改dnsmasq配置文件，指定使用上述两个我们自定义的配置文件：



`vi /etc/dnsmasq.conf`

追加下述两个配置

```

resolv-file=/etc/resolv.dnsmasq

addn-hosts=/etc/dnsmasqhosts

```

退出容器：

`exit`

重启容器：



`docker restart dns-server`



之后把网络连接的DNS改为DNS服务器的地址就可以了

