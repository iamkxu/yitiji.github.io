---
title: "MOMAP软件使用教程"
permalink: /hzwsoftware/momap-manual/
excerpt: "MOMAP软件使用教程"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
---

### MOMAP软件介绍
&emsp;&emsp;MOMAP(Molecular Materials Property Prediction Package)是一款研究和设计有机分子材料发光和传输机理以及定量预测发光效率的软件。目前广泛应用于OLED发光和传输机理研究、新型OLED设计以及有机显示与照明材料、有机场效应材料、有机太阳能电池、以及有机光检测、生物传感、有机光通讯等领域。
### MOMAP安装环境
&emsp;&emsp;一体机版MOMAP安装目录为：/public/Hzwtech/MOMAP。在该目录下我们还安装了一些运行MOMAP所需要的软件，如momap-1.0.2.dev-mpich2，mpich2，lapack，fftw等。在目录momap下的MOMAP_env文件即为MOMAP运行所需的环境变量配置文件。用户可以在Linux终端中使用source命令加载该配置文件，即运行：source /public/Hzwtech/MOMAP/ momap-1.0.2.dev-mpich2/MOMAP_env。此时可以通过which 命令确认环境是否加载完成。例如：

### MOMAP软件使用
&emsp;&emsp;一体机版MOMAP与单机版MOMAP在使用上基本一致。我们将选取MOMAP应用教程中Azulene体系为例，通过量化计算、光谱、辐射速率和非辐射速率的计算为用户演示一体机版MOMAP的使用。更多相关案例可登录MOMAP官网http://www.momap.net.cn/，参考《MOMAP用户使用手册》。
注：该简单教程为使用终端（terminal）进行MOMAP计算，请用户先鼠标右键打开一个终端（terminal）。
Azulene体系
（1）量化计算
1.	进入指定文件夹及查看相关文件
使用cd命令进入案例Azulene体系量化计算的文件夹:/home/hzw/MOMAP_example/Azulene/gaossian
使用ls命令查看gaussian目录下的文件夹

其中后缀为com的文件为高斯软件的输入文件，后缀为log、chk和fchk的文件为高斯软件的输出文件。其中基态的输入文件azulene-s0.com可以用cat命令查看：

    这里计算高斯任务需用户自备高斯软件。计算后会得到azulene-s0.log、azulene-s0.chk结果。使用formchk使用formchk命令转化二进制的checkpoint文件为文本文件：formchk azulene-s0.chk，将会得到azulene-s0.fchk。
    激发态的输入文件azulene-s1.com可以用cat命令查看：

    计算后会得到azulene-s1.log、azulene-s1.chk结果。使用formchk使用formchk命令转化二进制的checkpoint文件为文本文件：formchk azulene-s1.chk，将会得到azulene-s1.fchk。
（2）振动分析
    收集上一部分计算得到的基态和激发态的计算结果文件，包括日志文件（azulene-s0.log、azulene-s1.log）和格式化的Checkpoint文件（azulene-s0.fchk、azulene-s1.fchk），注意需保证振动结果无虚频（在频率计算文件中搜索Frequencies，注意F大写，可以找到频率信息），将这些文件都放在一个文件夹（evc）中，evc计算需要的输入文件包含红色框内：

    用cat命令查看momap.inp文件：

    用cat命令查看nodefile文件：

evc这一步计算时间比较短，不需要调用pbs排队系统执行任务，可直接运行momap -input momap.inp -nodefile nodefile，程序正常结束后，得到下一步计算的输入文件evc.cart.dat（或evc.dint.dat）。
（3）辐射速率
这一步的文件可以用ls查看：

    其中红色框内为输入文件，可以用cat命令查看momap.inp文件：

    在一体机中使用的是pbs排队系统来提交MOMAP计算任务，相应的命令都写在momap.pbs脚本中，使用cat命令查看momap.pbs文件：

其中source /public/Hzwtech/MOMAP/momap-1.0.2.dev-mpich2/MOMAP_env和source /public/Hzwtech/MOMAP/momap-1.0.2.dev-mpich2/bin/.environment为加载运行MOMAP所需的环境变量。
momap -input $jobname -nodefile $PBS_NODEFILE为运行命令。
使用qsub momap.pbs进行计算，并通过qstat命令进行计算任务查看。

计算结束后，在计算目录会产生输出文件：spec.tvcf.fo.dat、spec.tvcf.ft.dat、spec.tvcf.log、spec.tvcf.spec.dat。其中在spec.tvcf.log文件末端可以查找到辐射速率。

Notes：如果需要修改输入文件，可以通过使用vi命令实现，具体使用见Linux基础。
（4）非辐射速率
1.	计算内转换过程不仅需要分子基态S0与激发态S1的构型优化结果、频率计算结果，还需要包含与非绝热耦合矩阵元相关的azulene-nacme.log文件。非绝热耦合计算时使用的计算方法、泛函等尽量与构型优化时保持一致。
    使用cat命令查看kic/nacme/azulene-nacme.com文件：

使用高斯计算后得到azulene-nacme.log输出文件。
2.	收集高斯计算的基态、激发态计算结果文件，包括日志文件（azulene-s0.log和azulene-s1.log）和格式化的Checkpoint文件（azulene-s0.fchk和azulene-s1.fchk），还有与非绝热耦合矩阵元相关的azulene-nacme.log文件。将这些文件都放在目录azulene/kic/evc/中。其中这一步的输入文件如图红框内所示：

evc这一步计算时间比较短，不需要调用pbs排队系统执行任务，可直接运行momap -input momap.inp -nodefile nodefile，程序正常结束后，得到下一步计算的输入文件evc.cart.dat和evc.cart.nac。
3.	收集上一步计算的evc.cart.dat和evc.cart.nac，构建momap.inp、nodefile和momap.pbs。红色框内为输入文件：
收集高斯计算的基态、激发态计算结果文件，包括日志文件（azulene-s0.log和azulene-s1.log）和格式化的Checkpoint文件（azulene-s0.fchk和azulene-s1.fchk），还有与非绝热耦合矩阵元相关的azulene-nacme.log文件。将这些文件都放在目录azulene/kic/evc/中。其中这一步的输入文件如图红框内所示：

可以用cat命令查看momap.inp：

cat命令查看momap.pbs：

    使用qsub命令提交任务，计算后得到ic.tvcf.fo.dat、ic.tvcf.ft.dat、ic.tvcf.log。其中非辐射速率在ic.tvcf.log文件末端，如图所示：
