# 说明

本网站主要是用于子域名为gitea时的网站，可用作小型git服务。

本网站主要运行gitea，当成功启动后将会占用以下端口：

- 65182:http端口，可用于web界面访问、使用git进行源代码推送与拉取。如需在80端口访问需要使用反向代理服务器转发。
- 65183:ssh端口，可用于使用git进行源代码推送与拉取。如需在其他端口访问可通过[rinetd](http://www.rinetd.com/)转发或者直接修改[docker-compose.yml](docker-compose.yml)

# 首次使用

首次使用可通过配置好的地址访问web界面，本机使用可使用[localhost:65182](localhost:65182)访问web界面。

首次访问将进入配置界面，填写要求如下：

- 数据库采用 SQLite3,若需要其他数据库类型，可采参考[官网说明](https://docs.gitea.com/installation/install-with-docker)，需要修改[docker-compose.yml](docker-compose.yml)。
- 基础URL需要填写从外部访问gitea时的配置，如外部访问gitea的服务器的基础URL为[http://gitea.hyhsystem.cn:3000](http://gitea.hyhsystem.cn:3000 ) , 而不能填写容器的相关配置。基础URL主要影响使用git进行源代码推送与拉取的操作。
- SSH 服务端口需要填写22,填写其他值可能导致docker容器的转发失效。
- HTTP 服务端口需要填写3000，填写其他值可能导致docker容器的转发失效。

# 数据迁移

当需要更换服务器或者其它需要转移数据的场合时，可进行数据迁移。

之所以采用容器进行服务搭建，是因为可以方便的迁移。

## 外部访问路径不变时

直接备份本目录数据，尤其是data目录即可，直接可通过make命令（可能需要root权限）部署。

备份本目录时一定要注意文件/文件夹的uid/gid要与原来一致，否则可能因为权限问题不能顺利启动服务。

## 外部访问路径改变时

在完成 外部访问路径不变时 的操作后，还要修改data/gitea/conf/app.ini里的相应配置。

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
- 打包与解包操作均需要配置完成后(即执行完成首次使用的操作)才能使用。
- 若当前用户不是root用户且不在docker用户组，请在下列操作中使用sudo提升权限，否则将操作失败。
- 此处的操作需要在当前目录下执行。
- 数据库采用 SQLite3，其它数据库类型不可使用打包与解包操作。

## 打包

打包数据库与git仓库（不含gitea配置）。

```bash
make pack
```

## 解包

解包数据库与git仓库（不含gitea配置）。

```bash
make unpack
```

# 采用gitea.hyhsystem.cn的打包数据

由于gitea.hyhsystem.cn的数据量较大,不适合在git上存储，因此需要单独手动下载。

链接: https://pan.baidu.com/s/1KvQROA0zB9sTV-8nxGAQSQ 提取码: 8888

将文件夹中的其中一个gitea_data*.tar.gz（一般可直接下载最新的打包数据）下载后，重命名为gitea_data.tar.gz并放入当前文件夹，最后执行解包操作后即可访问。

## 注意事项

若当前用户不是root用户且不在docker用户组，请自行使用sudo提升权限，否则将操作失败。

### gitea版本问题

通常gitea.hyhsystem.cn采用的是最新版本的gitea,若gitea版本不够新，可能导致无法启动。

因此，当下载最新版打包数据后，需要执行`docker pull gitea/gitea`将拉取最新镜像，然后使用`make clean && make`重新部署gitea。

### gitea用户密码问题

默认管理员账号HEYAHONG，为了安全起见用户密码已清除（此时是无法正常登陆gitea的）。

此时可使用密码重置功能，具体如下：

```bash
#进入已经运行的docker容器
docker exec -it --user git gitea.hyhsystem.cn bash
#使用gitea的管理命令修改密码,其中新密码需要需要替换为实际的密码
gitea admin user change-password --username HEYAHONG --password <新密码>
#退出容器
exit

```

 
