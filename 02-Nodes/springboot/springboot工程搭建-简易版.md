---
tags:
  - 计算机/后端/springboot
---

## 创建项目引入依赖

在 springboot initialzr 中创建项目，引入必要的依赖（如 web -stater，[[mybatis]]）
部分依赖可能需要手动引入


## 配置数据库链接

在配置文件中配置好数据库的路径，用户名，密码，驱动等
这部分可以使用 ai 辅助生成

可以在测试中测试数据库链接

## 构建基础的工程包结构

- Controller
- Service
	- Impl
- Mapper/repostiry
- Pojo（基础）
	- entity
	- dto/vo

- 可选
	- 配置类，错误类等等
## 生成表结构和基础的实体类

这一步要按照数据库设计时的规划进行


## 生成统一响应体

基础的字段有
- Code
- Msg
- Data

其中 data 建议使用 T 泛型，不要使用 object，原因见[[为什么使用泛型而不是any或者object]]

可以加上
- Timestamp
- Path 等字段