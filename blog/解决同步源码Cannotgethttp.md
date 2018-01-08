http://www.cnblogs.com/aaronLinux/p/5862235.html
====


[Issue]repo/repo init-解决同步源码Cannot get http://gerrit.googlesource.com/git-repo/clone.bundle
1. 前两天想搭建freescale L3.0.35_4.1.0_BSP包，结果LTIB环境搭建好，也编译出rootfs/uboot/kernel的Image了，但是准备移植uboot的时候发现uboot-200908版本的board/freescale下面并没有imx6的板子支持，不但rpm源码包中没有，uboot逛网200908版本也没有imx6的支持，到时patch下面有，但是不知道打哪一个，而网上查看别人未提及有此问题，所以极有可能是自己犯错。最终还是放弃了L3.0.35这个版本的使用。

2. 今天开始转来搞L3.14.52_1.1.0_BSP, 在搭建Yocto环境的时候，按照《Freescale Yocto Project User's Guide》操作，刚操作到repo和repo init就进行不下去了。因为要去google down些源文件，估计是天朝的网络问题，搞了半天的FQ，也没搞定，急了一头汗，最后找到下面的方法，终于是过了，有必要mark一下。

3. repo: repo用于管理多个项目集合，因为一个产品可能包含多个git项目，不同git项目可以形成不同产品，这样可以通过repo来进行管理。可以理解repo就是一系列的git project的集合。

repo init只是用来更新repo的配置和脚本集。
repo sync用来下载当前repo配置的所有项目。
以下转自：http://www.cnblogs.com/dinphy/p/5669384.html

同步分支cm12.1初始化出现的问题：

fatal: Cannot get https://gerrit.googlesource.com/git-repo/clone.bundle
fatal: error [Errno 101] Network is unreachable

解决方法,先单独克隆repo

git clone https://gerrit-googlesource.lug.ustc.edu.cn/git-repo
然后将git-repo里面的repo文件复制到bin目录，然后chmod a+x ~/bin/repo.

再在同步源码的工作目录新建.repo文件夹，把git-repo重命名为repo复制到.repo目录下：

重新初始化：

repo init -u git://github.com/CyanogenMod/android.git -b cm-12.1
同步开始

repo sync -c -j8
