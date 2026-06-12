## 更改PVE网卡的命名方式

默认情况下系统的网卡命名是以设备位置命名的，例如nep3s0表示这个网卡位于总线上的第三个端口的第一个设备。

参考：[网络配置 - Proxmox VE](https://pve.proxmox.com/wiki/Network_Configuration)

由于某些原因，在一些主板上变更PCIe设备时会导致板载网卡的顺序变化，此时就可能导致设备无法访问网络或是出现一些不可控的情况。因此必须把网卡的设备名固定下来，保证在任何情况下这个设备都不会变。

根据文档[可预测网络接口名称 (www.freedesktop.org)](https://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames/)的说明，默认情况网卡的命名方式是按/usr/lib/systemd/network/99-default.link来配置的，因此我们也可以在/usr/lib/systemd/network/里创建一个新的配置文件，只要文件名前面的数值比99小就可以优先按我们的配置文件来设置，比如70-myconfig.link。那么这个配置文件怎么写呢？

例如现在我想把网卡按MAC地址命名，因为网卡的MAC地址是不会因为设备的插拔或是移动了位置而改变的，甚至挪到了其他机器上它也依然还是它。

现在在/usr/lib/systemd/network/里创建一个70-mac.link的文件，然后里面填入下面的内容

```
[Match]
MACAddress=00:11:22:07:c1:fb 00:1c:06:0a:f1:fc

[Link]
NamePolicy=mac
AlternativeNamesPolicy=mac
MACAddressPolicy=persistent
```

配置说明参考[systemd.link (www.freedesktop.org)](https://www.freedesktop.org/software/systemd/man/systemd.link.html)

修改好后systemctl reload systemd-udevd.service重新刷新udev设备配置即可。更新后还得在/etc/network/interfaces中修改或增加对应的接口配置，修改好后ifreload -a重载即可。