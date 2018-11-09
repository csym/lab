# Docker Image实践

## 新建express程序
```
npm install express-generator -g
express wxserver
cd wxserver
cnpm i
cnpm run start
```
> 测试运行地址
http://101.132.159.46:3000/

## 新建doker文件

> 参考文件1

```
# Originally written for Centos-Dockerfiles by
#   chail <chaileicsu@163.com>
FROM node:8.11.2
MAINTAINER chail <chaileicsu@163.com>
LABEL System="CentOS7"
LABEL Build docker build --rm --tag centos/wxserver .
######set timezone
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 
########install ssh
#RUN yum -y update; yum clean all
#########install tools 
#########NODEJS
ADD ./wxserver /wxserver
RUN cd /wxserver; npm install
EXPOSE 3000
ENV NODE_ENV production
WORKDIR /
#CMD ["pm2","start","wxserver_start.sh","--no-daemon"]
CMD ["npm","start"]

```

> 参考文件2 Dockerfile.wxserver

```
# Originally written for Centos-Dockerfiles by

FROM centos:latest
MAINTAINER chail <chaileisu@163.com>

LABEL System="CentOS7"
LABEL Node=8.11.2

LABEL Build docker build --rm --tag centos/ccnaserver .

########set timezone
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 

########install ssh
RUN yum -y update; yum clean all

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 3000

#########NODEJS
ADD ./node-v8.11.2 /node-v8.11.2
RUN ln -s /node-v8.11.2/bin/node /usr/bin/node && \
ln -s /node-v8.11.2/bin/npm /usr/bin/npm 
RUN npm install cnpm -g
RUN ln -s /node-v8.11.2/bin/cnpm /usr/bin/cnpm
RUN cnpm install pm2 -g
RUN ln -s /node-v8.11.2/bin/pm2 /usr/bin/pm2

ADD ./ccnaserver /ccnaserver
RUN cd /ccnaserver; cnpm install

ADD ./wxserver /wxserver
RUN cd /wxserver; cnpm install

EXPOSE 3001
EXPOSE 3000

COPY ./ecosystem.config.js /ecosystem.config.js
COPY ./start_server.sh /ccnaserver/start_server.sh
RUN chmod 755 /ccnaserver/start_server.sh 

ENV NODE_ENV production
WORKDIR /


CMD ["pm2","start","./ecosystem.config.js","--no-daemon"]
#CMD ["node","/wxserver/bin/www"]

```

## Dockerfile 使用方式

docker build --tag ccnaserver:0.0.1 .


sudo docker run -d -p 3000:3000 -p 3001:3001 ccnaserver:0.0.1

## 参考文档

- [docker file命令参考](https://www.cnblogs.com/lighten/p/6900556.html)
- [pm2用法详解+ecosystem.config] (https://www.cnblogs.com/lggggg/p/6970395.html)
- [Linux系统（Centos）下安装nodejs并配置环境](https://blog.csdn.net/qq_21794603/article/details/68067821) 
