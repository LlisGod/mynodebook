---
tags:
  - 计算机/后端/springboot
---


- 可用把对资源的请求拦截，进行一些通用操作，如：登录校验，统一编码处理，敏感字符处理等
- ![[filter功能演示.png]]


定义 Filter 类

实现三个方法
- Init
- Dofilter
- Destroy

配置
加上@WebFilter (urlPatterns = "/\*")注解，配置拦截路径

在启动类上加上
@ServletComponentScan 注解