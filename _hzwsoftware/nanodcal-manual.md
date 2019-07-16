---
title: "Nanodcal软件使用教程"
permalink: /hzwsoftware/nanodcal-manual/
excerpt: "Nanodcal软件使用教程"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## Nanodcal软件介绍

&emsp;&emsp;Nanodcal是一款基于非平衡格林函数-密度泛函理论（NEGF-DFT）的第一性原理软件，主要应用于模拟器件材料中的非线性、非平衡的量子输运过程，是目前国内唯一一款拥有自主知识产权的基于第一性原理的输运软件。可预测材料的电流-电压特性，电子透射几率等众多输运性质。

&emsp;&emsp;迄今为止，Nanodcal已成功应用于1维、2维、3维、无自旋、共线自旋、非共线自旋材料物性、分子电子器件、自旋电子器件、光电流器件、多电极器件、半导体电子器件设计等重要研究课题中，并将逐步推广到更广阔的电子输运性质研究的领域。

## Nanodcal安装环境

&emsp;&emsp;鸿之微Nanodcal“一体机”已经预装Nanodcal，用户不需要自行安装，可以直接调用。Nanodcal预装在`/software/Nanodcal`目录中，其中包含了Nanodcal的程序、文档和实例。下面将根据案例，介绍如何运行Nanodcal软件。

&emsp;&emsp;在该目录下我们还安装了一些运行Nanodcal所需要的软件，如MATLAB、OPENMPI等。Nanodcal是基于Matlab语言开发，数据密集部分采用C语言，Matlab-C组合保证了Nanodcal计算效率，是目前为止最为高效的LCAO代码。故而编译安装Nanodcal之前需提前安装Matlab软件和对应的C编译器。为了满足库函数编译的要求，还需要安装Openmpi并行环境来提高并行的效率。

&emsp;&emsp;用户可以在Linux终端中使用`module load`和`module unload`命令加载或卸载Nanodcal运行所需的环境变量配置文件，即运行：`module load mpi/openmpi-1.8.5-RESCU`。此时可以通过`which` 命令确认环境是否加载完成。需要注意的是，一体机中安装了多个mpi，使用特定mpi时请先`module unload`当前的mpirun例如：

```sh
[hzwtech@hzwtech ~]$ which mpirun
/public/mpi/openmpi-1.8.5/bin/mpirun
[hzwtech@hzwtech ~]$ module unload mpi/openmpi-1.8.5
[hzwtech@hzwtech ~]$ module load mpi/openmpi-1.8.5-Nanodcal
[hzwtech@hzwtech Nanodcal]$ module load Matlab/R2015B
```

用户可以通过`which` 命令确认环境是否加载完成。例如：

```sh
[hzwtech@hzwtech Nanodcal]$ which matlab
/software/MatlabR2015B/bin/matlab
[hzwtech@hzwtech Nanodcal]$ which mpirun
/public/mpi/openmpi-1.8.5-Nanodcal/bin/mpirun
```

## Nanodcal软件使用

&emsp;&emsp;Nanodcal的作业递交脚本及需要加载的环境变量参见`/public/pbs_script_example`目录下的`nanodcal.pbs`文件。您可以将`nanodcal.pbs`文件，使用`cp`命令拷贝到您需要进行计算的目录，然后使用`qsub nanodcal.pbs`命令，提交任务，进行计算。您可以按照您的具体需求适当修改`nanodcal.pbs`文件的内容。

&emsp;&emsp;在用户主目录下有一个文件夹（`soft_test`）,其中存放了一些Nanodcal的例子。我们以一个黑磷的器件体系为例，通过学习这个例子可以快速了解如何在鸿之微Nanodcal“一体机"中使用Nanodcal软件。

### 单机运行

1. 需要先加载Nanodcal软件的环境变量：
- `module load gcc/4.7.4-Nanodcal`
- `module unload mpi/openmpi-1.8.5`
- `module load mpi/openmpi-1.8.5-Nanodcal`
- `module load Matlab/R2015B`
2. 首先进入工作目录：`cd /home/hzwtech/soft_test/nanodcal_test/BP`
3. 在这个目录中存在四个文件：
- `BackElectrode`
- `Device`
- `FrontElectrode`
- `P_LDA-DZP.nad`
4. 进入工作目录：`cd /BackElectrode`
- 输入：`matlab -nodisplay`
- 输入：`nanodcal scf.input`
5. 在这个目录中存在一个文件：`scf.input`（系统的输入文件）
6. 自洽计算完成，需要28s时间。
7. 计算结果：日志文件`log.txt`和数据文件`NanodcalObject.mat`

### 并行计算

1. 进入工作目录：`cd /BackElectrode`
2. 在这个目录中存在一个文件：`scf.input`（系统的输入文件）
3. 拷贝作业脚本文件到此目录：`cp /public/pbs_script_example/nanodcal.pbs ./`
4. 修改计算资源：根据具体情况，如修改为开启4个超线程，调用16核的计算资源，即 `nodes=1:ppn=16，-n 4 `
5.  输入下面的命令，开始Nanodcal计算,总共需要大约1分钟。
`qsub nanodcal.pbs`
6. 查看任务运行状态：`qstat`
- 如果状态是Q，说明任务在排队中，等待其它任务计算完成；
- 如果状态是R，说明任务正在计算中，请耐心等待；
- 如果状态是C，说明任务已经完成，可以查看计算结果。
7. 计算结果：日志文件`log.txt`和数据文件`NanodcalObject.mat`

## 演示案例：F-C-B的黑磷器件体系

- 案例文件夹：`/home/hzwtech/soft_test/nanodcal_test/BP`
- 计算内容：自洽、能带、电子透射谱
- 计算步骤：
  1. 电极的自洽计算
  2. 中心区的自洽计算
  3. 物理性质计算：电子透射谱计算、态密度计算

### 1.	电极的自洽计算

- 使用cd命令进入案例BP体系的文件夹: `/home/hzwtech/soft_test/nanodcal_test/BP`

```sh
[hzwtech@hzwtech BP]$ ll
total 424
drwxrwxr-x. 2 hzwtech hzwtech     55 Jul  8 16:43 BackElectrode
drwxrwxr-x. 2 hzwtech hzwtech     78 Jul  8 13:58 Device
drwxrwxr-x. 2 hzwtech hzwtech     62 Jul  8 13:58 FrontElectrode
-rw-rw-r--. 1 hzwtech hzwtech 433119 Jul  8 13:58 P_LDA-DZP.nad
[hzwtech@hzwtech BP]$ cd BackElectrode/
[hzwtech@hzwtech BackElectrode]$
```

- 再切换到`BackElectrode`目录，对于自洽计算而言，需要准备参数文件`scf.input`和基组文件`P_LDA-DZP.nad`。使用cat命令查看scf.input文件：

```sh
[hzwtech@hzwtech BackElectrode]$ cat scf.input
%%What quantities should be calculated
calculation.name = scf  %%定义计算内容


%Basic setting
calculation.occupationFunction.temperature = 100
calculation.realspacegrids.E_cutoff = 80 Hartree
calculation.xcFunctional.Type = LDA_PZ81    %%定义交换关联泛函
calculation.k_spacegrids.number = [ 100 1 10 ]'  %% K点数目：真空取1


system.centralCellVectors = [[4.374 0 0]' [0 15 0]' [0 0 3.3133]'] %%晶格基矢
system.spinType = NoSpin   %%没有自旋


%Iteration control
calculation.SCF.monitoredVariableName = {'rhoMatrix','hMatrix','totalEnergy','bandEnergy','gridCharge','orbitalCharge'}
calculation.SCF.convergenceCriteria = {1e-04,1e-04,[],[],[],[]}  %%收敛标准
calculation.SCF.maximumSteps = 200
calculation.SCF.mixMethod = Pulay  %%自洽方法
calculation.SCF.mixRate = 0.1
calculation.SCF.mixingMode = H
calculation.SCF.startingMode = H
%calculation.SCF.donatorObject = NanodcalObject.mat



%Basic set
system.neutralAtomDataDirectory = '../'  %%../代表上一个目录
system.atomBlock = 4                     %%原子总数
AtomType OrbitalType X Y Z
P	LDA-DZP	1.44604440	6.41709180	0.82832500
P	LDA-DZP	0.74095560	8.58290820	0.82832500
P	LDA-DZP	2.92795560	6.41709180	2.48497500
P	LDA-DZP	3.63304440	8.58290820	2.48497500
end

```

&emsp;&emsp;准备好输入文件之后，就可以在一体机上递交作业了。

- PBS系统作业递交方法

&emsp;&emsp;拷贝作业脚本文件到当前目录：`cp /public/pbs_script_example/nanodcal.pbs ./`

&emsp;&emsp;修改需要计算资源：根据具体情况，如修改为开启4个超线程，调用16核的计算资源，即 `nodes=1:ppn=16，-n 4 `

&emsp;&emsp;输入命令`qsub nanodcal.pbs`，提交Nanodcal计算，此案例总共需要大约35s。

注：mpirun -hostfile $PBS_NODEFILE -npernode 4 matlab -nodisplay -r "nanodcal -parallel -doexit scf.input" 调用4个核进行Nanodcal的并行计算。其中scf.input是输入文件的名字，请用户根据计算的文件名字来进行相应的修改。调用多少核数也是根据用户体系大小情况进行变动。
{: .notice}

- 查看任务运行状态：`qstat`
```sh
[hzwtech@hzwtech BackElectrode]$ qstat
Job ID                    Name             User            Time Use S Queue
------------------------- ---------------- --------------- -------- - -----
19.hzwtech                 ...dcal_job_name hzwtech                0 R batch
```

- 输出日志文件及结果查看

```sh
-rw-rw-r--. 1 hzwtech hzwtech     293 Jul  8 16:56 CalculatedResults.mat
-rw-------. 1 hzwtech hzwtech       0 Jul  8 16:55 _err.dat
-rw-rw-r--. 1 hzwtech hzwtech    4929 Jul  8 16:56 log.txt             %%日志文件
-rw-rw-r--. 1 hzwtech hzwtech 5795894 Jul  8 16:56 NanodcalObject.mat  %%数据文件
-rw-r--r--. 1 hzwtech hzwtech     445 Jul  8 14:16 nanodcal.pbs
-rw-------. 1 hzwtech hzwtech    6245 Jul  8 16:56 _out.dat
-rw-rw-r--. 1 hzwtech hzwtech    1094 Jul  8 13:58 scf.input
drwxrwxr-x. 2 hzwtech hzwtech      10 Jul  8 16:56 temporarydata       %%临时存储文件夹
-rw-rw-r--. 1 hzwtech hzwtech     179 Jul  8 16:56 TotalEnergy.mat
```
- `/home/hzwtech/soft_test/nanodcal_test/BP/FrontElectrode` 自洽同上

### 2. 中心区的自洽计算

- 使用cd命令进入案例BP体系的文件夹: `/home/hzwtech/soft_test/nanodcal_test/BP/Device`

- 使用`ls`命令查看`/Device`目录下的文件，对于自洽计算而言，需要准备参数文件`scf.input`和基组文件`P_LDA-DZP.nad`。`scf.input`如下：

```sh
[hzwtech@hzwtech Device]$ cat scf.input
%%What quantities should be calculated
calculation.name = scf


%Basic setting
calculation.occupationFunction.temperature = 100
calculation.realspacegrids.E_cutoff = 80 Hartree
calculation.xcFunctional.Type = LDA_PZ81
calculation.k_spacegrids.number = [ 1 1 10 ]'   %%K点数目：真空，输运方向取1，周期取多


%Description of electrode
system.numberOfLeads = 2
system.typeOfLead1 = front         %%跟输运方向相关
system.voltageOfLead1 = 0
system.objectOfLead1 = ../FrontElectrode/NanodcalObject.mat
system.typeOfLead2 = back
system.voltageOfLead2 = 0
system.objectOfLead2 = ../BackElectrode/NanodcalObject.mat


%Contour integral
%calculation.complexEcontour.lowestEnergyPoint = 1.5 Hartree
calculation.complexEcontour.numberOfPoints = 40
calculation.realEcontour.interval = 0.0272114
calculation.realEcontour.eta = 0.0272114


system.centralCellVectors = [[26.244 0 0]' [0 15 0]' [0 0 3.3133]']
system.spinType = NoSpin


%Iteration control
calculation.SCF.monitoredVariableName = {'rhoMatrix','hMatrix','totalEnergy','bandEnergy','gridCharge','orbitalCharge'}
calculation.SCF.convergenceCriteria = {1e-04,1e-04,[],[],[],[]}
calculation.SCF.maximumSteps = 200
calculation.SCF.mixMethod = Pulay
calculation.SCF.mixRate = 0.1
calculation.SCF.mixingMode = H
calculation.SCF.startingMode = H
%calculation.SCF.donatorObject = NanodcalObject.mat



%Basic set
system.neutralAtomDataDirectory = '../'
system.atomBlock = 24
AtomType OrbitalType X Y Z
P	LDA-DZP	0.74095560	8.58290820	0.82832500
P	LDA-DZP	25.50304440	8.58290820	2.48497500
P	LDA-DZP	24.79795560	6.41709180	2.48497500
P	LDA-DZP	1.44604440	6.41709180	0.82832500
P	LDA-DZP	23.31604440	6.41709180	0.82832500
P	LDA-DZP	22.61095560	8.58290820	0.82832500
P	LDA-DZP	2.92795560	6.41709180	2.48497500
P	LDA-DZP	3.63304440	8.58290820	2.48497500
P	LDA-DZP	5.82004440	6.41709180	0.82832500
P	LDA-DZP	10.19404440	6.41709180	0.82832500
P	LDA-DZP	14.56804440	6.41709180	0.82832500
P	LDA-DZP	18.94204440	6.41709180	0.82832500
P	LDA-DZP	5.11495560	8.58290820	0.82832500
P	LDA-DZP	9.48895560	8.58290820	0.82832500
P	LDA-DZP	13.86295560	8.58290820	0.82832500
P	LDA-DZP	18.23695560	8.58290820	0.82832500
P	LDA-DZP	7.30195560	6.41709180	2.48497500
P	LDA-DZP	11.67595560	6.41709180	2.48497500
P	LDA-DZP	16.04995560	6.41709180	2.48497500
P	LDA-DZP	20.42395560	6.41709180	2.48497500
P	LDA-DZP	8.00704440	8.58290820	2.48497500
P	LDA-DZP	12.38104440	8.58290820	2.48497500
P	LDA-DZP	16.75504440	8.58290820	2.48497500
P	LDA-DZP	21.12904440	8.58290820	2.48497500
end
```

- PBS系统作业递交方法: 拷贝作业脚本文件到此目录，执行：`qsub nanodcal.pbs`

- 输出日志文件及结果查看

```sh
-rw-rw-r--. 1 hzwtech hzwtech     293 Jul  8 16:56 CalculatedResults.mat
-rw-------. 1 hzwtech hzwtech       0 Jul  8 16:55 _err.dat
-rw-rw-r--. 1 hzwtech hzwtech    4929 Jul  8 16:56 log.txt             %%日志文件
-rw-rw-r--. 1 hzwtech hzwtech 5795894 Jul  8 16:56 NanodcalObject.mat  %%数据文件
-rw-r--r--. 1 hzwtech hzwtech     445 Jul  8 14:16 nanodcal.pbs
-rw-------. 1 hzwtech hzwtech    6245 Jul  8 16:56 _out.dat
-rw-rw-r--. 1 hzwtech hzwtech    1094 Jul  8 13:58 scf.input
drwxrwxr-x. 2 hzwtech hzwtech      10 Jul  8 16:56 temporarydata       %%临时存储文件夹
-rw-rw-r--. 1 hzwtech hzwtech     179 Jul  8 16:56 TotalEnergy.mat
```
注：根据输运方向的不同，电极类型也所不同，参见文件：`cat /software/Nanodcal/documentations/input-reference.pdf`：
{: .notice}

```
left：[0 0 -1] means the direction of -z
right：[0 0 1] means the direction of +z
top：[0 1 0] means the direction of +y
bottom：[0 -1 0] means the direction of -y
front：[-1 0 0]	means the direction of -x
back： [1 0 0] means the direction of +x
```

### 3. 能带计算

- 使用cd命令进入案例BP体系的文件夹: `/home/hzwtech/soft_test/nanodcal_test/BP/BackElectrode`

- 准备好计算能带的文件`BandStructure.input`，上传到此目录。

```sh
[hzwtech@hzwtech BackElectrode]$ cat BandStructure.input
system.object = NanodcalObject.mat
calculation.name = bandstructure
%calculation.bandStructure.symmetryKPoints = {'G','X','S','Y','G'}
%calculation.bandStructure.coordinatesOfTheSymmetryKPoints = [0 0 0;0 0 0.5;0.5 0 0.5;0.5 0 0;0 0 0]
calculation.bandStructure.numberOfKPoints = 200
calculation.bandStructure.plot = true
calculation.control.xml = true
```

- 修改nanodcal.pbs文件：

```sh
mpirun -hostfile $PBS_NODEFILE -n 4  matlab -nodisplay -r "nanodcal -parallel -doexit scf.input"
修改为：
mpirun -hostfile $PBS_NODEFILE -n 4  matlab -nodisplay -r "nanodcal -parallel -doexit BandStructure.input"
```
- 递交计算即可`qsub nanodcal.pbs`

- 输出文件：`BandStructure.fig` 和 `BandStructure.mat`

### 4. 电子透射谱计算

- 使用cd命令进入案例BP体系的文件夹: `/home/hzwtech/soft_test/nanodcal_test/BP/Device`

- 准备好计算电子透射谱的文件`Transmission.input`，上传到此目录。

```sh
[hzwtech@hzwtech Device]$ cat Transmission.input
system.object = NanodcalObject.mat
calculation.name = transmission
calculation.transmission.kSpaceGridNumber = [ 1 1 100 ]  %%K点选取
calculation.transmission.energyPoints = [-2:0.02:2]  %%能量范围
calculation.transmission.plot = true
calculation.control.xml = true
```

- 修改nanodcal.pbs文件：

```sh
mpirun -hostfile $PBS_NODEFILE -n 4  matlab -nodisplay -r "nanodcal -parallel -doexit scf.input"
修改为：
mpirun -hostfile $PBS_NODEFILE -n 4  matlab -nodisplay -r "nanodcal -parallel -doexit Transmission.input"
```
- 递交计算即可`qsub nanodcal.pbs`

- 输出文件：`Transmission.fig` 和 `Transmission.mat`

- 下载数据到本地计算机后，打开MATLAB界面，画图有两种方法：

第一种：>> `nanodcal -plot transmission Transmission.mat`

第二种：直接双击Transmission.fig文件

更多案例的计算以及数据处理过程，请参考Nanodcal中文应用教程。
