---
tags:
  - 计算机/技术原理
---


是 json 格式的数据，进行 base 64 编码后得到的字符串，形如

Fdsagaggdsxxxxxx.
Greewgrewxxxxxx. 
FdsueHUhbh_uxxxxxxxx

## 组成

3 个部分
- Header：
	- 加密方式（签名算法）
	- 令牌类型
- PayLoad：
	- 基础信息：签发时间，主题等
	- 自定义信息：用户名，有效时长等
- 签名
	- 使用加密算法，基于前两个部分以及你自定义的密钥生成（不是 base 64 编码）

### Java 中使用 jwt
使用 jjwt 依赖

Jwts. Bulider( ) 生成
Jwts. Parser( ) 校验


将上述操作包装为一个类 jwtUtils，就可以在其他接口使用