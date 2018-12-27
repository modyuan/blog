# SSH无密码登录设置

为啥要设置ssh无密码登录？

我们先来看一下分布式系统的一键启动流程， 在matser机器上运行脚本，脚本检测有多少slavers，然后通过ssh登录到slavers，进入到相同的目录(或者通过$XXX_HOME环境变量进入对应的目录),然后启动slave进程。

不同的机器密码可能不一样，这里如果ssh 还需要输入密码进行登录的话，就不是一键启动了。

因此，ssh无密码登录是必须设置的。

## 1）SSH无密码原理

Master作为客户端，要实现无密码公钥认证，连接到服务器Salvers（Workers）上时，需要在Master上生成一个密钥对，包括一个公钥和一个私钥，而后将公钥复制到所有的Slave上。当Master通过SSH连接Salve时，Salve就会生成一个随机数并用Master的公钥对随机数进行加密，并发送给Master。Master收到加密数之后再用私钥解密，并将解密数回传给Slave，Slave确认解密数无误之后就允许Master进行连接了。这就是一个公钥认证过程，其间不需要用户手工输入密码。



## 2）Master机器上设置无密码登录

a. Master节点利用ssh-keygen命令生成一个无密码密钥对。

在Master节点上执行以下命令：

运行后询问其保存路径时直接回车采用默认路径。生成的密钥对：id_rsa（私钥）和id_rsa.pub（公钥），默认存储在"/home/用户名/.ssh"目录下。

```c
ssh-keygen –t rsa –P '' 

root@qp-zhang:/var/spark# ssh-keygen -t rsa -P ''
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Created directory '/root/.ssh'.
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
a0:40:48:9f:e7:47:ac:bc:d1:a2:32:1a:ef:24:11:37 root@qp-zhang
The key's randomart image is:
+--[ RSA 2048]----+
|.o.              |
|... . .          |
|. Eo ..o         |
| o o+.+.         |
|.  .* oS         |
| .  . =          |
|oo.. .           |
|.=o              |
|..o              |
+-----------------+ 

root@qp-zhang:/var/spark# cat /root/.ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDROi8G+KJw+ywkkMvVY/Dqmgb/AGl8sXuaR root@qp-zhang 
```
PS：如果上述命令没找到，那么你可能需要安装一下ssh server了。以下是ubuntu安装命令，其它系统可以找度娘

` sudo apt-get install openssh-server `



然后通过ssh-copy-id 命令，把id copy到salver机器上，这里需要输入密码（注意：同样这里hostname如果不是域名之类的话，必须在hosts设置ip，以便能够访问）
```c
root@qp-zhang:/var/spark# ssh-copy-id root@qpzhangdeMac-mini.local
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
Password:

Number of key(s) added: 1

Now try logging into the machine, with:  "ssh 'root@qpzhangdeMac-mini.local'"
and check to make sure that only the key(s) you wanted were added. 



root@qpzhangdeMac-mini:/private/var/spark $ls -all ~/.ssh/
total 8
drwxr-xr-x  3 root  wheel  102  3 23 15:25 .
drwxr-x---  10 root  wheel  340  3 23 15:14 ..
-rw-------  1 root  wheel  395  3 23 15:25 authorized_keys 
```
然后我们可以测试登录：
```c
root@qp-zhang:/var/spark# ssh 'root@qpzhangdeMac-mini.local'
Last login: Mon Mar 23 15:15:38 2015 from 10.60.215.93
qpzhangdeMac-mini:~ root# exit
logout 
```
到此，设置完毕。

如果没有ssh-copy-id命令的话，就通过scp命令将id_rsa.p，拷贝到远程机器上在。然后加入authorized_keys.

比如： cat myid_rsa.pub >>　authorized_keys