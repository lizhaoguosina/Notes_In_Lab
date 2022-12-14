---
title: CADD基础操作指南
auth: 李兆国
---

# 写在前面 #

本文是结合本人在两个CADD实验室中，对于两个实验室新生的观察和分析，总结的小白入门共通问题。</br>
本文旨在写出一个类似手册样的文章，便于大家进行检索和使用。</br>
本文有参考 唐赟教授 所著《药物设计学》以及《药学专业实验》两书中关于CADD部分，在此谨向唐赟教授致以敬意与感谢。</br>
本文若有不足及谬误之处，欢迎邮件至lzgcn@outlook.com探讨，本人将不胜感激。

## Linux基础问题 ##

Linux操作系统是大多CADD软件需要的操作系统环境。通常有Ubuntu为代表的Debian系统和以CentOS为代表的RedHat系统。两者在安装软件时略有差别，但作为初学者并不需要了解这些。</br>
Linux系统通常只有命令行模式(CLI)，并不具有Windows的桌面环境(GUI)，所以需要掌握命令行的操作。每开一个命令行窗口，我们可以认为新建了一个“终端”(terminal)。</br>
Linux系统通常可以使用ssh服务登录到远程服务器上进行任务提交和运行。
常用的ssh软件有XShell、putty以及Windows自带的powershell都可以运行。
其中Xshell是收费软件，但可以申请free的学术版本或家用版本。其他软件为free版本，可以自由使用。</br>

### Linux常用命令 ###

通常情况下，在各类教程中，以下文这种形式来表示运行命令。
```shell
$ <>
```

$ 开头指的是， 后面的部分是需要在命令行中运行的命令，而<>内的部分是根据自己的具体情况进行更改。</br>
输入命令时**不需要**输入**最开始**的那个$字符。本文也采用相同的表述方式。</br>

```shell
$ man <command>
```
man 命令通常用于查找其他命令的具体说明，在计算机领域有一种缩写叫做 **RTFM** 这是 *Read The Fxxking Man* 的缩写
当你在互联网上提问但收到了这样的缩写，请使用 man 命令仔细查找官方文档说明</br>
通常情况下，man command 和 command --help是等价的，可以自由选择查看方式。

```shell
$ ls -lah </folder>
```
ls 是用来展示指定目录下文件。不带有 /folder 的时候表示当前目录。</br>
常用的参数有-l -a -h 
其中，
-l 表示显示更加全面信息，
-a 表示展示包括隐藏文件在内的所有文件，
-h 表示采用人类友好的形式显示文件大小(采用K、M、G、T等单位输出)</br>
这里有个小问题，对于文件夹而言，默认显示是4.0k大小而不是包含文件夹内文件的大小总和。
如何查看一个文件夹及其中所有文件的大小，我们会在后面详细说明。</br>

```shell
$ du -sh </folder>
```
du命令用来统计一个文件夹及其内部包含的所有文件的大小总和。</br>
常用参数有-s -h
其中，
-s 表示统计所有文件的大小，不带参数则会将所有文件大小输出，
-h 表示使用人类友好的方式输出文件大小(采用K、M、G、T等单位输出)</br>

```shell
$ cd </folder>
```
cd 命令是用来切换当前目录。</br>
有以下几个特殊的文件夹位置需要注意：</br>
. 指的是当前目录</br>
.. 指的是上一级目录(发现没有？这两个的差别是 . 个数的差别)</br>
~ 指的是当前用户的主目录(通常是/home/Username)</br>
cd 后面没有任何参数的话，相当于~参数

```shell
$ cp -Rvf <src> <drc>
```
cp 命令是用来复制粘贴文件(或者我们叫它 拷贝 ？) </br>
src 指的是源文件，drc指的是目标位置</br>
常用的参数有 -R -v -f -i
其中,
-R 表示递归地复制文件及文件夹内的内容， 
-v 表示详细展示复制的文件/文件夹的名字，
-f 表示如果目标位置有同名文件，则覆盖，
-i 表示遇到同名文件则提示是否覆盖，不能与-f命令同时使用</br>

```shell
$ mv <src> <drc>
```
mv 命令是用来移动文件/文件夹位置或者更改文件/文件夹名字。src和drc的含义是一致的。

```shell
$ pwd
```
pwd命令指的是，显示当前目录位置

```shell
$ rm -rvf </folder>
```
rm命令是删除命令。请注意，在这里的删除，并不像Windows一样，有回收站这种可以撤销的操作，运行之后就直接删除了，**无法撤销删除** </br>
常用的参数有 -r -v -f
其中，
-R 表示递归地删除文件及文件夹内的内容， 
-v 表示详细展示删除的文件/文件夹的名字，
-f 表示强制删除，
-i 表示删除每个文件前进行提示，不能与-f命令同时使用</br>

```shell
$ mkdir </folder>
```
mkdir命令是创建文件夹

```shell
$ cat <filename>
```
cat命令指的是查看文件内容。请注意，如果文件内容较多的话，会一次性输出所有文件内容。具体情况就是迅速的划过所有信息，而你只能看到最后几行。

```shell
$ more
```
more命令会将显示内容限制在一页的大小。在显示完一页之后，按回车键(ENTER)可以逐行更新，按空格键可以进入下一行，按q退出显示。

```shell
$ nohup <command> &
```
nohup命令指，command执行之后不再接受键盘的输入。通常会在command最后加上&符号以达到后台运行而不被中断的效果。只要当前终端不关闭即可一直在后台运行下去。

```shell
$ jobs
```
jobs命令可以用来查看之前提交在后台运行的软件的详细情况。如果正常运行则会以Running开头，被中断或者结束可能不会有显示。

```shell
$ which <command>
```
which 命令用于查找某个命令所代表的软件的安装位置。对于系统内置软件，通常位置是在 /bin /usr/bin 等目录下。这个命令依赖一个叫做PATH的变量(有些地方叫它环境变量)

```shell
$ & && | \
```
这几个是shell里面常用到的符号。</br>
其中单个 & 指将前面的命令放在后台进行运行。</br>
两个&&通常用于两条命令之间，让前一条命令成功执行后执行后一条命令。</br>
而|符号(位于ENTER建上方，需要按下Shift键之后再按才能输入)则是利用其形象，在CLI中起到“管道”的作用。即将|前命令的输出作为|后命令的输入。</br>
\符号在不同位置有着不同的含义。</br>
在一行命令的结尾处表示，命令还未输入完毕但这一行已经没有空间了，在输入这个符号之后再按下回车键则可以在下一行输入剩余的命令。但在系统看起来，你只是输入了一整行命令而已。</br>
而如果是在一行命令中出现\符号，则是为了输入一些特定的字符，此时我们将其称之为“转义字符”。比如你需要查看一个文件名中带有&符号的文件，如果你直接输入这个文件名，系统会认为你需要在后台运行命令(还记得上面关于&符号的解释吗？)，此时，输入文件名时就要在文件名前输入\字符以表示此处的&只是个字符，并不代表任何命令。

Linux下常用命令已经介绍结束。现在进入一些常见的报错及处理方式。

### Linux常见报错与处理 ###

注意：及时寻求他人帮忙而不是随意从网上找解决方案通常是更好的选择。倘若寻找了错的解决方法，可能会毁了师兄师姐多年的心血。及时向师兄师姐寻求帮助通常是让所有人更省事的解决方法。

1. 提示command not found，怎么处理？</br>
首先检查一下输入的命令，是否存在拼写错误？是否存在大小写错误？如果前两者的回答均为否，可以使用which命令来检查环境变量内是否真的有这个命令，没有的话请联系管理员来处理(实验室内就是大师兄大师姐啦)
2. 提示permission denied，怎么处理？</br>
首先确认你想要写入的目录是不是在自己这个用户的目录下面。对于一些系统关键的目录，是不允许随意读写的，所以**不要**盲目拷贝网上查到的命令来运行！对于所有带sudo的命令，请务必确认自己知道自己在干什么，以及干完之后可能的后果！
3. 提示我python版本不对/python模块不对，怎么处理？需要自己重新安装吗？</br>
**永远不要**试图更改系统的python版本！python作为构建Linux的重要软件，如果随意更改，很可能造成系统所有软件都不可以使用的严重后果！</br>
对于服务器而言，通常都装有一个叫conda的软件(使用时出现前面所述问题按前文处理)。可以使用conda创建不同的虚拟环境，在虚拟环境中可以很方便更改python版本和构建不同的模块。具体使用见下：</br>
```shell
$ conda create -n <env_name>
```
创建一个新的虚拟环境，env_name按照自己的需要更改
```shell
$ conda activate <env_name>
```
进入环境名为<env_name>的虚拟环境
```shell
$ conda install -c <channel_name> <package1_name> <package2_name>
```
在当前环境中安装名为<package1_name> 和<package2_name>的软件和模块。</br>
其中-c <channel_name>用来指定conda在哪个channel中寻找<package1_name> 和<package2_name>这两个软件或模块。</br>
通常channel可以省略，此时使用的是default，用的比较多的是conda-forge这个channel。
```shell
$ conda deactivate
```
退出当前激活的虚拟环境。
4. 提示GLIBC…… Not Found，怎么处理？</br>
这是一系列的问题，通常是自行编译安装了gcc，然后采用这个gcc编译软件但对应的库没有成功编译。请及时联系管理员(同上，大师兄大师姐)进行解决。**永远不要**从网上找教程试图自行安装对应库。因为C语言(包括C++)也是构建Linux所必不可少的软件，随意更改同样很可能造成系统所有软件都不可以使用的严重后果！</br>

Linux部分先歇口气，接下来进入CADD最常见的部分，分子动力学模拟(MD)的粗浅教程

## 分子动力学模拟(MD) ##

分子动力学模拟(MD)，指用软件模拟蛋白-小分子/蛋白-蛋白/小分子-小分子的生理活动状态。由于模拟软件、初始状态、蛋白处理方式、力场选择、模拟时长的差别，很可能无法完美复现论文的数据。请务必从论文中确认该作者使用的软件和动力学参数。

### 名词解释 ###

系综：这是一个物理化学的词，具体解释可以参考《物理化学(第十一版)》(侯文华 等 译)。在分子动力学模拟过程中，可以用来指代整个体系内的不变量。众所周知，描述一个体系可以使用 NPTV 四个参数进行描述。通常就是限定其中三个参数进行模拟，常用系综包括 NPT NTV NPV 三种。但不同的模拟体系采用的系综有所不同，不同的课题组倾向的系综也有所不同。
力场：这是个可以说是模拟中专用的词。我们知道，分子与分子间存在多种相互作用力，分子内部也存在多种作用力。全部进行量子化学计算显然不现实，可以采用牛顿力学进行一定程度的简化。同样，不同的作用力对于整体的效果也有所不同。因此，采用合适的描述力场是进行成功模拟的第一步。

### Amber ###

Amber作为模拟界的执牛耳者，很多论文都采用此软件进行模拟。通常可以分为模拟以及结果分析两大部分，下文提供官方教程，可以根据链接进行学习和训练。</br>
模拟的具体操作可以跟随这两个官方教程进行：
1. 丙二酸模拟</br>
[原始文档](https://ambermd.org/tutorials/basic/tutorial0/index.php)以及[中文文档](https://jerkwin.github.io/2018/01/17/AMBER%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8BB0-AMBER%E5%88%86%E5%AD%90%E5%8A%A8%E5%8A%9B%E5%AD%A6%E6%A8%A1%E6%8B%9F%E5%85%A5%E9%97%A8/)
2. 模拟绿色荧光蛋白及构建修饰的氨基酸残基</br>
[原始文档](https://ambermd.org/tutorials/basic/tutorial5/index.php)以及[中文文档](https://jerkwin.github.io/2018/01/07/Amber%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8BB5-%E6%A8%A1%E6%8B%9F%E7%BB%BF%E8%89%B2%E8%8D%A7%E5%85%89%E8%9B%8B%E7%99%BD%E5%8F%8A%E6%9E%84%E5%BB%BA%E4%BF%AE%E9%A5%B0%E7%9A%84%E6%B0%A8%E5%9F%BA%E9%85%B8%E6%AE%8B%E5%9F%BA/)

对于结果处理，又可以分为两部分，RMSD分析和结合自由能分析(MMPBSA)。RMSD分析在上文丙二酸模拟中已经有所展示，MMPBSA可以跟随官方教程进行学习：</br>
1. MMPBSA</br>
[原始文档](https://ambermd.org/tutorials/advanced/tutorial3/index.php)以及[中文文档](https://jerkwin.github.io/2018/01/28/AMBER%E9%AB%98%E7%BA%A7%E6%95%99%E7%A8%8BA3-MM-PBSA/)

### Gromacs ###

Gromacs则是进行粗粒化模拟的典型代表，通常是与Martini力场(这个力场就是进行粗粒化处理的力场，不过名字嘛……总让我想起某种鸡尾酒)一起使用。粗粒化模拟的基本原理是将一个氨基酸拆分成主链及侧链两部分，不再分析每部分内部的改变。具体可以从[这个网站](https://jerkwin.github.io/9999/10/31/GROMACS%E4%B8%AD%E6%96%87%E6%95%99%E7%A8%8B/)上学习，或者从《药学专业实验》这本书的对应章节进行学习。

### 常见问题 ###

1. 什么是MPI？怎么用？</br>
MPI是一个经典的并行处理库，所谓并行指的就是，同时跑很多进程或线程进行操作，可以显著降低整个模拟过程的用时。请注意，只有命令结尾带.mpi的命令才能够使用mpi。同样的还有cuda，cuda是用显卡进行加速，只能在有显卡的机器上才能够运行。具体形式如下：</br>
```shell
$ mpirun -np <num> <commands>
```
请注意，-np之后的数字，最好是2的倍数，且不要超过机器的核心数，否则不能加速反而会减速。

我已经很久没有做过模拟了，上述有问题请尽快联系我。接下来进行CADD的第二个大部分：量化计算

## 量子化学计算 ##

作为药学专业的孩子，类药五规则应该是倒背如流了吧？但如何在真实合成前确定一个新的化合物对应的参数则是一大难点。还好量子物理提供了思路，即通过量子化学计算的方法，可以比较精确的确定化合物的种种性质。如果是不依赖任何假设和近似，则也可以称为“第一性原理计算”或者“从头计算”。但实际上通常使用的是密度泛函方法(DFT)进行计算(请注意，此处的DFT并不是 离散傅里叶变化 的缩写，查阅文献时请注意)。</br>
常用软件是Gaussian(高斯)。这个软件是收费软件，在不确定课题组有没有购买前切勿写在英文论文内，否则有概率被销售代理打电话要求购买licence。现在学术界流传较广的版本有09版和16版，使用方法都几乎差不多。</br>
具体可以去[这个网站](http://sobereva.com/)查找具体博文或者去[这个网站](http://bbs.keinsci.com/forum.php)寻求解答，同样可以从《药学专业实验》这本书的对应章节进行学习。

写了一天有点累了，快进到第三部分：深度学习/机器学习/人工智能

## AI方向 ##

在万物皆可AI的情况下，用AI解决CADD的具体问题似乎是个必然的方向。传统的一些算法，如随机森林、支持向量机等，已经有很多现有的软件进行可视化操作，包括但不限于Weka、Orange3 、Matlab等。这些软件几乎做到了，导入数据 --> 选择算法和检验方法 --> 喝咖啡等结果就好，几乎不需要专门写python脚本。但并不是说写脚本不重要，事实上，这些软件能够处理的数据量相较于真实数据量是很小的，可以作为初步分析但完全用来做AI还是有些心有余而力不足。如果可以自己写脚本自己训练则是个不错的选择。</br>
今天最后部分，扫个尾，讲一些杂项

## 杂项 ##

很多时候，你要的初始数据是不存在的。比如某个突变体的蛋白结构，比如一个新的化合物等等。是不是就无法进行处理了？并不是。对于蛋白质而言，现在已经有像AlphaFlod2等计算方法计算其可能的构象，还有种叫做“同源模建”的方法，利用同一蛋白质家族的其他已知蛋白质晶体结构，推测未知的蛋白质结构。对于新化合物和已知蛋白质，或者新蛋白质和已知化合物而言，还有一种叫做分子对接(dock)的方法进行操作，也可以比较好的解释可能的相互作用原理。</br>
这么多项目，有没有什么简单点的操作？有的。有个叫薛定谔(schrodinger)的软件，几乎能够完成上述所有任务。而且软件内带的说明很详细。可以从[这个文档](https://zhuanlan.zhihu.com/p/401697711)开始学习</br>
本文将在作者学习到新知识/发现问题/重新摘抄书籍/喝到比花魁还好喝的咖啡时及时更新
