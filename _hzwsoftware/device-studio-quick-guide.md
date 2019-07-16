---
title: "Device Studio使用手册"
permalink: /hzwsoftware/device-studio-quick-guide/
excerpt: "Device Studio使用手册"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

 &emsp;&emsp;Device Studio程序是基于QT的图形界面软件，所以必须通过图形界面才能使用。如果用户想要在“一体机””中使用Device Studio，建议直接在一体机中进行操作，尽量不要使用第三方中终端软件进行调用，如使用支持图形界面的第三方终端软件进行调用，Device Studio会出现卡顿，死机等现象。

## 开始使用Device Studio

1.	双击桌面`DeviceStudio`图标，打开Device studio软件主界面。

2.	选择`Create a new Project`，点击`OK`。
![image-center](/assets/images/device-studio-image/ds-1.png){: .align-center}

3.	输入新建项目名称，这里以`test`为例，之后点击`Save`保存。
![image-center](/assets/images/device-studio-image/ds-2.png){: .align-center}

4.	点击左上角的`File-Import Local`，单击`Si.hzw`文件并点击`Open`， `Si.hzw`的路径为`DeviceStudio/material/3Dmaterial/Semicondutor`。
![image-center](/assets/images/device-studio-image/ds-3.png){: .align-center}

5.	双击`Project`栏下的`Si.hzw`，之后点击`Simulator-RESCU-SCF Calculation`生成`RESCU`的自洽的输入文件。在本例中我们将`Basic settings`中的`k-point Sampling`设置为`[5, 5, 5]`，之后点击`Generate files`生成自洽计算输入文件。
![image-center](/assets/images/device-studio-image/ds-4.png){: .align-center}

6.	生成`RESCU`的能带计算的输入文件：`Simulator-RESCU-Analysis`，双击`BandStructure`，点击`Generate files`生成相应文件；
![image-center](/assets/images/device-studio-image/ds-5.png){: .align-center}

7.	在`Project`栏下，可以看到`RESCU-Crystal`下生成了`scf.input`，`Bandstructure.input`文件。
![image-center](/assets/images/device-studio-image/ds-6.png){: .align-center}

8.	右键`scf.input`文件，选择`run`，设置并行核数，本例中设置为8，并点击`run`。
![image-center](/assets/images/device-studio-image/ds-7.png){: .align-center}

9.	之后进行能带计算：右键`Bandstructure.input`文件，选择`run`，设置并行核数，本例中设置为`8`，并点击`run`，可以看到在`Job Manager`中两个任务正在运行。
![image-center](/assets/images/device-studio-image/ds-8.png){: .align-center}

10.	等待任务计算完成，`Job Manage`r中的状态变为`Finished`，此时可以看到`Project`栏下多了计算日志文件`RESCUlog.out`和能带输出结果`Bandstructure.txt`文件。
![image-center](/assets/images/device-studio-image/ds-9.png){: .align-center}

11.	右键`Bandstructure.txt`，选择`Show View`，显示能带计算结果。
![image-center](/assets/images/device-studio-image/ds-10.png){: .align-center}

12.	计算完成。
