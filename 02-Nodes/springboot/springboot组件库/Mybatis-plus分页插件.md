---
tags:
  - 计算机/后端/springboot
---


由于版本更新，这个插件从 mp 中分离出来，需要单独引入

此时需要先再上面（原有的dependencies 外面）引入
```xml
<dependencyManagement>  
    <dependencies>        
    	<dependency>            
    		<groupId>com.baomidou</groupId>  
            <artifactId>mybatis-plus-bom</artifactId>  
            <version>3.5.15</version>  
            <type>pom</type>  
            <scope>import</scope>  
        </dependency>    
    </dependencies>
</dependencyManagement>
```
这是 mp 的版本管理依赖

然后引入
```xml
<dependency>  
    <groupId>com.baomidou</groupId>  
    <artifactId>mybatis-plus-jsqlparser</artifactId>  
</dependency>
```

由于引入了版本管理软件，指定了版本，这个依赖不用指定版本
包括先前引入的 mp 也可以不用指定依赖


最后，根据官网的配置就可以使用了

加入类似的配置文件

```java
@Configuration  
@MapperScan("com.ll.mymanager.mapper")  
public class MybatisPlusConfig {  
  
    /**  
     * 添加分页插件  
     */  
    @Bean  
    public MybatisPlusInterceptor mybatisPlusInterceptor() {  
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();  
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.POSTGRE_SQL)); // 如果配置多个插件, 切记分页最后添加  
        // 如果有多数据源可以不配具体类型, 否则都建议配上具体的 DbType        return interceptor;  
    }  
}
```



在 Service 中创建 Page 对象设置 pageNum 和 pageSize

Mapper 返回的参数为 Ipage 对象
包含查询总数和页面内容等信息
类似：
```java
@Override  
public IPage<Emp> page(Integer pageNum, Integer pageSize) {  
    Page<Emp> page = new Page<>(pageNum, pageSize);  
    IPage<Emp> emps = empMapper.page(page);  
    return emps;  
}
```