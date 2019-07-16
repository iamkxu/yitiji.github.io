---
title: "分子动力学-Lammps安装教程"
permalink: /hzwconfig/lammps-install/
excerpt: "分子动力学-Lammps安装教程"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
---

## Lammps自动安装脚本
&emsp;&emsp;本页中为您提供了Lammps自动安装脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令添加可执行权限，执行即可完成在鸿之微“一体机”上自动安装Lammps。

```sh
#!/bin/bash

# ---------- define the variables ---------- #

# directory of remote file server
ftp_server_ip=" "
ftp_file_path="pub"

# prefix the module install path
module_path="/public/modules-4.2.2"

# ---------- install openmpi-1.8.5 for Lammps ---------- #

cd /software # 上传 openmpi-1.8.5.tar.gz 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/openmpi-1.8.5.tar.gz
tar -zxvf openmpi-1.8.5.tar.gz
cd openmpi-1.8.5
make distclean
mkdir BUILD_Lammps
cd BUILD_Lammps
../configure --prefix=/public/mpi/openmpi-1.8.5
make
make install

# ---------- configure the openmpi-1.8.5 module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p mpi # 创建存放不同配置文件的文件夹
cd mpi

# 编辑openmpi-1.8.5的modulefile文件
cat > openmpi-1.8.5 << !
#%Module1.0
module-whatis   "openmpi-1.8.5"
conflict  mpi
prepend-path    PATH               /public/mpi/openmpi-1.8.5/bin
prepend-path    LD_LIBRARY_PATH    /public/mpi/openmpi-1.8.5/lib/
!
#修改module启动文件
cat >> $module_path/init/modulerc <<!
module load mpi/openmpi-1.8.5
!

echo 'openmpi-1.8.5 for lammps install finished' >> /public/system_config/lammps_install.log

# ---------- install fftw-2.1.5 for lammps ---------- #

cd /software # 上传 fftw-2.1.5.tar.gz 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/fftw-2.1.5.tar.gz

tar -zxvf fftw-2.1.5.tar.gz
cd fftw-2.1.5
mkdir BUILD_Lammps
cd BUILD_Lammps
../configure --prefix=/public/fftw/fftw-2.1.5
make
make install

# ---------- configure the fftw-2.1.5 module enviroment ---------- #

cd  $module_path/modulefiles
mkdir -p fftw # 创建存放不同配置文件的文件夹
cd fftw
# 编辑fftw-2.1.5的modulefile文件
cat > fftw-2.1.5 <<!
#%Module1.0
module-whatis   "fftw-2.1.5"

prepend-path    LD_LIBRARY_PATH         /public/mpi/fftw-2.1.5/lib/
!

#修改module启动文件
cat >> $module_path/init/modulerc <<!
module load fftw/fftw-2.1.5
!

echo 'fftw-2.1.5 for lammps install finished' >> /public/system_config/lammps_install.log

# ---------- install jpeg library for lammps ---------- #

# 上传 jpegsrc.v9c.tar.gz 源码至/software路径下
cd /software
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/jpegsrc.v9c.tar.gz

tar -zxvf jpegsrc.v9c.tar.gz
cd jpeg-9c
./configure --prefix=/public/jpeg/jpeg-9c
make
make install

# ---------- configure the jpeg module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p jpeg # 创建存放不同配置文件的文件夹
cd jpeg

 # 编辑jpeg-9c的modulefile文件
cat > jpeg-9c <<!
#%Module1.0
module-whatis   "jpeg-9c"

prepend-path    LD_LIBRARY_PATH         /public/jpeg/jpeg-9c/lib/
!

#修改module启动文件
cat >> $module_path/init/modulerc <<!
module load jpeg/jpeg-9c
!

echo 'jpeg-9c for lammps install finished' >> /public/system_config/lammps_install.log

# ---------- install yasm & ffmpeg library for lammps ---------- #

cd /software # 上传 yasm-1.3.0.tar.gz 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/yasm-1.3.0.tar.gz

tar -zxvf yasm-1.3.0.tar.gz
cd yasm-1.3.0
./configure --prefix=/public/yasm/yasm-1.3.0
make
make install

# ---------- configure the yasm module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p yasm # 创建存放不同配置文件的文件夹
cd yasm

# 编辑yasm-1.3.0的modulefile文件
cat > yasm-1.3.0 <<!
#%Module1.0
module-whatis   "yasm-1.3.0"

prepend-path    PATH                /public/yasm/yasm-1.3.0/bin
prepend-path    LD_LIBRARY_PATH         /public/yasm/yasm-1.3.0/lib/
!

#修改module启动文件
cat >> $module_path/init/modulerc  <<!
module load yasm/yasm-1.3.0
!

echo 'yasm for lammps install finished' >> /public/system_config/lammps_install.log

# ---------- install yasm & ffmpeg library for lammps ---------- #

module load yasm/yasm-1.3.0

cd /software # 上传 ffmpeg-4.1.tar.bz2 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Library/ffmpeg-4.1.tar.bz2

tar -jxvf ffmpeg-4.1.tar.bz2
cd ffmpeg-4.1
./configure --prefix=/public/ffmpeg/ffmpeg-4.1
make
make install

# ---------- configure the ffmpeg module enviroment ---------- #

cd $module_path/modulefiles
mkdir -p ffmpeg # 创建存放不同配置文件的文件夹
cd ffmpeg

# 编辑ffmpeg-4.1的modulefile文件
cat > ffmpeg-4.1  <<!
#%Module1.0
module-whatis   "ffmpeg-4.1"

prepend-path    PATH            /public/ffmpeg/ffmpeg-4.1/bin
prepend-path    LD_LIBRARY_PATH         /public/ffmpeg/ffmpeg-4.1/lib/
!

#修改module启动文件
cat >> $module_path/init/modulerc <<!
module load ffmpeg/ffmpeg-4.1
!

echo 'ffmpeg for lammps install finished' >> /public/system_config/lammps_install.log

# ---------- install lammps ---------- #

# 加载编译环境
module load mpi/openmpi-1.8.5
module load fftw/fftw-2.1.5
module load jpeg/jpeg-9c
module load yasm/yasm-1.3.0
module load ffmpeg/ffmpeg-4.1

cd /software # 上传 lammps-stable.tar.gz 源码至/software路径下
wget ftp://$ftp_server_ip/$ftp_file_path/Software/Lammps/lammps-stable.tar.gz

tar -xvf lammps-stable.tar.gz
cd lammps-12Dec18 # 当前稳定版本lammps-12Dec18
cd src
make yes-all
make no-lib

# 编译并行版Lammps,修改Make文件

cd MAKE/OPTIONS
cp Makefile.g++_openmpi  Makefile.g++_openmpi.bak

sed -i 's/^.*LMP_INC =.*$/LMP_INC = -DLAMMPS_GZIP -DLAMMPS_JPEG -DLAMMPS_FFMPEG/g' Makefile.g++_openmpi

sed -i 's/^.*MPI_INC =.*$/MPI_INC =      -I\/public\/mpi\/openmpi-1.8.5\/bin\/include/g' Makefile.g++_openmpi
sed -i 's/^.*MPI_LIB =.*$/MPI_LIB =	    -L\/public\/mpi\/openmpi-1.8.5\/bin\/lib -lmpi/g' Makefile.g++_openmpi

sed -i 's/^.*FFT_INC =.*$/FFT_INC =    	-I\/public\/fftw\/fftw-2.1.5\/include/g' Makefile.g++_openmpi
sed -i 's/^.*FFT_LIB =.*$/FFT_LIB =	    -L\/public\/fftw\/fftw-2.1.5\/lib -lfftw/g' Makefile.g++_openmpi

sed -i 's/^.*JPG_INC =.*$/JPG_INC =	   -I\/public\/jpeg\/jpeg-9c\/include/g' Makefile.g++_openmpi
sed -i 's/^.*JPG_PATH =.*$/JPG_PATH = 	 -L\/public\/jpeg\/jpeg-9c\/lib/g' Makefile.g++_openmpi
sed -i 's/^.*JPG_LIB =.*$/JPG_LIB =	   -ljpeg/g' Makefile.g++_openmpi

cd /software/lammps-12Dec18/src
make g++_openmpi
cp lmp_g++_openmpi /public/bin/lammps-20181212-mpi

# 编译Lammps中msi2lmp.exe格式转换工具

cd /software/lammps-12Dec18/tools/msi2lmp/src
make
cp msi2lmp.exe /public/bin/msi2lmp.exe

echo 'lammps install finished' >> /public/system_config/lammps_install.log

```

## Lammps测试与提交pbs任务脚本

&emsp;&emsp;下文中为您提供了Lammps测试与提交pbs任务脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令t添加可执行权限，执行即可完成在鸿之微“一体机”上自动Lammps测试与提交pbs任务脚本

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
mkdir lammps_test
cd lammps_test

cp /public/pbs_script_example/lammps.pbs .
cp /public/software_test_example/lammps_test_example.tar.gz .

tar -zxvf lammps_test_example.tar.gz

cd lammps_test_example

cp ../lammps.pbs .

qsub lammps.pbs
EOF

echo 'lammps test job submit finished, type "qstat" to check the job status'

```
