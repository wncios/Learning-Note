# Linux内核准备
## 获取内核源码
- Linux内核官方网站：http://www.kernel.org
- git：
``` shell
git clone https://github.com/torvalds/linux.git
```

- - - - -
## 安装内核源码
``` shell
$ cd /usr/src/kernel
$ tar xzvf linux-2.6.32.tar.gz
```
- - - - -
## 使用补丁
``` shell
diff -Nur newfile oldfile
patch -p1 < ../patch-x.y.z
```
``` shell
for i in `ls ../patch`; \ 
do patch -p1 < ../patch/"$i";done
```
- - - - -
## 内核源码树

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
## 编译内核
内核配置
-  make config
- make menuconfig
- make defconfig
- make xconfig

配置之后会产生.config文件
内核编译：
- make -jn
- make modules_install
- make install
- depmod

模块安装到/lib/modues



