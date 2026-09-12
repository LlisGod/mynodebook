---
tags:
  - AS/草稿
  - 计算机/操作指南
---

# Nvm 使用

**这是一个 node 版本管理系统**
官网安装
打开 cmd

## 命令 ：
Nvm install 20 ：安装 node 20 版本
Nvm ls ： 查看 node 环境和版本号
Nvm use （版本号）：使用该版本
Node -v ：如果成功输出 node版本号则成功运行

# 初始化 vue项目

#### 手动创建项目
先 cd 到项目路径
Npm init vite-app （项目名称）

安装依赖包
Npm i

运行项目
Npm run dev

但这样不好，有很多重复的工作，所以使用脚手架创建
## 脚手架创建
创建最新版本（使用 vite 脚手架）更快更好
```sh
npm create vue@latest
```

逐渐淘汰的旧项目（官方上个版本的脚手架）
```sh
vue create  （项目名称）
```
可以选择 vue 2 还是 vue 3
自定义等等

两者的启动命令不同，
`npm run dev`

# 部署发布
当你准备将应用发布到生产环境时，请运行：

```sh
 npm run build
```


此命令会在 ./dist 文件夹中为你的应用创建一个生产环境的构建版本。关于将应用上线生产环境的更多内容，请阅读生产环境部署指南。