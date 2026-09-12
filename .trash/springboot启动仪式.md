---
tags:
  - 计算机/后端/springboot
---

# Spring Boot 项目启动 Checklist

一个标准的，易于维护的项目，应该具有清晰明确的结构
这是一种较为合适的，清晰的项目构建指南

## 📁 项目结构文件树
```
src/main/java/com/yourproject/
├── Application.java                # 启动类
├── common/                         # 通用组件
│   ├── response/                   # 响应体
│   │   ├── ApiResponse.java
│   │   └── PageResponse.java
│   ├── constant/                   # 常量
│   │   └── ResponseCode.java
│   └── util/                       # 工具类
├── config/                         # 配置类
│   ├── WebConfig.java              # 跨域配置
│   └── DatabaseConfig.java         # 数据库配置
├── controller/                     # 控制层
├── service/                        # 服务层
│   └── impl/                       # 服务实现类
├── repository/                     # 数据访问层
├── entity/                         # 实体类
├── dto/                            # 数据传输对象
│   ├── request/                    # 请求DTO
│   └── response/                   # 响应DTO
├── exception/                      # 异常处理
└── enums/                          # 枚举类（可选）

src/main/resources/
├── application.yml                 # 主配置文件
├── application-dev.yml             # 开发环境
└── application-prod.yml            # 生产环境
```

## ✅ 启动 Checklist

### 1. **项目初始化**
- [ ] 创建 Spring Boot 项目，选择必要依赖（Web, JPA, MySQL 等）
- [ ] 规划并创建基础包结构（MVC 三层结构）

### 2. **通用组件搭建**
- [ ] 创建 `common/response` 包，定义 `ApiResponse` 标准响应体
- [ ] 创建 `common/constant` 包，定义状态码枚举（可选但推荐）
- [ ] 创建 `common/util` 包，预留工具类位置

### 3. **配置类设置**
- [ ] 创建 `config` 包，添加跨域配置 `WebConfig`
- [ ] 按需添加数据库、安全等配置类
- [ ] 配置多环境配置文件（dev/test/prod）

### 4. **异常处理机制**
- [ ] 创建 `exception` 包，定义业务异常 `BusinessException`
- [ ] 创建全局异常处理器 `GlobalExceptionHandler`
- [ ] 完善参数验证异常处理

### 5. **数据层设计**
- [ ] 创建 `entity` 包，定义 JPA 实体类
- [ ] 创建 `repository` 包，定义数据访问接口
- [ ] 配置数据库连接和 JPA 参数

### 6. **业务逻辑层**
- [ ] 创建 `service` 包和 `service/impl` 包
- [ ] 定义服务接口和实现类
- [ ] 注意业务异常的正确抛出
	- 创建 `service` 包，定义**服务接口**
    
	- 创建`service/impl`包，编写**具体实现类**
    
	- 在实现类上添加 `@Service` 注解
    
	- Controller中**依赖接口**而非具体实现类


### 7. **数据传输对象**
- [ ] 创建 `dto/request` 包，定义请求 DTO（带参数验证）
- [ ] 创建 `dto/response` 包，定义业务响应 DTO
- [ ] 实现 DTO 与 Entity 的转换方法（手动/Mapper）

### 8. **控制层实现**
- [ ] 创建 `controller` 包，编写 API 接口
- [ ] 使用标准响应体返回结果
- [ ] 添加参数验证注解

### 9. **配置完善**
- [ ] 配置数据库连接信息
- [ ] 配置 JPA 相关参数
- [ ] 配置跨域白名单（前端地址）
- [ ] 配置日志输出格式

## 🎯 关键注意点

1. **包职责清晰** - 每个包有明确单一职责
2. **异常统一处理** - 避免在 Controller 中 try-catch
3. **响应格式统一** - 所有接口返回 `ApiResponse`
4. **参数验证** - 在 DTO 中使用 Validation 注解
5. **配置分离** - 不同环境使用不同配置
6. **安全考虑** - 敏感信息不返回给前端

## 🚀 快速验证
启动后测试一个简单接口，确认：
- 响应格式符合 `ApiResponse` 标准
- 异常处理正常工作
- 跨域配置生效
- 数据库连接正常

按照这个 Checklist，你就能构建一个规范、易维护的企业级 Spring Boot 项目！