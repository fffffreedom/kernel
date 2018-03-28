# Redis数据导入工具优化过程总结

> http://www.cnblogs.com/me115/p/4605273.html

## 用到的工具

time, gprof, pstrace(?strace)

## 过程

使用time工具查看每次执行的耗时，分别包含用户时间和系统时间；   
使用pstrace打印实时运行，查询进程主要的系统调用，发现耗时点；   
使用gprof统计程序的耗时汇总，集中精力优化最耗时的地方；  

## 使用简介

1. 对g++的所有编辑和连接选项都必须要加上-pg（第一天由于没有在连接处加上-pg选项，导致无法出统计报告）；  
2. 执行完程序后，本目录会产生gmon.out文件；  
3. gprof redistool gmou.out > report,生成可读文件report，打开report集中优化最耗时的函数； 

## 优化点

还是使用工具去获取热点，再进行逐一的优化！  

1. 文件内存映射  
2. 函数改写 && 内联  
3. 去除调试符和优化监测符号  

## gprof

使用 GNU profiler 来提高代码运行速度  
https://www.ibm.com/developerworks/cn/linux/l-gnuprof.html  
