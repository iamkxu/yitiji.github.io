---
title: "RESCU安装教程"
permalink: /hzwconfig/rescu-install/
excerpt: "RESCU安装教程"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
---

## RESCU自动安装脚本

&emsp;&emsp;本页中为您提供了RESCU自动安装脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令添加可执行权限，执行即可完成在鸿之微一体机上自动安装RESCU。

```sh
#!/bin/bash

# ---------- define the variables ---------- #

# directory of remote file server
ftp_server_ip=" "
ftp_file_path="pub"

# prefix the module install path
module_path="/public/modules-4.2.2"

# ---------- install gcc-4.7.4 for RESCU ---------- #

cd /software # 上传 gcc-4.7.4.tar.gz 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/gcc-4.7.4.tar.gz

tar -zxvf gcc-4.7.4.tar.gz
cd gcc-4.7.4
mkdir BUILD_RESCU
cd BUILD_RESCU
../configure --prefix=/public/gcc/gcc-4.7.4-RESCU --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib
make -j8
make install -j8

# ---------- configure the gcc-4.7.4-RESCU module enviroment ---------- #

cd $module_path/modulefiles/gcc
cat > 4.7.4-RESCU << !
#%Module1.0
module-whatis "GCC 4.7.4-RESCU"
conflict gcc

set gcc_path /public/gcc/gcc-4.7.4-RESCU
prepend-path PATH \$gcc_path/bin
prepend-path MANPATH	\$gcc_path/share/man
prepend-path LD_LIBRARY_PATH \$gcc_path/lib64/:\$gcc_path/lib
!

module unload gcc/4.4.7
module load gcc/4.7.4-RESCU

echo 'gcc-4.7.4 for rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install openmpi-1.8.5 for RESCU ---------- #

# 上传 openmpi-1.8.5.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/openmpi-1.8.5.tar.gz

tar -zxvf openmpi-1.8.5.tar.gz
cd openmpi-1.8.5
make distclean
mkdir BUILD_RESCU
cd BUILD_RESCU
../configure --prefix=/public/mpi/openmpi-1.8.5-RESCU --disable-dlopen --enable-static --disable-shared CC=gcc CXX=g++ F77=gfortran FC=gfortran  CFLAGS=-fPIC  CXXFLAGS=-fPIC  FFLAGS=-fPIC  FCFLAGS=-fPIC
make
make install

# ---------- configure the openmpi-1.8.5-RESCU module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p mpi # 创建存放不同配置文件的文件夹
cd mpi

# 编辑openmpi-1.8.5-RESCU的modulefile文件
cat > openmpi-1.8.5-RESCU << !
#%Module1.0
module-whatis "openmpi-1.8.5-RESCU"
conflict mpi

prepend-path PATH /public/mpi/openmpi-1.8.5-RESCU/bin
prepend-path LD_LIBRARY_PATH /public/mpi/openmpi-1.8.5-RESCU/lib/
!

echo 'openmpi-1.8.5 for rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install lapack-3.5.0 for RESCU ---------- #

# 上传 lapack-3.5.0.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/lapack-3.5.0.tar.gz

tar -zxvf lapack-3.5.0.tar.gz
cd lapack-3.5.0
make blaslib
make lapacklib

echo 'lapack-3.5.0 for rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install OpenBLAS-0.2.20 for RESCU ---------- #

# 上传 OpenBLAS-0.2.20.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/OpenBLAS-0.2.20.tar.gz

tar -zxvf OpenBLAS-0.2.20.tar.gz
cd OpenBLAS-0.2.20
make CC=gcc FC=gfortran NO_AVX2=1

echo 'OpenBLAS-0.2.20 for rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install scalapack-2.0.2 for RESCU ---------- #

# 上传 scalapack-2.0.2.tar.gz源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/scalapack-2.0.2.tar.gz

tar -zxvf scalapack-2.0.2.tar.gz
cd scalapack-2.0.2
make lib

echo 'scalapack-2.0.2 for rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install libxc-3.0.0 for RESCU ---------- #

# 上传 libxc-3.0.0.tar.gz源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/libxc-3.0.0.tar.gz

tar -zxvf libxc-3.0.0.tar.gz
cd libxc-3.0.0
mkdir BUILD
cd BUILD
../configure --prefix=/public/libxc/libxc-3.0.0 CFLAGS="-O2 -fPIC"
make
make install

echo 'libxc-3.0.0 for rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install Matlab.R2015B.64 for RESCU ---------- #

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

echo 'Matlab.R2015B.64 for rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install RESCU ---------- #

# 加载编译环境
module unload gcc/4.4.7
module load gcc/4.7.4-RESCU
module unload mpi/openmpi-1.8.5
module load mpi/openmpi-1.8.5-RESCU
module load Matlab/R2015B

# 上传 rescu.tar.gz源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/RESCU/rescu-2.1.0.tar.gz

tar -zxvf rescu-2.1.0.tar.gz
cd rescu-2.1.0

echo "export OPENBLAS_NUM_THREADS=1">barc
source ./barc

echo "lapack='/software/lapack-3.5.0/liblapack.a';">lapackpath
echo "blas='/software/OpenBLAS-0.2.20/libopenblas.a -lz -lgfortran';">blaspath
echo "scalapack='/software/scalapack-2.0.2/libscalapack.a';">scalapackpath
echo "xc='/public/libxc/libxc-3.0.0';">libxcpath

cat lapackpath blaspath scalapackpath libxcpath >smilibxc.m
rm -f lapackpath blaspath scalapackpath libxcpath

cd src
matlab -nodisplay -r "makeRESCU -agreeToLicense y -mpi -smi -libxc ../smilibxc.m;exit"

cd ../license
/sbin/ifconfig > out.log
#grep 'inet addr' out.log |awk -F ":" '{print $2}'|awk '{print $1}' > ip.dat # for centos6
grep 'inet ' out.log |awk '{print $2}' > ip.dat # for centos7
rm -f out.log

echo 'rescu install finished' >> /public/system_config/rescu_install.log

# ---------- install MCR ---------- #

cd /software/MatlabR2015B/toolbox/compiler/deploy/glnxa64 # 找到MCR 安装包
unzip MCRInstaller.zip
./install -mode silent -agreeToLicense yes -destinationFolder /public/MCR/

# ---------- configure the MCR module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p MCR # 创建存放不同配置文件的文件夹
cd MCR

# 编辑Matlab.R2015B的modulefile文件
cat > v90 << !
#%Module1.0
module-whatis "MCR-v90"

prepend-path LD_LIBRARY_PATH /public/MCR/v90/runtime/glnxa64
prepend-path LD_LIBRARY_PATH /public/MCR/v90/bin/glnxa64
prepend-path LD_LIBRARY_PATH /public/MCR/v90/sys/os/glnxa64
prepend-path LD_LIBRARY_PATH /public/MCR/v90/sys/opengl/lib/glnxa64
!

#修改module启动文件
cat >> $module_path/init/modulerc << !
module load MCR/v90
!

echo 'MCR for rescu install finished' >> /public/system_config/rescu_install.log

module load MCR/v90

chown -R root:root /software/
chown -R root:root /public/
chmod -R +rx /software/rescu-2.1.0

cd /software/rescu-2.1.0
mcc -m rescu -a /software/rescu-2.1.0/src -a /software/rescu-2.1.0/license -R '-nodisplay,-singleCompThread'

echo 'mcc verison rescu install finished' >> /public/system_config/rescu_install.log
```

## RESCU测试与提交pbs任务脚本

&emsp;&emsp;下文中为您提供了RESCU测试与提交pbs任务脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令t添加可执行权限，执行即可完成在鸿之微一体机上自动RESCU测试与提交pbs任务脚本

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
mkdir rescu_test
cd rescu_test

cp /public/pbs_script_example/rescu.pbs .
cp /public/software_test_example/rescu_test_example.tar.gz .

tar -zxvf rescu_test_example.tar.gz

cd rescu_test_example

cp ../rescu.pbs .

qsub rescu.pbs
EOF

echo 'rescu test job submit finished, type "qstat" to check the job status'
```
