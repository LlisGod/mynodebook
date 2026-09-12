---
tags:
  - 计算机/后端/node
  - 计算机/操作指南
---
## 1. 项目初始化与规划

### 1.1 创建项目

```bash
mkdir node-backend-tutorial
cd node-backend-tutorial
npm init -y
```
创建项目目录
切换到项目目录
[[npm常用命令#Npm init -y|npm init -y]] ： 建立 package 文件，记录安装的所有依赖
### 1.2 安装基础依赖

```bash
//生产依赖
npm install express mongoose dotenv cors
//开发依赖
npm install --save-dev nodemon mocha chai supertest eslint prettier
```

上述代码分别安装了
- 生产依赖
	- `express`：Node.js 最流行的 Web 框架，用于快速构建 RESTful API 或服务器端应用。
	- `mongoose`：MongoDB 的对象模型工具（ODM），提供 Schema 结构、数据校验等功能，简化数据库操作。
	- `dotenv`：环境变量管理工具，将敏感配置（如数据库密码、API密钥）存储在 .env 文件中，避免硬编码。
	- `cors`：跨域资源共享中间件，允许不同域的前端应用访问你的 API（解决浏览器跨域限制）。
- 开发依赖
	- `nodemon`：开发时自动重启 Node 服务器，修改代码后无需手动重启，提升开发效率。
	- `mocha`：流行的 JavaScript 测试框架，用于编写和运行单元测试、集成测试。
	- `chai`：断言库（断言 = 预期结果判断），与 Mocha 配合使用，提供更友好的测试语法。
	- `supertest`：HTTP 请求测试工具，用于模拟对 API 端点的请求并验证响应（常用于接口测试）。
	- `eslint`：代码质量检查工具，强制统一代码风格，发现潜在错误（如未定义变量、错误缩进）。
	- ``prettier``：代码格式化工具，自动统一代码风格（如缩进、分号、引号），常与 ESLint 配合使用。

参考这个文件 [[node.js依赖包]]

### 1.3 项目结构

```
node-backend-tutorial/
├── .env                  # 环境变量
├── .eslintrc.json        # ESLint 配置
├── .prettierrc           # Prettier 配置
├── package.json
├── src/
│   ├── app.js            # Express 应用入口
│   ├── config/           # 配置文件
│   │   └── db.js         # 数据库连接
│   ├── controllers/      # 控制器
│   ├── models/           # 数据模型
│   ├── routes/           # 路由
│   ├── middlewares/      # 中间件
│   ├── services/         # 业务逻辑
│   ├── utils/            # 工具函数
│   └── tests/            # 测试文件
└── docs/                 # API 文档
    └── api-spec.md       # API 接口文档
```

[[一个 express 项目里面有哪些内容]]

## 2 . 编写 API 文档 (docs/api-spec. Md)
 [[API文档示例]]
根据对应功能，设计 RestfulAPI
实现基础的诸如用户注册，登录，注销账号，查找 id 等接口
这一步不用管函数的具体实现，只需要在意输入，输出是什么即可

