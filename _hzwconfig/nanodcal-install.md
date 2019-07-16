---
title: "Nanodcal安装教程"
permalink: /hzwconfig/nanodcal-install/
excerpt: "Nanodcal安装教程"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## Nanodcal自动安装脚本
&emsp;&emsp;本页中为您提供了Nanodcal自动安装脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令添加可执行权限，执行即可完成在鸿之微“一体机”上自动安装Nanodcal。

```sh
#!/bin/bash

# ---------- define the variables ---------- #

# directory of remote file server
ftp_server_ip=" "
ftp_file_path="pub"

# prefix the module install path
module_path="/public/modules-4.2.2"

# ---------- install gcc-4.7.4 for Nanodcal ---------- #

# 上传 gcc-4.7.4.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/gcc-4.7.4.tar.gz

tar -zxvf gcc-4.7.4.tar.gz
cd gcc-4.7.4
mkdir BUILD_Nanodcal
cd BUILD_Nanodcal
../configure --prefix=/public/gcc/gcc-4.7.4-Nanodcal --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib
make -j8
make install -j8

# ---------- configure the gcc-4.7.4-Nanodcal module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p gcc # 创建存放不同配置文件的文件夹
cd gcc

# 编辑gcc-4.7.4-Nanodcal的modulefile文件
cat > 4.7.4-Nanodcal << !
#%Module1.0
module-whatis "gcc-4.7.4-Nanodcal"
conflict gcc

prepend-path PATH /public/gcc/gcc-4.7.4-Nanodcal/bin
prepend-path LD_LIBRARY_PATH /public/gcc/gcc-4.7.4-Nanodcal/lib/
!

module unload gcc/4.4.7
module load gcc/4.7.4-Nanodcal

echo 'gcc-4.7.4 for nanodcal install finished' >> /public/system_config/nanodcal_install.log

# ---------- install openmpi-1.8.5 for Nanodcal ---------- #

# 上传 openmpi-1.8.5.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/openmpi-1.8.5.tar.gz

tar -zxvf openmpi-1.8.5.tar.gz
cd openmpi-1.8.5
make distclean
mkdir BUILD_Nanodcal
cd BUILD_Nanodcal
../configure --prefix=/public/mpi/openmpi-1.8.5-Nanodcal --disable-dlopen --enable-static --disable-shared CC=gcc CXX=g++ F77=gfortran FC=gfortran  CFLAGS=-fPIC  CXXFLAGS=-fPIC  FFLAGS=-fPIC  FCFLAGS=-fPIC
make
make install

# ---------- configure the openmpi-1.8.5-Nanodcal module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p mpi # 创建存放不同配置文件的文件夹
cd mpi

# 编辑openmpi-1.8.5-Nanodcal的modulefile文件
cat > openmpi-1.8.5-Nanodcal << !
#%Module1.0
module-whatis "openmpi-1.8.5-Nanodcal"
conflict mpi

prepend-path PATH /public/mpi/openmpi-1.8.5-Nanodcal/bin
prepend-path LD_LIBRARY_PATH /public/mpi/openmpi-1.8.5-Nanodcal/lib/
!

echo 'openmpi-1.8.5 for nanodcal install finished' >> /public/system_config/nanodcal_install.log

# ---------- install Matlab.R2015B.64 for Nanodcal ---------- #

# 上传 Matlab.R2015B.64.tar.gz源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/Matlab.R2015B.64.tar.gz

tar -zxvf Matlab.R2015B.64.tar.gz # 解压免安装包即可

# ---------- configure the Matlab.R2015B.64 module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p Matlab # 创建存放不同配置文件的文件夹
cd Matlab

# 编辑Matlab.R2015B的modulefile文件
cat > R2015B << !
#%Module1.0
module-whatis "Matlab-R2015B"

prepend-path PATH /software/MatlabR2015B/bin
!

echo 'Matlab.R2015B.64 for nanodcal install finished' >> /public/system_config/nanodcal_install.log

# ---------- install Nanodcal ---------- #

# 加载编译环境
module unload gcc/4.4.7
module load gcc/4.7.4-Nanodcal
module unload mpi/openmpi-1.8.5
module load mpi/openmpi-1.8.5-Nanodcal
module load Matlab/R2015B

# 上传 nanodcal20181201.zip源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Nanodcal/nanodcal20181201.zip

unzip nanodcal20181201.zip
cd Nanodcal

matlab -nodisplay -r "mex -setup; nanodcal -install I_Accept_The_End_User_License_Agreement; exit"

cd /software/Nanodcal
mkdir license
cd license
# 上传license.dat文件至license目录
/sbin/ifconfig > out.log
#grep 'inet addr' out.log |awk -F ":" '{print $2}'|awk '{print $1}' > ip.dat # for centos6
grep 'inet ' out.log |awk '{print $2}' > ip.dat # for centos7
rm -f out.log

echo 'nanodcal install finished' >> /public/system_config/nanodcal_install.log

```

## Nanodcal测试与提交pbs任务脚本

&emsp;&emsp;下文中为您提供了Nanodcal测试与提交pbs任务脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令t添加可执行权限，执行即可完成在鸿之微“一体机”上自动Nanodcal测试与提交pbs任务脚本

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
mkdir nanodcal_test
cd nanodcal_test

cp /public/pbs_script_example/nanodcal.pbs .
cp /public/software_test_example/nanodcal_test_example.tar.gz .

tar -zxvf nanodcal_test_example.tar.gz

cd nanodcal_test_example

cp ../nanodcal.pbs .

qsub nanodcal.pbs
EOF

echo 'nanodcal test job submit finished, type "qstat" to check the job status'
```
