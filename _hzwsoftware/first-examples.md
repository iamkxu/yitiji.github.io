---
title: "FIRSTt软件的使用介绍"
permalink: /hzwsoftware/first-examples/
excerpt: "FIRST软件的使用介绍"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## RESCU软件介绍
&emsp;&emsp;RESCU(Real space Electronic Structure CalcUlator)，由于其新颖的算法和超高的并行效率，使其在大体系下有着很抢眼的表现，在使用相同资源及不牺牲精度的情况下能够计算更大体系的电子结构；同时 RESCU 适用于各类计算资源，从笔记本电脑、桌面单机、到16核、64核、256核、到更大的超算、包括用 GPU 加速等等，可以用来计算一千、数千、上万乃至更大体系的电子结构性质。RESCU 是解决超大体系 KS-DFT 问题的里程碑式的新方法，正在被应用于金属、半导体、绝缘体、液体、DNA、1维、2维、3维、表面、分子、磁性、非磁性、杂质、固体等等不同系统的 KS-DFT 计算。
## RESCU安装环境
&emsp;&emsp;一体机版RESCU安装目录为：`/software/rescu-2.1.0`。在`/public`目录下我们还安装了一些运行RESCU所需要的软件，如openmpi，scalapack，lapack，openblas，libxc，mcr等。在该目录下的RESCU_env文件即为RESCU运行所需的环境变量配置文件。用户可以在Linux终端中使用`source`命令加载该配置文件，即运行：`source /public/Hzwtech/RESCUPackage/RESCU_env`。此时可以通过`which` 命令确认环境是否加载完成。例如：
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```

## RESCU软件使用
&emsp;&emsp;一体机版RESCU与单机版RESCU在使用上基本一致，唯一的区别在于一体机版的RESCU是在Linux下使用命令行的方法运行RESCU，而单机版的RESCU是在matlab中输入运行命令运行RESCU。因此我们将选取RESCU应用教程中Si体系为例，通过自洽、能带和态密度的计算为用户演示一体机版RESCU的使用。其他RESCU应用教程中的案例用户可以根据该案例自行练习。

**注：** 该简单教程为使用终端（terminal）进行RESCU计算，请用户先鼠标右键打开一个终端（terminal）。
{: .notice--warning}

### Si体系(无自旋体系degenerate)
- #### (1) 自洽计算
##### 1. 进入指定文件夹及查看相关文件

&emsp;&emsp;使用`cd`命令进入案例Si体系的文件夹:`/home/hzw/RESCU_example/Si/REAL`
使用`ls`命令查看`REAL`目录下的文件，对于自洽计算而言需要准备主要参数文件`scf.input`，原子结构文件`Si.xyz`和基组文件`Si_DZP.mat`。

&emsp;&emsp;使用`cat`命令查看`scf.input`文件：

```sh
[hzw@hzwhpc examples]$ cat al_lcao_scf.input
LCAO.status = true
info.savepath = 'results/al_lcao_scf'
info.calculationType = 'self-consistent'
atom.element = 1
atom.xyz = [0 0 0]
domain.latvec = ~eye(3)/2*7.652445391; %  positions in angstrom
domain.lowres = 2/3
element(1).species = 'Al'
element(1).path = './Al_AtomicData.mat'
functional.list = {'XC_LDA_X','XC_LDA_C_PW'}
eigensolver.emptyBand = 8
kpoint.gridn = [3,3,3]
option.saveLevel=2

```
&emsp;&emsp;使用`cat`命令查看`Si.xyz`文件：

```sh
[hzw@hzwhpc examples]$ cat Si8.xyz
8
Si8
Si 0.26            0         0
Si 0         5.1052    5.1052
Si 5.1052         0    5.1052
Si 5.1052    5.1052         0
Si 2.5526    2.5526    2.5526
Si 2.5526    7.6578    7.6578
Si 7.6578    2.5526    7.6578
Si 7.6578    7.6578    2.5526
```

##### 2. 使用pbs排队系统进行自洽计算

&emsp;&emsp;在一体机中使用的是pbs排队系统来提交RESCU计算任务，相应的命令都写在rescu.pbs脚本中，而rescu.pbs脚本存放在/home/hzw/bin目录下。

&emsp;&emsp;使用cp命令将rescu.pbs文件复制到/home/hzw/RESCU_example/Si/REAL，并使用ls命令查看是否成功。
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```

&emsp;&emsp;使用cat命令查看rescu.pbs文件：
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```

蓝色方框前面的为pbs排队系统中的固定格式，具体含义详见pbs介绍章节。这里我们只介绍蓝色方框中的两句命令。

&emsp;&emsp;第一句：source /public/Hzwtech/RESCUPackage/RESCU_env 为加载运行RESCU所需的环境变量。

&emsp;&emsp;第二句：mpirun -np 4 RESCU --smi -i scf.input 为使用mpirun和scalapack进行RESCU并行计算，整个计算调用了4个核。更细节的理解如下：

&emsp;&emsp;整句话分解成mpirun -np 4和RESCU --smi -i scf.input两部分，可以理解为mpirun调用RESCU进行计算。其中mpirun –np 4 为mpirun调用4核进行计算，-np为mpirun的内部命令；RESCU --smi -i scf.input中--smi表示在RESCU计算过程中将使用scalapack并行加速，-i scf.input表示RESCU计算过程中将scf.input作为输入（input）的文件，i即为input的简写。因此RESCU --smi -i scf.input表示RESCU将scf.input文件作为输入文件并使用scalapack进行并行加速。

&emsp;&emsp;在/home/hzw/RESCU_example/Si/REAL目录下使用qsub rescu.pbs进行自洽计算，并通过qstat命令进行计算任务查看。

```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```

##### 3.	输出日志文件及结果查看
&emsp;&emsp;自洽结束后，在计算目录会产生输出日志文件：RESCUlog.out文件和计算结果文件夹results，在results中存在着主要输出文件Si_scf.mat和Si_scf.h5。
&emsp;&emsp;Notes:1.如果需要修改scf.input或者Atom.xyz文件，可以通过使用vi命令实现，具体使用见Linux基础。2.真实的计算案例中请用户谨慎的选取实空间格点数目domain.lowres及倒空间格点数目 kpoint.gridn，这两个参数与计算结果的精度和可靠性休戚相关，其中domain.lowres尽量不大于0.4。3.自洽计算的输出文件Si_scf.mat和Si_scf.h5需要使用matlab来加载，如果一体机中用户没有安装matlab，用户可以将Si_scf.mat和Si_scf.h5下载到本地电脑用matlab进行加载查看。4.本自洽计算中只使用了4核进行计算，用户在计算真实案例时可以通过修改-np后面的数字来修改核数。5.在计算过程中用户需要查看RESCUlog.out文件中## PARALLEL ##的内容来确认是否开启了mpirun并行和scalapack并行，如下图所示：
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```
&emsp;&emsp;Number of processes = 4表示计算中mpirun并行所调用的核数；Parallel config (smi,gpu,k-para) = (1,0,0) 表示开启了scalapack并行进行加速，其中smi表示的就是scalapack。在计算过程中请务必确保并行信息无误。

- #### (2) 能带计算

##### 1.	进入指定文件夹及查看相关文件
&emsp;&emsp;使用cat命令查看band.input文件：
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```
##### 2.	使用pbs排队系统进行自洽计算
&emsp;&emsp;能带计算的输入文件为band.input文件，因此需要修改下提交任务的脚本，为了不覆盖之前使用的rescu.pbs脚本。我们使用cp命令将rescu.pbs复制并重命名为band.pbs。
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```
&emsp;&emsp;vi打开band.pbs并将RESCU --smi -i scf.input改为RESCU --smi -i band.input。
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```
&emsp;&emsp;在/home/hzw/RESCU_example/Si/REAL目录下使用qsub band.pbs进行能带计算，并通过qstat命令进行计算任务查看。
```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```
&emsp;&emsp;在任务计算过程中可以通过tail –f 命令来实时输出RESCUlog.out文件的内容：（tail –f命令可以通过Ctrl + c命令退出）

```sh
[hzw@hzwhpc ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
```

##### 3.	输出日志文件及结果查看

&emsp;&emsp;能带计算结束后，在计算目录会产生输出日志文件：RESCUlog.out文件，能带数据文件BandStructure.txt和计算结果文件夹results，在results中存在着主要输出文件会产生主要的输出文件：Si_bs.mat和Si_bs.h5。

##### 4.	生成能带图及能带数据分析

&emsp;&emsp;计算结束之后，用户可以使用Si_bs.mat生成能带图，具体操作如下：
首先，加载RESCU环境：source /public/Hzwtech/RESCUPackage/RESCU_env；
之后，在计算目录输入：RESCU -p ./results/Si_bs.mat命令调用RESCU进行画图，其中-p为plot的简写，./results/Si_bs.mat表示调用当前文件夹下results文件夹下的Si_bs.mat文件。

&emsp;&emsp;两条命令运行结束之后计算目录已经生成了Si_bs_plot.fig，将该文件在matlab中打开，如果用在一体机中没有安装matlab，建议用户将该文件下载到有matlab的电脑上进行查看。

&emsp;&emsp;得到能带结构图如下：

![image-center](/assets/images/rescu-image/4-1-Si-band-energy.png){: .align-center}
<center>图4-1 Si的能带图</center>

&emsp;&emsp;使用cat命令查看BandStructure.txt文件，可以看到BandStructure.txt有524行数据，第1行只有一个数字12，代表高对称点数目；第2行是高对称点的字母表示；第3行是高对称点对应的序号；第4行有三个数字260、8、1，其分别表示K点数目能带数目、体系是否有自旋，1表示体系没有自旋；第5-264行是260个K点的坐标；第265-524行是8条能带的数据。

**Notes**：能带计算需要调用自洽计算的结果，因此必须要先进行自洽计算之后才能进行能带计算。更多的能带计算参数请详见RESCU应用教程。
{: .notice--warning}

- #### (3) 态密度计算

##### 1.	进入指定文件夹及查看相关文件

&emsp;&emsp;使用cat命令查看dos.input文件：

##### 2.	使用pbs排队系统进行自洽计算

&emsp;&emsp;态密度计算的输入文件为dos.input文件，同样的需要修改下提交任务的脚本。我们使用cp命令将rescu.pbs复制并重命名为dos.pbs。

&emsp;&emsp;vi打开dos.pbs并将RESCU --smi -i scf.input改为RESCU --smi -i dos.input。
在/home/hzw/RESCU_example/Si/REAL目录下使用qsub dos.pbs进行能带计算，并通过qstat命令进行计算任务查看。

##### 3.	输出日志文件及结果查看

&emsp;&emsp;态密度计算结束后，在计算目录会产生输出日志文件：RESCUlog.out文件，态密度数据文件DensityOfStates.txt和计算结果文件夹results，在results中存在着主要输出文件会产生主要的输出文件：Si_dos.mat和Si_dos.h5。

##### 4.	生成能带图及能带数据分析

&emsp;&emsp;计算结束之后，用户可以使用Si_dos.mat生成能带图，具体操作如下：
首先，加载RESCU环境：source /public/Hzwtech/RESCUPackage/RESCU_env；
之后，在计算目录输入：RESCU -p ./results/Si_dos.mat命令调用RESCU进行画图。
两条命令运行结束之后计算目录已经生成了Si_dos_plot.fig，将该文件在matlab中打开，如果用在一体机中没有安装matlab，建议用户将该文件下载到有matlab的电脑上进行查看。
得到能带结构图如下：
![image-center](/assets/images/rescu-image/4-2-Si-dos.png){: .align-center}
<center>图4-2 Si的总态密度图</center>

&emsp;&emsp;使用cat命令查看DensityOfStates.txt文件，可以看到DensityOfStates.txt有1001行数据，第1行有三个数字1000 1 1，1000代表指定能量范围内的能量点个数、1代表总态密度、1代表degenerate；第2-6表示是否开了投影态密度；7-1006行表示每一个能量点对应的态密度值。

Notes：态密度计算只需要调用自洽计算的结果，并不需要能带计算结果，这里放在能带计算之后只是个人习惯而已。这里只演示了总态密度计算的过程。投影态密度、局域态密度的等更多计算案例请详见RESCU应用教程。
{: .notice--warning}
