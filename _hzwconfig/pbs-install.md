---
title: "任务管理Torque PBS安装"
permalink: /hzwconfig/pbs-install/
excerpt: "任务管理Torque PBS安装"
last_modified_at: 2019-08-18T15:53:52-04:00
toc: true
---
## Torque PBS自动安装脚本

&emsp;&emsp;本页中为您提供了任务管理Torque PBS自动安装脚本，您可以直接将下文中脚本内容复制后使用，然后使用`chmod +x`命令添加可执行权限，执行即可完成在鸿之微一体机上自动安装Torque PBS。

```sh
# ---------- install torque pbs ---------- #

# 安装依赖
yum -y install openssl openssl-devel
yum -y install libxml2 libxml2-devel
yum -y install boost boost-devel

# 上传 torque-6.1.3.tar.gz 源码至/software路径下
cd /software
sshpass -p $scp_server_pw scp $scp_server_user@$scp_server_ip:$scp_file_path/Software/System/torque-6.1.3.tar.gz .
tar -zxvf torque-6.1.3.tar.gz
cd torque-6.1.3
./configure # 安装在默认路径/usr/local/sbin,/usr/local/bin
make
make install # 卸载命令 make uninstall

# 添加系统服务并设为开机启动

systemctl enable trqauthd.service
systemctl enable pbs_server.service
systemctl enable pbs_mom.service

cp contrib/init.d/pbs_sched /etc/init.d/

chkconfig --add pbs_sched
chkconfig pbs_sched on

echo y | ./torque.setup root # qterm为终止pbs_server的运行,若不终止，无法初始化

# 查看逻辑CPU的个数

np=`cat /proc/cpuinfo| grep "processor"| wc -l`

cat > /var/spool/torque/server_priv/nodes << !
`hostname`   np=$np
!

systemctl restart trqauthd.service
# 如果服务启动失败，编辑 /etc/hosts 加入本机ip和主机名

for i in pbs_server pbs_sched pbs_mom ; do systemctl restart $i ; done

ps -e | grep pbs
ps -e | grep trqauthd
```
