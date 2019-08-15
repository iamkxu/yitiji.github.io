---
title: "鸿之微-大体系KS-DFT计算一体机"
permalink: /hzwhardware/rescu-software/
excerpt: "鸿之微-大体系KS-DFT计算一体机"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
gallery:
  - url: /assets/images/rescu-image/rescu_main_function.png
    image_path: /assets/images/rescu-image/rescu_main_function.png
    alt: "RESCU主要功能"
    title: "RESCU主要功能"

---

&emsp;&emsp;鸿之微大体系KS-DFT计算一体机是鸿之微开发的一款搭载RESCU计算软件的科学模拟平台。

---

&emsp;&emsp;&emsp;[<i class="fas fa-shopping-cart"></i> 选择硬件](/hzwhardware/rescu-hardware/){: .btn .btn--success .btn--large}
{: .text-right}

&emsp;&emsp;RESCU是一款仅仅用小型计算机就能研究超大体系的KS-DFT计算软件。 RESCU是Real space Electronic Structure CalcUlator（实空间电子结构计算程序）的缩写，它的核心是一种全新、极其强大、并行效率超高的KS-DFT自洽计算方法。

&emsp;&emsp;RESCU可以使用一体机种有限的计算机资源，并且还可以使用GPU进行加速计算，来计算包含一千、数千、上万体系的电子结构性质。RESCU是解决超大体系KS-DFT问题的里程碑式的新方法，正在被应用于金属、半导体、绝缘体、液体、DNA、1维、2维、3维、表面、分子、磁性、非磁性、杂质、固体等等不同系统的KS-DFT计算。

## 主要功能

{% include gallery id="gallery" caption="RESCU主要功能" %}

## 理论计算和软件特点

- 利用数值化原子轨道（NAO )生成非常有效的初始希尔伯特子空间。

- 利用Chebyshev滤波，完全避免了在全希尔伯特空间求解本征值来精确求解KS-DFT的过程。

- 通过新颖的计算过程，将求解KS-DFT方程的O()度规大大改善。即使当电子数N达到数万，RESCU的计算度规也仅仅为O()。

- 在256核的小型计算平台上，使用实空间离散格点基矢，计算超过8000个原子规模的体系；而若用NAO基矢，体系的规模升高至约15000个原子的别。在更大的计算机上，可以期待研究超大的系统。

- 当系统包含数万个电子时，RESCU采用全新的partial Rayleigh-Ritz算法保证了超高的计算效率。

- 计算精度：使用实空间离散格点基矢时与其他标准的平面波基矢KS-DFT软件在同一级别；用NAO基矢时与其他标准的 LCAO基矢KS-DFT软件在同一级别。

---

## 参考论文

>[1]	Michaud-Rioux V, Zhang L, Guo H. RESCU: A real space electronic structure method[J]. Journal of Computational Physics, 2016,307:593-613.

>[2]	Kang P, Zhang W, Michaud-Rioux V, et al. Moiré impurities in twisted bilayer black phosphorus: Effects on the carrier mobility[J]. Physical Review B, 2017,96(19):195406.

>[3]	Kang P, Michaud-Rioux V, Kong X, et al. Calculated carrier mobility of h-BN/γ-InSe/h-BN van der Waals heterostructures[J]. 2D Materials, 2017,4(4):45014.

>[4]	Chowdhury F A, Sadaf S M, Shi Q, et al. Optically active dilute-antimonide III-nitride nanostructures for optoelectronic devices[J]. Applied Physics Letters, 2017,111(6):61101.

>[5]	Hu C, Michaud-Rioux V, Yao W, et al. Moiré Valleytronics: Realizing Dense Arrays of Topological Helical Channels[J]. Physical Review Letters, 2018,121(18):186403.

>[6]	Lin W, Li J, Wang W, et al. Electronic Structure and Band Gap Engineering of Two-Dimensional Octagon-Nitrogene[J]. Scientific Reports, 2018,8(1):1674.

>[7]	Lin W, Liang S, He C, et al. Stabilities and novel electronic structures of three carbon nitride bilayers[J]. Scientific Reports, 2019,9(1):1025.

>[8]	Hu C, Michaud-Rioux V, Kong X, et al. Dirac electrons in Moiré superlattice: From two to three dimensions[J]. Physical Review Materials, 2017,1(6):061003.

>[9]	Shi Q, Chen Y, Chowdhury F A, et al. Band engineering of GaSbN alloy for solar fuel applications[J]. Physical Review Materials, 2017,1(3):034602.

>[10]	Liu D, Chen X, Hu Y, et al. Raman enhancement on ultra-clean graphene quantum dots produced by quasi-equilibrium plasma-enhanced chemical vapor deposition[J]. Nature Communications, 2018,9(1):193.

>[11]	Chen Y, Chen J, Michaud-Rioux V, et al. Efficient evaluation of nonlocal operators in density functional theory[J]. Physical Review B, 2018,97(7):075139.

>[12]	Yuan W, Niu G, Xian Y, et al. In Situ Regulating the Order–Disorder Phase Transition in Cs2AgBiBr6 Single Crystal toward the Application in an X-Ray Detector[J]. Advanced Functional Materials, 2019,29(20):1900234.

>[13]	Jin H, Michaud-Rioux V, Gong Z, et al. Size dependence in two-dimensional lateral heterostructures of transition metal dichalcogenides[J]. Journal of Materials Chemistry C, 2019,7(13):3837-3842.

>[14]	Chen X, Xu Y, Wang J, et al. Valley filtering effect of phonons in graphene with a grain boundary[J]. Physical Review B, 2019,99(6):064302.

>[15]	Wang H, Wu H, Xian Y, et al. Controllable CsxFA1-xPbI3 Single-Crystal Morphology via Rationally Regulating the Diffusion and Collision of Micelles toward High-Performance Photon Detectors[J]. ACS Applied Materials & Interfaces, 2019,11(14):13812-13821.
