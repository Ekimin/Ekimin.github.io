---
title: Mac中用BC分区安装多系统失败导致磁盘空间丢失
tags:
  - Mac
  - Filevault
  - Bootcamp
categories: Mac
date: 2017-06-02 09:09:42
updated:
comments:
permalink:
---


今天在用bootcamp在mac上安装win时发生了错误，分出去的空间丢失了，几经折腾最后终于找回了磁盘空间，在这里记录下过程。

<!--more-->

使用diskutil list可以看到磁盘一部分空间不见了：
```bash
EkimindeMacBook-Pro:~ ekimin$ diskutil list
/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         251.0 GB   disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:          Apple_CoreStorage Macintosh HD            250.0 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS Macintosh HD           +121.0 GB   disk1
                                 Logical Volume on disk0s2
                                 09E2A3DC-C2F7-400B-99C0-AF0E0AC0A421
                                 Unlocked Encrypted
```

整个硬盘是250G，mac占121G，剩下的空间不见了- -

各种搜索，网上普遍的方案有以下几种：

1. 重新用bootcamp安装一次win，然后删除Win分区后，丢失的空间会恢复。
2. 重启按command+s 进入单用户模式，输入fsck -fy， 提示OK以后，输入reboot重启后恢复。

很不辛，上两种方法都没有解决，最后在威锋网中看到一个网友的解决方法，原来是filefault在搞怪：http://bbs.feng.com/read-htm-tid-10628217.html

关掉filevault重启后，丢失的空间又出现了。可以看到，120G恢复成了250G了。

```bash
EkimindeMacBook-Pro:~ ekimin$ diskutil list
/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         251.0 GB   disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:                  Apple_HFS Macintosh HD            250.0 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS Macintosh HD           +250.0 GB   disk1
                                 Logical Volume on disk0s2
                                 09E2A3DC-C2F7-400B-99C0-AF0E0AC0A421
                                 Unencrypted
```

总结：初步分析，出现这种情况，可能是filevault对新分区加密了，原来磁盘并不能读到新分区的信息。