---
tags:
  - 计算机/SQL
---

# SQL 值类型详解

SQL 函数可以返回多种类型的数据，理解这些类型对于编写高效可靠的函数至关重要。下面我将全面介绍 SQL 函数中可用的返回值类型：

## 一、基础数据类型

### 1. 数值类型
| 类型             | 描述                   | 示例                  | 使用场景                     |
|------------------|------------------------|-----------------------|-----------------------------|
| `INT`            | 整数                   | `RETURNS INT`         | 计数、ID、状态码            |
| `DECIMAL(p,s)`   | 精确小数               | `RETURNS DECIMAL(10,2)` | 金额、精确计算              |
| `FLOAT`          | 近似浮点数             | `RETURNS FLOAT`       | 科学计算、不需要精确的场景  |
| `NUMERIC(p,s)`   | 等同于 DECIMAL         | `RETURNS NUMERIC(8,4)` | 高精度计算                  |
| `TINYINT`        | 小整数（-128 到 127） | `RETURNS TINYINT`     | 布尔值、小范围状态码        |
| `BIGINT`         | 大整数                 | `RETURNS BIGINT`      | 大数量计数                  |

**数值类型示例：**
```sql
CREATE FUNCTION calculate_tax(amount DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
BEGIN
    RETURN amount * 0.15; -- 计算15%的税
END;
```

### 2. 字符串类型
| 类型             | 描述                   | 示例                  | 使用场景                     |
|------------------|------------------------|-----------------------|-----------------------------|
| `VARCHAR(n)`     | 可变长度字符串         | `RETURNS VARCHAR(100)` | 名称、描述、短文本          |
| `CHAR(n)`        | 固定长度字符串         | `RETURNS CHAR(10)`    | 代码、固定格式值            |
| `TEXT`           | 长文本                 | `RETURNS TEXT`        | 大段文本、描述              |
| `NVARCHAR(n)`    | Unicode 可变字符串     | `RETURNS NVARCHAR(50)` | 多语言支持                  |

**字符串类型示例：**
```sql
CREATE FUNCTION format_phone(phone VARCHAR(20)) 
RETURNS VARCHAR(20)
BEGIN
    RETURN CONCAT('(', SUBSTRING(phone,1,3), ') ', SUBSTRING(phone,4,3), '-', SUBSTRING(phone,7,4));
END;
```

### 3. 日期时间类型
| 类型             | 描述                   | 示例                  | 使用场景                     |
|------------------|------------------------|-----------------------|-----------------------------|
| `DATE`           | 日期                   | `RETURNS DATE`        | 生日、事件日期              |
| `TIME`           | 时间                   | `RETURNS TIME`        | 特定时间点                  |
| `DATETIME`       | 日期和时间             | `RETURNS DATETIME`    | 时间戳、完整时间            |
| `TIMESTAMP`      | 时间戳                 | `RETURNS TIMESTAMP`   | 记录修改时间                |
| `YEAR`           | 年份                   | `RETURNS YEAR`        | 仅需要年份的场景            |

**日期类型示例：**
```sql
CREATE FUNCTION get_next_monday() 
RETURNS DATE
BEGIN
    DECLARE today DATE;
    SET today = CURDATE();
    
    -- 计算下一个周一
    RETURN CASE 
        WHEN WEEKDAY(today) = 0 THEN today + INTERVAL 7 DAY
        ELSE today + INTERVAL (7 - WEEKDAY(today)) DAY
    END;
END;
```

### 4. 布尔类型
| 类型             | 描述                   | 示例                  | 使用场景                     |
|------------------|------------------------|-----------------------|-----------------------------|
| `BOOLEAN`        | 真/假值                | `RETURNS BOOLEAN`     | 状态检查、条件判断          |
| `BOOL`           | 等同于 BOOLEAN         | `RETURNS BOOL`        |                             |

**布尔类型示例：**
```sql
CREATE FUNCTION is_valid_email(email VARCHAR(255)) 
RETURNS BOOLEAN
BEGIN
    RETURN email REGEXP '^[A-Z0-9._%-]+@[A-Z0-9.-]+\\.[A-Z]{2,4}$';
END;
```

## 二、复杂数据类型

### 1. 表类型 (TABLE)
**描述**：返回整个结果集（多行多列）  
**支持数据库**：SQL Server, PostgreSQL  
**特点**：函数可以像表一样在 FROM 子句中使用

```sql
-- SQL Server 示例
CREATE FUNCTION get_high_value_products(@min_price DECIMAL(10,2))
RETURNS TABLE
AS
RETURN (
    SELECT product_id, product_name, price
    FROM products
    WHERE price > @min_price
);
```

使用方式：
```sql
SELECT * FROM get_high_value_products(100.00);
```

### 2. 游标类型 (CURSOR)
**描述**：返回可遍历的结果集指针  
**使用场景**：需要逐行处理结果集  
**注意**：调用程序需要知道如何处理游标

```sql
-- SQL Server 示例
CREATE FUNCTION get_customer_orders(customer_id INT)
RETURNS CURSOR
AS
BEGIN
    DECLARE order_cursor CURSOR FOR
        SELECT order_id, order_date, total_amount
        FROM orders
        WHERE customer_id = customer_id;
    
    OPEN order_cursor;
    RETURN order_cursor;
END;
```

### 3. JSON/XML 类型
**描述**：返回结构化数据格式  
**支持数据库**：MySQL 5.7+, PostgreSQL, SQL Server 2016+

**JSON 示例 (MySQL):**
```sql
CREATE FUNCTION get_product_info(product_id INT)
RETURNS JSON
BEGIN
    RETURN (
        SELECT JSON_OBJECT(
            'id', p.id,
            'name', p.name,
            'price', p.price,
            'category', c.name
        )
        FROM products p
        JOIN categories c ON p.category_id = c.id
        WHERE p.id = product_id
    );
END;
```

## 三、特殊返回类型

### 1. VOID / 无返回值
**描述**：某些数据库允许函数不返回值  
**使用场景**：执行操作但不需返回结果（通常使用存储过程更好）

```sql
-- PostgreSQL 示例
CREATE FUNCTION log_activity(user_id INT, action TEXT)
RETURNS VOID
AS $$
BEGIN
    INSERT INTO activity_log(user_id, action, timestamp)
    VALUES (user_id, action, NOW());
END;
$$ LANGUAGE plpgsql;
```

### 2. 自定义类型
**描述**：使用用户定义的类型作为返回值  
**支持数据库**：PostgreSQL, Oracle

```sql
-- PostgreSQL 示例
CREATE TYPE address_type AS (
    street VARCHAR(100),
    city VARCHAR(50),
    zip_code VARCHAR(20)
);

CREATE FUNCTION get_customer_address(customer_id INT)
RETURNS address_type
AS $$
BEGIN
    RETURN (
        SELECT street, city, zip_code
        FROM addresses
        WHERE customer_id = customer_id
    );
END;
$$ LANGUAGE plpgsql;
```

## 四、返回值类型选择指南

1. **考虑数据特性**：
   - 精确计算 → `DECIMAL`
   - 日期操作 → `DATE` / `DATETIME`
   - 状态检查 → `BOOLEAN`

2. **性能考量**：
   - 小数据集 → 基本类型
   - 大数据集 → TABLE 类型

3. **兼容性**：
   - 跨数据库 → 使用通用类型（INT, VARCHAR, DATE）
   - 特定数据库 → 使用高级特性（JSON, TABLE）

4. **错误处理**：
   - 可能返回 NULL 时，在文档中明确说明
   - 使用 `RETURN NULL` 处理错误情况

```sql
CREATE FUNCTION safe_divide(a INT, b INT)
RETURNS DECIMAL(10,4)
BEGIN
    IF b = 0 THEN
        RETURN NULL; -- 避免除零错误
    END IF;
    
    RETURN CAST(a AS DECIMAL) / b;
END;
```

## 五、最佳实践

1. **类型一致性**：确保 RETURN 语句返回的值类型与声明完全匹配
2. **适当精度**：为数值类型指定足够的精度和小数位
3. **长度限制**：为字符串类型设置合理的最大长度
4. **NULL 处理**：明确函数是否可能返回 NULL
5. **文档说明**：注释中说明返回值的含义和格式

```sql
/**
 * 计算员工奖金
 * @param salary 基本工资
 * @param performance_rating 绩效评分 (1-5)
 * @returns DECIMAL(10,2) 奖金金额，可能为NULL
 */
CREATE FUNCTION calculate_bonus(salary DECIMAL(10,2), performance_rating INT)
RETURNS DECIMAL(10,2)
BEGIN
    IF performance_rating NOT BETWEEN 1 AND 5 THEN
        RETURN NULL; -- 无效输入
    END IF;
    
    RETURN salary * (0.05 * performance_rating);
END;
```

理解并正确使用这些返回值类型，将使你的 SQL 函数更加强大、灵活且可靠。