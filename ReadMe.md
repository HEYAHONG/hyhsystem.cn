# 说明

hyhsystem.cn网站源代码

# 网站

- [@](@)：作为子域名为www或不使用子域名时的网站源代码。网址：[http://hyhsystem.cn](http://hyhsystem.cn)
- [docs](docs):作为子域名为docs的网站源代码,[github pages](https://heyahong.github.io)的备份访问方式。网址:[http://docs.hyhsystem.cn](http://docs.hyhsystem.cn)
- [ntp](ntp):作为子域名为ntp的网站源代码,提供备份的时间同步服务，可使用ntp.ntsc.ac.cn作为主要时间同步服务器。
- [gitea](gitea):作为子域名为gitea的网站源代码,提供git服务，一般用于备用，主要使用[https://github.com](https://github.com)。
- [mediawiki](mediawiki):作为子域名为mediawiki的网站源代码,用作笔记记录，可访问[https://mediawiki.hyhsystem.cn](https://mediawiki.hyhsystem.cn)。


注意：为适应在一个服务器上搭建多个虚拟主机（可使用同一端口使用域名区分不同虚拟主机），通常部署完成后会启动监听非http端口的docker容器,因此，如需使用http端口，需要配置nginx反向代理将请求转发至相应网站的端口。

# 要求

## docker

由于使用了docker技术,需要保证docker命令与docker-compose命令能正常运行。

## make

由于采用了make作为主要构建工具，需要保证make命令能正常运行。

## git

由于采用了git作为更新某些子模块的工具，需要保证git命令能正常运行。


## coreutils

在Makefile中使用了coreutils的相关工具，因此需要安装coreutils（对于ubuntu系统而言，一般无需手动安装）

## findutils

在Makefile中使用了findutils的相关工具，因此需要安装findutils（对于ubuntu系统而言，一般无需手动安装）

# 操作

注意：本源代码不得放在具有空格的路径下，否则会出现异常。若当前用户不是root用户且不在docker用户组，请在下列操作中使用sudo提升权限，否则将操作失败。

在当前目录执行操作将对所有服务进行操作，如无需部分服务，可进入需要的网站的目录进行操作。

## 部署

```bash
make
```

## 清理

```bash
make clean
```

