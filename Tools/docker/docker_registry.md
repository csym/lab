# docker私有仓库实践registry

## 安装步骤
```
 sudo docker pull registry
```

## 启动
> 正常方式启动
```
sudo docker run -d -p 5000:5000 --restart always --name registry -v /home/chail/registry_home:/var/lib/registry registry
```

> 证书方式启动
sudo docker run -d -p 5000:5000 --restart always --name registry -v /home/chail/registry_home:/var/lib/registry registry

docker run -d -p 443:443 --name registry_https  --privileged=true -v /home/chail/registry_home:/var/lib/registry -v /home/chail/certs:/certs -e REGISTRY_HTTP_ADDR=0.0.0.0:443 -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key registry

docker run -d -p 443:443 --name registry_https  --privileged=true -v /home/chail/registry_home:/var/lib/registry -v /home/chail/certs:/certs -e REGISTRY_HTTP_ADDR=0.0.0.0:443 -e registry



## 配置相关

## 上传下载测试

### 本机上传image
```
sudo docker tag rabbitmq:management localhost:5000/rabbitmq_test
sudo docker push localhost:5000/rabbitmq_test
```
> 成功

### 本机下载image

> 删除本地image后，重新拉取image
```
sudo docker pull localhost:5000/rabbitmq_test
```

### 远程上传image

### 远程下载image


## 常用命令
> 查看Registry中所有镜像信息
    curl http://<ip>:5000/v2/_catalog

## 异常记录
### 无权限

```
[root@iZuf696gwsgxtm28j1h7d2Z tmp]# docker pull 101.132.116.192:5000/rabbitmq_test
Using default tag: latest
Error response from daemon: Get https://101.132.116.192:5000/v2/: http: server gave HTTP response to HTTPS client
[root@iZuf696gwsgxtm28j1h7d2Z tmp]# 
```

## 参考文档
- [registry](https://hub.docker.com/_/registry/)
- [Docker Registry安装说明](https://www.cnblogs.com/shoufengwei/p/7288464.html)
- [http: server gave HTTP response to HTTPS client](https://www.cnblogs.com/lkun/p/7990466.html)
- [搭建一个支持HTTPS的私有DOCKER Registry](https://blog.csdn.net/xcjing/article/details/70238273)
- [发布本地Docker镜像到阿里云的Docker Hub](https://blog.csdn.net/upshi/article/details/78988030)
