## 树莓派根据特定信息启动

#### 根据屏幕信息设置分辨率等：

使用`tvservice -n`获取当前屏幕信息，如`device_name=DWE-HDMI`

这个屏幕是800*480分辨率的，自动识别有些问题，需要固定设置，编辑`/boot/config.txt`，在里面加上一些内容：

```
[EDID=DWE-HDMI]
hdmi_group=2
hdmi_mode=14
framebuffer_width=848
framebuffer_height=480
```

针对这个屏幕设置HDMI的输出模式，并指定分辨率。需要注意的是，最新的树莓派会在最下面有个`[all]`的配置，我们的配置需要放在这个配置前面，避免被覆盖掉。

官方文档： https://www.raspberrypi.org/documentation/configuration/config-txt/conditional.md