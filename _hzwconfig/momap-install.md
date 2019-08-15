---
title: "MOMAP安装教程"
permalink: /hzwconfig/momap-install/
excerpt: "MOMAP安装教程"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
---

## MOMAP自动安装脚本

&emsp;&emsp;本页中为您提供了MOMAP自动安装脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令添加可执行权限，执行即可完成在鸿之微一体机上自动安装MOMAP。

```sh
#!/bin/bash

# ---------- define the variables ---------- #

# directory of remote file server
ftp_server_ip=" "
ftp_file_path="pub"

# prefix the module install path
module_path="/public/modules-4.2.2"

# ---------- install mpich2 for MOMAP ---------- #

# 上传 mpich2-1.5.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/mpich2-1.5.tar.gz
tar -zxvf mpich2-1.5.tar.gz
cd mpich2-1.5
mkdir BUILD_MOMAP
cd BUILD_MOMAP
../configure --prefix=/public/mpi/mpich2-1.5 --enable-f77 --enable-fc --enable-static --enable-shared
make
make install

# ---------- configure the mpich2-1.5 module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p mpi # 创建存放不同配置文件的文件夹
cd mpi

# 编辑mpich2-1.5的modulefile文件
cat > mpich2-1.5 << !
#%Module1.0
module-whatis "mpich2-1.5"
conflict mpi

prepend-path PATH /public/mpi/mpich2-1.5/bin
prepend-path LD_LIBRARY_PATH /public/mpi/mpich2-1.5/lib/
!

module unload mpi/openmpi-1.8.5
module load mpi/mpich2-1.5

echo 'mpich2-1.5 for momap install finished' >> /public/system_config/momap_install.log

# ---------- install lapack for MOMAP ---------- #

# 上传 lapack-3.7.0.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/lapack-3.7.0.tar.gz
tar -zxvf lapack-3.7.0.tar.gz
cd lapack-3.7.0
patch -p0 < shared-lapack-3.7.0.patch
export LD_LIBRARY_PATH=/software/lapack-3.7.0:$LD_LIBRARY_PATH
cd BLAS/SRC
make
cd ../..
make

echo 'lapack for momap install finished' >> /public/system_config/momap_install.log

# ---------- install fftw-3.3.5 for MOMAP ---------- #

# 上传 fftw-3.3.5.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/fftw-3.3.5.tar.gz
tar -zxvf fftw-3.3.5.tar.gz
cd fftw-3.3.5
./configure --prefix=/public/fftw/fftw-3.3.5 --enable-threads --enable-openmp --enable-static --enable-shared --enable-fortran  F77=gfortran
make
make install

echo 'fftw-3.3.5 for momap install finished' >> /public/system_config/momap_install.log

# ---------- install MOMAP ---------- #

# 上传 momap-1.0.3.dev-mpich2.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Momap/momap-1.0.3.dev-mpich2.tar.gz
tar -zxvf momap-1.0.3.dev-mpich2.tar.gz
cd momap-1.0.3.dev-mpich2
# 上传license至/software/momap-1.0.3.dev-mpich2路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Momap/license.lic
cp environment.example ./bin/.environment

# 修改以下四行MOMAP相关安装路径信息
sed -i 's/MOMAP_ROOT=\/home\/hzw\/MOMAP\/momap-1.0.3.dev-mpich2/MOMAP_ROOT=\/software\/momap-1.0.3.dev-mpich2/g' ./bin/.environment
sed -i 's/MOMAP_LAPACK_LIB=\/home\/hzw\/MOMAP\/lapack-3.7.0/MOMAP_LAPACK_LIB=\/software\/lapack-3.7.0/g' ./bin/.environment
sed -i 's/MOMAP_FFTW_LIB=\/home\/hzw\/MOMAP\/fftw-3.3.5\/lib/MOMAP_FFTW_LIB=\/public\/fftw\/fftw-3.3.5\/lib/g' ./bin/.environment
sed -i 's/MOMAP_MPI_ROOT=\/home\/hzw\/MOMAP\/mpich2-1.5/MOMAP_MPI_ROOT=\/public\/mpi\/mpich2-1.5/g' ./bin/.environment

echo 'export PATH=/software/momap-1.0.3.dev-mpich2/bin:$PATH' >> /etc/profile.d/hzw_software.sh

echo 'momap install finished' >> /public/system_config/momap_install.log

```

## MOMAP测试与提交pbs任务脚本

&emsp;&emsp;下文中为您提供了RESCU测试与提交pbs任务脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令t添加可执行权限，执行即可完成在鸿之微一体机上自动MOMAP测试与提交任务脚本

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
mkdir momap_test
cd momap_test

cp /public/pbs_script_example/momap.pbs .
cp /public/software_test_example/momap_test_example.tar.gz .

tar -zxvf momap_test_example.tar.gz

cd momap_test_example/photophysics

cp ../../momap.pbs .

qsub momap.pbs
EOF

echo 'momap test job submit finished, type "qstat" to check the job status'

```
