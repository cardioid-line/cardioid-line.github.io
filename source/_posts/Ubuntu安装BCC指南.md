---
title: Ubuntu安装BCC指南
date: 2023-02-23 15:00:00
author: Cardioid
top: false
hide: false
cover: false
toc: false
mathjax: true
summary: 基于eBPF的BCC安装教程
categories: 
tags: 计算机与安全技术
---
# 前言
eBPF（Extended Berkeley Packet Filter，扩展伯克利数据包过滤器），是一个在Linux操作系统中运行沙盒程序的工具，从而让我们可以在Linux操作系统中 ~~为所欲为~~ 进行网络数据包过滤、**安全监控与分析**等。
eBPF本身原理比较复杂 ~~我还没学会~~，详见官网：http://www.ebpf.io/
eBPF会将C语言源代码编译成eBPF字节码，然后钩到Linux操作系统上等待被触发。~~这辈子也别想让程序设计基础63分的我写C~~ 。为了能够使用Python等更加高级的语言使用eBPF的功能，开发人员设计了一些eBPF工具链以解放程序员，例如**BCC**、Cilium、Bpftrace等。
BCC（BPF Compiler Collection）是一个基于eBPF的工具包，该工具包提供了一些库，使得我们可以利用Python写eBPF程序，还提供了一些命令行和**示例** ~~感谢这些示例，我连Python都不用写啦！~~。
**本文介绍如何在Ubuntu**（最常见的一个Linux操作系统）**中安装BCC。** 读至文章末尾，相信你一定可以运行BCC示例。
# 开始之前
你需要确保
+ 在VmWare中安装好Ubuntu（版本号：22.04 LTS）虚拟机；
+ 保证虚拟机网络正常；（如果不正常可以重启一下虚拟机碰碰运气）
+ 安装VMware Tools；（这个不是必须的，但不弄好多别扭啊）
+ 换源！[ubuntu换源_须臾所学的博客-CSDN博客](https://blog.csdn.net/qq_45878098/article/details/126037838)
+ 记得随时拍快照。
# 安装依赖
下面的命令行均在主目录的终端执行。
先更新升级。
``` powershell
sudo apt-get update
sudo apt-get upgrade
```
将Python的默认版本改为python3。
``` powershell
sudo apt-get install python-is-python3
```
安装如下依赖。
``` powershell
sudo apt-get -y install bison
sudo apt-get -y install build-essential
sudo apt-get -y install cmake
sudo apt-get -y install flex
sudo apt-get -y install git
sudo apt-get -y install libedit-dev
sudo apt-get -y install zlib1g-dev
sudo apt-get -y install libelf-dev
sudo apt-get -y install python3-distutils
sudo apt-get install llvm-12
sudo apt-get install libclang-12-dev
```
# 编译安装
在虚拟机里进入网页[Releases · iovisor/bcc (github.com)](https://github.com/iovisor/bcc/releases)，下载安装包，把安装包解压放在主目录。
此时你的主目录里面有个bcc-src-with-submodule的文件夹，该文件夹内部有bcc文件夹，bcc文件夹内部有很多文件夹和文件。
注意：**此处不能在物理机下载安装包再用VMware Tools把安装包复制到虚拟机内部，这样会报莫名其妙的错误！
下面的命令行均在bcc-src-with-submodule的终端执行。
``` powershell
mkdir bcc/build
cd bcc/build
cmake ..
make
sudo make install
cmake -DPYTHON_CMD=python3 ..
pushd src/python/
make
sudo make install
popd
export PYTHONPATH=$(dirname `find /usr/lib -name bcc`):$PYTHONPATH
```
# 大功告成
在bcc-src-with-submodule的终端执行下面的命令行。
``` powershell
cd /usr/share/bcc/tools
ls
```
你应当看到下图这些BCC示例。
<img src="/images/2023/3.png" width="50%" height="50%"/>
这些示例均可以直接使用。以cachestat为例，命令行如下。
``` powershell
sudo ./cachestat
```
如果可以看到终端中正在生成如下的表格，则表明安装成功！
<img src="/images/2023/4.png" width="30%" height="30%"/>
# 注
本文参考了：[Ubuntu 18.04 LTS上编译安装BCC_银杏丷的博客-CSDN博客](https://blog.csdn.net/weixin_43793731/article/details/128364192)，对此表示感谢！
如有问题可在评论区提问。~~如果我刚好能解答的话我就解答了~~