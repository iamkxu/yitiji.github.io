---
title: "CentOS 操作系统使用简介"
permalink: /hzwsoftware/centos-introduce/
excerpt: "CentOS 操作系统使用简介"
last_modified_at: 2019-04-18T15:53:52-04:00
toc: true
gallery:
  - url: /assets/images/yitiji-introduce/yitiji-desktop-0.png
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-0.png
    alt: "点击账户名`hzwtech`"
    title: "点击账户名`hzwtech`"
  - url: /assets/images/yitiji-introduce/yitiji-desktop-1.png
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-1.png
    alt: "选择`Cinnamon`桌面"
    title: "选择`Cinnamon`桌面"
  - url: /assets/images/yitiji-introduce/yitiji-desktop-2.png
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-2.png
    alt: "输入密码、点击`Sigh in`登陆系统"
    title: "输入密码、点击`Sigh in`登陆系统"
gallery2:
  - url: /assets/images/yitiji-introduce/yitiji-desktop-3.png
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-3.png
    alt: "鸿之微“一体机”桌面示意图"
    title: "鸿之微“一体机”桌面示意图"
  - url: /assets/images/yitiji-introduce/yitiji-desktop-4.png
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-4.png
    alt: "鸿之微“一体机”桌面示意图"
    title: "鸿之微“一体机”桌面示意图"
gallery3:
  - url: /assets/images/yitiji-introduce/yitiji-desktop-5.png
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-5.png
    alt: "文件管理&文本编辑"
    title: "文件管理&文本编辑"
gallery4:
  - url: /assets/images/yitiji-introduce/yitiji-desktop-6.png
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-6.png
    alt: "打开**`Terminal`命令行终端**"
    title: "打开**`Terminal`命令行终端**"
gallery5:
  - url: /assets/images/yitiji-introduce/yitiji-desktop-7.gif
    image_path: /assets/images/yitiji-introduce/yitiji-desktop-7.gif
    alt: "使用`WinSCP`和`XShell`软件，从Windows设备远程登陆到鸿之微“一体机”"
    title: "使用`WinSCP`和`XShell`软件，从Windows设备远程登陆到鸿之微“一体机”"
---

&emsp;&emsp;CentOS（Community Enterprise Operating System，中文意思是社区企业操作系统）是Linux发行版之一，CentOS是一个基于Red Hat Linux提供的可自由使用源代码的企业级Linux发行版本，而且在RHEL的基础上修正了不少已知的Bug，相对于其他Linux发行版，其稳定性值得信赖。

## 开始使用

&emsp;&emsp;我们在安装操作系统时，桌面环境在预装的`GNOME`、`KDE`之外，还为您定制了`Cinnamon`桌面环境，`Cinnamon`桌面具有接近`Windows`操作习惯等优点，帮您在初次接触`Linux`系统桌面计算时获得最优良的用户体验。您可以根据自己的使用习惯选择喜欢或习惯的桌面。

### 进入桌面

&emsp;&emsp;当您正常开机点亮鸿之微“一体机”后，您可按下图中操作流程进入桌面，需要操作处参见图中白色箭头。

{% include gallery caption="点击账户名`hzwtech`，选择桌面（图中以`Cinnamon`为例），输入密码`hzw@123`、点击`Sigh in`登陆系统" %}

&emsp;&emsp;进入桌面后，可以看到如下图所示桌面，下文中以`Cinnamon`桌面为例。`Cinnamon`桌面与`Windows`桌面很多相似之处，例如，底部有一个类似任务栏的面板。操作习惯也与`Windows`操作习惯接近。下面介绍`CentOS`最重要的几样功能组件，更详细的使用方法您可以在互联网上直接搜索相关信息，`GNOME`、`KDE`桌面的使用方法也基本类似。

{% include gallery id="gallery2" caption="鸿之微“一体机”桌面示意图" %}

&emsp;&emsp;鼠标点击屏幕左下角的`Menu`图标可以显示开始菜单，这与`Windows`的`开始`菜单类似，随系统预装的应用软件都可以找到相应的快捷方式，点击即可开始使用。任务栏右下角分别有`语言`、`账户`、`网络`等设置，点击可以进行设置，通常情况不建议您将系统语言设置为`中文`，因为这会给后续操作，如使用`cd`命令切换路径等操作时带来一些麻烦，如果您为了更快的熟悉操作系统，也可以将系统语言设置为`中文`。

### 文件管理&文本编辑
&emsp;&emsp;`Cinnamon`桌面中文件管理&文本编辑使用方式与`Windows`中资源管理器类似，左键双击桌面`pbs_scripts_example`文件夹图标，可以打开此文件夹如下图所示。

{% include gallery id="gallery3" caption="文件管理&文本编辑" %}

&emsp;&emsp;继续左键双击`pbs_scripts_example`文件夹中文本文件`rescu.pbs`图标，系统会默认使用文本编辑器`gedit`打开此文件。`gedit`文本编辑器是纯文本编辑器，类似于`Windows`的记事本，可以用来编辑各计算软件的输入文件，查看输出文件等。如果发现图标右下角有小锁图标，这是由于您当前使用的账户没有修改此文件的权限，您只能查看此文件不能修改。

注：关于`Linux`文件权限，您有必要在互联网上了解更多，这是您以后进行科学计算中非常重要的操作基础。
{: .notice--info}

### 命令行终端&常用命令
&emsp;&emsp;现代Linux发行版，我们可以通过图形界面完成大部分常规操作。但是`Linux`系统精髓和奥义集中于
**`Terminal`命令行终端**，通过**`Terminal`命令行终端**才可以使用Linux操作系统的全部功能，同时还可以编写**`shell`**脚本实现各种自定义组合操作，完全释放`Linux`系统的强大潜能。

&emsp;&emsp;在后面空白处点击右键，打开菜单点击`Open in Terminal`，便可以打开**`Terminal`命令行终端**。在`Terminal`命令行终端中，可以敲入命令，然后按键盘回车`Enter`执行。

{% include gallery id="gallery4" caption="**`Terminal`命令行终端**" %}

&emsp;&emsp;Linux系统集成上千条命令，每条命令拥有许多不同的选项，以完成不同的功能。通常情况难以完全记住这些命令和他们的选项。在鸿之微“一体机”中我们精心制作了一张包含了**80%**常用操作命令的**桌面壁纸**，帮助您快速查找，在大部分情况下不需要上网查阅完成需要的操作。

注：在下面我们列出来您需要掌握的最常用的命令。这些命令的帮助信息可以通过在命令后添加选项`--help`来查看，例如：输入`ls --help`就会显示`ls`命令的帮助信息。还有一种查看帮助的方法就是使用`man`命令，例如：输入`man ls`就会显示`ls`命令的帮助信息。
{: .notice--info}

#### 文件和目录操作
```shell
ls # 显示当前目录内容
ll # 以列表的方式显示文件信息，每个文件一行
cd /home # 进入'/home'目录
cd .. # 返回上一级目录
cd ~ # 进入用户的主目录
cd - # 返回上次所在的目录
pwd # 显示当前所在目录
mkdir dir1 # 在当前目录下创建'dir1'文件夹
rm file1 # 删除当前目录下'file1’文件
rm -rf dir1 # 删除当前目录下'dir1'文件夹及其内文件
cp file1 file2 # 将file1文件复制为file2文件
cp dir1/* . # 复制目录dir1下的所有文件到当前工作目录
cp -r dir1 dir2 # 复制目录dir1下文件到目录dir2
mv dir1 dir2 # 将目录'dir1'重命名为'dir2’
mv file1 file2 # 将目录'file1'重命名为'file2'
```

#### 文本查看与编辑
```shell
vi file1 # 使用vi编辑器编辑'file1'
cat file1 # 查看文本文件'file1’内容
cat -n file1 # 查看'file1'的内容并标示行数
head -5 file1 # 查看'file1'的前五行
tail -3 file1 # 查看'file1'的最后五行
less file1 # 查看一个长文本文件'file1’的内容
```

#### 文件执行与查找
```shell
chmod +x file1 # 给文件'file1'加上可执行权限
source file1 # 执行可执行文件'file1’
chown user1 file1 # 将文件'file1'的所有人属性改为'user1’
chown -R user1 dir1 # 将目录'dir1'及其目录下所有文件夹的所有人属性改为'user1’
chown user1:group1 file1 #改变文件'file1'的所有人和群组属性为为'user1'和'group1’

find / -name file1 # 从 /目录开始查找文件名为'file1'的文件和目录
whereis code1 # 显示'code1'二进制文件、源码或man的位置
which code1 # 显示'code1'二进制文件或可执行文件的完整路径 # 文件压缩和解压
tar -zcvf file1.tar.gz file1 # 把'file1'打包并压缩成'file1.tar.gz’文件
tar -zxvf file1.tar.gz # 解压并释放压缩包文件'file1.tar.gz'至当前目录
unzip file1.zip # 解压zip格式的压缩包'file1.zip'到当前目录
```

#### 网络配置与服务
```shell
ssh user1@node1 # 使用'user1'用户身份远程访问node1主机
ifconfig # 查看网络配置信息
ping www.baidu.com # 用来ping命令查看网络是否接通
service network restart # 重启网络服务
# 查看网卡配置文件信息，网卡名为'eth0’
cat /etc/sysconfig/network-scripts/ifcfg-eth0
cat /etc/hosts # 查看主机名和IP的映射关系配置文件
```
#### 系统信息和管理
```shell
top # 实时查看系统负载，输入q退出
free -h # 查看内存和使用情况
pstree # 以树状图显示程序
date # 显示系统日期
lscpu # 查看cpu的信息
uname -a # 查看内核信息

su - # 切换到root权限
shutdown -h now # 关机
reboot # 重启
logout # 注销

useradd user1 # 创建用户'user1’
userdel user1 # 删除用户'user1’
passwd # 修改用户密码
```

#### 磁盘空间、文件夹大小查看
```shell
df -h # 查看已经挂载的分区磁盘列表和使用空间
du -sh * # 查看当前目录下每个子文件夹占用磁盘空间
du -sh dir1 # 查看目录'dir1'占用磁盘空间
cat /etc/fstab # 查看分区挂载配置信息
lsblk # 查看磁盘块设备信息
```

#### 软件包安装和升级
```shell
vi ~/.bashrc # 编辑用户环境变量文件
source ~/.bashrc # 执行.bashrc配置文件
yum install package1 # 下载并安装一个软件包'package1’
rpm -qa | grep package1 # 查看是否装有'package1'软件包
```

#### Torque pbs任务管理命令
```shell
# 鸿之微“一体机”软件PBS脚本文件路径
cd /public/pbs_script_example

qsub jobfile1 # 提交任务文件'jobfile1'
qdel id1 # 删除Job ID为'id1'的计算任务
qstat # 查看当前已提交任务计算状态
qnodes # 查看各计算节点硬件状态
```

### 远程登录
&emsp;&emsp;鸿之微“一体机”连接到互联网后，您还可以远程登录鸿之微“一体机”。远程登录时，需要提前知道鸿之微“一体机”的网络`ip`地址（`ip`地址可以通过在终端中输入`ifconfig`命令来获得）。

如果您登陆的设备（例如您使用`Windows`办公电脑进行远程登陆）跟鸿之微“一体机”处于同一**局域网**内，您可以使用鸿之微“一体机”的局域网`ip`登陆。否则，您需要为鸿之微“一体机”配置**公网ip**。
{: .notice--warning}

#### 从Linux设备远程登陆到鸿之微“一体机”
&emsp;&emsp;如果您在另外一台linux机器上登录鸿之微“一体机”，直接在终端中使用`ssh`命令加上用户名和网络地址即可。

```sh
ssh hzwtech@192.168.1.100 #其中192.168.1.100为鸿之微“一体机”网络地址，需要根据实际情况调整
```

#### 从Windows设备远程登陆到鸿之微“一体机”
&emsp;&emsp;如果您在`Windows`机器上登录鸿之微“一体机”（如您的办公电脑，这也是最常见的情况），您需要使用支持`ssh`的软件，推荐您使用`XShell`或`MobaXterm`。如果需要跟鸿之微“一体机”进行文件传输，需要使用支持scp的软件，最常用的免费软件就是`WinSCP`。您也可以根据自己的使用习惯选择。这些软件使用中均需要提前知道鸿之微“一体机”的网络`ip`地址
- [XShell](https://www.netsarang.com/zh/xshell/)（官网可注册家庭/教育免费版）
- [MobaXterm](https://moba.en.softonic.com/) （官方免费）
- [WinSCP](https://winscp.net/eng/docs/lang:chs) （官方免费）

{% include gallery id="gallery5" caption="使用`WinSCP`和`XShell`软件，从Windows设备远程登陆到鸿之微“一体机”" %}

### Vim文本编辑器
&emsp;&emsp;在**终端模式**下，通常没有图形界面，如`gedit`等需要图形界面的文本编辑器会无法工作。如果在**终端模式**下需要进行文本编辑，需要使用特殊的文本编辑工具，其中**`Vim`**文本编辑器是最常用的编辑工具之一。

&emsp;&emsp;此处仅是浅显地描述最常用的功能，更多信息请上网查阅**`Vim`**文本编辑器的使用方法。Vim分成**命令模式**，**插入模式**和**替换模式**。

&emsp;&emsp;**插入模式**和**替换模式**跟其它的文本编辑器类似，**命令模式**是Vim特有的。Vim启动以后就进入**命令模式**，可以使用**h(左)** **j(下)** **k(上)** **l(右)**键进行光标移动。在命令模式中输入**`i`**进入插入模式，**插入模式**和**替换模式**可以通过插入键来切换。在**插入模式**和**替换模式**中，我们可以对文件内容进行相应的修改，修改完成以后可以按`Esc`键可以退回到**命令模式**。`Vim`中集成了许多操作命令，下面介绍其中最常用的一些命令。
```sh
:q — 退出Vim
:q! — 退出Vim，忽略对文本的编辑
:w — 把文本文件保存
:wq — 把文本文件保存，并退出Vim
gg — 到文件头部
G — 到文件尾部
11G — 跳转到11行，数字可以变化，例如123G就是跳转到123行
x — 删除当前光标所在的字符
dd — 删除当前光标所在的行
yy — 复制当前光标所在的行
p — 粘贴被删除或者复制的文本
u — 回复上一个操作
CTRL-r — 重做上次撤销的操作
```

&emsp;&emsp;[<i class="far fa-file-alt"></i> 继续学习鸿之微“一体机”教程-计算模拟环境使用简介](/hzwsoftware/environment-introduce/){: .btn .btn--success }
