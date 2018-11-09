# jenkins和docker实现自动化构建部署

## 目标
- 从git上获取项目；--ok
- 对项目文件进行操作；--ok
- ssh到目标服务器；--ok
- 上传本地文件到目标服务器指定目录；--ok
- ssh 使用非root用户测试；---ok
- 不从git上获取项目，仅上传本地文件；--ok
- ssh 到多个server；--ok
- 代码修改后自动触发构建；--ok
- 构建完成后发送邮件提醒；--ok
- 代码本地编译后，再进行构建；--ok
- 在目标服务器上停止docker 容器；---ok 需要时间比较久
- 在目标服务器上下载新的docker image；--ok
- 在目标服务器上启动新的docker image；--ok


## 实践

### 构建脚本参考
shell
```
rm -rf /var/jenkins_home/workspace/dispatch_bst/bst.tar.gz
tar -zcvf /tmp/bst.tar.gz -C /var/jenkins_home/workspace/dispatch_bst/ . --exclude="*.git"
mv /tmp/bst.tar.gz /var/jenkins_home/workspace/dispatch_bst/
```

ssh
```
source /tmp/bst.tar.gz
pwd
who am i
mkdir /home/sytwx/work/bst
cd /home/sytwx/work/bst
cp /home/sytwx/work/bst.tar.gz .
tar -zxvf bst.tar.gz

```

47.52.104.116 上启动docker
```
sudo docker rm -f jenkins
sleep 2
sudo docker pull jenkins
sleep 2
sudo docker run -d --name myjenkins -p 8010:8080 -p 5010:50000 -v /home/chail/jenkins_home:/var/jenkins_home jenkins
```

## 远程触发
浏览器输入如下url 可以远程触发构建
```
http://101.132.116.192:8010/job/testLoginServerAndUploadFile/build?token=123456
```
## 周期性构建
略

## 代码修改后触发
> Poll SCM
* * * * *



## tips
1. 在源码管理设置完成后，可以直接进行构建，拉取源码至本地目录；
2. 构建过程中增加ssh上传配置，可以将本地文件，直接上传到远程服务器指定目录；Remote directory，需要填写ssh配置根目录下面的子目录；
3. 

## 遇到问题
1. jenkins 挂在home目录后启动报错，处理方式，前台启动，查看错误日志，发现是权限的问题，处理权限问题；

2. email设置时，连接不成功，验证不成功；
 连接不成功原因：阿里云禁用了25端口；
 验证不成功原因：填错了163客户端认证吗；


## 参考文档

- [Jenkins（十二）修改用户使用sudo不再需要密码](https://blog.csdn.net/yejianyun1/article/details/54581053)
- [jenkins和docker实现自动化构建部署](https://blog.csdn.net/bingoxubin/article/details/78720976)
- [Centos创建sudo用户并且免输sudo密码](https://blog.csdn.net/levy_cui/article/details/51143188)
- [jenkins构建触发器定时任务Build periodically和Poll SCM【转载】](https://www.cnblogs.com/caoj/p/7815820.html)
- [Jenkins基础入门-1-Jenkins简单介绍和环境安装](https://blog.csdn.net/u011541946/article/details/78003772)
- [Jenkins基础入门-5-用户和权限管理](https://blog.csdn.net/u011541946/article/details/78007078)
- [Jenkins入门到App打包实战](https://blog.csdn.net/u011541946/article/category/7175041)
- [jenkins配置邮件及增强版邮件通知](https://blog.csdn.net/u013066244/article/details/78665075)