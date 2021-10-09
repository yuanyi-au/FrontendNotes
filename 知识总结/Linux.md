- [Linux](#linux)
  - [Linux 系统组成](#linux-系统组成)
    - [内核](#内核)
    - [Shell](#shell)
    - [文件系统](#文件系统)
  - [vi 编辑器](#vi-编辑器)

# Linux 

## Linux 系统组成

Linux系统一般有4个主要部分：内核、shell、文件系统和应用程序

内核、shell和文件系统一起形成了基本的操作系统结构，它们使得用户可以运行程序、管理文件并使用系统

### 内核

内核由如下几部分组成：内存管理、进程管理、设备驱动程序、文件系统和网络管理等

### Shell
shell是系统的用户界面，提供了用户与内核进行交互操作的一种接口

### 文件系统

    \/bin：是系统的一些指令。bin为binary的简写，主要放置一些系统的必备执行档例如:cat、cp、chmod df、dmesg、gzip、kill、ls、mkdir、more、mount、rm、su、tar等。

    \/sbin：一般是指超级用户指令。主要放置一些系统管理的必备程式例如:cfdisk、dhcpcd、dump、e2fsck、fdisk、halt、ifconfig、ifup、 ifdown、init、insmod、lilo、lsmod、mke2fs、modprobe、quotacheck、reboot、rmmod、 runlevel、shutdown等。
	
    \/usr/bin：是你在后期安装的一些软件的运行脚本。主要放置一些应用软体工具的必备执行档例如c++、g++、gcc、chdrv、diff、dig、du、eject、elm、free、gnome*、 gzip、htpasswd、kfm、ktop、last、less、locale、m4、make、man、mcopy、ncftp、 newaliases、nslookup passwd、quota、smb*、wget等。
	
    \/usr/sbin：放置一些用户安装的系统管理的必备程式例如:dhcpd、httpd、imap、in.*d、inetd、lpd、named、netconfig、nmbd、samba、sendmail、squid、swap、tcpd、tcpdump等。

    \/dev：任何设备均以文件形式存在于该文件夹内(通过mount命令挂载成用户直接可用的文件系统)
	
    \/media：挂载的可移动设备
	
    \/etc：配置文件所在目录
	
    \/proc：是一种内核和内核模块用来向进程(process) 发送信息的机制(所以叫做/proc)。这个伪文件系统让你可以和内核内部数据结构进行交互，获取 有关进程的有用信息，在运行中(on the fly) 改变设置(通过改变内核参数)。 与其他文件系统不同，/proc 存在于内存之中而不是硬盘上。
	
    \/tmp：临时文件，关机时自动销毁

    \/var：系统产生的不可自动销毁的缓存文件、日志记录。（系统和程序运行后产生的数据、不对外提供服务、只能用户手动清理）（包括mail、数据库文件、日志文件）
	
## vi 编辑器

vi可以分为三种状态，分别是命令模式（command mode）、插入模式（Insert mode）和底行模式（last line mode）

按一下字母「i」就可以进入「插入模式（Insert mode）」

在「插入模式（Insert mode）」按一下「ESC」键转到「命令行模式（command mode）」

在「命令行模式（command mode）」下，按一下「：」冒号键进入「Last line mode」

命令|作用
-|-
cd|变更目录 Change Directory
pwd|显示当前所在的目录 Print Working Directory
mkdir|新建目录 Make Directory
rmdir|删除一个空的目录 Remove Directory
cp|复制目录或档案 Copy
rm|删除目录或档案 Remove
mv|移动目录或档案，也可用作重命名 Move
rename|重命名档案或目录
ls|检视目录或档案 List Show
ll|等同于 [ ls -l ]
alias|指定命令选项，如:[ ll ] 等同于 [ ls -l ]
\rm|在指令前加反斜杠，可以忽略 [ alias ] 的指定选项
touch|修改档案时间，也可用作建立空档案
basename|获取路径的档案名
dirname|获取路径的目录名
id|查看用户账号属性
cat|由第一行开始显示档案内容 Concatenate
tac|由最后一行开始显示档案内容，是 [ cat ] 倒着写
file|观察文件类型
which|搜索指令的完整文件名，通过 [ PATH ] 变量搜索 
type|搜索指令的完整文件名，通过 [ PATH ] 变量搜索
whereis|根据关键词搜索文件，数据库搜索
locate|根据 /var/lib/mlocate 内的数据库记载，找出用户输入的关键词文件名
find|搜索文件，硬盘搜索
$PATH|执行文件路径的变量(类似于WINDOWS中的环境变量)
~/.bashrc|环境参数配置文件