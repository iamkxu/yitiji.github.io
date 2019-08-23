---
title: "FIRST案例"
permalink: /hzwsoftware/first-examples/
excerpt: "FIRST案例"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
---

## 演示案例：TADF-OLED发射层材料虚拟筛选

&emsp;&emsp;本案例通过结合数据库检索以及机器学习算法，从200万的有机分子数据库中筛选出最有潜力作为OLED发射层材料的分子结构。流程如下：建立项目 -> 数据库检索 -> 导入样本材料&数据可视化 -> 机器学习训练 -> 机器学习筛选。

### 案例流程：

&emsp;&emsp;案例文件夹：`/home/hzwtech/soft_test/first_test/OLED`

#### 1. 建立项目

&emsp;&emsp;打开本地机器上的FIRST软件，新建一个项目。
![image-center](/assets/images/first-introduce/example1.png)

#### 2. 数据库检索

&emsp;&emsp;点击左上角`Database-QuickMol`打开`QuickMol`有机分子数据库的界面。
![image-center](/assets/images/first-introduce/example2-QuickMol.png)

&emsp;&emsp;界面上有`2D`，`3D`, `Fragment`，`Element`共4类描述符可供选择，并附有中英文解释。输入各个描述符的取值范围后检索可以排除不符合的材料，缩小候选材料数量。后续版本可以根据用户需求自动推荐描述符和取值范围。
![image-center](/assets/images/first-introduce/example3-QuickMol.png)


#### 3.	导入样本材料&数据可视化

&emsp;&emsp;点击左上角`Sample-Molecule`。
![image-center](/assets/images/first-introduce/example4.png)

&emsp;&emsp;点击右上角`Import`按钮，导入不能发光的分子(`/home/hzwtech/soft_test/first_test/OLED/nonOLED`文件夹中的`.mol`文件)，并再`Class No.`中标记为`0`类；再点击`Import`按钮，导入能够发光的分子，标记为`1`类。共导入2类327个有机分子。

&emsp;&emsp;在描述符界面中选择`BertzCT`, `BalabanJ`, `Kappa1`, `Kappa2`, `Kappa3`, `LabuteASA`, `RotatableBondsRatio`, `qed`。点击右下角`Generate`，生成数据文件`molecule_redefine.csv`。
![image-center](/assets/images/first-introduce/example5.png)

&emsp;&emsp;选中数据表的3列, 例如`BertzCT`, `BalabanJ`, `Kappa1`，然后在数据表菜单栏中选择`Plot-Scatter3DP`，可以绘制出带颜色标度的准3维散点图。`Plot`菜单中还有柱状图, 小提琴图, 关联热力图等选项可供用户用于数据可视化。
![image-center](/assets/images/first-introduce/example6.png)

#### 4.	机器学习训练
&emsp;&emsp;数据表菜单`AI-Classification-Logistic`，选择逻辑回归分类算法对数据进行训练，得到把分子准确判断为能否作为OLED材料的分类器。

&emsp;&emsp;把`Class`设为目标变量`target variable`, `SMILES`设为注释变量`annotated variable`, 其余变量作为独立变量`Independent variable`。
![image-center](/assets/images/first-introduce/example7.png)

&emsp;&emsp;选择参数后点击`Run`按钮，完成训练。
![image-center](/assets/images/first-introduce/example8.png)

#### 5.	机器学习筛选
&emsp;&emsp;训练完成后在项目文件夹中生成了与数据表同名的文件夹`molecule_redefine`，机器学习训练得到的模型为`.pkl`后缀。

![image-center](/assets/images/first-introduce/example9.png)

&emsp;&emsp;点击`Database-QuickMol`, 选择`AI Descriptor`, 点击`load`按钮加载训练好的`.pkl`后缀的机器学习模型，选择`1`类。点击`filter`按钮完成筛选。
![image-center](/assets/images/first-introduce/example10.png)
