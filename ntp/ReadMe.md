# 说明

本网站主要是用于子域名为ntp时的服务器。提供备份的时间同步服务，可使用ntp.ntsc.ac.cn作为主要时间同步服务器。

**NTP必须占用UDP 123端口（若已被其它服务占用将导致启动失败）**

# 解除UDP 123端口占用

有的系统默认启用了ntp服务，将占用UDP 123端口，导致ntp服务无法启动。

通过以下命令关闭系统原有的NTP服务(若非root用户请自行使用sudo提权):

```bash
systemctl stop ntp
systemctl stop systemd-timesyncd
systemctl mask ntp
systemctl mask systemd-timesyncd
```

# 测试

NTP服务不像HTTP那样可通过浏览器访问。

可安装ntpdate，采用ntpdate测试：

```bash
ntpdate -q ntp.hyhsystem.cn
```

