---
title: "计算模拟环境使用简介"
permalink: /hzwsoftware/environment-introduce/
excerpt: "CentOS简介."
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

&emsp;&emsp;鸿之微“一体机”已为您集成了一套完整的计算模拟环境，包括：
- 程序编译环境 [GCC](https://gcc.gnu.org/)
- 并行计算模块 [OpenMPI](https://www.open-mpi.org/)
- 作业管理系统 [Torque pbs](http://www.adaptivecomputing.com/products/torque/)
- 环境变量管理工具 [Environment Modules](http://modules.sourceforge.net/)

注：这些软件都是免费的开源的自由软件，存在庞大的用户群，本章针对这些软件做简要的介绍，更详细的使用方法可以在互联网上直接搜索相关信息。
{: .notice--info}

## 预装软件安装路径

&emsp;&emsp;鸿之微“一体机”为您预装的计算和管理软件均安装在以下路径，您可以在有需求时查看：
- `/public # 软件安装文件夹`
- `/public/bin # 可执行软件存放文件夹`
- `/software # 软件安装包存放文件夹`

## 程序编译环境

&emsp;&emsp;鸿之微“一体机”中安装通用编译器GCC，可以编译C语言(gcc)、C++语言(g++)和Fortran语言(gfortran)。如果您有其他计算模拟软件或者自己编写的程序，可以使用上述编译器编译并在鸿之微“一体机”上运行。

例如：您有一个用`Fortran`语言编写的蒙特卡洛模拟程序`mc.f`，首先需要通过网络或者移动存储设备复制到器件模拟一体机上，然后使用命令`gfortran -o mc mc.f`获得一个可执行程序`mc`，最后使用命令`./mc`在鸿之微“一体机”上运行。
{: .notice--success}

## 并行计算模块

&emsp;&emsp;并行计算模块是计算模拟环境中非常重要的一部分，大型的计算模拟程序都支持并行计算，这样可以突破单机限制使计算模拟的速度更快和模拟更大尺度的系统。在鸿之微“一体机”上，我们安装了OpenMPI软件，支持C语言(mpicc)、C++语言(mpic++)和Fortran语言(mpif77和mpif90)。

例如，您有一个用`Fortran`语言编写的并行蒙特卡洛模拟程序`mcp.f`，首先需要通过网络或者移动存储设备复制到鸿之微“一体机”上，然后使用命令`mpif90 -o mcp mcp.f`获得一个可执行程序`mcp`，最后使用命令`mpirun -n 8 ./mcp`在鸿之微“一体机”上运行`8`核并行计算（并行数量可以自行修改，最好不要超过“一体机”的总核数）。
{: .notice--success}

&emsp;&emsp;鸿之微“一体机”针对不同计算软件分别安装不同并行计算环境，如果需要使用并行计算，需针对特定的软件在计算前手动加载环境变量，详见后文[环境变量加载](/hzwsoftware/environment-introduce/#环境变量加载)。

## 环境变量加载

&emsp;&emsp;在 Linux 超算平台上，通常会安装有不同版本的多种编译器和其他软件等，如常用的编译器有 intel 和 gnu，常用的 MPI 并行库包括 intel mpi，openmpi，mpich2 等，而且对于同一软件，还包含不同的版本或采用不同编译设置得到的可执行程序和链接库等。在使用这些程序时，经常需要对环境变量进行修改。并且由于程序编译时会调用不同类型编译器或第三库，这时程序之间还存在着依赖关系。这使得当执行某个特定版本的程序时，环境变量的修改变得十分复杂。

&emsp;&emsp;Environment Modules软件包是一个简化 shell 初始化的工具，它允许用户在使用 modulefiles 进行会话期间轻松修改其环境。每个模块文件都包含为应用程序配置 shell 所需的信息。鸿之微“一体机”以为您预装Environment Modules软件包，Modules软件包安装路径为`/public/modules-4.2.2`。您可以用以下命令修改您需要加载的环境变量。

```sh
module avial                   #当前已有modules
module list                    #当前加载modules列表
module load module-name       #加载module-name
moudule unload module-name    #卸载module-name
module switch module-name     #切换到module-name
```

## 作业管理系统

&emsp;&emsp;作业管理系统提供计算作业的提交、调度、执行等功能，对于大型计算机集群作业管理系统是整个系统中非常重要的一部分。在鸿之微“一体机”中已为您预装了常用作业管理系统`Torque PBS`。

&emsp;&emsp;您使用`hzwtech`用户登陆鸿之微“一体机”桌面时，可以在桌面看到一个名为`pbs_script_example`的文件夹。或者，您远程登陆时可以使用`cd`命令切换到路径`/public/pbs_script_example`。在`pbs_script_example`文件夹中存放的是用于不同计算软件的作业管理系统的提交实例。实例脚本的文件名是`nanodcal.pbs`等。

```sh
[hzwtech@hzwtech]$ pwd
/public/pbs_script_example
[hzwtech@hzwtech]$ ls
lammps.pbs  nanodcal.pbs  pbs_example.pbs rescu.pbs  vasp.pbs
```

&emsp;&emsp;以`nanodcal.pbs`文件为例，文件内容如下。
```sh
#!/bin/sh
#PBS -N nanodcal_job_name
#PBS -o _out.dat
#PBS -e _err.dat
#PBS -q batch
#PBS -l nodes=1:ppn=1
#PBS -l walltime=240:00:00

# load the needed environment path

module load gcc/4.7.4-Nanodcal
module unload mpi/openmpi-1.8.5
module load mpi/openmpi-1.8.5-Nanodcal
module load Matlab/R2015B

NCPU=`wc -l < $PBS_NODEFILE`
cd $PBS_O_WORKDIR

mpirun -hostfile $PBS_NODEFILE -n $NCPU matlab -nodisplay -r "nanodcal -parallel -doexit input.txt"
```

- 其中第1行是系统脚本注释，不用修改；第2-7行是作业管理系统参数；第8-15行是需要执行的命令行。
- 第2行定义作业名称，此处为`nanodcal_job_name`，将会显示在作业队列中。如果您有多个作业需要提交，建议修改这个名称以方便在作业队列中区分。
- 第3行和第4行为将程序运行中的标准输出和标准错误，输出到`_out.dat`和`_err.dat`文件中。
- 第5行定义任务提交到哪个队列中，此处为`batch`队列。
- 第6行定义需要的计算资源，即这个作业需要多少个节点`nodes`和每个节点多少个核`ppn`。在鸿之微“一体机”中，通常只有一个计算节点，所以`nodes`此处=`1`，至于`ppn`=几，您可根据需要自行设置，但是不能大于“一体机”的总核数。
- 第7行定义计算时间，即这个计算作业最长允许计算的时间，超过最长时间，任务会被系统自动终止。此处为最长时间是240小时，这个参数您需要根据作业的计算时长自行设定，一般可以设置较大，如`2400:00:00`。
- 第8-12行为使用`module`命令加载`nanodcal`计算中需要加载的环境变量。
- 第13行为统计本次本次计算中需要用到的总核数，以用于`mpirun`并行计算。
- 第14行表示脚本需要从当前目录切换到工作目录，工作目录就是提交作业的目录。这一行是必须的，缺省的当前目录是用户根目录，需要切换到工作目录才能进行计算，这一行必须放在其他需要执行的命令行之前。
- 第15行是需要执行的`nanodcal`并行计算命令行。
- 您可以根据需要把这些行修改成自己需要的计算命令。
{: .notice}

### PBS作业管理系统常用命令
```sh
qsub jobfile1 # 提交任务文件'jobfile1'
qdel id1 # 删除Job ID为'id1'的计算任务
qstat # 查看当前已提交任务计算状态
qnodes # 查看各计算节点硬件状态
```
### PBS作业管理系统作业提交流程

&emsp;&emsp;首先使用`cd`命令切换到路径`/public/pbs_script_example`。当前“一体机”中没有任何计算作业，输入`qstat`命令，可以看到没有任何显示结果。
```sh
[hzwtech@hzwtech]$  qstat
[hzwtech@hzwtech]$
```
&emsp;&emsp;使用`qsub`命令提交6次例子`pbs_example.pbs`，同时可以得到它们的任务编号。
```sh
[hzwtech@hzwtech]$ qsub pbs_example.sh
11.hzwtech
[hzwtech@hzwtech]$ qsub pbs_example.sh
12.hzwtech
[hzwtech@hzwtech]$ qsub pbs_example.sh
13.hzwtech
[hzwtech@hzwtech]$ qsub pbs_example.sh
14.hzwtech
[hzwtech@hzwtech]$ qsub pbs_example.sh
15.hzwtech
[hzwtech@hzwtech]$ qsub pbs_example.sh
16.hzwtech
```
&emsp;&emsp;接下来再次输入`qstat`命令查看计算队列。
```sh
[hzwtech@hzwtech]$ qstat
Job ID         Name        User         Time Use  S Queue
-------------- ----------- ----------- ---------- - -----
11.hzwtech     example     hzwtech             0  R batch
12.hzwtech     example     hzwtech             0  R batch
13.hzwtech     example     hzwtech             0  R batch
14.hzwtech     example     hzwtech             0  Q batch
15.hzwtech     example     hzwtech             0  Q batch
16.hzwtech     example     hzwtech             0  Q batch
```
&emsp;&emsp;现在可以在计算队列中看到刚才提交的6个作业，每个作业申请8核，如果一体机有24核，那么将会只有前三个作业在计算`R`，后三个作业还在排队`Q`（倒数第二列表示计算状态）。

&emsp;&emsp;如果想要删除一个作业，要先确定任务编号，然后运行`qdel`命令，随后查看计算队列确认被中止。
```sh
[hzwtech@hzwtech]$ qdel 16
[hzwtech@hzwtech]$ qstat
Job ID         Name        User         Time Use  S Queue
-------------- ----------- ----------- ---------- - -----
11.hzwtech     example     hzwtech             0  R workq
12.hzwtech     example     hzwtech             0  R workq
13.hzwtech     example     hzwtech             0  R workq
14.hzwtech     example     hzwtech             0  Q workq
15.hzwtech     example     hzwtech             0  Q workq
16.hzwtech     example     hzwtech             0  C workq
```
&emsp;&emsp;现在可以在看到编号为16计算作业的状态发生变化，从原来的等待状态`Q`变成完成状态`C`，说明任务16已经中止，不会被执行。

&emsp;&emsp;等待几分钟后，再次查看计算队列，所有提交的例子都已经完成。
```sh
hzw@hzwtech:~/pbs_guide> qstat
Job ID         Name        User         Time Use  S Queue
-------------- ----------- ----------- ---------- - -----
11.hzwtech     example     hzwtech      00:00:00  C workq
12.hzwtech     example     hzwtech      00:00:00  C workq
13.hzwtech     example     hzwtech      00:00:00  C workq
14.hzwtech     example     hzwtech      00:00:00  C workq
15.hzwtech     example     hzwtech      00:00:00  C workq
16.hzwtech     example     hzwtech             0  C workq
```
&emsp;&emsp;鸿之微“一体机”祝您使用愉快 :smile: ！
