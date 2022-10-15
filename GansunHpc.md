---
tags:
- lizhaoguo
author:
- lizhaoguo
title:
- 省计算中心信息
---

# 省计算中心相关信息 #

PS. 每次打开docx太麻烦，直接写个md得了

## 编译器和库位置 ##

编译器、运行库、环境

```shell
# Intel 编译器
$ /public/software/compiler/intel/intel-compiler-2017.5.239 
# MKL
$ /public/software/compiler/intel/intel-compiler-2017.5.239/mkl
# openmpi
$ /public/software/mpi/openmpi
# intelmpi
$ /public/software/mpi/intelmpi

# .bashrc配置

# 激活Intel编译器
$ source /public/software/compiler/intel/intel-compiler-2017.5.239/bin/compilervars.sh intel64

# 激活MKL
$ source /public/software/compiler/intel/intel-compiler-2017.5.239/mkl/bin/mklvars.sh intel64

# 激活openmpi
$ source /public/software/profile.d/mpi_openmpi-intel-2.1.2.sh

# 激活intelmpi
$ source /public/software/profile.d/mpi_intelmpi-2017.4.239.sh
# or
$ source /public/software/mpi/intelmpi/2017.4.239/bin64/mpivars.sh intel64
```

## srun 提交任务 ##

sinfo:</br>

- 结果分析：

  |PARTITION|AVAIL|TIMELIMIT|NODES|STATE|NODELIST|
  |---|---|---|---|---|---|
  |队列名|up可用，inact不可用|时长限制|节点数|节点状态idle是空闲节点，alloc是已被占用节点，comp是正在释放资源的节点，其他状态的节点都不可用|节点列表|

- 常用选项:

  |示例|功能|
  |---|---|
  |sinfo -n node1|node1使用情况|
  |sinfo -p com|com队列情况|

squeue:</br>

- 结果分析:

  |JOBID|PARTITION|NAME|USER|ST|TIME|NODES|NODELIST|
  |---|---|---|---|---|---|---|---|
  |作业ID|队列名|作业名|用户|作业状态R表示正常运行，PD表示在排队，CG表示正在退出，S是管理员暂时挂起|作业运行时间|作业使用的节点数|于运行作业（R状态）显示作业使用的节点列表；对于排队作业（PD状态），显示排队的原因|
  
- 常用选项:

  | 命令示例           | 功能                              |
  | ------------------ | --------------------------------- |
  | squeue -j 123456   | 查看作业号为123456的作业信息      |
  | squeue -u paratera | 查看超算账号为 paratera的作业信息 |
  | squeue –p com      | 查看提交到com队列的作业信息       |
  | squeue -w node1    | 查看使用到node1节点的作业信息     |

srun:</br>

- 常用选项:

  | 命令示例     | 功能                                |
  | ------------ | ----------------------------------- |
  | -N 3         | 指定节点数为3                       |
  | -n 20        | 指定进程数为20                      |
  | -c 20        | 指定每个进程（任务）使用的CPU核为20 |
  | -p com       | 指定提交作业到com队列               |
  | -w node[1-2] | 指定提交作业到 node1、node2 节点    |
  | -x node[1-2] | 排除 node1、node2 节点              |
  | -o out.log   | 指定标准输出到 out.log 文件         |
  | -e err.log   | 指定重定向错误输出到 err.log 文件   |
  | -J JOBNAME   | 指定作业名为 JOBNAME                |
  | -t 20        | 限制运行 20 分钟                    |

sbatch:</br>

- 示例脚本(参数与srun相同)

  ```shell
  #!/bin/bash 
  #SBATCH -N 2 
  #SBATCH -n 4 
  #SBATCH -c 20 
  #SBATCH -t 60 
  
  srun -n 4 A.exe 
  ```

scancel:</br>

| 命令示例           | 功能                        |
| ------------------ | --------------------------- |
| scancel 123456     | 取消作业号为123456的作业    |
| scancel -n test    | 取消作业名为test的作业      |
| scancel -p com     | 取消提交到com队列的作业     |
| scancel -t PENDING | 取消正在排队的作业          |
| scancel-w node1    | 取消运行在node1节点上的作业 |
| scancel-u          | 取消指定用户的作业          |
