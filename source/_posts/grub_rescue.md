**针对双系统Linux开机出现grub rescue的解决方案**

创建时间：2017年9月24日(星期天) 凌晨1:17 | 分类：项目笔记 | 天气：多云 

昨天把电脑拿去送修换了主板，期间取了机械盘回来，只留了固态去测试。回来后发现Linux系统无法进入，重刷EFI分区后导致UUID错误系统无法识别，开机出现grub rescue的问题。

根据百度到的解决方法总结如下：

首先在grub rescue下输入ls查看磁盘分区，然后依次ls (hd0,gpt0)/boot/grub去试，直到找到正确的启动引导分区。然后使用set命令去设置根目录和配置启动目录

> set root=(hd0,gpt2)/boot/grub

> set prefix=(hd0,gpt2)/boot/grub

然后加载模块

> insmod normal

> normal

这样就可以进入启动界面了。但是这不能解决根本问题，因为重启后还是会这样，所以进系统后需要更新grub。

使用root运行

> update-grub

> grub-install /dev/sda

然后重启即可~注意这里只能是/dev/sda或者/dev/sdb之类的，/dev/sda1这些都不对，不要加分区。

如果在启动系统时出错进入了安全模式，那是因为fstab无法正确挂载分区导致的，用命令blkid查看所有磁盘的ID和fstab里的是否一致，如果有不一致的就更新uuid之类的盘符，这样重启即可正常进入系统。