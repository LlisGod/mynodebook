---
tags:
  - 计算机/后端/springboot
---


MyBatis 是一个**持久层框架**，它定制了 SQL 通过 XML 或注解来进行配置与执行，并将 Java 对象和数据库中的记录互相映射。

简单来说，它介于你的 Java 应用程序和数据库之间，帮你处理那些繁琐的 JDBC 代码和结果集转换。

### 3. 两种主要的开发方式

你可以通过两种方式来告诉 MyBatis 要执行什么 SQL：

#### 方式一：XML 配置文件（传统方式，适合复杂 SQL）

这是最灵活、最强大的方式。SQL 和 Java 代码完全分离。

**UserMapper.java (接口)**
```java
public interface UserMapper {
    User selectUserById(int id);
}
```

**UserMapper.xml (映射文件)**


```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mapper.UserMapper">
    <!-- 
        id 对应接口的方法名
        resultType 指定返回结果要封装成什么类型的对象
    -->
    <select id="selectUserById" resultType="com.example.model.User">
        select id, username, password from user where id = #{id}
    </select>
</mapper>
```

使用时：


```java

try (SqlSession session = sessionFactory.openSession()) {
    UserMapper mapper = session.getMapper(UserMapper.class);
    User user = mapper.selectUserById(1);
    System.out.println(user);
}

```
#### 方式二：注解（适合简单 SQL）

直接把 SQL 写在接口的方法上。

```java
public interface UserMapper {
    @Select("SELECT * FROM user WHERE id = #{id}")
    User selectUserById(int id);
}
```

### 4. 核心功能特性

1. **参数映射 (`#{}` 和 `${}`)**：
    
    - `#{property}`：**预编译**方式。MyBatis 会将其替换成 `?`，然后使用 JDBC 的 `PreparedStatement` 设置参数，**可以防止 SQL 注入**。这是最常用的方式。
        
    - `${property}`：**字符串替换**。MyBatis 直接将该变量的值拼接到 SQL 语句中，**存在 SQL 注入风险**，一般只在动态传入表名或列名时使用。
        
2. **结果映射 (`ResultMap`)**：
    
    - 这是 MyBatis 的灵魂之一。当数据库列名和 Java 对象属性名不一致时，可以通过 `ResultMap` 来定义映射规则。

```xml
    <resultMap id="userResultMap" type="User">
        <!-- 主键用 id 标签 -->
        <id property="id" column="user_id" />
        <!-- 普通字段用 result 标签 -->
        <result property="userName" column="user_name"/>
        <result property="email" column="email_address"/>
    </resultMap>```
    <select id="selectAllUsers" resultMap="userResultMap">
        select user_id, user_name, email_address from user
    </select>
    
```
    
3. **动态 SQL**：
    
    - 这是 MyBatis 的另一大杀器。可以根据传入的参数条件，动态地拼接 SQL，帮你在 Java 代码里避免了写一堆 `if...else` 去拼接字符串。
        
    - **常用标签：** `<if>`, `<choose> (<when> , <otherwise>)`, `<where>`, `<set>`, `<foreach>`, `<trim>`。

 ```xml
    <select id="findActiveBlogWithTitleLike" resultType="Blog">
        SELECT * FROM blog
        <where>  <!-- where 标签会自动处理掉第一个条件多余的 AND 或 OR -->
            <if test="state != null">
                AND state = #{state}
            </if>
            <if test="title != null">
                AND title LIKE #{title}
            </if>
        </where>
    </select>
    <!-- 批量插入的 foreach 示例 -->
    ```
    <insert id="batchInsert">
        insert into user (id, name) values
        <foreach collection="list" item="item" separator=",">
            (#{item.id}, #{item.name})
        </foreach>
    </insert>
    
 ```
    
4. **缓存**：
    - **一级缓存**（默认开启）：作用在同一个 `SqlSession` 中。两次相同的查询，第二次会直接从缓存取。
        
    - **二级缓存**（需手动开启）：作用在 `Mapper` (namespace) 级别，跨 `SqlSession` 共享。