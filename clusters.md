---
title: clusters_build
top: 1000
tags: SERVER
---

# 将多台较旧的服务器整合成计算集群 #

## 第一次构建NFS ##

选取仍然能够进入系统并储存显著多的服务器，进行NFS安装，并将其挂载出来。</br>
将NFS挂载至其余服务器上，将/home /root 文件夹进行tar压缩储存。</br>
将处理结束的服务器断电，下线，查看是否影响业务。

## 安装OS ##

选择ubuntu 20.04 lts 版进行安装。</br>
集群分为master, NFS, workers 三部分。</br>
master为登录，上传，提交任务的节点，采用desktop系统。</br>
NFS为集群储存位置，上传的文件全部储存在这里。选择ZFS文件系统进行储存(可选，多硬盘)。采用server系统。</br>
worker为真正执行计算的节点，但不需要登录使用，在master上配置免密登录和mpi。采用server系统。</br>

|type|number|harddisk|
|:---:|:---:|:---:|
|master|1/2|1|
|worker|n|1|
|NFS|2|n|

master同时起到网关作用，构建内网以避免污染上游网络。</br>
此处参考raspberry router项目。

## 第二次构建NFS ##

将内容移动至集群的NFS中，格式化硬盘后重新安装NFS，加入集群。

## 挂载NFS ##

将NFS挂载至/data目录，在目录中创建opt home等文件夹并做软链处理。

## 安装软件 ##

软件有Anaconda Amber20 Gromacs 
