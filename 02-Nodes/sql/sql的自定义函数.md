---
tags:
  - 计算机/SQL
---


##  基本形式
Create Funcition 函数名称（参数数据类型）
Return 返回值类型
Begin
	逻辑部分
	Return 结果；
End；

## 示例 ：
 
 ```sql
 CREATE FUNCTION add_numbers(a INT, b INT)
 RETURNS INT
 BEGIN
 	DECLARE result INT;
 	SET result = a + b;
 	RETURN result;
 END;
 ```

## 语句逻辑

### 🧱 SQL 函数定义完整语法结构


```SQL
DELIMITER $$  -- 1. 修改结束符（MySQL需要）

CREATE FUNCTION 函数名(参数1 数据类型, 参数2 数据类型, ...)  -- 2. 函数声明
RETURNS 返回值类型  -- 3. 指定返回类型
[特性声明]  -- 4. 可选特性（如DETERMINISTIC）
BEGIN     -- 5. 函数体开始
    DECLARE 变量名 数据类型 [DEFAULT 默认值];  -- 6. 声明变量
    SET 变量名 = 值;  -- 7. 变量赋值
    -- 8. 业务逻辑（SQL语句/流程控制）
    RETURN 返回值;   -- 9. 返回结果
END$$     -- 10. 函数体结束

DELIMITER ;  -- 11. 恢复结束符
```

### 1. **`DELIMITER`** - 修改语句结束符（mysql 必要）

一般将结束符改为 `$$` ，在函数定义完成后改回来

为啥？因为正常写 sql 查询语句和函数定义语句都使用‘’；‘’
所以定义时要临时改写避免冲突

### 2.**`参数 参数类型`** -特殊的参数声明格式

Sql 规矩特殊，定义参数时要将数据类型放在数据名称后面
比如：num INT 
### 3. `DECLARE` - 声明变量

**结构**：`DECLARE 变量名 数据类型 [DEFAULT 默认值];`  
**特点**：

- 必须放在 `BEGIN` 后第一个非注释位置
    
- 可同时声明多个变量
    
- 默认值可选

### 4. 参数赋值
1. 直接赋值：`SET 变量 = 值；`
2. 查询赋值：`SELECT 列 INTO 变量 FROM……``


### 5. 控制语句

```sql
IF 条件 THEN 操作
	ELSEIF 操作 ELSE 操作
END IF;
```

```sql
CASE
	WHEN …… THEN……
	WHEN…… THEN……
	……
END CASE;
```

```sql
WHILE   DO
	……
END WHILE;
```

### 6 . RETURNS - 指定返回值类型
作用：定义函数返回值的类型
常见类型：INT, VARCHAR, DECIMAL, DATE, BOOLEAN 等
示例：

`RETURNS DECIMAL (10,2)  -- 返回带 2 位小数的十进制数`
参见： [[sql返回值类型]] 
### 特性声明是什么？
常用特性：

- ``DETERMINISTIC``：相同输入总是相同输出（如数学计算）

-  `NOT DETERMINISTIC`：结果可能变化（如包含 RAND ()）

-  `READS SQL DATA`：函数会读取数据但不修改

-  `MODIFIES SQL DATA`：函数会修改数据

特征声明类似部门工牌或者虎符，比如你看到雷部赦令，就知道人家要调用雷池而不能调用水火

其作用就是提高查询效率，可以不用，但不能没有