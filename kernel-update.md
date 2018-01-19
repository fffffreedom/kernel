# kernel-update.md
升级内核有多种方法，具体使用哪种，需要根据情况来定。  
- 升级到【随机】版本  
这里的随机并不太妥，主要是看repo里的版本，这里的版本会是比较新的版本。  
具体信息下网站：  
http://elrepo.org/tiki/tiki-index.php  
可升级到的内核版本：  
http://elrepo.org/tiki/Download  
CentOS6.8升级Kernel内核到4.8.x  
https://www.xshell.net/linux/1591.html  
```
a. 导入Public
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

b. 安装 ELRepo
根据当前的操作系统版本选择：  
To install ELRepo for RHEL-7, SL-7 or CentOS-7:
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
To install ELRepo for RHEL-6, SL-6 or CentOS-6:
rpm -Uvh http://www.elrepo.org/elrepo-release-6-8.el6.elrepo.noarch.rpm

c. 升级 Kernel
选择内核的版本  
kernel-lt（LongTerm：长期维护版本）
yum --enablerepo=elrepo-kernel install kernel-lt -y 
kernel-ml（MainLine: 主线维护版本）
yum --enablerepo=elrepo-kernel install kernel-ml -y
一般选择长期版本!

d. 更改Grub
vim /etc/grub.conf
根据安装好以后的内核位置，修改 default 的值。  
一般是修改为0，因为 default 从 0 开始，一般新安装的内核在第一个位置，所以设置default=0。  
保存后重启，使用uname -r查看内核版本即可。
```
- 升级到【指定】版本
  - 下载已有rpm包
    google kernel rpm  
    http://rpmfind.net/linux/rpm2html/search.php?query=kernel  
    如果还是找不到，则只能用下面这种方法了！
  - 下载源码
    www.kernel.org  
    可基于当前的内核配置，来编译新版本的内核源码，编译过程就先不细看了。参见：  
    https://wiki.centos.org/zh/HowTos/Custom_Kernel
