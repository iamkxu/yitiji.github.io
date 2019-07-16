---
title: "Nanodcal快速使用手册"
permalink: /hzwsoftware/nanodcal-quick-guide/
excerpt: "Nanodcal快速使用手册"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

&emsp;&emsp;鸿之微Nanodcal“一体机”已经预装Nanodcal，用户不需要自行安装，可以直接调用。Nanodcal预装在`/software/Nanodcal`目录中，其中包含了Nanodcal的程序、文档和实例。

&emsp;&emsp;Nanodcal的作业递交脚本及需要加载的环境变量参见`/public/pbs_script_example`目录下的`nanodcal.pbs`文件。您可以将`nanodcal.pbs`文件，使用`cp`命令拷贝到您需要进行计算的目录，然后使用`qsub nanodcal.pbs`命令，提交任务，进行计算。您可以按照您的具体需求适当修改`nanodcal.pbs`文件的内容。

例如，你有一个输入文件input.txt，那么只需要在命令行终端中输入“nanodcal.sh input.txt”就可以直接开始计算。这样操作只是进行串行计算，如果用户需要进行并行计算需要加载环境变量脚本，所使用的命令是“source /home/hzw/bin/nanodcal_environment.sh”，然后在命令行终端中输入“mpirun -n 8 nanodcal.sh -parallel input.txt”就可以进行并行计算，其中并行进程数可以根据实际情况修改，但不要超过器件模拟一体机的总核数。
{: .notice--success}

&emsp;&emsp;在用户根目录下有一个文件夹（nanodcal_guide）,其中存放了一些nanodcal的例子。这些例子都包含输入文件以及作业管理系统的提交脚本。nanodcal程序的使用方法，请用户自行查阅nanodcal的帮助文件，此处不在重复说明。

&emsp;&emsp;我们以一个周期性系统为例，其它的几个例子的使用方法都相同的。通过学习这些例子可以了解如何在鸿之微Nanodcal“一体机"中使用nanodcal。
- 首先进入工作目录：cd nanodcal_guide/bulk
- 在这个目录中存在两个文件：bulk.input（系统的输入文件）和run.sh（作业管理系统的提交脚本）
- 输入下面的命令，开始nanodcal计算，我们使用8核进行计算，总共需要大约1分钟。
`qsub run.sh`
- 在nanodcal计算过程中可以使用qstat命令查看计算任务的运行状态。如果状态是Q，说明任务在排队中，等待其它任务计算完成；如果状态是R，说明任务正在计算中，请耐心等待；如果状态是C，说明任务已经完成，可以查看计算结果。
{: .notice--primary}
