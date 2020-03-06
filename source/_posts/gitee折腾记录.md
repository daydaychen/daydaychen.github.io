---
title: gitee折腾记录
date: {{ date }}
tags: gitee
categories: 
comments: true
---

# gitee.com 介绍

码云(gitee.com) 代码托管·协作开发平台，开发者近 400 万，托管项目超过 600 万，汇聚几乎所有本土原创开源项目，并于 2016 年推出企业版，提供企业级代码托管服务，成为开发领域领先的 SaaS 服务提供商。

# 1. 注册 gitee.com 账户

> 打开 [gitee官网](https://gitee.com/)， 注册账号并登录

# 2. 创建仓库
> 创建仓库 ![](https://pic.downk.cc/item/5e60e39498271cb2b8b33976.jpg)

> 键入仓库信息 ![](https://pic.downk.cc/item/5e60e41598271cb2b8b378d9.jpg)

- 仓库名称：任意
- 归属/路径：保持默认即可
- 仓库介绍：非必填
- 是否开源：
    - 私有：拉取代码需要验证用户名和密码
    - 公开：任何人无需验证身份即可直接拉取代码
- 选择语言：项目代码的语言
- 添加.gitignore: 该文件用于指定上传代码时不包含项目内的某些文件，譬如包含密码的文件、缓存、项目环境、日志等无用文件，
- [ ] 使用Readme 文件初始化这个仓库：用于介绍项目的文件，建议使用
- [ ] 使用Issue 模板文件初始化这个仓库：初次配置可忽略
- [ ] 使用Pull Request模板文件初始化这个仓库：初次配置可忽略
- 选择分支模型：初始化仓库时默认只有一个master分支，根据需要设定

> 点击![](https://pic.downk.cc/item/5e60e7c098271cb2b8b5272d.jpg)

> 仓库首页 ![](https://pic.downk.cc/item/5e60e98198271cb2b8b60b38.jpg)

# 3. 创建本地仓库/拉取远程仓库


## 3.1 创建本地仓库

> 在bash环境下操作

```bash
# 创建目录并切换到该目录
$ mkdir local_repo && cd local_repo

# 初始化仓库
$ git init
```

## 3.2 拉取远程仓库

> 获取远程仓库链接

![](https://pic.downk.cc/item/5e60ea9a98271cb2b8b6bf3b.jpg)

> 拉取代码

```bash
# 拉取远程仓库到本地目录learn_git
$ git clone https://gitee.com/daydaychen/learn_git.git learn_git
Cloning into 'learn_git'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
Checking connectivity... done. 
```

# 4. 跟踪文件/取消跟踪

> 

```bash
# 当前目录下有两个文件
dayday:learn_git$ ls
total 8.0K
-rw-rw-rw- 1 dayday dayday  953 Mar  5 20:06 README.en.md
-rw-rw-rw- 1 dayday dayday 1.3K Mar  5 20:06 README.md

# 新建文件
$ echo "hello world" > day1_work.md

# 查看工作区状态
dayday:learn_git$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files: # <-- 有以下文件未跟踪
  (use "git add <file>..." to include in what will be committed)

        day1_work.md

nothing added to commit but untracked files present (use "git add" to track)

# 跟踪day1_work.md
$ git add day1_work.md
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed: # <-- 有以下文件已修改未提交
  (use "git reset HEAD <file>..." to unstage)
        new file:   day1_work.md

# 提交修改
dayday:learn_git$ git commit -m 'first commit'
[master b52affd] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 day1_work.md

# 查看工作区状态
dayday:learn_git$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
(use "git push" to publish your local commits)
nothing to commit, working directory clean
```

# 5. 创建分支/切换分支/删除分支
```bash
# 列出所有分支
dayday:learn_git$ git branch --list
* master

# 创建dev分支
dayday:learn_git$ git branch dev
dayday:learn_git$ git branch --list
dev
* master

# 切换到dev分支
dayday:learn_git$ git checkout dev
Switched to branch 'dev'
dayday:learn_git$ git branch --list
* dev
master

# 切换回master分支
dayday:learn_git$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
(use "git push" to publish your local commits) 

# 删除dev分支
dayday:learn_git$ git branch -d dev
Deleted branch dev (was b52affd). 
```
# 8. 合并分支

```bash
# 创建dev分支并切换过去
dayday:learn_git$ git checkout -b dev
Switched to a new branch 'dev'

# 新建文件
dayday:learn_git$ echo "dev branch working..." > dev.md

# 查看dev分支工作区
dayday:learn_git$ ls
total 8.0K
-rw-rw-rw- 1 dayday dayday  953 Mar  5 20:06 README.en.md
-rw-rw-rw- 1 dayday dayday 1.3K Mar  5 20:06 README.md
-rw-rw-rw- 1 dayday dayday   12 Mar  5 20:12 day1_work.md
-rw-rw-rw- 1 dayday dayday   22 Mar  5 20:44 dev.md

# 跟踪dev.md
dayday:learn_git$ git add .

# 提交更改
dayday:learn_git$ git commit -m 'add dev.md'
[dev a6986a9] add dev.md
1 file changed, 1 insertion(+)
create mode 100644 dev.md

# 切换到master分支
dayday:learn_git$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
(use "git push" to publish your local commits) 

dayday:learn_git$ git branch
dev
* master

# 查看master分支工作区
dayday:learn_git$ ls
total 8.0K
-rw-rw-rw- 1 dayday dayday  953 Mar  5 20:06 README.en.md
-rw-rw-rw- 1 dayday dayday 1.3K Mar  5 20:06 README.md
-rw-rw-rw- 1 dayday dayday   12 Mar  5 20:12 day1_work.md

# 比较master分支和dev分支的区别
dayday:learn_git$ git diff master dev
diff --git a/dev.md b/dev.md
new file mode 100644
index 0000000..c38f76b
--- /dev/null
+++ b/dev.md
@@ -0,0 +1 @@
+dev branch working...

# 合并dev分支到master分支
dayday:learn_git$ git merge dev
Updating b52affd..a6986a9 Fast-forward
dev.md | 1 + 1 file changed, 1 insertion(+)

# 查看master分支工作区
create mode 100644 dev.mddayday:learn_git$ ls
total 8.0K
-rw-rw-rw- 1 dayday dayday  953 Mar  5 20:06 README.en.md
-rw-rw-rw- 1 dayday dayday 1.3K Mar  5 20:06 README.md
-rw-rw-rw- 1 dayday dayday   12 Mar  5 20:12 day1_work.md
-rw-rw-rw- 1 dayday dayday   22 Mar  5 20:53 dev.md
```


# 11. 推送的远程仓库

```bash
# 查看工作区状态
dayday:learn_git$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
(use "git push" to publish your local commits)
nothing to commit, working directory clean 

# 推送到远程仓库
dayday:learn_git$ git push origin master
Username for 'https://gitee.com': daydaychen
Password for 'https://daydaychen@gitee.com':
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 537 bytes | 0 bytes/s, done.
Total 6 (delta 1), reused 0 (delta 0)
remote: Powered by GITEE.COM [GNK-3.8]
To https://gitee.com/daydaychen/learn_git.git
4618244..a6986a9  master -> master  
```

# 12 向一个项目贡献

> fork 仓库

> 拉取远程仓库到本地

> 提交修改并推送到远程仓库

> 提交Pull Request

# 13 仓库设置说明
> 仓库成员管理

添加成员，按角色分配仓库的读写权限

> 部署公钥管理

该操作通过非对称加密密钥对对仓库进行访问验证，无需每次输入用户名密码

![](https://pic.downk.cc/item/5e60fefc98271cb2b8c187aa.jpg)