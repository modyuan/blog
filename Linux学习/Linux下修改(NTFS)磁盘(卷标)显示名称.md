我的笔记本装的双系统，Windows和Ubuntu，用Ubuntu一段时间了，越来越喜欢Linux，努力学习。。。



Ubuntu下自动加载的的磁盘卷标都是不好辨认长串，不直观，然后就想修改磁盘显示的卷标，特意查了下资料，看到了

[这里](http://wiki.ubuntu.org.cn/%E9%87%8D%E5%91%BD%E5%90%8DUSB%E7%A3%81%E7%9B%98%E6%8C%82%E8%BD%BD%E5%88%86%E5%8C%BA%E5%8D%B7%E6%A0%87)



文中说的很详细，提到了修改命令：



编辑ext2/ext3/FAT32/NTFS磁盘分区卷标



根据不同的磁盘分区类型,分别有3个程序可供选用.



Mtools 适用于 FAT32 格式分区.

ntfsprogs 适用于 NTFS 格式分区.

e2label适用于 ext2 和 ext3 型格式分区.


有需要的可以去看看详细的，比较简单，我的具体步骤(我的是ntfs分区)：


1.查看当前所有分区

```
sudo fdisk -l 
```



显示结果类似于：

```

Device Boot Start End Blocks Id System

/dev/sda1 * 63 275659334 137829636 7 HPFS/NTFS/exFAT

Partition 1 does not start on physical sector boundary.

/dev/sda2 275659396 1953523711 838932158 f W95 Ext'd (LBA)

Partition 2 does not start on physical sector boundary.

/dev/sda5 275659398 695116484 209728543+ 7 HPFS/NTFS/exFAT

Partition 5 does not start on physical sector boundary.

/dev/sda6 695116548 1114573634 209728543+ 7 HPFS/NTFS/exFAT

Partition 6 does not start on physical sector boundary.

/dev/sda7 1114573698 1534030784 209728543+ 7 HPFS/NTFS/exFAT

Partition 7 does not start on physical sector boundary.

/dev/sda8 1534030848 1848662464 157315808+ 7 HPFS/NTFS/exFAT

/dev/sda9 1848663216 1852663215 2000000 82 Linux swap / Solaris

/dev/sda10 1852663808 1953523711 50429952 83 Linux
```

2.先卸载要修改名称的分区：

```
sudo umount /dev/sda5
```


3.修改名称:

```
sudo ntfslabel /dev/sda5 music
```



注：ntfslabel会修改名称后自动重新加载，不用再执行mount命令