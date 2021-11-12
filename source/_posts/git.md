---
title: Git 总结
date: 2021-10-31 20:19:05
description: 免费的开源的分布式版本控制系统
tags:
  - Git
---


## 下载

- [官方下载地址](https://git-scm.com/downloads)
- [官方命令使用文档](https://git-scm.com/book/zh/v2)

## 初始化和配置

- 初始化

```初始化
git init
```

- 配置名称、邮箱

```配置名称、邮箱
git config --global user.name "你的名称"
git config --global user.email 你的邮箱
```

## 克隆远程仓库和添加远程仓库

`origin`为本地给远程仓库创建的快捷名称，可以随便命名。添加过远程仓库后可以直接使用`git pull origin 分支名`和`git push origin 分支名`来拉取和上传文件

- 克隆远程仓库
  ```git clone 远程仓库```

- 添加远程仓库
  ```git remote add origin 远程仓库```

## 提交流程步骤

- 将文件添加到暂存区
  - 所有文件
    `git add .`
  - 单个文件
    `git add 文件路径`

- 将文件从暂存区提交到本地仓库，`"说明"`为本次提交的文件说明
  ```git commit -m "说明"```

- 拉取远程仓库文件，防止冲突
  ```拉取git pull 远程仓库 分支名```

- 将本地仓库文件上传到远程仓库
  ```git push 远程仓库 分支名```

## 分支操作

- 切换分支
  ```git checkout 分支名```
- 创建分支
  ```git branch 分支名```
- 创建并切换分支
  ```git checkout -b 分支名```
- 删除分支
  ```git branch -d 分支名```
- 将指定分支合并到当前分支
  ```git merge 分支名```
- 将当前仓库的指定 commit 合并到当前分支
  ```git cherry-pick 版本号```

> 注：当合并分支出现冲突时，请手动修改冲突

## 临时存储

- 添加到临时存储，`描述`为当前临时存储信息的描述。不携带`save "描述"`，默认为`save "上一次提交的commit的描述"`
  ```git stash```
  ```git stash save "描述"```

- 查看存储列表
  ```git stash list```

- 取出存储。区别：`apply`取出不删除记录。`pop`取出并删除记录
  ```git stash pop stash@{$num}```
  ```git stash apply stash@{$num}```

- 删除存储。`drop`删除某条，`clear`删除所有
  ```git stash drop stash@{$num}```
  ```git stash clear```

- 显示某条存储diff
  ```git stash show stash@{$num}```

> 注：不携带`stash@{$num}`默认都是第一个，使用`stash@{$num}`区别每一条存储。

## 版本回退(后悔药)

- 查看提交日志
  `git log`
- 回退到上一个版本
  `git reset --hard HEAD^`
- 回退固定版本
  `git reset --hard 版本号`
- 查看提交状态
  `git status`
- 查看简洁的提交状态
  `git status -s`
- 查看暂存区与工作区的区别
  `git diff`
- 撤销文件，`<file>`为文件名
  `git checkout -- <file>`

## 常见问题

1. 问题描述：当你要把两个项目的分支或者两个不共享共同历史记录的分支合并时。会报以下错误：`fatal: refusing to merge unrelated histories`
  解决方案：允许不相关的历史: `--allow-unrelated-histories`
  举个例子： `git merge 分支名 --allow-unrelated-histories`

2. 问题描述：提交不符合规范的代码时，husky代码检查会报以下错误：`husky - pre-push hook failed (add --no-verify to bypass)`
  解决方案：绕过 husky 中的`pre-commit`和`commit-msg`钩子：`--no-verify`
  举个例子： `git commit --no-verify -m "说明"`

3. 其他问题：

- 解除合并状态:
  ```git merge --abort```

- 删除远程仓库文件
  ```git rm filename```
  ```git rm --cached filename```

- 合并分支将所有commit合成一个commit
  ```git merge 分支名 --squash```

- 查看git安装目录
  ```which git```
  ```where git```

## SSH key

> 使用ssh开发可以避免很多问题

1. 生成公钥（一路回车）
```ssh-keygen -t rsa -C "你的邮箱"```

2. 查看公钥
```cat ~/.ssh/id_rsa.pub```

3. 将生成的公钥添加到git服务器

4. 检测是否连接成功
```ssh -T git@github.com```
