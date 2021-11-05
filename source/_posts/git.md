---
title: Git 总结
date: 2021-10-31 20:19:05
description: 免费的开源的分布式版本控制系统
tags:
  - Git
---


## 下载

- [官方下载地址](https://git-scm.com/downloads)

## 初始化和配置

- [官方命令使用文档](https://git-scm.com/book/zh/v2)
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
