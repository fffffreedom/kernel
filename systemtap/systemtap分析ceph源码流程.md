# systemtap分析ceph源码流程

> http://blog.csdn.net/a1454927420/article/details/77455759  

## 安装

请见 [systemtap介绍](https://github.com/fffffreedom/linux/blob/master/systemtap/systemtap%E4%BB%8B%E7%BB%8D.md)  

## 安装ceph调试信息包

下载[ceph调试包](http://debuginfo.centos.org/centos/7/storage/x86_64/)并安装，ceph调试包的版本号，必须和所部署的ceph的版本号一致。  

## 编写systemtap脚本

### 查看osd进程pid
```
ps aux | grep osd
```

### 编写脚本
```
# osd.stp
probe process("ceph-osd").function("OSD::*").call, process("ceph-osd").function("OSDService::*").call
{
  printf("%s -> %s\n", thread_indent(4), ppfunc());
}
```
process表示程序的名称，function表示函数的名字，如果OSD::*，表示输出所有OSD类的相关函数。
printf表示打印出，用法类似c语言的printf，thread_indent表示打印线程号，ppfunc()表示打印出函数名。  

### 运行脚本
```
stap -x {osd-pid} osd.stp
```

### 查看结果
