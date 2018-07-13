# 1、Linux内核准备
## 1.1 获取内核源码
- Linux内核官方网站：http://www.kernel.org
- git：
``` shell
git clone https://github.com/torvalds/linux.git
```
- - - - -
## 1.2 安装内核源码
``` shell
$ cd /usr/src/kernel
$ tar xzvf linux-2.6.32.tar.gz
```
- - - - -
## 1.3 使用补丁
``` shell
diff -Nur newfile oldfile
patch -p1 < ../patch-x.y.z
```
``` shell
for i in `ls ../patch`; \ 
do patch -p1 < ../patch/"$i";done
```
- - - - -
## 1.4 内核源码树

| 目录 | 描述 |
| :---: | :---: |
| arch | 特定体系结构的源码 |
| block | 块设备I/O层 |
| crypto | 加密API |
| Documention | 内核源码文档 |
| drivers | 设备驱动程序 |
| firmware | 使用某些驱动程序而需要的设备固件 |
| fs | VFS和各种文件系统 |
| include | 内核头文件 |
| init | 内核引导和初始化 |
| ipc | 进程间通信 |
| kernel | 像调度程序这样的核心子系统 |
| lib | 管理内核函数 |
| mm | 内核管理子系统和VM |
| net | 网络子系统 |
| samples | 实例，实例代码 |
| scripts | 编译内核使用的脚本 |
| security | Linux安全模块 |
| sound | 语音子系统 |
| usr | 早期用户空间代码（initramfs）|
| tools | 在Linux开发中有用的工具 |
| virt | 虚拟化基础结构 |
- - - - -
## 1.5 编译内核
内核配置
- `make menuconfig`
- `make config`
- `make xconfig`
- `make defconfig`

- `make clean`
- `make distclean`

配置之后会产生.config文件  
内核编译：
- `make`
- `make modules`
- `make modules_install`
- `make install`
- `/sbin/depmod 2.6.31.8`
- `make -jn`

模块安装到 `/lib/modues`
- - - - -
## 1.6 内核开发的特点
- 内核编程时既不能访问C库也不能访问标准的C头文件；
- 内核编程时必须使用GNU C；
- 内核编程时缺乏想用户空间那样的内存保护机制；
- 内核编程时难以执行浮点运算；
- 内核给每个进程只有一个很小的定长堆栈；
- 由于内核支持异步中断、抢占和SMP，因此必须时刻注意同步和并发；
- 要考虑可移植性的重要性。

- 查看内核打印消息：`dmesg`或`cat /var/log/message`
- 清空当前内核消息：`dmesg -c`
- 插入内核模块：`insmod HelloWorld.ko`
- 查看内核模块：`lsmod`
- 删除内核模块：`rmmod HelloWorld.ko`
- - - - -

# 2、进程管理和进程调度
# 2.1 进程
- 进程就是处于运行期的程序以及相关资源的总称，是操作系统进行资源分配和调度的基本单位，进程通常包含一些资源，例如打开的文件描述符，挂起的信号，内核内部数据，处理器状态，一个或者多个具有内存映射的内存地址空间及一个或多个执行线程，还有用来存放全局变量的数据段等。
- 执行线程是在进程中的活动对象，是CPU调度的最小单位，每个线程拥有独立的程序计数器，进程栈和一组进程寄存器。
- Linux系统下查看进程：
``` shell
# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1  43272  3672 ?        Ss   Jun07   0:26 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root         2  0.0  0.0      0     0 ?        S    Jun07   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    Jun07   0:21 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S<   Jun07   0:00 [kworker/0:0H]
root         7  0.0  0.0      0     0 ?        S    Jun07   0:00 [migration/0]
root         8  0.0  0.0      0     0 ?        S    Jun07   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        R    Jun07   2:29 [rcu_sched]
root        10  0.0  0.0      0     0 ?        S    Jun07   0:10 [watchdog/0]
root        12  0.0  0.0      0     0 ?        S    Jun07   0:00 [kdevtmpfs]
root        13  0.0  0.0      0     0 ?        S<   Jun07   0:00 [netns]
root        14  0.0  0.0      0     0 ?        S    Jun07   0:00 [khungtaskd]
root        15  0.0  0.0      0     0 ?        S<   Jun07   0:00 [writeback]
root        16  0.0  0.0      0     0 ?        S<   Jun07   0:00 [kintegrityd]
root        17  0.0  0.0      0     0 ?        S<   Jun07   0:00 [bioset]
root        18  0.0  0.0      0     0 ?        S<   Jun07   0:00 [kblockd]
root        19  0.0  0.0      0     0 ?        S<   Jun07   0:00 [md]
root        25  0.0  0.0      0     0 ?        S    Jun07   0:00 [kswapd0]
root        26  0.0  0.0      0     0 ?        SN   Jun07   0:00 [ksmd]
...
```
`ps`命令参数含义：
- `a`：打印所有进程
- `u`：以用户为主打印进程
- `x`：显示所有进程，不区分终端


