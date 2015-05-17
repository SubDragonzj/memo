origin post:

http://blog.csdn.net/sun_168/article/details/8046164
====


想必大家都有这样的经历，在自己的机器上安装双系统。这里我简单根据我的经验来介绍下安装ubuntu和win系统的引导问题。首先声明一点，这里说的ubuntu 9.04及其以上版本，因为这些版本采用的是grub引导的。

如果先安装win系统，再安装ubuntu系统，那么一切OK，在操作系统的启动页面中会出现选项来选择是启动ubuntu系统还是win系统。因为ubuntu的grub引导程序兼容win的引导程序，所以这样可以。但是如果把这两个系统的安装顺序给颠倒过来就不行了，因为win系统的引导程序不兼容grub，这时启动选项中只有win系统，而看不到ubuntu系统，这时就需要修复一下ubuntu系统的grub程序了。

利用安装盘，进入ubuntu系统（这里不是安装ubuntu系统，只是试用<try> ubuntu），打开terminate，运行一下命令：
sudo -i                #获得root权限

fdisk -l               #查看系统分区情况，找到linux系统的分区编号，一般是sdaX

mount /dev/sdaX /mnt      #将linux分区挂载到mnt下

grub-install --root-directory=/mnt /dev/sda                #重新安装grub到linux分区

update-grub            #更新grub文件

然后重新启动系统即可。

重启机器后，我们可能还会遇到另一个问题，启动选项中还有以前的win操作系统，或者进入新安装的win操作系统，出现如下错误：

error：no such device XXXXXXXXXXX

其实出现这种错误的主要原因在于新系统所在盘的uuid改变了，所以出现这种错误。我们只需要进入ubuntu系统，在终端中用root权限执行命令update-grub就行了，就一切搞定了。

==================

P.S.
我之前安装win7+ubuntu，grub安装到/dev/sda，后来删掉win7重新安装win8：
$ sudo -i
~# grub-install /dev/sda
~# update-grub
~# reboot
