---
title: "FIRST软件的使用教程"
permalink: /hzwsoftware/first-manual/
excerpt: "FIRST软件的使用教程"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
---

## FIRST软件介绍
&emsp;&emsp;Fast vIRtual Screening Tool (快速虚拟筛选工具, FIRST)是一款基于材料学原理、机器学习算法的材料设计/筛选软件。其主要功能有：

1.	材料虚拟筛选
2.	材料性能预测
3.	结合材料学原理的机器学习
4.	数据挖掘与可视化
5.	材料数据库检索

&emsp;&emsp;目前为止, FIRST已应用于二维铁磁材料、含锂固态电解质材料、有机光电发射层材料等等的虚拟筛选，并将逐步推广到更多的材料领域中。

## FIRST安装

&emsp;&emsp;一体机FIRST安装目录为: `/software/FIRST`。双击桌面FIRST图标或者`/software/FIRST/bin/First`即可打开FIRST软件。

## FIRST软件使用

&emsp;&emsp;双击桌面`FIRST`图标，打开软件主界面，新建一个项目。
 ![image-center](/assets/images/first-introduce/manual1.png)

### 1.数据库筛选
 &emsp;&emsp;点击菜单栏中的`Database`，选择`QuickMol`、`QuickCrystal`、`ACED`数据库中的一种，分别进入对应界面，
 ![image-center](/assets/images/first-introduce/manual2.png)

  &emsp;&emsp;在`Descriptor`选项卡中输入各个描述符的取值范围，单击`Search`对材料进行筛选。右下角显示筛选的结果，单击可在上方显示其结构。
 ![image-center](/assets/images/first-introduce/manual3.png)

  &emsp;&emsp;在`AI Descriptor`选项卡中，点击`load`按钮加载训练好的`.pkl`后缀的机器学习模型，点击`filter`按钮进行机器学习筛选。
   ![image-center](/assets/images/first-introduce/manual4.png)

### 2.导入样本材料
 &emsp;&emsp;点击菜单栏中的`Sample`，选择`Molecule`或`Crystal`，进入对应界面。在右侧分别填入`Class No.`的值以标记类别，点击右上角`Import`按钮导入对应文件。在描述符界面中选择描述符，点击右下角`Generate`，生成`.csv`数据文件。
  ![image-center](/assets/images/first-introduce/manual5.png)

### 3.数据分析与可视化
 &emsp;&emsp;选择`.csv`数据文件对应的数据表，菜单栏中的`File`用于导出文件，`Compute`用于对描述符进行计算，`Statistics`对数据进行概况分析并导出。
  ![image-center](/assets/images/first-introduce/manual6.png)

 &emsp;&emsp;选择`Plot`，可以选择多列数据，绘制散点图、柱状图、小提琴图等。
  ![image-center](/assets/images/first-introduce/manual7.png)
  ![image-center](/assets/images/first-introduce/manual8.png)

### 4.机器学习建模
&emsp;&emsp;选择`.csv`数据表菜单栏中的`AI`，可以使用`Logistics`、`SVM`、`Linear`、`LASSO`等分类、回归方法进行机器学习建模。选择参数后点击`Run`按钮，完成训练。
  ![image-center](/assets/images/first-introduce/manual9.png)
  
  ![image-center](/assets/images/first-introduce/manual10.png)
