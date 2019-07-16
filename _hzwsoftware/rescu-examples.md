---
title: "RESCU案例"
permalink: /hzwsoftware/rescu-examples/
excerpt: "RESCU案例"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## Si体系(无自旋体系degenerate)的自洽、能带、态密度计算
### 一、自洽计算
- #### 1. 进入指定文件夹及查看相关文件

&emsp;&emsp;使用`cd`命令进入案例Si体系的文件夹:`/home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL`
使用`ls`命令查看`REAL`目录下的文件，对于自洽计算而言需要准备主要参数文件`scf.input`，原子结构文件`Atom.xyz`和基组文件`Si_DZP.mat`。

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

- #### 2. 使用pbs排队系统进行自洽计算

&emsp;&emsp;在一体机中使用的是pbs排队系统来提交RESCU计算任务，相应的命令都写在rescu.pbs脚本中，而rescu.pbs脚本存放在`/home/hzwtech/Desktop/pbs_script_example`目录下。

&emsp;&emsp;使用cp命令将rescu.pbs文件复制到`/home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL`。
```sh
[hzwtech@hzwtech REAL]$ cd /home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL
[hzwtech@hzwtech REAL]$ cp /home/hzwtech/Desktop/pbs_script_example/rescu.pbs .
[hzwtech@hzwtech REAL]$ ls # 使用ls命令查看是否复制成功。
Atom.xyz  band.input  dos.input  rescu.pbs  scf.input  Si_DZP.mat
```

&emsp;&emsp;在`/home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL`目录下使用`qsub rescu.pbs`进行自洽计算，并通过`qstat`命令进行计算任务查看。
```sh
[hzwtech@hzwtech REAL]$ qsub rescu.pbs
23.hzwtech
[hzwtech@hzwtech REAL]$ qstat
Job ID                    Name             User            Time Use S Queue
------------------------- ---------------- --------------- -------- - -----
23.hzwtech                 rescu_job_name   hzwtech                0 R batch
```

- #### 3.	输出日志文件及结果查看
&emsp;&emsp;自洽结束后，在计算目录会产生输出日志文件：resculog.out文件和计算结果文件夹results，在results中存在着主要输出文件Si_scf.mat和Si_scf.h5。

- 如果需要修改scf.input或者Atom.xyz文件，可以通过使用vi命令实现。
- 真实的计算案例中请用户谨慎的选取实空间格点数目domain.lowres及倒空间格点数目 kpoint.gridn，这两个参数与计算结果的精度和可靠性休戚相关，其中domain.lowres尽量不大于0.4。
- 自洽计算的输出文件Si_scf.mat和Si_scf.h5需要使用matlab来加载，如果一体机中用户没有安装matlab，用户可以将Si_scf.mat和Si_scf.h5下载到本地电脑用matlab进行加载查看。
- 在计算过程中用户需要查看resculog.out文件中## PARALLEL ##的内容来确认是否开启了mpirun并行和scalapack并行，resculog.out文件中：Number of processes = 4表示计算中mpirun并行所调用的核数；Parallel config (smi,gpu,k-para) = (1,0,0) 表示开启了scalapack并行进行加速，其中smi表示的就是scalapack。在计算过程中请务必确保并行信息无误。
{: .notice}

### 二、 能带计算

- #### 1.	进入指定文件夹及查看相关文件
&emsp;&emsp;使用cat命令查看band.input文件：
```sh
[hzwtech@hzwtech REAL]$ cat band.input
info.calculationType = 'band-structure'
info.savepath        = './results/Si_bs'
rho.in               = 'results/Si_scf'
```
- #### 2.	使用pbs排队系统进行自洽计算
&emsp;&emsp;能带计算的输入文件为band.input文件，因此需要修改下提交任务的脚本，为了不覆盖之前使用的rescu.pbs脚本。我们使用cp命令将rescu.pbs复制并重命名为band.pbs。

```sh
[hzwtech@hzwtech REAL]$ cp rescu.pbs band.pbs
```

&emsp;&emsp;`vi`打开band.pbs，将最后一行中`rescu --smi -i scf.input`改为`rescu --smi -i band.input`。

```sh
[hzwtech@hzwtech REAL]$ cat band.pbs
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
mpirun --map-by node:PE=1 matlab -nojvm -nosplash -nodisplay -singleCompThread -r "addpath(genpath('/software/rescu-2.1.0/src'));rescu --smi -i band.input"
```

&emsp;&emsp;在`/home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL`目录下使用`qsub band.pbs`进行自洽计算，并通过`qstat`命令进行计算任务查看。
```sh
[hzwtech@hzwtech REAL]$ qsub band.pbs
23.hzwtech
[hzwtech@hzwtech REAL]$ qstat
Job ID                    Name             User            Time Use S Queue
------------------------- ---------------- --------------- -------- - -----
24.hzwtech                 rescu_job_name   hzwtech                0 R batch
```

&emsp;&emsp;在任务计算过程中可以通过`tail –f`命令来实时输出resculog.out文件的内容：（`tail –f`命令可以通过`Ctrl + c`命令退出）

- #### 3.	输出日志文件及结果查看

&emsp;&emsp;能带计算结束后，在计算目录会产生输出日志文件：resculog.out文件，能带数据文件BandStructure.txt和计算结果文件夹results，在results中存在着主要输出文件会产生主要的输出文件：Si_bs.mat和Si_bs.h5。

- #### 4.	生成能带图及能带数据分析

&emsp;&emsp;计算结束之后，用户可以使用Si_bs.mat生成能带图，具体操作如下：
首先，加载RESCU环境：
```sh
module load Matlab/R2015B
matlab -nojvm -nosplash -nodisplay -singleCompThread -r "addpath(genpath('/software/rescu-2.1.0/src'));"
```
&emsp;&emsp;之后，在计算目录输入：`rescu -p ./results/Si_bs.mat`命令调用RESCU进行画图，其中-p为plot的简写，`./results/Si_bs.mat`表示调用当前文件夹下results文件夹下的Si_bs.mat文件。

&emsp;&emsp;两条命令运行结束之后计算目录已经生成了Si_bs_plot.fig，将该文件在matlab中打开，如果用在一体机中没有安装matlab，建议用户将该文件下载到有matlab的电脑上进行查看。

&emsp;&emsp;得到能带结构图如下：

![image-center](/assets/images/rescu-image/4-1-Si-band-energy.png){: .align-center}
<center> 图1. Si的能带图</center>

&emsp;&emsp;使用`cat`命令查看BandStructure.txt文件，可以看到BandStructure.txt有524行数据，第1行只有一个数字12，代表高对称点数目；第2行是高对称点的字母表示；第3行是高对称点对应的序号；第4行有三个数字260、8、1，其分别表示K点数目能带数目、体系是否有自旋，1表示体系没有自旋；第5-264行是260个K点的坐标；第265-524行是8条能带的数据。

注：能带计算需要调用自洽计算的结果，因此必须要先进行自洽计算之后才能进行能带计算。
{: .notice}

### 三、 态密度计算

- #### 1.	进入指定文件夹及查看相关文件

&emsp;&emsp;使用cat命令查看dos.input文件：
```sh
[hzwtech@hzwtech REAL]$ cat dos.input
info.calculationType = 'dos'
info.savepath        = './results/Si_dos'
rho.in               = './results/Si_scf'
kpoint.sampling ='tetrahedron+blochl'
kpoint.gridn         = [15,15,15]
smearing.sigma = 0.01;
dos.range = [-5 , 5]
dos.resolution = 0.01
smearing.sigma = 0.01
units.energy = 'eV'
```

- #### 2.	使用pbs排队系统进行自洽计算

&emsp;&emsp;态密度计算的输入文件为dos.input文件，同样的需要修改下提交任务的脚本。我们使用cp命令将rescu.pbs复制并重命名为dos.pbs。

```sh
[hzwtech@hzwtech REAL]$ cp rescu.pbs dos.pbs
```

&emsp;&emsp;`vi`打开dos.pbs并将`rescu --smi -i scf.input`改为`rescu --smi -i dos.input`。
在`/home/hzwtech/Desktop/software_test_example/rescu_example_Si/REAL`目录下使用`qsub dos.pbs`进行能带计算，并通过`qstat`命令进行计算任务查看。

- #### 3.	输出日志文件及结果查看

&emsp;&emsp;态密度计算结束后，在计算目录会产生输出日志文件：resculog.out文件，态密度数据文件DensityOfStates.txt和计算结果文件夹results，在results中存在着主要输出文件会产生主要的输出文件：Si_dos.mat和Si_dos.h5。

- #### 4.	生成能带图及能带数据分析

&emsp;&emsp;计算结束之后，用户可以使用Si_bs.mat生成能带图，具体操作如下：
首先，加载RESCU环境：
```sh
module load Matlab/R2015B
matlab -nojvm -nosplash -nodisplay -singleCompThread -r "addpath(genpath('/software/rescu-2.1.0/src'));"
```
&emsp;&emsp;之后，在计算目录输入：`rescu -p ./results/Si_dos.mat`命令调用RESCU进行画图，其中-p为plot的简写，`./results/Si_bs.mat`表示调用当前文件夹下results文件夹下的Si_bs.mat文件。

&emsp;&emsp;两条命令运行结束之后计算目录已经生成了Si_dos_plot.fig，将该文件在matlab中打开，如果用在一体机中没有安装matlab，建议用户将该文件下载到有matlab的电脑上进行查看。

得到态密度结构图如下：
![image-center](/assets/images/rescu-image/4-2-Si-dos.png){: .align-center}
<center>图2. Si的总态密度图</center>

&emsp;&emsp;使用`cat`命令查看DensityOfStates.txt文件，可以看到DensityOfStates.txt有1001行数据，第1行有三个数字1000 1 1，1000代表指定能量范围内的能量点个数、1代表总态密度、1代表degenerate；第2-6表示是否开了投影态密度；7-1006行表示每一个能量点对应的态密度值。

注：态密度计算只需要调用自洽计算的结果，并不需要能带计算结果，这里放在能带计算之后只是个人习惯而已。这里只演示了总态密度计算的过程。投影态密度、局域态密度的等更多计算案例请详见RESCU应用教程。
{: .notice--warning}
