# 说明

本网站主要是用于子域名为mediawiki时的网站，可用作小型git服务。

本网站主要运行mediawiki，当成功启动后将会占用以下端口：

- 65184:http端口，可用于访问mediawiki内容。如需在80端口访问需要使用反向代理服务器转发。

# 首次使用

首次使用可通过配置好的地址访问web界面，本机使用可使用[localhost:65184](localhost:65184)访问web界面。

首次访问将进入将要求进入安装程序并产生LocalSettings.php，填写要求如下：

- 数据库采用 SQLite,数据库目录填写/var/www/html/sqlite/data，若需要其他数据库类型，需要自行备份数据库。
- 下载LocalSettings.php需要放入当前目录的html目录，并使用`make clean && make`重新部署当前网站。

# 数据迁移

当需要更换服务器或者其它需要转移数据的场合时，可进行数据迁移。

之所以采用容器进行服务搭建，是因为可以方便的迁移。

## 外部访问路径不变时

直接备份本目录数据，尤其是html目录即可，直接可通过make命令（可能需要root权限）部署。

备份本目录时一定要注意文件/文件夹的uid/gid要与原来一致，否则可能因为权限问题不能顺利启动服务。

## 外部访问路径改变时

在完成 外部访问路径不变时 的操作后，还要修改html/LocalSettings.php里的相应配置。

# nginx反向代理服务器设置

为使cgi程序能够获取正确的IP地址，需要在占用80端口的nginx的location配置中添加下列代码：

```nginx
proxy_set_header   Host             $host;
proxy_set_header   X-Real-IP        $remote_addr;
proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;

```

# 操作

本网站除了进行通用的操作部署和清理之外，还支持打包与解包操作(可用于网站迁移)。

注意：
- 打包需要配置完成后(即执行完成首次使用的操作)才能使用,解包操作可直接执行，无需完成首次使用的操作。
- 若当前用户不是root用户且不在docker用户组，请在下列操作中使用sudo提升权限，否则将操作失败。
- 此处的操作需要在当前目录下执行。
- 数据库采用 SQLite3并且按照 首次使用 的说明部署，否则需要自行备份数据库内容，其它数据库类型不可使用打包与解包操作。
- 此操作可能会遇到权限问题，可使用chown/chmod解决。

## 打包

打包网站。

```bash
make pack
```

## 解包

解包网站。

```bash
make unpack
```

# 采用mediawiki.hyhsystem.cn的打包数据

由于mediawiki.hyhsystem.cn的数据量较大,不适合在git上存储，因此需要单独手动下载。

链接: https://pan.baidu.com/s/1FDldUnqccUEO9xWsEvjuTQ?pwd=8888 提取码: 8888 

将文件夹中的其中一个mediawiki_data*.tar.gz（一般可直接下载最新的打包数据）下载后，重命名为mediawiki_data.tar.gz并放入当前文件夹，最后执行解包操作后即可访问。

## 注意事项

- 若当前用户不是root用户且不在docker用户组，请自行使用sudo提升权限，否则将操作失败。

### mediawiki版本问题

通常mediawiki.hyhsystem.cn采用的是最新版本的mediawiki,若mediawiki版本不够新，可能导致无法启动。

因此，当下载最新版打包数据后，需要执行`docker pull mediawiki`将拉取最新镜像，然后使用`make clean && make`重新部署mediawiki。

```bash
#部署后执行以下命令更新数据库
docker exec --user www-data -it mediawiki.hyhsystem.cn php  /var/www/html/maintenance/update.php
```



### mediawiki用户密码问题

默认管理员账号HEYAHONG，密码已清除。

```bash
#部署后执行以下命令设置密码,其中新密码需要需要替换为实际的密码(需要符合mediawiki对密码复杂度的要求)
docker exec --user www-data -it mediawiki.hyhsystem.cn php  /var/www/html/maintenance/changePassword.php --user HEYAHONG --password <新密码>

```

