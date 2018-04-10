# Linux内核kdump服务

## 简介

kdump是一种先进的基于kexec的内核崩溃转储机制。在系统崩溃、死锁或者死机时，kdump使用 kexec启动第二个内核，该内核叫做捕获内核，
它会将此时内存中的所有运行状态和数据信息收集到一个vmcore文件中，以便于内核工程师分析崩溃原因，一旦内存信息收集完成，系统将自动重启。

### 优点

1.	vmcore文件保存了内核的调用栈、log、内存信息等，对后续问题定位非常有帮助；  
2.	信息收集完成后，系统将自动重启，有助于业务的快速恢复；（了解到，目前我司服务器崩溃后，并不会自动重启）  
3.	启动kdump服务，并不会对服务器的性能造成影响；  

### 启动kdump服务

正常情况下，kdump所需要的软件包都会随系统一起安装。查看及配置过程如下（都需要root权限）：  
- 1.	查看软件是否安装
```
rpm -q kexec-tools
kexec-tools-2.0.0-273.el6.x86_64
```
若无输出，则需要安装软件包：  
```
yum install kexec-tools
```
- 2.	查看是否有给捕获内核预留空间
```
cat /proc/cmdline
crashkernel=129M@48M
```
若输出的命令行中没有crashkernel字段，则需要修改系统启动参数，如下：  
```
vim /boot/grub/grub.conf
kernel /vmlinuz-2.6.32-696.1.1.el6.x86_64 ro …… quiet crashkernel=auto
```
- 3.	启动kdump服务
```
Centos 6.x
chkconfig kdump on	      # 开机功能启动
service kdump start/restart # 启动/重启kdump服务
service kdump status	  # 查看kdump服务是否正常启动（Kdump is operational）
Centos 7.x
systemctl enable kdump.service
systemctl start kdump.service
```
- 4.	reboot生效
- 5.	测试配置是否生效
```
echo 1 > /proc/sys/kernel/sysrq
echo c > /proc/sysrq-trigger
```
系统将会panic，一段时间后，系统会重启。重启之后，会在/var/crash/127.0.0.1-[time]/目录下生成vmcore文件。  

## 搭建使用crash工具分析vmcore的环境

使用crash工具分析vmcore时，需要使用带debug信息的vmlinux，而当前服务器里运行的vmlinux是不带debug信息的。我们可以在编译内核源码时，
打开调试配置选项，来生成带debug信息的vmlinux。但目前，我们没有内核源码，也就无法编译了。

uname -a，获取当前服务器的内核版本：  
```
Linux HOSTNAME 2.6.32-642.el6.x86_64 #1 SMP Tue May 10 17:27:01 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```
则需要下载两个rpm包，分别是：  
```
kernel-debuginfo-2.6.32-642.el6.x86_64.rpm
kernel-debuginfo-common-x86_64-2.6.32-642.el6.x86_64.rpm
```
【注】上述两个rpm包的版本一定要生成vmcore的服务器的内核匹配，不然使用crash分析时会因为版本不一样而无法进行！

使用wget命令下载：  
```
wget -c http://debuginfo.centos.org/6/x86_64/kernel-debuginfo-2.6.32-642.el6.x86_64.rpm
wget -c http://debuginfo.centos.org/6/x86_64/kernel-debuginfo-common-x86_64-2.6.32-642.el6.x86_64.rpm
```

然后安装：  
```
rpm -ivh kernel-debuginfo-common-x86_64-2.6.32-642.el6.x86_64.rpm
rpm -ivh kernel-debuginfo-2.6.32-642.el6.x86_64.rpm
```
安装成功后，即可在/usr/lib/debug/lib/modules/2.6.32-642.el6.x86_64/目录下找到带有vmlinux：  
```
$ file /usr/lib/debug/lib/modules/2.6.32-642.el6.x86_64/vmlinux
/usr/lib/debug/lib/modules/2.6.32-642.el6.x86_64/vmlinux: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```
最后就可以使用crash命令调试vmcore了，命令行如下：  
```
crash  /usr/lib/debug/lib/modules/`uname -r`/vmlinux \
/var/crash/127.0.0.1-2017-05-09-11\:50\:55/vmcore
```
其中，127.0.0.1-2017-05-09-11\:50\:55 是ip和vmcore生成时间的组合。
