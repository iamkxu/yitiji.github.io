---
title: "Nanodcal案例"
permalink: /hzwsoftware/nanodcal-examples/
excerpt: "Nanodcal案例"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## 演示案例：黑磷结构
&emsp;&emsp;案例文件夹：
`/home/hzwtech/soft_test/nanodcal_test/BP/BackElectrode`

###  Device Studio软件介绍
&emsp;&emsp;Device Studio（DS）作为鸿之微平台型的图形界面软件，能够进行电子器件的结构搭建与仿真；能够进行晶体结构和纳米器件的建模；能够生成Nanodcal、MOMAP、RESCU、VASP、Gaussian、NWChem、PWmat、STEMS的输入文件并进行存储和管理；可以根据用户需要将输入文件传递给远程或当地的Nanodcal、RESCU、MOMAP、PWmat、STEMS软件进行计算，并控制计算流程；可以将计算结果进行可视化显示和分析。

&emsp;&emsp;本案例中将演示如何使用Device Studio在鸿之微Nanodcal一体机完成整个计算流程。

### 计算流程

#### 1. Device Studio建模及生成对应的输入文件
- 打开本地机器上的Device Studio软件，新建一个文件夹，如图：
 ![avatar](/assets/images/nanodcal-image/11.jpg)

- 点击左上角的`File`-`Import Local`，找到`blackphosphorus.hzw`文件并点击`Open`
（`*\DeviceStudio\material\2Dmaterials\Novelty_material`），如图：
  ![avatar](/assets/images/nanodcal-image/22.jpg)

- 得到BP的结构，如图：
 ![avatar](/assets/images/nanodcal-image/33.jpg)

- 点击菜单栏`Build`-`Redefine Crystal`，在OA方向选择扩胞6倍，点击`Preview`进行预览，再`Build`即可，如图：
 ![avatar](/assets/images/nanodcal-image/44.jpg)

- 通过快捷键 `3D Viewer` ，沿着`xz view`查看视图，如下：
 ![avatar](/assets/images/nanodcal-image/55.jpg)

- 点击菜单栏`Build`-`Convert to Device`，根据输运方向选择对应的电极类型。本例中输运方向为OA，即x方向，固选择电极类型为Front与Back electrode,点击`Build`，转换为器件，如图：
![avatar](/assets/images/nanodcal-image/66.jpg)

- 器件的结构模型如下图：
 ![avatar](/assets/images/nanodcal-image/77.jpg)

- 生成Nanodcal的自洽的输入文件：
点击`Simulator`-`Nanodcal`-`SCF Calculation`生成Nanodcal的自洽的输入文件。在本例中我们将`Basic settings`中的`k-point Sampling`设置为[1,1,10]，其余采用默认值。之后点击`Generate files`生成自洽计算输入文件。如下图：
![avatar](/assets/images/nanodcal-image/88.jpg)

- 通过以上步骤就生成了Nanodcal的自洽的输入文件`scf.input`，可在Project栏进行查看，如图：
![avatar](/assets/images/nanodcal-image/99.jpg)

- 当然如果要生成其他计算文件，比如电子透射谱的输入文件，可通过以下方式：点击`Simulator`-`Nanodcal`-`Analysis`生成电子透射谱的输入文件。如图：
![avatar](/assets/images/nanodcal-image/101.jpg)

注意：实际计算过程中K点数目需要测试。随着K增多，选择一个稳定的数值下的最小K点数目。
{: .notice}

- 在`Project`窗口即可看到对应的输入文件，如图：
![avatar](/assets/images/nanodcal-image/102.jpg)

- 右击要查看的文件，选择`Open Containing Folder`即可找到文件所在位置进行查看。

#### 2. 自洽计算

&emsp;&emsp;2.1 将上述文件通过Xftp软件上传到一体机的指定计算目录即可进行计算，比如：`/home/hzwtech/soft_test/nanodcal_test`。

&emsp;&emsp;2.2 我们以文件夹：`/home/hzwtech/soft_test/nanodcal_test/BP`为例：

- 使用cd命令进入案例BP体系的文件夹: `/home/hzwtech/soft_test/nanodcal_test/BP/BackElectrode`

- 使用`ls`命令查看`BackElectrode`目录下的文件，对于自洽计算而言，需要准备参数文件`scf.input`和基组文件`P_LDA-DZP.nad`。

- 拷贝作业脚本文件到此目录：`cp /public/pbs_script_example/nanodcal.pbs ./`

- 修改计算资源：根据具体情况，如修改为开启4个超线程，调用16核的计算资源，即 `nodes=1:ppn=16，-n 4 `

- 输入命令`qsub nanodcal.pbs`，开始Nanodcal计算,总共需要大约35s。

- `/home/hzwtech/soft_test/nanodcal_test/BP/FrontElectrode` 自洽同上

- `/home/hzwtech/soft_test/nanodcal_test/BP/Device` 自洽同上

#### 3. 能带计算

&emsp;&emsp;3.1 使用cd命令进入案例BP体系的文件夹:`/home/hzwtech/soft_test/nanodcal_test/BP/BackElectrode`

&emsp;&emsp;3.2 自洽完成后，进行能带计算。能带输入文件如下：

```sh
[hzwtech@hzwtech BackElectrode]$ cat BandStructure.input
system.object = NanodcalObject.mat
calculation.name = bandstructure
calculation.bandStructure.symmetryKPoints = {'G','X','S','Y','G'}  %%根据实际需求修改
%calculation.bandStructure.coordinatesOfTheSymmetryKPoints = [0 0 0;0 0 0.5;0.5 0 0.5;0.5 0 0;0 0 0]
calculation.bandStructure.numberOfKPoints = 200
calculation.bandStructure.plot = true
calculation.control.xml = true
```
&emsp;&emsp;3.3 修改nanodcal.pbs文件：

```sh
mpirun -hostfile $PBS_NODEFILE -n 4  matlab -nodisplay -r "nanodcal -parallel -doexit scf.input"
修改为：
mpirun -hostfile $PBS_NODEFILE -n 4  matlab -nodisplay -r "nanodcal -parallel -doexit BandStructure.input"
```
&emsp;&emsp;3.4 递交计算即可`qsub nanodcal.pbs`

&emsp;&emsp;3.5 输出文件：`BandStructure.fig` 和 `BandStructure.mat`

&emsp;&emsp;3.6 下载数据到本地计算机后，打开MATLAB界面，画图有两种方法：

第一种：>> `nanodcal -plot bandstructure BandStructure.mat`

第二种：直接双击BandStructure.fig文件,如图：
![avatar](/assets/images/nanodcal-image/103.jpg)

更多案例的计算以及数据处理过程，请参考Nanodcal中文应用教程。
