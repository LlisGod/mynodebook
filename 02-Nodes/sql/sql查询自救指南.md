---
tags:
  - 计算机/SQL
  - 计算机/后端
---
# 提纲

- SQL 语句是什么
- SQL 要做什么
- 有哪些基本的查询模式
- 函数？
- 组织一句 SQL 语句的顺序
- 视窗是什么？
- 好几个表，好几个筛选条件，怎么办？


# 内容

## 执行顺序
- From （从哪个表查）
- JOIN - 链接相关表
- Where - 行数据过滤
- GROUP BY - 分组数据
- HAVING - 过滤分组
-  SELECT - 选择列
- DISTINIC - 去重
- ORDER BY - 排序
- LIMIT/OFFSET - 限制返回行数

## 书写顺序
```SQL
SELECT [DISTINIC] 要查的列
FROM 基础表
[JOIN 关联表 ON 关联条件]
WHERE 行筛选条件
GROUP BY 分组列（比如班级，性别）
HAVING 分组过滤（比如高二以上）
ORDER BY 排序依据的列 [DESC(倒序)]
LIMIT 限制条数
```

- 嵌套查找就是子查找的意思