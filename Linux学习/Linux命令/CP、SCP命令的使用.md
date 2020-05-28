# CP、SCP命令的使用

## CP 命令

使用权限：所有使用者

使用方式：

    cp [options] source dest
    cp [options] source... directory

说明：将一个档案拷贝至另一档案，或将数个档案拷贝至另一目录。



    -a 尽可能将档案状态、权限等资料都照原状予以复制。

    -r 若 source 中含有目录名，则将目录下之档案亦皆依序拷贝至目的地。

    -f 若目的地已经有相同档名的档案存在，则在复制前先予以删除再行复制。



范例：



将档案 aaa 复制(已存在)，并命名为 bbb :



    cp aaa bbb



将所有的C语言程式拷贝至 Finished 子目录中 :



    cp *.c Finished



----



## SCP 命令

不同的Linux之间copy文件常用有3种方法：

>第一种就是ftp，也就是其中一台Linux安装ftp Server，这样可以另外一台使用ftp的client程序来进行文件的copy。

第二种方法就是采用samba服务，类似Windows文件copy 的方式来操作，比较简洁方便。

第三种就是利用scp命令来进行文件复制。



scp是有Security的文件copy，基于ssh登录。



scp 可以在 2个 linux 主机间复制文件；



命令基本格式： 



    scp [可选参数] file_source file_target



从 本地 复制到 远程 



    scp local_file remote_username@remote_ip:remote_folder 

    或者 
    scp local_file remote_username@remote_ip:remote_file 
    
    或者 
    scp local_file remote_ip:remote_folder 

    或者 
    scp local_file remote_ip:remote_file



第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名； 

第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名； 



反过来从远程复制到本地也一样，只需要把两个参数的位置换一下即可。

要想复制目录在scp后加`-r`选项即可。





可能有用的几个参数 :



    -v 和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误 .
    -C 使能压缩选项 .
    -P 选择端口  （注意这里的P是大写！和ssh的小写p不一样！）
    -4 强行使用 IPV4 地址 .
    -6 强行使用 IPV6 地址 .



注意两点：

1.如果远程服务器防火墙有特殊限制，scp便要走特殊端口，具体用什么端口视情况而定，命令格式如下：

注意：这里是大写P指定端口，而ssh中是小写p指定端口

    #scp -P 4588 remote@www.abc.com:/usr/local/sin.sh /home/administrator

2.使用scp要注意所使用的用户是否具有可读取远程服务器相应文件的权限。

rcp命令和scp类似，但是没有加密，它是不安全的。



 



