---
title: 常用MD软件安装
auther: lizhaoguo
---

# 常用MD软件安装 #

常用MD软件包括Amber Gromacs等。如果实验室没有安装而你要用的话，鉴于Amber和Gromcas两个软件的安装涉及到编译，总是叫人头大，今天记录一下Amber简单安装和进阶安装吧。</br>
当然，如果你的实验室已经安装了Amber Gromacs之类的软件且你不是初学者的话，这篇文章对你来说作用就不是很大了。</br>
ToDo: Gromacs安装</br>
Tips: 本站采用的是Hexo建站，同一篇文章链接会变动，不要简单的放到收藏夹里就觉得ok了，记得善用搜索

## Amber的简易安装 ##

Amber官网提供了一个手册，跟着手册来处理似乎很容易……然而问题是，默认的安装过程中会从Amber官网获取更新以及下载miniconda安装包，这些都是在国外的网站，很容易因为访问不佳导致安装失败，所以我们需要更贴合实际的操作方法。</br>
Oh, 我忽略了一个重要的点，即Amber是不支持Windows操作系统的。如果你想体验一下的话，建议使用Linux系统。</br>
虽然我推荐新手从Ubuntu入门Linux，但问题在于，在Ubuntu上安装Amber属实是痛苦PlusPlus版，我个人更推荐opensuse或者contos以及ta的近亲fedora等操作系统。</br>
新手不要试图将自己的电脑装成双系统，双系统会使你的痛苦翻倍，入门的话，简简单单装个虚拟机，在虚拟机里面体验一下就好了</br>

### Amber安装前准备 ###

1. 操作系统的选择。有些Linux的版本是存在问题的，对于初学者，只推荐opensuse leap 15.3以及ubuntu 18.04则两个版本的Linux。（Why? 因为这两个操作系统是我踩过坑的，可以给出很多详细的建议。请注意版本号，不要用其他版本号之后又来问我为什么出问题了……）
2. 选择好操作系统之后，确定一下自己的电脑里面有没有gcc g++ gfortran cmake make这几个软件，可以用以下方法来验证

|软件名称|检查是否存在|若不存在的安装方法(Ubuntu 18.04)|若不存在的安装方法(opensuse leap 15.3)|
|---|---|---|---|
|gcc|`which gcc`|`sudo apt install gcc -y`|`sudo zypper in gcc -y`|
|g++|`which g++`|`sudo apt install g++ -y`|`sudo zypper in gcc-c++ -y`|
|gfortran|`which gfortran`|`sudo apt install gfortran -y`|`sudo zypper in gcc-fortran -y`|
|cmake|`which cmake`|`sudo apt install cmake -y`|`sudo zypper in cmake -y`|
|make|`which make`|`sudo apt install make -y`|`sudo zypper in make -y`|

怎样判断是否存在呢？以gcc为例(问我那个 $ 什么意思？淦，回去重读CADD那篇啦)：

```shell
which gcc
```

如果输入完并回车后有这样的显示：

```shell
/usr/bin/gcc
```

就是已经存在了，如果没有这样的显示，就是没有安装需要安装。</br>
3. 当然，你可能会发现，安装软件的时候很慢甚至无法安装，这时候你需要访问镜像站修改你的镜像源以加快访问速度。</br>
常用镜像源有[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn) [北外镜像源](https://mirrors.bfsu.edu.cn)。请牢记这两个镜像源位置，因为在之后的安装过程中也是需要的。</br>
使用方法很简单，比如你要查看opensuse源怎么用，找到页面的opensuse位置，会发现旁边有一个小问号，点击进去就有帮助了。本教程中使用到的镜像都有说明，请勿担心。</br>
4. 还记得我们上面说过的吗？默认的Amber安装会下载miniconda。所以，我们去镜像站下载一个anaconda安装吧，两个差不多但anaconda更好配置conda镜像源。在安装过程的 `init`一步，记得选择`yes`。同样，在安装好后记得修改镜像源</br>
5. 为什么要装conda？是因为我们需要单独创建一个python环境，安装Amber需要的python包。所以我们需要新建一个环境并在新环境里面安装必要的包以及后续安装。我建议新环境的名称是Amber，但你也可以换成其他。在下文中，我们都默认这个环境的名字是amber。</br>

```shell
conda create -n amber
conda activate amber
conda install numpy scipy matplotlib bison flex boost blas zlib libzip python=3.8
```

安装结束
6. 确保在接下来的安装中，都在amber这个虚拟环境中

### Amber的第一次安装 ###

我们先来安装最简单的，即单线程版的Amber。</br>
将从师兄师姐那里拿到的Amber安装包，解压出来，进入文件夹之后，你会看到一个名字叫build的目录，打开里面的run_cmake文件，我们只需要修改自己的那部分就好。</br>
我们主要的修改部分是这几行，不在这几行的请**不要**修改！</br>

```shell
# Assume this is Linux:
...
-DDOWNLOAD_MINICONDA=FALSE \
-DCHECK_UPDATES=FALSE -DPRINT_PACKAGING_REPORT=TRUE \
-DUSE_CONDA_LIBS=TRUE -DBUILD_GUI=FALSE\
-DPYTHON_EXECUTABLE= \
```

我现在来解释一下上面的是啥。</br>
首先，你需要修改的部分位于

```shell
# Assume this is Linux:
```

下面，不然你修改了也是白搭。</br>

其次，`-DDOWNLOAD_MINICONDA=FALSE` 指的是禁止下载miniconda，
`-DCHECK_UPDATES=FALSE` 指的是禁用检查更新，
`-DPRINT_PACKAGING_REPORT=TRUE` 指的是输出安装过程中用到的依赖包的位置，方便安装出问题之后查出问题源头，
`-DUSE_CONDA_LIBS=TRUE` 指的是采用conda环境中的包来编译需要的库
`-DBUILD_GUI=FALSE` 指的是不编译涉及到图形界面的版本。有些时候在有xorg环境下会因为驱动问题报错
`-DPYTHON_EXECUTABLE=` 请注意，在 = 之后是需要你根据自己的实际情况进行填写的。这里填写的部分是上面创建的amber环境中的python位置。比如说：

```shell
$ conda activate amber
$ which python
/home/42/anaconda3/envs/amber/bin/python
```

这一段代码表示，在这台测试机上，amber环境中的python位置是 `/home/42/anaconda3/envs/amber/bin/python`，所以要将上面的改成 `-DPYTHON_EXECUTABLE=/home/42/anaconda3/envs/amber/bin/python \` </br>

请注意，不要随意删除每行末尾的 \ 符号，不然会死的很惨……</br>
修改并保存之后，关闭这个文件，运行以下命令</br>

```shell
./run_cmake
```

这时你会看到很多输出，不要担心，一般没有问题。</br>
当运行结束之后，你会发现这个目录下多出了许多文件以及文件夹，其中包括一个叫Makefile的文件，不要去修改他们。如果你运行完并没有这个文件，请将此时所有的文件打包之后邮件发我一份。<lzgcn@outlook.com> </br>
我们现在假设你已经成功了，在运行结束后，会有一行以`test -f`开头的语句，复制粘贴下来，后面要用。在这个目录下运行这个命令：

```shell
make
```

你可能会看到很多输出，其中很可能有Warning，但不用担心，只要最后没有error就表示没有问题。</br>
我们假设在`make`之后没有报error，我们此时就可以接着运行`make install`命令了。</br>
一般而言，这个过程会比`make`快很多，同样，在结束后没有报error的情况下，修改你的shell配置文件，通常是bash的配置文件，就按下面的语句进行修改并保存即可：

```shell
nano ~/.bashrc
```

将上面保存的`test -f`开头的语句写在这个文件最下方即可。退出登录后重新登录，就可以用Amber了。

### Amber的第二次安装 ###

接下来，就是编译并行版本了</br>
在编译前，首先要安装openmpi。openmpi的安装和常规linux下编译安装软件并无二样。</br>
首先，先去[openmpi官网](https://www.open-mpi.org/)下载安装包。</br>
下载完成后，解压压缩包，创建一个名为build的目录，进入build目录，开始下面的操作</br>

```shell
CC=gcc CXX=g++ FC=gfortran ../configure --prefix=
```

上面的配置中，`CC CXX FC`是和你编译amber时选择的编译器一致，`--prefix=`后面跟着你要把openmpi安装的位置。</br>
在配置完成后，和上面一样，用`make && make install` 就可以安装了。</br>
安装完成后，依然要配置.bashrc文件来是的配置激活。</br>
配置文件可以参考下面这部分</br>

```shell
export OPENMPI=
export PATH=$OPENMPI/bin:$PATH
export LD_LIBRARY_PATH=$OPENMPI/lib:$LD_LIBRARY_PATH
export INCLUDE=$OPENMPI/include:$INCLUDE
export CPATH=$OPENMPI/include:$CPATH
export MANPATH=$OPENMPI/share/man:$MANPATH
```

其中，第一行的OPENMPI后面要跟着上面配置时候的安装位置</br>
在安装openmpi结束，能够通过`which mpirun`找到 mpirun 这个命令且确实是在你的安装位置后，保证**仍然在amber虚拟环境下**，编辑 run_cmake，使得`-DMPI=TRUE`保存，退出，然后就是和上面相同的操作了。</br>
注意，在某些系统上，在make这一步时，会在make python相关包的时候报错。出现这一步时，请找到conda/env/下面对应环境中的ld文件，用系统的ld替换掉即可。</br>

### Amber的第三次安装 ###

到这一步，应该装好了并行版并且替换掉了原有的单线程版的amber。但在实际中，用cuda进行加速才是使用最多的场景。</br>
可以用`sudo lspci | grep 'NVIDIA'`查看自己是否有nvidia显卡`nvidia-smi -q`查看自己是否安装好nvidia驱动，用`which nvcc` 查看是否装好了cuda工具包</br>
在确定自己有nvidia显卡并且装好cuda工具包之后，编辑 run_cmake，使得`-DCUDA=TRUE`，保存，退出，重新像上面一样运行，安装。

### 在计算中心上的操作 ###

一般在计算中心上，会存在多种编译器以及多种mpi库。在这种情况下，openmpi的安装就是可选项。采用计算中心提供的方法，激活编译器以及mpi，就可以完成剩余操作了。</br>
需要注意的是，计算中心支持跨节点并行计算。此时需要依赖他们预先编译好的mpi库才可以实现。因此在计算中心上，**不要**自行编译openmpi。</br>

### Amber的一些可选配置 ###

在run_cmake文件中，有这样一个选项`-DCOMPILER=GNU`，这个选项对应的值，可以选择 "GNU", "INTEL", "PGI", "CRAY", "MSVC", "CLANG" 你可以选自自己熟悉的编译器，也可以选择计算中心提供的编译器。通常GNU与其他的选项编译结果并无较大差别，但也有数据表明，Intel 编译器在Intel的CPU上表现略好于其他。

## Gromacs的简易安装 ##

### 背景简介 ###

Gromacs是另一款常用的分子动力学模拟软件，官方简称为gmx。部分Linux发行版提供官方安装包(单线程版)。在2020年之后的版本中，由于涉及到fftw这些计算库的安装和之前不同，因此个人推荐安装版本为2019.06。[官网下载地址](https://manual.gromacs.org/2019.6/download.html)

### 计算库安装 ###

推荐安装fftw为计算库，[fftw官网下载链接](https://www.fftw.org/download.html)</br>
不同的cpu涉及不同的指令集，而不同的指令集在进行浮点数运算时效率有较大差别。因此，可以先用`lscpu`命令，查看cpu支持的指令集有哪些，随后在fftw的编译安装过程中启用对应的部分，具体可以查看[这个网址](https://www.fftw.org/fftw3_doc/Installation-on-Unix.html)</br>
与前文所述一致，先解压，然后新建build文件夹，随后进入build文件夹之后，进行编译安装。</br>
Gromacs 2019.06 版本，推荐使用下述配置</br>

```shell
../configure --prefix= --enable-sse2 --enable-avx --enable-float --enable-shared 
```

与上文一致，`--prefix`后跟着的是fftw的安装目录，在生成 Makefile 之后，`make && make install` 即可完成安装。</br>
Gromacs依旧依赖mpi以及cuda(虽然都是可选项)，所以如果有对应的部分，请参照上文amber的安装部分进行安装并配置。

### Gromacs安装 ###

首先，依旧是在解压后的目录里，新建一个名为build的文件夹。(这是个好习惯，未来只要涉及到configure 配置并make安装的，这样做可以节省很多麻烦)</br>
进入build文件夹之后，执行下述命令。</br>

```shell
cmake .. -DCMAKE_INSTALL_PREFIX= -DGMX_MPI=ON -DGMX_BUILD_OWN_FFTW=ON -DCMAKE_PREFIX_PATH=
```

此处需要注意的是，与amber简单易懂的命令而言，Gromacs更多的是直面cmake命令。</br>
cmake命令中，以`-D`开头的部分，是配置选项。`-DCMAKE_INSTALL_PREFIX=`后面跟着的就是Gromacs的安装位置，`-DCMAKE_PREFIX_PATH=`后面跟着的就是其他在安装过程中需要的库的所在位置。在此处就是之前fftw的安装路径。</br>
随后执行`make && makeinstall` 命令，即可安装

## NAMD 及同一组织其余软件安装 ##

### NAMD简介 ###

NAMD是伊利诺伊大学香槟分校(ps. 冰白葡萄酒比普通葡萄酒好喝)开发的一款可以进行拉伸分子动力学模拟的软件。</br>
拉伸分子动力学，指的就是给蛋白质小分子复合物一个额外的拉力，看小分子与蛋白质结合是否稳定。</br>

### 安装简介 ###

NAMD的安装，有点简单又有点恶心人。简单的意思是，官网提供了[二进制包](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=NAMD)，直接下载之后，传到服务器上，解压，添加可执行文件到PATH中就能运行。</br>
但是，和一切提供二进制包的工具一样，官方提供的二进制包不一定适合你的机器……所以官方提供了源码供你编译安装。官网也“很贴心”地提供了[编译安装指南](https://www.ks.uiuc.edu/Research/namd/2.14/notes.html#compiling)。但是，恶心的地方降临了……NAMD的多线程实现，并不是依赖mpi的，而是它内置了一个多线程调度机制，而这个机制，有时候是与mpi冲突的！</br>
因此，此处不再对NAMD编译安装提供支持。一切以NAMD官方提供的编译指南为准。

### VMD ###

VMD同样是伊利诺伊大学香槟分校开发的一款软件，不过通常是用于展示分子变化过程以及计算拉伸分子动力学过程中的力与功。</br>
与NAMD的安装类似，官网也提供了一种类似二进制包的[东西](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD)。但是在解压之后，需要进入目录，用root权限，运行`make install`进行安装。如果你运行的是`make`，绝对会报错。

## 后记 ##

很匆忙的完成了这部分，但在动力学模拟中，常用的软件就是上述这些。本文仅涉及安装过程，并不涉及具体运算以及数据处理部分。这些将在未来完成。
