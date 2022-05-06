---
title: 包管理工具
date: 2021-11-12 18:34:12
description: 常用的包管理工具总结
tags:
    - n
    - nrm
categories: 工具
---

## 包管理工具 npm

### 安装

[node官方下载地址](https:#nodejs.org/en/download/)

### 使用

```npm常用命令
# 检查版本
npm -v
# 下载
npm install xx
# 卸载
npm uninstall xx
```

- 发布NPM包

<font color='red'> 注意: 发布包时必须是 npm 源。通过 `包安装源管理工具 nrm` 查看命令 </font>

```发布NPM包
# 初始化
npm init
# 账号登录
npm login
# 上传
npm publish
# 删除
npm unpublish 包名@版本号
# 修改版本号 原来版本自加1(可以手动修改)
npm version patch
```

## 包安装源管理工具 nrm

### 安装

`
npm install -g nrm
`
### 使用

```nrm常用命令
# 查询所有支持源
nrm ls
# 查看当前源
nrm current
# 切换指定的源
nrm use [name]
# 帮助
nrm help
# 跳转到指定源官网
nrm home [name]
# 添加源
nrm add [name] [url] [home]
# 删除源
nrm del [name]
```

## node管理工具 n

### 安装

`
npm install -g n
`

### 使用

```n常用命令
# 查看已有版本
n list
# 切换版本
n
# 安装某个版本
n <version>
# 安装最新版本
n latest
# 安装最新稳定版本
n stable
# 删除某个版本
n rm <version>
```
