---
title: "鸿之微-有机发光材料设计一体机"
permalink: /hzwhardware/oled-material-software/
excerpt: "鸿之微-有机发光材料设计一体机"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
gallery:
  - url: /assets/images/hardware-image/oled-diplay.jpg
    image_path: /assets/images/hardware-image/oled-diplay.jpg
    alt: "MOMAP部分应用领域"
    title: "MOMAP部分应用领域"
  - url: /assets/images/hardware-image/oled-roadmap.jpg
    image_path: /assets/images/hardware-image/oled-roadmap.jpg
    alt: "MOMAP部分应用领域"
    title: "MOMAP部分应用领域"
  - url: /assets/images/hardware-image/oled-spectra_calc.jpg
    image_path: /assets/images/hardware-image/oled-spectra_calc.jpg
    alt: "MOMAP部分应用领域"
    title: "MOMAP部分应用领域"
gallery2:
  - url: /assets/images/hardware-image/oled-team.png
    image_path: /assets/images/hardware-image/oled-team.png
    alt: "研发及技术支撑团队"
    title: "研发及技术支撑团队"
---

&emsp;&emsp;鸿之微-有机发光材料设计一体机是鸿之微开发的一款搭载MOMAP计算软件的科学模拟平台。

{% include gallery id="gallery" caption="MOMAP部分应用示意图" %}

&emsp;&emsp;MOMAP(Molecular Materials Property Prediction Package)是一款研究和设计有机分子材料发光和传输机理以及定量预测发光效率的软件。<sup>[1-3]</sup> 目前广泛应用于OLED发光和传输机理研究、新型OLED设计以及有机显示与照明材料、有机场效应材料、有机太阳能电池、以及有机光检测、生物传感、有机光通讯等领域。

{% include gallery id="gallery2" caption="研发及技术支撑团队" %}

&emsp;&emsp;&emsp;[<i class="fas fa-shopping-cart"></i> 选择硬件](/hzwhardware/oled-material-hardware/){: .btn .btn--success .btn--large}
{: .text-right}

## 理论贡献和软件特点

- 在多维耦合谐振子模型下，采用热振动关联函数方法发展了一系列单线态之间及单线态与三线态之间的辐射和无辐射跃迁速率的解析公式；
- 充分考虑了激发态与基态势能面之间的平移、扭曲、Duschinsky转动、Herzberg-Teller效应等，具有较大的普适性；
- 计算分子体系大；
- 成功应用于有机光电性能的机理研究、为实验合成提供了分子模板、为新型OLED材料的发展做出了贡献：
    - 合理解释了聚集诱导发光现象、成功设计和合成了不含有自由转动的芳香环的奇异AIE分子；
    - 理论设计预测了高效的光伏聚合物和蓝光有机金属磷光配合物；
    - 理论设计预测了高效的红外/近红外聚合物；
    - 在国际上首次实现了复杂有机分子和有机金属配合物的发光效率（荧光效率和磷光效率）的定量预测。

## 主要功能

### 定量预测分子荧光量子产率
&emsp;&emsp;在简谐近似下考虑Duschinsky转动效应，根据第一原理与MOMAP给出的内转换的关联函数可以对分子荧光性质进行定量计算，计算的辐射和无辐射速率与实验测量结果都非常吻合<sup>[4]</sup>。

### 定量预测分子磷光量子产率
&emsp;&emsp;MOMAP包含计算Ir（III）配合物的光致发光寿命和效率的通用方法，并考虑所有可能的竞争激发态失活过程，可以有效阐明其温度依赖效应。这种方法基于量子化学计算、激发态衰变速率方程以及动力学建模相结合，被证明是一种有效可靠的方法，适用于众多的Ir（III）配合物<sup>[5]</sup>。

### 从分子水平上解释OLED激发态电子结构和衰变过程
&emsp;&emsp;为了理解低激发态结构的性质，我们开发了用于计算激发态结构的密度矩阵重整化群（DMRG）理论及其对称化方案，给出了统一的关联函数方程用于计算激发态辐射和非辐射衰减速率。文献中强调了低频模式混合（Duschinsky旋转）对非辐射衰减的影响<sup>[6]</sup>。

### 高效率的OLED材料开发和计算方法
&emsp;&emsp;提出一种振动关联函数来评估激发态衰减速率，不仅合理估计了量子效率，而且还给出了聚集诱导发射（AIE）的定量计算<sup>[7]</sup>。

### AIE理论验证
&emsp;&emsp;通过计算研究，提出了直接验证AIE过程的方法，即利用共振拉曼光谱（RRS）来探索AIE的微观机制<sup>[8]</sup>。

### 内转换速率
&emsp;&emsp;通过分析热振动关联函数来计算非辐射衰减速率常数（knr），考虑基于算符分裂近似的激子耦合效应（ECE）。第一性原理计算表明ECE对于H和J聚集体都使knr增加。另外，研究发现ECE对于AIEgens （聚集诱导的放射性发光素）是次要的<sup>[9]</sup>]。

### 有机/碳材料光电性质评估
&emsp;&emsp;描述了相关密度泛函理论（DFT）方法的最新进展，通过计算电子-振动耦合来检验光发射效率，载流子迁移率和热电优值<sup>[10]</sup>。

### 多尺度模拟有机半导体的电荷传输
&emsp;&emsp;采用量子化学，蒙特卡洛和分子动力学模拟相结合的多尺度方法的开发和应用，以评估有机半导体的电荷迁移率<sup>[11]</sup>。

注：使用MOMAP软件计算发表论文须引用论文<sup>[1-3]</sup>。
{: .notice--warning}

## 参考论文

>[1] Niu, Y.; Li, W.; Peng, Q.; Geng H.; Yi, Y.; Wang, L.; Nan, G.; Wang D.; Shuai Z., Molecular Physics, 2018, doi: 10.1080/00268976.2017.1402966.

>[2] Peng, Q.; Yi, Y.; Shuai, Z.; Shao, J., J. Am. Chem. Soc., 2007, 129, 9333-9339.

>[3] Niu, Y.; Peng, Q.; Shuai, Z., Sci. China, Ser. B, 2008, 51, 1153-1158.

>[4] Towards quantitative prediction of molecular luminescence quantum efficiency: role of Duschinsky rotation, J. Am. Chem. Soc. 2007, 129(30), 9333-9339

>[5] General Approach To Compute Phosphorescent OLED Efficiency J. Phys. Chem. C, 2018, 122, 6340–6347

>[6] Excited states structure and processes: Understanding organic light-emitting diodes at the molecular level. Physics Reports 2014, 537(4): 123-156

>[7] Organic Light-emitting Diodes: Theoretical Understanding of Highly Efficient Materials and Develop-ment of Computational Methodology. Natl. Sci. Rev., 2017, 4, 224

>[8] Spectroscopic Signature of the Aggregation-Induced Emission Phenomena Caused by Restricted Nonradiative Decay: A Theoretical Proposal. J. Phys. Chem. C, 2015, 119(9), 5040-5047

>[9] Spectroscopic Signature of the Aggregation-Induced Emission Phenomena Caused by Restricted Nonradiative Decay: A Theoretical Proposal. J. Phys. Chem. C, 2015, 119(9), 5040-5047

>[10] Computational Evaluation of Optoelectronic Properties for Organic/Carbon Materials. Accounts of Chemical research, 2014, 47(11), 3301-3309

>[11] Computational Evaluation of Optoelectronic Properties for Organic/Carbon Materials. Accounts of Chemical research, 2014, 47(11), 3301-3309
