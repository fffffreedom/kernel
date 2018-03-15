# systemtap介绍

## install

### yum安装所需的工具包

```
yum install systemtap systemtap-runtime gcc elfutils kernel-devel
```

### 自动安装内核信息包

> https://spacewander.gitbooks.io/systemtapbeginnersguide_zh/content/2_1_InstallationAndSetup.html  

在较新的系统上，仅需以root权限运行下面的命令：

```
stap-prep
```

如果不起作用，就按下面手动安装吧。  

### 手动安装内核信息包

```
kernel-debuginfo
kernel-debuginfo-common

https://rpmfind.net/linux/centos/7.4.1708/updates/x86_64/Packages/kernel-devel-3.10.0-693.5.2.el7.x86_64.rpm
```

根据`uname -r`输出的版本信息：  
```
# http://debuginfo.centos.org/7/x86_64/
kernel-debuginfo-3.10.0-693.5.2.el7.x86_64.rpm
kernel-debuginfo-common-x86_64-3.10.0-693.5.2.el7.x86_64.rpm

# https://rpmfind.net/linux/RPM/centos/updates/7.4.1708/x86_64/Packages/kernel-devel-3.10.0-693.5.2.el7.x86_64.html
kernel-devel-3.10.0-693.5.2.el7.x86_64.rpm
```

下载相应版本的rpm包，然后 `yum localinstall` 这些rpm包。  

## 验证

以root权限运行：  
```
stap -v -e 'probe vfs.read {printf("read performed\n"); exit()}'
```

输出：  
```
Pass 1: parsed user script and 466 library scripts using 226616virt/39680res/3312shr/36504data kb, in 360usr/20sys/385real ms.
Pass 2: analyzed script: 1 probe, 1 function, 7 embeds, 0 globals using 370760virt/180120res/4636shr/180648data kb, in 1720usr/270sys/1993real ms.
Pass 3: using cached /root/.systemtap/cache/16/stap_1698184177b6ebf69726fa2c03f932d8_2685.c
Pass 4: using cached /root/.systemtap/cache/16/stap_1698184177b6ebf69726fa2c03f932d8_2685.ko
Pass 5: starting run.
read performed
Pass 5: run completed in 0usr/20sys/336real ms.
```

## reference

SystemTap介绍  
http://blog.csdn.net/hnwyllmm/article/details/50912362  

听阿里云CDN安防技术专家金九讲SystemTap使用技巧  
https://yq.aliyun.com/articles/174916?utm_content=m_28902  

Linux 下的一个全新的性能测量和调式诊断工具 Systemtap, 第 3 部分  
https://www.ibm.com/developerworks/cn/linux/l-cn-systemtap3/  

