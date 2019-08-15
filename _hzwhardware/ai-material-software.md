---
title: "鸿之微-机器学习材料设计一体机"
permalink: /hzwhardware/ai-material-software/
excerpt: "鸿之微-机器学习材料设计一体机"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
gallery:
  - url: /assets/images/first-introduce/FIRST1.png
    image_path: /assets/images/first-introduce/FIRST1.png
    alt: "FIRST流程"
    title: "FIRST流程"
gallery2:
  - url: /assets/images/first-introduce/FIRST2.png
    image_path: /assets/images/first-introduce/FIRST2.png
    alt: "FIRST的机器学习"
    title: "FIRST的机器学习"
gallery3:
  - url: /assets/images/first-introduce/FIRST3.gif
    image_path: /assets/images/first-introduce/FIRST3.gif
    alt: "FIRST数据库与描述符"
    title: "FIRST数据库与描述符"
  - url: /assets/images/first-introduce/FIRST4.gif
    image_path: /assets/images/first-introduce/FIRST4.gif
    alt: "FIRST的描述符推荐算法基于多次采样评价"
    title: "FIRST的描述符推荐算法基于多次采样评价"
---

&emsp;&emsp;鸿之微-机器学习材料设计一体机搭载了鸿之微FIRST材料信息学虚拟筛选软件，采用人工智能的方式对海量材料进行快速筛选。

---

&emsp;&emsp;&emsp;[<i class="fas fa-shopping-cart"></i> 选择硬件](/hzwhardware/ai-material-hardware/){: .btn .btn--success .btn--large}
{: .text-right}

## 主要功能

&emsp;&emsp;新材料的设计和发现，通常需要研究人员在候选材料中筛选排除性能不达标的材料，手段包括实验，计算仿真等等。受到实验或仿真的时间或成本的制约，研究人员只能在很有限的范围内筛选合适的材料。

&emsp;&emsp;FIRST(Fast vIRtual Screening Tool 快速筛选工具)一体机针对材料筛选的应用场景，提供了完善先进的解决方案，实现低成本高效率超大范围筛选材料，助力新材料的快速精准研发。适用范围包括有机光电材料，热电材料，二维材料，锂电池电极/电解质材料等等。

### FIRST筛选流程

&emsp;&emsp;FIRST融合了材料信息学、统计学、机器学习的多种方法，能够快速有效地筛选出目标材料。流程是：输入样本材料--->特征化--->机器学习--->虚拟筛选，见图1。特征化是把材料的组成和结构转换为描述符，例如分子摩尔质量，晶体平均电负性等等。目前FIRST支持的分子描述符和晶体描述符均已超过200，包括2D, 3D, Fragment, Element四种类型，参见图3。描述符可以用于数据库检索，也可以作为机器学习算法的输入，从而得到材料的类别、关键性能等与描述符的对应关系模型。随后机器学习模型在FIRST的数据库中进行材料的筛选。

{% include gallery caption="图1. FIRST流程" %}

### FIRST学习方法

&emsp;&emsp;FIRST集成了多种决策树，逻辑回归，随机森林，支持向量机等经典机器学习方法，并根据材料学实际情况作了大量改进，能完成材料分类、回归拟合、聚类等等任务。当用户对材料的某种性质感兴趣时，可以通过输入样本材料结构及该性质的数据，利用FIRST的机器学习模块进行训练得到模型，见图2。根据模型到FIRST的无机晶体材料数据库或有机小分子材料数据库中进行筛选。

{% include gallery id="gallery2" caption="图2. FIRST的机器学习" %}


### FIRST数据库

&emsp;&emsp;FIRST内置了强大的无机晶体数据库和有机小分子数据库，材料数量分别高达32万，100多万，目前仍在快速增长中。FIRST支持多种检索方式，包括材料性质(描述符)检索，相似度检索，元素数量检索，机器学习模型检索等等。为了减轻用户学习成本，FIRST内置的智能推荐系统能根据用户的样本材料分析最优的描述符和取值范围，推荐筛选条件。就目前看来，这种独有的智能检索方式尚未在同类材料数据库中出现。

{% include gallery id="gallery3" caption="图3. 1) FIRST的描述符和数据库；2) FIRST的描述符推荐算法基于多次采样评价" %}

---
