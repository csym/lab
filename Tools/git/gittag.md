# Git Tag 常用命令

## 打标签

> tag是对历史提交的一个id的引用

### 查看当前项目已有标签
> git tag

```
git clone http:xxxx.git
$ git tag
uweb_V1_0_0_180928
uweb_V1_0_0_180929

```

### 新增标签

> git tag -a <tagname> -m "标签"

```
$ git tag -a sms_server_v1_0_0_181105 -m "版本发布20181105"
$ git tag
sms_server_v1_0_0_181105

```

### 推送标签到远程仓库
> git push origin --tags

> 可以在远程仓库，看到tag；


## 从标签上拉取分支

### 只看不改
git checkout tag  不建议直接修改
```
Note: checking out 'xxx'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at aeeaf5a huangyj - 增加版本号配置显示.

```

### 从tag拉取分支

> git checkout -b 分支名称 tag名称

```
git co -b tagTest sms_server_v1_0_0_181105
```

> 强制覆盖分支 git checkout -B 分支名称 tag名称

### 修改后提交

> 本地分支上修改后，切换到主分支上，进行合并操作（注意处理冲突）；
```
git checkout master
git diff master tagTest

处理冲突
git add .
git commit -m ""
git push origin master
```



