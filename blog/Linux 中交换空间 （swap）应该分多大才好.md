origin post:
http://www.linuxidc.com/Linux/2013-05/84252.htm
====


前一段时间，我们机房中一台Linux服务器运行缓慢，系统服务出现间歇性停止响应，让我过去处理一下这一问题，登录到服务器之后，发现此服务器的物理内存是16G，而最初装机的时候，系统管理人员却只分配了4G的虚拟内存。查看内存的使用状况，物理内存并没有完全耗尽，但虚拟内存已经耗尽，整个系统CPU负载和磁盘IO都非常高。

知道了问题所在是由于交换分区不足导致，那么解决方法就是：将虚拟内存通过虚拟文件的方式增加到16G，系统运行状况明显好转。其实虚拟内存并不是等到物理内存用尽了才使用的，是否尽量的使用或不使用swap，在内核空间有一个参数控制。

[root@web ~]# cat /proc/sys/vm/swappiness

60

swappiness=0 的时候表示最大限度使用物理内存，然后才是swap空间；swappiness＝100 的时候表示积极的使用swap分区，并且把内存上的数据及时的搬运到swap空间里面。对于现在动辄几十GB、上百GB物理内存的服务器来说，究竟为其Linux系统设置多大的交换分区合适呢？为此，我引用红帽官方里的一段文字进行简单说明一下，嘿嘿。

目前红帽官方推荐交换分区的大小应当与系统物理内存的大小保持线性比例关系，不过在小于2GB物理内存的系统中，交换分区大小应该设置为内存大小的两倍，如果内存大小多于2GB，交换分区大小应该是物理内存大小加上2GB。其原因在于，系统中的物理内存越大， 对于内存的负荷可能也越大。但是，如果物理内存大小扩展到数百GB，这样做就没什么意义了，大家说对吧！

实际上，系统中交换分区的大小并不取决于物理内存的量，而是取决于系统中内存的负荷。Red Hat Enterprise Linux 可以在这样的情况下工作：完全没有交换分区，而且系统中匿名内存页和共享内存页小于3/4的物理内存量。在这种情况下，系统会将匿名内存页和共享内存页锁定在物理内存中，而使用剩余的物理内存来缓冲文件系统数据(pagecache)，当内存耗尽时，系统内核只会回收利用这些pagecache内存。

考虑到以下情况：

1）安装系统时难以确定内存的负荷，如何设置交换分区大小

2）系统中物理内存越大，所需交换分区就会越少

因此，在Red Hat Enterprise Linux 中，以下是设置合适的交换分区大小的规则：
----------------------------------
物理内存        交换分区(SWAP)
<= 4G           至少4G
4~16G	        至少8G
16G~64G	        至少16G
64G~256G	至少32G
----------------------------------
注：

1.但我们平时安装系统时，默认都分内存的2倍，因为现在有硬盘空间都很大，也不在乎那几十G的空间，嘿嘿！（其实也是为了省事）

2.其它操作系统也是类似。

