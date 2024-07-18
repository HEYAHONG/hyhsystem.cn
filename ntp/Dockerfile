FROM debian:latest
MAINTAINER HEYAHONG 2229388563@qq.com
WORKDIR /
ENV DEBIAN_FRONTEND noninteractive
RUN find /etc/apt -type f  | xargs sed -i "s/deb.debian.org/mirrors.ustc.edu.cn/g"  && apt-get update && apt-get full-upgrade -yyy && apt-get install -yyy openntpd
COPY ntpd.conf /etc/openntpd/ntpd.conf
RUN mkdir -p /var/run/openntpd
# 暴露端口
EXPOSE 123/udp
CMD [ "ntpd","-d","-f","/etc/openntpd/ntpd.conf" ]

