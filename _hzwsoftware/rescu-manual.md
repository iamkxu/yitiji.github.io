---
title: "RESCU软件使用教程"
permalink: /hzwsoftware/rescu-manual/
excerpt: "RESCU软件使用教程"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## RESCU软件介绍

&emsp;&emsp;RESCU(Real space Electronic Structure CalcUlator)，由于其新颖的算法和超高的并行效率，使其在大体系下有着很抢眼的表现，在使用相同资源及不牺牲精度的情况下能够计算更大体系的电子结构；同时 RESCU 适用于各类计算资源，从笔记本电脑、桌面单机、到16核、64核、256核、到更大的超算、包括用 GPU 加速等等，可以用来计算一千、数千、上万乃至更大体系的电子结构性质。RESCU 是解决超大体系 KS-DFT 问题的里程碑式的新方法，正在被应用于金属、半导体、绝缘体、液体、DNA、1维、2维、3维、表面、分子、磁性、非磁性、杂质、固体等等不同系统的 KS-DFT 计算。

## RESCU安装环境

&emsp;&emsp;一体机版RESCU安装目录为：`/software/rescu-2.1.0`。在`/public`目录下我们还安装了一些运行RESCU所需要的软件，如openmpi，scalapack，lapack，openblas，libxc，mcr等。用户可以在Linux终端中使用`module load`和`module unload`命令加载或卸载RESCU运行所需的环境变量配置文件，即运行：`module load mpi/openmpi-1.8.5-RESCU`。此时可以通过`which` 命令确认环境是否加载完成。需要注意的是，一体机中安装了多个mpi，使用特定mpi时请先`module unload`当前的mpirun例如：

```sh
[hzwtech@hzwtech ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
[hzwtech@hzwtech ~]$ module unload mpi/openmpi-1.8.5
[hzwtech@hzwtech ~]$ module load mpi/openmpi-1.8.5-RESCU
[hzwtech@hzwtech ~]$ which mpirun
/public/mpi/openmpi-1.8.5-RESCU/bin/mpirun
```

## RESCU软件使用
&emsp;&emsp;一体机版RESCU与单机版RESCU在使用上基本一致，唯一的区别在于一体机版的RESCU是在Linux下使用命令行的方法运行RESCU，而单机版的RESCU是在matlab中输入运行命令运行RESCU。因此我们将选取RESCU应用教程中Si体系为例，通过自洽、能带和态密度的计算为用户演示一体机版RESCU的使用。其他RESCU应用教程中的案例用户可以根据该案例自行练习。

**注：** 该简单教程为使用终端（terminal）进行RESCU计算，请用户先鼠标右键打开一个终端（terminal）。
{: .notice--warning}

### 在terminal终端下运行RESCU软件

#### 1. 进入指定文件夹及查看相关文件

&emsp;&emsp;使用`cd`命令进入案例的文件夹:`/home/hzwtech/Desktop/software_test_example`。解压Si体系案例文件压缩包：`tar -zxvf rescu_example_Si.tar.gz`。切换到Si体系案例文件夹中：`cd rescu_example_Si`。使用`ls`命令查看`REAL`目录下的文件，对于自洽计算而言需要准备主要参数文件`scf.input`，原子结构文件`Atom.xyz`和基组文件`Si_DZP.mat`。

&emsp;&emsp;使用`cat`命令查看`scf.input`文件：

```sh
[hzwtech@hzwtech REAL]$ cat scf.input
info.calculationType = 'self-consistent'
info.savepath        = './results/Si_scf'
domain.latvec        = [[0 2.715 2.715]; [2.715 0 2.715]; [2.715 2.715 0]]
element(1).species   = 'Si'
element(1).path      = './Si_DZP.mat'
atom.xyz         = './Atom.xyz'
kpoint.gridn         = [7,7,7]
domain.lowres        = 0.6
units.length        = 'Angstrom'
```
&emsp;&emsp;使用`cat`命令查看`Atom.xyz`文件：

```sh
[hzwtech@hzwtech REAL]$ cat Atom.xyz
2
AtomType  X Y Z
  Si        0.67883750        0.67883750        0.67883750
  Si        2.03651250        2.03651250        2.03651250
```

#### 2. 使用pbs排队系统进行自洽计算

&emsp;&emsp;在一体机中使用的是pbs排队系统来提交RESCU计算任务，相应的命令都写在rescu.pbs脚本中，而rescu.pbs脚本存放在`/home/hzwtech/Desktop/pbs_script_example`目录下。

&emsp;&emsp;使用cp命令将rescu.pbs文件复制到`/home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL`。
```sh
[hzwtech@hzwtech REAL]$ cd /home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL
[hzwtech@hzwtech REAL]$ cp /home/hzwtech/Desktop/pbs_script_example/rescu.pbs .
[hzwtech@hzwtech REAL]$ ls # 使用ls命令查看是否复制成功。
Atom.xyz  band.input  dos.input  rescu.pbs  scf.input  Si_DZP.mat
```

&emsp;&emsp;使用cat命令查看rescu.pbs文件：
```sh
[hzwtech@hzwtech REAL]$ cat rescu.pbs
#!/bin/sh
#PBS -N rescu_job_name
#PBS -o _out.dat
#PBS -e _err.dat
#PBS -q batch
#PBS -l nodes=1:ppn=1
#PBS -l walltime=240:00:00

# load the needed environment path

module load gcc/4.7.4-RESCU
module unload mpi/openmpi-1.8.5
module load mpi/openmpi-1.8.5-RESCU
module load Matlab/R2015B

NCPU=`wc -l < $PBS_NODEFILE`
cd $PBS_O_WORKDIR

source /software/rescu-2.1.0/barc
mpirun --map-by node:PE=1 matlab -nojvm -nosplash -nodisplay -singleCompThread -r "addpath(genpath('/software/rescu-2.1.0/src'));rescu --smi -i scf.input"
```

- module前面的为pbs排队系统中的固定格式，具体含义详见[作业管理系统pbs介绍](/hzwsoftware/environment-introduce/#作业管理系统)。module命令为加载RESCU计算所需要的环境，这里我们只介绍module之后的命令。
- 第一句：`source /software/rescu-2.1.0/barc`为加载运行RESCU所需的环境变量，该barc文件内容为`export OPENBLAS_NUM_THREADS=1`，意为声明OpenBLAS的进程数。
- 第二句：为使用mpirun和scalapack进行RESCU并行计算。更细节的理解如下：rescu --smi -i scf.input中--smi表示在RESCU计算过程中将使用scalapack并行加速，-i scf.input表示RESCU计算过程中将scf.input作为输入（input）的文件，i即为input的简写。
{: .notice}

&emsp;&emsp;在`/home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL`目录下使用`qsub rescu.pbs`进行自洽计算，并通过`qstat`命令进行计算任务查看。
```sh
[hzwtech@hzwtech REAL]$ qsub rescu.pbs
23.hzwtech
[hzwtech@hzwtech REAL]$ qstat
Job ID                    Name             User            Time Use S Queue
------------------------- ---------------- --------------- -------- - -----
23.hzwtech                 rescu_job_name   hzwtech                0 R batch
```

#### 3.	输出日志文件及结果查看

&emsp;&emsp;自洽结束后，在计算目录会产生输出日志文件：resculog.out文件和计算结果文件夹results，在results中存在着主要输出文件Si_scf.mat和Si_scf.h5。

更详细的案例请参考[RESCU案例](/hzwsoftware/rescu-examples/)。

注：正确完成计算需要提前准备好License，否则将会出现报错，在`_err.out`文件中可以找到`License validation failed. No license.dat file.`等报错信息。
{: .notice--warning}
