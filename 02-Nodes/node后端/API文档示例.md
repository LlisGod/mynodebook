---
tags:
  - 计算机/后端/node
---

**示例**

# 用户管理 API 文档

## 基础信息
- 基础路径: `/api/v1`
- 认证: Bearer Token (JWT)

## 用户相关接口

### 1. 注册用户

**URL**: `/users/register`

**Method**: `POST`

**Request Body**:
```json
{
  "username": "string, 必填, 唯一, 3-20字符",
  "email": "string, 必填, 有效邮箱格式",
  "password": "string, 必填, 最少6字符"
}
```

**Success Response**:
```json
{
  "success": true,
  "data": {
    "user": {
      "_id": "string",
      "username": "string",
      "email": "string",
      "createdAt": "ISO 日期字符串"
    },
    "token": "JWT 令牌"
  }
}
```
### 2. 用户登录

**URL**: `/users/login`

**Method**: `POST`

**Request Body**:
```json
{
  "email": "string, 必填",
  "password": "string, 必填"
}
```

**Success Response**:

```Json
{
  "success": true,
  "data": {
    "user": {
      "_id": "string",
      "username": "string",
      "email": "string"
    },
    "token": "JWT 令牌"
  }
}
```

### 错误响应

**所有错误响应格式**:

```Json

{
  "success": false,
  "error": {
    "code": "错误代码",
    "message": "错误描述"
  }
}
```

- **常见错误代码**:
 400: 请求参数错误
 
 401: 未授权
 
 403: 禁止访问
 
 404: 资源不存在
 
 500: 服务器内部错误

  