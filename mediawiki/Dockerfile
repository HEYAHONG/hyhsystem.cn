FROM mediawiki:latest
MAINTAINER HEYAHONG 2229388563@qq.com
WORKDIR /var/www/html
ENV DEBIAN_FRONTEND noninteractive
RUN find /etc/apt -type f  | xargs sed -i "s/deb.debian.org/mirrors.ustc.edu.cn/g"  && apt-get update && apt-get -yyy full-upgrade && apt-get install -yyy ghostscript
VOLUME ["/var/www/html/images","/var/www/html/sqlite","/var/www/html/resources/assets"]
# 暴露端口
EXPOSE 80
# 按照正常系统进行启动
CMD [ "apache2-foreground" ]
