---
title: koa基础框架搭建
date: 2019-07-15 12:41:51
tags: 
  - Node.js 
  - Koa
---
## 创建项目
创建一个项目目录，然后快速生成一个 package.json 文件
```
npm init -y
```
安装koa
```
npm install koa -S
```
创建app.js
```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Wise Wrong';
});

app.listen(3000);
```
在 package.json 中添加启动指令
!["start":"node app.js"](/script.png)
一个基础的koa项目就完成了
如果觉得可以手动搭建麻烦，可以使用脚手架 koa-generato 来生成项目
```
npm install koa-generator -g
koa2 project_name
```
再执行 npm install 安装依赖，npm start 启动项目
##