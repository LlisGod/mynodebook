---
tags:
  - 计算机/后端/springboot
---
PageHelper 是第三方提供的 mybatis 分页查询的插件

![[PageHelper-1.png]]

使用PageHelper
![[PageHelper-2.png]]
主要作用是简化分页查询的操作

在xml 文件中引入依赖
->在mapper 层定义对应查询方法（无需考虑分页）
->在service 层中使用PageHelper


如果要使用count 等功能
可以将返回的List 强行转为page 对象
```Java
Page<Emp> p = (Page<Emp>) empList;
//就可以使用各种方法如
long count = p.getTotal();

```



## 实现机制

拦截原始的sql 语句，分别执行以下两句
- 自动执行count(0) 查询，计算原始查询的条数，删除不必要的排序操作（order by）
- 然后添加limit 执行分页查询

相当于一个中间层/拦截器，隐藏了重复的代码工作


## 注意

- 使用PageHelper 则定义的sql 语句末尾不能添加分号（;）
- 使用的PageHelper 只会对其后第一个查询生效，如果多次调用Mapper 里的方法，只有第一次生效



# 白雪警告

Mybatis-plus 内置一个分页插件，这意味着使用 mybatis-plus 是更好更新的选择