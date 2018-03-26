# 使用truss-strace-ltrace诊断软件的疑难杂症

> https://www.linuxidc.com/Linux/2014-09/106665.htm  

进程无法启动，软件运行速度突然变慢，程序的"Segment Fault"等等都是让每个Unix系统用户头痛的问题，本文通过三个实际案例演示如何使用truss、strace和ltrace这三个常用的调试工具来快速诊断软件的"疑难杂症"。  

truss和strace用来 跟踪一个进程的系统调用或信号产生的情况，而 ltrace用来 跟踪进程调用库函数的情况。truss是早期为System V R4开发的调试程序，包括Aix、FreeBSD在内的大部分Unix系统都自带了这个工具；而strace最初是为SunOS系统编写的，ltrace最早出现在GNU/Debian Linux中。这两个工具现在也已被移植到了大部分Unix系统中，大多数Linux发行版都自带了strace和ltrace，而FreeBSD也可通过Ports安装它们。  

你不仅可以从命令行调试一个新开始的程序，也可以把truss、strace或ltrace绑定到一个已有的PID上来调试一个正在运行的程序。三个调试工具的基本使用方法大体相同，下面仅介绍三者共有，而且是最常用的三个命令行参数：  

```
-f ：除了跟踪当前进程外，还跟踪其子进程。
-o file ：将输出信息写到文件file中，而不是显示到标准错误输出（stderr）。
-p pid ：绑定到一个由pid对应的正在运行的进程。此参数常用来调试后台进程。
```

三个调试工具的输出结果格式也很相似，以strace为例：  

```
brk(0)                                  = 0x8062aa8
brk(0x8063000)                          = 0x8063000
mmap2(NULL, 4096, PROT_READ, MAP_PRIVATE, 3, 0x92f) = 0x40016000
```

每一行都是一条系统调用，等号左边是系统调用的函数名及其参数，右边是该调用的返回值。 truss、strace和ltrace的工作原理大同小异，都是使用ptrace系统调用跟踪调试运行中的进程，详细原理不在本文讨论范围内，有兴趣可以参考它们的源代码。  
