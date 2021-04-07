---
title: Git 常用命令及工具
date: 2021.04.06
tags: Git
categories: Technology  
mathjax: true 
---

主要介绍 Git 常用命令及工具的使用。
![](https://wyiyi.github.io/amber/contents/git/git.png)


## 常用命令：
 ```
# 从远程仓库克隆仓库代码到本地
$ git clone http://xxx.git
# 查看分支
$ git branch -a
# 创建分支
$ git checkout -b new-branch
# 切换分支
$ git checkout new-branch
# 查看分支状态
$ git status
# 本地提交代码
$ git add .
$ git commit -m "test commmit"
# push 之前本地先 pull 最新代码
$ git pull origin master
# 遇到冲突，查看哪些文件冲突
$ git diff
# 将本地分支推送到其它分支
$ git push origin master
 ```

![](https://wyiyi.github.io/amber/contents/git/bash.png)

![](https://wyiyi.github.io/amber/contents/git/bash1.png)

![](https://wyiyi.github.io/amber/contents/git/bash2.png)

![](https://wyiyi.github.io/amber/contents/git/idea1.png)

![](https://wyiyi.github.io/amber/contents/git/idea.png)


## 可视化界面操作
### 1、IDEA 
![](https://wyiyi.github.io/amber/contents/git/idea2.png)
- Local history 可以查看之前修改该文件的记录
- Commit File 正常提交文件
- Compare with.. 和远程比较
- Compare with Branch... 和远程分支进行比较
- Show History 对该文件的历史修改记录
- Rollback... 还原文件最初的状态

![](https://wyiyi.github.io/amber/contents/git/idea3.png)
- pull 拉取远程分支到本地
- push 推送本地分支到远程分支

### 2、ACAP
![](https://wyiyi.github.io/amber/contents/git/acap.png)

![](https://wyiyi.github.io/amber/contents/git/acap2.png)

### 3、TortoiseGit 工具
![](https://wyiyi.github.io/amber/contents/git/tortoiseGit.png)

### 3、Gitlab
- 提交合并：

![](https://wyiyi.github.io/amber/contents/git/gitlab.png)

- 冲突处理：

![](https://wyiyi.github.io/amber/contents/git/gitlab2.png)

![](https://wyiyi.github.io/amber/contents/git/gitlab1.png)
