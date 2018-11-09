# github博客搭建

## 安装

需要安装hexo-deployer-git hexo模块
```
npm install hexo-deployer-git --save
```

## 发布文章

```
hexo new xxx
```

## 分类和标签
```
---
title: title #文章標題
date: 2016-06-01 23:47:44 #文章生成時間
categories: "Hexo教程" #文章分類目錄 可以省略
tags: #文章標籤 可以省略
     - 标签1
     - 标签2
 description: #你對本頁的描述 可以省略
---
```

## 主题
[miho](https://github.com/WongMinHo/hexo-theme-miho)

## 常用指令
```
hexo g #完整命令为hexo generate,用于生成静态文件
hexo s #完整命令为hexo server,用于启动服务器，主要用来本地预览
hexo d #完整命令为hexo deploy,用于将本地文件发布到github上
hexo n #完整命令为hexo new,用于新建一篇文章

hexo d -g #生成静态资源，上传
```

## 遇到的坑
>
hexo d 时报错：ERROR Deployer not fount： git

## 地址
[博客地址](https://chaileicsu.github.io/)

## 参考资料
- [hexo.io](https://hexo.io/)
- [一、搭建篇-使用Github-hexo搭建个人博客教程—总结自己爬过的坑](https://yq.aliyun.com/articles/117271?spm=5176.10695662.1996646101.searchclickresult.78761967QLiSiB)
- [搭建GitHub博客，使用Hexo](https://yq.aliyun.com/articles/71630?spm=a2c4e.11153940.blogcont71629.16.15181b18lmrktO)
- [用Hexo搭建的Github博客换肤和发文章](https://yq.aliyun.com/articles/71629?spm=a2c4e.11153940.blogcont71630.20.6e6b7b180ta3h1)
