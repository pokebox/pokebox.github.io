# 给手机安装ssh服务器并绑定域名随时远程

由于有随时访问手机的需求，同时也怕自己手机丢，给手机安装ssh服务器以便自己随时随地远程到手机进行操作。

#### 准备

[magisk](https://github.com/topjohnwu/Magisk)

[ssh模块](https://github.com/Magisk-Modules-Repo/ssh)

[MacroDroid](https://www.macrodroid.com/)自动化工具

域名服务器，如[DNSpod](https://www.dnspod.cn/)

------

先给手机root，然后刷magisk，然后下载ssh模块并用magisk刷入。

##### 配置SSH

------

根据说明，安装好模块后，需要在`/data/ssh/root/.ssh/authorized_keys`和`/data/ssh/shell/.ssh/authorized_keys`写入公钥，可以使用ssh-keygen生成公钥然后`cat id_rsa.pub > authorized_keys`完成写入。因为这个模块并不支持密码登录，加上需要随时可以远程，一个强密钥是必要的。

由于/data需要root才能访问，所以需要magisk给应用授权root，这里可以使用adb授权后用vi编辑，也可以安装`juiceSSH`连接local，su之后编辑。再或者使用`CX文件管理器`手动编辑也可以。

如果需要更改端口号或其他ssh的配置，和这普通Linux上一样，编辑`/data/ssh/sshd_config`文件，保存后重启手机即可。

完成编辑之后可以用`ssh root@手机IP -i id_rsa密钥`尝试访问。

在确保局域网下能正常访问手机后，为了能在任何地方都可以直接访问手机，这里使用的是IPv6地址进行访问。国内现在部署ipv6已经非常全面，三大运营商现在都提供了ipv6地址，用v6可以不需要任何穿透工具即可直接访问。

##### 配置MacroDroid自动更新DDNS信息

------

手机打开流量，关闭Wi-Fi，确保手机能正常访问网络，然后打开MacroDroid自动化工具，新建一个宏操作，触发器选择`IP地址更改`，动作选择`http请求`，这里我们使用查询的方式来获取自己的IP地址，这样能保证这个IP地址是一定可用的。在`HTTP请求`中，请求方法选择`GET`，网址输入`https://api-ipv6.ip.sb/ip`，使用ip.sb的API获取地址。

勾选`完成后才能后续动作`和`跟随重定向`，超时时间填个10秒，以避免长时间获取不到数据卡着。在响应部分勾选`将HTTP返回状态码保存在整型变量中`，然后设定一个变量名例如`http_ret`，之后我们用这个变量判断是否请求成功。

选择`将HTTP响应保存着字符串变量中`，然后设置个变量名，例如`ipv6_addr`，这个变量保存的值就是我们设备的IPv6地址。

完成后我们再添加一个HTTP请求，这里就是请求上传更新DNS的API，根据自己选择的DDNS供应商提供的API信息，自己填入具体的参数数据。

以DNSPod为例，请求方法使用`POST`，网址填入`https://dnsapi.cn/Record.Ddns`，到第二页`查询参数`中填入所需的各字段信息：

```
lang:en
login_token:xxxxx,xxxxxxxxxxxxxxxxx
format:json
domain:自己的域名
sub_domain:要更新的二级域名信息
record_line_id:0
record_id:xxxxxx
value:{v=ipv6_addr}
```

每行一个，这里的value就是要更新的ipv6地址信息，这里使用`{v=ipv6_addr}`就是刚才设置的变量。

在内容正文中还得再填一次token，内容类型选择`text/plain`，内容正文选文本，然后信息填入`login_token=xxxxx,xxxxxxxxxx`你的token信息。

完成后点击右上角的✓即可。

完成后可以测试一下动作，如果在DNSpod上能正确获取到记录，就可以使用你的域名来进行ssh访问了。

`ssh root@你的域名 -i 密钥文件 -p 端口号`

