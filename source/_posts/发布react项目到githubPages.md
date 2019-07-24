---
title: 发布react项目到githubPages
date: 2019-07-24 11:35:05
tags:
---

## 在github上建立项目仓库

如果已经开启了gitPages服务，项目名按照实际情况，如TodoList，到时地址会发布为`https://shiobom.github.io/TodoList/`

## 项目安装gh-pages

这个依赖是用来发布项目到gitPages的。

```
yarn add gh-pages --dev
```

配置package.json，添加如图所示script

![配置package.json的scripts](http://chuantu.xyz/t6/702/1563940025x977013264.png)

deploy命令里执行的 -b 表示发布到哪个分支下，-d 表示将哪个文件夹内容发布到指定的分支下

## 创建分支

因为 master 分支会开启gitpages服务，只能存放dist目录下的文件了，所以新建一个分支存放项目源代码

## 开启gitpages服务

github项目仓库 --> setting --> GitHub Pages --> 选择 source 源为 master
会出现一个链接，这个链接就是该项目的线上地址了。

## 部署项目

```
将项目打包生成 build 文件夹
yarn build 

部署项目
yarn deploy
```

