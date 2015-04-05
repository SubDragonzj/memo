http://blog.csdn.net/alien75/article/details/38921397
====



从网上了解到的种种下载android源码失败的处理，都提到repo同步是可以“续传”的，我也一直认为这个所谓的“续传”是“断点续传”的意思。直到我在下载android-x86这个开源项目时，才发现这个“续传”不是“断点续传”。

现象是这样的：由于服务器的不稳定，在下载到frameworks/base这个有几GB的project时老是失败，提示信息先后是“The remote end hung up unexpectedly”、“early EOF”、“index-packed failed”，查了一下是因为压缩文件太大，在网络不稳定的情况下容易出现超时现象导致repo同步过程结束并重新开始。但是续传多次失败后(编写了一个重复repo同步过程的脚本)，下载目录.repo占用空间却越来越大，按官方的说法下载完毕应该才9GB多，实际却达到了差不多20GB，这是为哪般呢？

原因要先分析git的工作原理，它在下载的时候是将project压缩成一个包下载，然后到本地解压缩的，这个压缩包的本地存放地址是project目录下的objects/pack。由于repo工具实际上使用的是git，frameworks/base对应的就是.repo/projects/frameworks/base.git/objects/pack，压缩包名称是tmp_pack_XXXXXX，后面六位是由repo调用的git根据当前时间随机生成的，当网络超时就会中断repo同步过程，重新开始repo同步过程到断点处会生成一个压缩文件名进行下载，这就导致原有的压缩文件成为垃圾文件。在实际情况下，这个目录下的垃圾文件达到了15GB，也就是没有下载完毕却空间占用有这么大的原因。

在网上找到一个办法说是在.gitconfig的core段加compression=-1，实测没有效果，原因可能是因为这个是针对git的clone命令的，而repo工具使用的是git的fetch命令。

在一个国外的博客(引用一)找到了另一种办法：其原理大致就是修改repo工具中的sync.py，在网络同步某一project失败的时候不是异常退出repo同步过程而是继续同步此project直到成功，但是这样只能解决某一project失败导致repo同步过程中断的问题，仍然无法避免垃圾文件的不断产生，而且它存在的一个问题就是当某一project一直无法完成时将导致它的垃圾文件越来越多。在新的repo工具中sync增加了一个“--force-broken”选项，可以在某个project同步失败的时候跳到下一个project执行而不是中断repo同步过程。关于repo的分析可以参考老罗的博客(引用二)。

在stackoverflow上(引用三、四)讲到了使用“--depth”方式减少压缩文件大小或使用bundle方式，但这两种方式都有局限性：前者仍然依赖于网络状况，后者需要官方或第三方提供bundle文件。

总结：
1、repo同步的续传机制仅到project这一级，当下载失败的时候是新建一个临时压缩文件下载，而不是在旧的压缩文件基础上断点续传。
2、repo同步的“--force-broken”参数可用于在一个project下载失败的时候继续下一个project而不是中断repo同步过程。
3、git断点续传仍无明确解决办法。临时文件名的生成及创建是在wrapper.c的git_mkstemps_mode函数中进行的。

引用：
1、repo sync problems - Android Eclair
http://android.amberfog.com/?p=230

2、Android源代码仓库及其管理工具Repo分析
http://blog.csdn.net/luoshengyang/article/details/18195205

3、fatal: early EOF fatal: index-pack failed
http://stackoverflow.com/questions/21277806/fatal-early-eof-fatal-index-pack-failed

4、How to complete a git clone for a big project on an unstable connection
http://stackoverflow.com/questions/3954852/how-to-complete-a-git-clone-for-a-big-project-on-an-unstable-connection


==================

我的解决办法是
$ cd .repo
$ find . -name "tmp_pack*" | xargs rm -f
