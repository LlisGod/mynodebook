---
tags:
  - 计算机/后端/springboot
---
作为一个辅助组件，lombok 让你免去繁琐的工作，只需要使用诸如@data 等注解就可以自动实现 getter，setter 方法，全参构造无参构造等等。搭配 idea 的快速匹配，可以大大节省时间

### @Builder 注解

使用此注解可以舍弃繁琐的 setter 构造或者 new，使用如下优雅的链式调用
```java
//方案一
User user = new User("张三", "zhangsan@example.com", 25, "北京市", "123456");

//方案二
User user = new User();
user.setName("张三");
user.setEmail("zhangsan@example.com");
user.setAge(25);
user.setAddress("北京市");
user.setPhone("123456");

//优雅!!!
User user = User.builder()
        .name("张三")
        .email("zhangsan@example.com")
        .age(25)
        .address("北京市")
        .phone("123456")
        .build();
```