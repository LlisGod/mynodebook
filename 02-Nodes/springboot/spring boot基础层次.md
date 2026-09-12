---
tags:
  - 计算机/后端/springboot
---

# 控制层

**@Controller**：

和前端通信，暴露在外的接口，调用服务的方法
通过 requestMapping 之类注解实现对应 get，post 方法

# 服务层

**@Service**

主要业务逻辑
和数据访问层交换数据

# 数据访问层

**@Repository**（如果你使用的是 springdataJPA）
和数据库链接，实现基本的 crud 操作（其实只要加了注解，jpa 就可以实现几乎所有操作）

比如 ``findById(id)`` 等等

# 实体类

**@Entity**
定义数据类型
比如 User
各个字段
比如 username，id，update 等等

注解为字段，主键等等
使数据访问层建立和数据库表的映射

用于实现数据层和服务层之间的通信



```java
package com.example.demo.entity;

import javax.persistence.*;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    // 构造方法
    public User() {}
    
    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    // Getter和Setter
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```


# Dto

用于实现服务之间，服务与控制层的数据交换

因为有时候不需要所有字段数据，或者为了隐私，比如只传用户名和 id，不传密码
需要加上 dto 转化为实体的逻辑，使用框架，或者手动实现统一的转化方法

小型服务可以不需要 dto，但开发习惯加上，方便项目后续维护

