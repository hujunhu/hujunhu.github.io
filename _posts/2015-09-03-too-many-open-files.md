---
layout: post
title: Too many open files 问题的解决
---
最近帮忙同事调试Linux程序，在程序运行一段时间后出现Too many open files(errno:24)错误。搜索发现与两个设置有关：系统级别的fs.file-max和程序级别的ulimit -n。

## 系统级别的fs.file-max

这个是操作系统中所有程序总共能同时打开的文件句柄数量。查看方法：

> cat /proc/sys/fs/file-max

其值为1万多，考虑到系统没有什么程序运行，1万多的句柄肯定够用。

> PS：不够的话，可以通过在/etc/sysctl.conf中插入fs.file-max=23456设置，也可使用命令echo 23456>/proc/sys/fs/file-max 或 sysctl相关命令设置。

## 程序级别的ulimit -n

该设置限制当前shell以及由它启动的进程的文件句柄上限。该值默认为1024。
因为调试的Linux程序，可能会同时使用的文件句柄不会超过100个，所以也与该设置无关。

综上，唯一有可能的就是文件句柄在使用后没有关闭释放句柄。“一切皆文件”是Linux的至理名言，所以在Linux中文件、设备、套接字都是文件，在使用中就会占用文件句柄。

查看进程使用了哪些“文件”，使用lsof命令。对比不同时间的该进程的文件句柄，发现socket句柄增长了。既然与socket有关，那就用netstat查看端口状态，发现一堆的udp句柄。估计是在udp使用之后没有关闭。询问程序原作者，找到错误的地方，问题解决。
