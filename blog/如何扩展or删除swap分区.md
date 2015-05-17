origin post:
http://www.linuxidc.com/Linux/2014-03/98311.htm#fromapp
====


背景：

由于安装Oracle 的时候，swap太小只划分了4G，后期发现交换分区太小，不满足使用，于是进行了swap分区的扩容

过程：

swap分区的扩展很简单，但是需要root用户权限

# dd if=/dev/zero of=/swap bs=1024M count=8(从/分区分出8x1024M大小的空间，挂在/swap上)

# mkswap /swap (格式化成swap格式)

# swapon /swap (激活/swap，加入到swap分区中)

# vim /etc/fstab (开机自启动新添加的swap分区)

—>添加

/swap swap swap defaults 0 0    #如果安装时有分区给/swap，会在分区表有定义，要注释掉

如果不想使用需要删除，只需要执行#swapoff /swap

