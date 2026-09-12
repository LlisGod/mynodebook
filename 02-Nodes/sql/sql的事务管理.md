---
tags:
  - 计算机/SQL
---


对于两条一起执行的 sql 指令，比如添加员工信息和员工工作信息两条 sql 语句。如果第一个执行失败第二个执行成功。那么此时添加的员工工作信息就是脏数据。为了让两次操作步调一致就要用到[[事务管理]]，特别是事务的原子性这个特点

那么再 sql 中怎么实现事务管理呢？

SQL 的事务管理主要通过 **事务控制语句** 来实现，确保一组操作要么全部成功，要么全部失败，保证数据的一致性和完整性。

```sql
-- 开始事务
START TRANSACTION;  -- 或 BEGIN;

-- 执行一系列SQL操作
INSERT INTO orders ...;
UPDATE products SET stock = stock - 1 WHERE id = ...;
INSERT INTO order_items ...;

-- 提交事务（确认所有修改）
COMMIT;

-- 回滚事务（撤销所有修改）
ROLLBACK;
```

