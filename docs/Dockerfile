FROM nginx:latest
MAINTAINER HEYAHONG 2229388563@qq.com
WORKDIR /var/www/html
ENV DEBIAN_FRONTEND noninteractive
RUN find /etc/apt -type f  | xargs sed -i "s/deb.debian.org/mirrors.ustc.edu.cn/g"  && apt-get update && apt-get full-upgrade -yyy
VOLUME ["/usr/share/nginx/html"]
# 暴露端口
EXPOSE 80
CMD [ "nginx","-g","daemon off;" ]

