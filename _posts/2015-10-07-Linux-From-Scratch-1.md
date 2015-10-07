---
layout: post
title: Linux From Scratch 1 - 准备工作
---
在开始之前需要做一些准备工作，包括新分区的创建和挂载、下载所需的软件包源码和补丁、工作环境设定等。

## 新分区的创建和挂载

Linux下常用的分区工具`fdisk/cfdisk`，网上资料很多，这里尝试使用`parted`分区工具。
使用以下命令进入分区交互模式：

``` bash
parted /dev/sda
```
因为是新的空白磁盘，需要先创建分区表，输入：

``` bash
mklabel msdos
```
创建分区，输入：

``` bash
mkpart primary 1049kB 8077MB
mkpart primary 8077MB -1s
```
输入`q`退出`parted`交互界面后，同步分区，输入：

``` bash
partprobe /dev/sda
```
创建文件系统，输入：

``` bash
mkfs -t ext3 /dev/sda1	# 创建ext3文件系统，存放文件
mkswap /dev/sda2	# 交换空间
```
执行以下命令挂载文件系统和交换空间：

``` bash
export LFS=/mnt/lfs
mkdir -pv $LFS
mount -v -t ext3 /dev/sda1 $LFS
swapon -v /dev/sda2
```
现在工作空间建立完毕。

## 软件包源码和补丁

本次编译安装使用的宿主机是LFS-6.3 liveCD，其上包含了所有需要软件包源码和补丁，在此不再累述。

## 工作环境设定

环境变量`$LFS`很重要，它的使用贯穿全书。检查环境变量`echo $LFS`结果是否为`/mnt/lfs`。

创建`$LFS/tools`目录，存放工具链，并做软链接`/tools -> /mnt/lfs/tools`：

``` bash
mkdir -v $LFS/tools
ln -sv $LFS/tools /
```
为了避免误操作、获得干净的工作环境，新建一个用户是必须的。这里是lfs。

``` bash
groupadd lfs
useradd -s /bin/bash -g lfs -m -k /dev/null lfs
```
命令行选项的含义：

> -s /bin/bash：指定 bash 作为 lfs 用户的默认 shell
> -g lfs：将 lfs 用户添加到 lfs 组
> -m：为 lfs 用户创建 home 目录
> -k /dev/null：这个参数通过修改输入位置为特殊的空设备来防止从框架目录(默认为 /etc/skel)拷贝文件
> lfs：这是所创建的组和用户的实际名字

修改密码：`passwd lfs`

改变目录的所有权：

``` bash
chown -v lfs $LFS/tools
chown -v lfs $LFS/sources
```
以lfs用户登录，或者直接：

``` bash
su - lfs
```
创建两个新的启动文件：

登陆shell脚本`.bash_profile`：

``` bash
cat > ~/.bash_profile << "EOF"
exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
EOF
```

非登陆shell脚本`.bashrc`：

``` bash
cat > ~/.bashrc << "EOF"
set +h
umask 022
LFS=/mnt/lfs
LC_ALL=POSIX
PATH=/tools/bin:/bin:/usr/bin
export LFS LC_ALL PATH
EOF
```

关于这两个脚本的区别，请参考[DotFiles](https://wiki.debian.org/DotFiles)

使脚本生效，输入：

``` bash
source ~/.bash_profile
```
至此，所有准备工作准备完毕。
