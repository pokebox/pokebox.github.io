## ubuntu 安装中文支持，让tty终端支持中文显示

**要在Ubuntu中添加中文支持并使TTY终端支持中文显示，可以按照以下步骤进行操作：**

1. 安装中文字体： 打开终端（Ctrl+Alt+T），然后运行以下命令以安装中文字体：

```
sudo apt-get install fonts-wqy-zenhei
```

1. 配置locale： 继续在终端中运行以下命令以编辑locale配置文件：

```
sudo nano /etc/default/locale
```

在打开的文件中，确保添加或修改以下行：

```
   LANG="zh_CN.UTF-8"
   LC_ALL="zh_CN.UTF-8"
```

按Ctrl+X保存文件并退出nano编辑器。

1. 重新生成locale： 运行以下命令以重新生成locale：

```
   sudo locale-gen zh_CN.UTF-8
   sudo update-locale
```

1. 配置TTY终端： 运行以下命令以编辑TTY终端配置文件：

```
sudo nano /etc/default/console-setup
```

在打开的文件中，找到并修改以下行：

```
   FONTFACE="Fixed"
   FONTSIZE="16x32"
```

按Ctrl+X保存文件并退出nano编辑器。

1. 重新启动系统： 为使更改生效，请重新启动您的系统：

```
sudo reboot
```

重新启动后，您的Ubuntu系统应该已经安装了中文支持，并且TTY终端可以显示中文内容了。



**如果要在Ubuntu中添加英文支持并使TTY终端支持英文显示，可以按照以下步骤进行操作：**

1. 配置locale： 打开终端（Ctrl+Alt+T），然后运行以下命令以编辑locale配置文件：

```
sudo nano /etc/default/locale
```

在打开的文件中，确保添加或修改以下行：

```
LANG="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
```

按Ctrl+X保存文件并退出nano编辑器。

1. 重新生成locale： 运行以下命令以重新生成locale：

```
sudo locale-gen en_US.UTF-8
   sudo update-locale
```

1. 配置TTY终端： 运行以下命令以编辑TTY终端配置文件：

```
sudo nano /etc/default/console-setup
```

在打开的文件中，找到并修改以下行：

```
FONTFACE="Fixed"
FONTSIZE="16x32"
```

按Ctrl+X保存文件并退出nano编辑器。

1. 重新启动系统： 为使更改生效，请重新启动您的系统：

```
sudo reboot
```

重新启动后，您的Ubuntu系统应该已经安装了英文支持，并且TTY终端可以显示英文内容了。