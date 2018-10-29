# gitbook 实践20181029
## 背景
> 在github上整理知识库，记录日常实践操作记录，同步生成gitbook，更新博客；

- github上编辑md文件，执行push操作；
- github 触发webhook;
- webhook 服务端程序，调度指定脚本；
- 指定脚本git pull 拉取最新代码；
- 指定脚本执行 gitbook build生成最新book html文件；
- 拷贝html文件到服务器目录下；
- 访问博客，显示更新后的内容；

## 安装及使用

### 安装gitbook
```
npm install gitbook-cli -g
gitbook -V
```
### 配置book.json，SUMMARY.MD
> 编辑book.json

```
{
    "title": "csym's knowledge repository",
    "description": "实践出真知，记录学习中的实践过程",
    "author": "csym",
    "output.name": "site",
    "language": "zh-hans",
    "gitbook": "3.2.3",
    "root": ".",
    "links": {
        "sidebar": {
            "Home": "https://www.clqcwp.com"
        }
    },
    "plugins": [
        "github@^2.0.0",
        "edit-link@^2.0.2",
        "anchors@^0.7.1",
        "include-codeblock@^3.0.2",
        "splitter@^0.0.8",
        "tbfed-pagefooter@^0.0.1",
        "expandable-chapters-small@^0.1.7",
        "anchor-navigation-ex@0.1.8"
    ],

    "pluginsConfig": {
        "theme-default": {
            "showLevel": true
        },
        "github": {
            "url": "https://github.com/csym/lab"
        },
        "include-codeblock": {
            "template": "ace",
            "unindent": true,
            "edit": true
        },
        "tbfed-pagefooter": {
            "copyright": "Copyright © csym 2018",
            "modify_label": "该文件修订时间：",
            "modify_format": "YYYY-MM-DD HH:mm:ss"
        },
        "edit-link": {
            "base": "https://github.com/csym/lab/edit/master",
            "label": "Edit This Page"
        },
        "anchor-navigation-ex": {
            "isRewritePageTitle": false,
            "tocLevel1Icon": "fa fa-hand-o-right",
            "tocLevel2Icon": "fa fa-hand-o-right",
            "tocLevel3Icon": "fa fa-hand-o-right"
        }


    }
}
```
> 编辑SUMMARY.MD

```
# Summary

* [Introduction](README.md)

-----
* [关于博客](ABOUT_BLOG.md)

-----
* [知识库](knowledge.md)
    * [操作系统](OS/os.md)
        * [Linux](OS/linux/linux.md)
        * [windows](OS/win/windows.md)
        * [Unix](OS/unix/unix.md)
* [工具库](tools.md)
    * [git相关](Tools/git/git.md)
        * [gitbook](Tools/git/gitbook.md)
        * [webhook](Tools/git/webhook.md)

```

### 运行gitbook

> 参考命令
```
gitbook init
gitbook install
gitbook serve
```
> 本地查看：http://localhost:4000/

## webhook 与github联动
### webhook安装
```
pip install webhookit
webhookit_config > ./webhook_for_github.conf
```
### webhook配置

> 修改配置文件

- 如果执行脚本在webhook本机，只需要修改如下两个参数:
    - repo_name/branch_name修改成自己的项目名称和分支名
    - SCRIPT写入自己要执行的脚本

```
[chail@iZuf65iwnpu8squsl3oo1gZ webhook]$ vim webhook_for_github.conf                                    
# -*- coding: utf-8 -*-
'''
Created on Oct-29-18 13:20:46

@author: hustcc/webhookit
'''


# This means:
# When get a webhook request from `repo_name` on branch `branch_name`,
# will exec SCRIPT on servers config in the array.
WEBHOOKIT_CONFIGURE = {
    # a web hook request can trigger multiple servers.
    'lab/master': [{
        # if exec shell on local server, keep empty.
        'HOST': '',  # will exec shell on which server.
        'PORT': '',  # ssh port, default is 22.
        'USER': '',  # linux user name
        'PWD': '',  # user password or private key.

        # The webhook shell script path.
        'SCRIPT': '/home/chail/lab/gitbook/gitbook_update.sh >> /home/chail/lab/gitbook/gitbook_update.l
og'
    }]
}
```

### webhook 启动
```
webhookit -c /home/chail/lab/gitbook/webhook/webhook_for_github.conf -p 3006
```
> 对外网址：http://101.132.116.192:3006/webhookit

### github 配置
> github 仓库中配置webhook

- 项目——setting——webhook——ADD webhook

    - payload URL：填写webhookURL
    - Content type ：application/json
触发条件可选，我这里选择的是Just the push event.

## 输出 PDF 文件
### 下载 Calibre
生成 PDF 文件依赖于 ebook-convert，需要安装 Calibre;

配置 Calibre 环境变量
如何配置环境变量参考这里，在 .bash_profile 文件加入：
```
# Calibre
export PATH=/Applications/calibre.app/Contents/MacOS:$PATH
```

更新刚配置的环境变量：
```
$ source .bash_profile
查看所有的配置路径：

$ echo $PATH
输出 PDF 文件
命令行：

$ gitbook pdf
将在根目录下生成了 book.pdf 文件
```

## 遇到问题

### pip 安装 

```
pip安装软件时出现Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build*的解决方案

```
> 解决方法：
```
sudo python -m pip install --upgrade --force pip 
sudo pip install setuptools==33.1.1
```
- 参考文章[Command "python setup.py egg_info" failed](https://blog.csdn.net/u011092188/article/details/64123561/)

## 参考资料
- [GitBook 使用](https://www.jianshu.com/p/09a1cac0a0d0) 
- [安装Gitbook](https://huangwj.app/ABOUT_BLOG.html)
- [webhookit 0.0.10](https://pypi.org/project/webhookit/)
- []()
