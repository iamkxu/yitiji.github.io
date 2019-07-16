---
title: "第一性原理-VASP安装教程"
permalink: /hzwconfig/vasp-install/
excerpt: "第一性原理-VASP安装教程"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## VASP自动安装脚本

&emsp;&emsp;本页中为您提供了VASP自动安装脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令添加可执行权限，执行即可完成在鸿之微“一体机”上自动安装VASP。

```sh
#!/bin/bash

# ---------- define the variables ---------- #

# directory of remote file server
ftp_server_ip=" "
ftp_file_path="pub"

# prefix the module install path
module_path="/public/modules-4.2.2"

# ---------- install intel icc compiler ---------- #

cd /software # 上传 intel_icc_2019 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/intel_icc/parallel_studio_xe_2019_update3_cluster_edition.tgz

tar zxvf parallel_studio_xe_2019_update3_cluster_edition.tgz
cd parallel_studio_xe_2019_update3_cluster_edition

wget ftp://$ftp_server_ip/$ftp_file_path/Software/intel_icc/silent.2019.cfg

./install.sh --silent=silent.2019.cfg

source /public/intel/bin/compilervars.sh  intel64

echo 'intel icc for vasp install finished' >> /public/system_config/vasp_install.log

# ---------- install openmpi-2.0.4 with intel icc compiler ---------- #

cd /software # 上传 openmpi-2.0.4.tar.gz 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/openmpi-2.0.4.tar.gz

tar -zxvf openmpi-2.0.4.tar.gz
cd openmpi-2.0.4
mkdir BUILD_openmpi_icc
cd BUILD_openmpi_icc
../configure --prefix=/public/mpi/openmpi-2.0.4-icc CC=icc  CXX=icpc  FC=ifort  F77=ifort
make -j 4
make install

# ---------- configure the openmpi-2.0.4-icc module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p mpi # 创建存放不同配置文件的文件夹
cd mpi

# 编辑openmpi-2.0.4-icc的modulefile文件
cat > openmpi-2.0.4-icc << !
#%Module1.0

module-whatis   "openmpi-2.0.4-icc"
conflict  mpi

prepend-path    PATH               /public/mpi/openmpi-2.0.4-icc/bin
prepend-path    LD_LIBRARY_PATH    /public/mpi/openmpi-2.0.4-icc/lib/
!

echo 'openmpi-2.0.4-icc for vasp install finished' >> /public/system_config/vasp_install.log

# ---------- install vasp5.4.1 ---------- #

module unload mpi/openmpi-1.8.5
module load mpi/openmpi-2.0.4-icc

cd /software # 上传 vasp5.4.1.tar源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/VASP/vasp.5.4.1.tar

tar -xvf vasp.5.4.1.tar
cd vasp.5.4.1/arch

cp makefile.include.linux_intel ../makefile.include
# 编译fftw 位于$(MKLROOT)/interfaces/fftw3xf/libfftw3xf_intel.a
cd $MKLROOT/interfaces/fftw3xf
make libintel64 # 生成：libfftw3xf_intel.a

cd /software/vasp.5.4.1
make all # 编译vasp；或者 make std

cp /software/vasp.5.4.1/bin/vasp_std /public/bin/vasp

echo 'vasp install finished' >> /public/system_config/vasp_install.log

```

## VASP测试与提交pbs任务脚本

&emsp;&emsp;下文中为您提供了VASP测试与提交pbs任务脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令t添加可执行权限，执行即可完成在鸿之微“一体机”上自动VASP测试与提交pbs任务脚本

```sh
#!/bin/bash

# ---------- define the variables ---------- #

# set the user you want to use for doing the test_example

test_user="hzwtech"

# ---------- do the example test ---------- #

su $test_user <<EOF
cd ~
mkdir soft_test
cd soft_test
mkdir vasp_test
cd vasp_test

cp /public/pbs_script_example/vasp.pbs .
cp /public/software_test_example/vasp_test_example.tar.gz .

tar -zxvf vasp_test_example.tar.gz

cd vasp_test_example

cp ../vasp.pbs .

qsub vasp.pbs
EOF

echo 'vasp test job submit finished, type "qstat" to check the job status'
```
