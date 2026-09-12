---
tags:
  - 计算机/后端/springboot
---
# SpringBoot 整合 Spring Cache + Redis 缓存（注解版）

## 一、核心原理

**Spring Cache** 是 Spring 提供的**抽象缓存层**，不绑定具体缓存组件，底层可以用 Redis、Ehcache、Caffeine 等。

我们搭配 Redis 做缓存：

- 只用**注解**就能实现缓存新增、查询、更新、删除
- 不用手动写 `RedisTemplate` 存取值
- 底层自动用 Redis 做持久化缓存

## 二、依赖（和之前 Redis 一样）

Xml

```xml
<!-- Spring Cache + Redis 起步依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```

## 三、配置 application. Yml

和之前 Redis 配置完全一样，不用额外加新配置：


```yaml
spring:
  redis:
    host: localhost
    port: 6379
    password:
    database: 0
    timeout: 10s
    lettuce:
      pool:
        max-active: 8
        max-idle: 8
        min-idle: 0
        max-wait: -1ms
```

## 四、开启缓存注解

启动类上加 **`@EnableCaching`**，也可以加在配置类上


```java
@SpringBootApplication
@EnableCaching // 开启Spring缓存注解
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## 五、依旧需要 Redis 序列化配置

就是刚才给你的 `RedisConfig` 配置类，**必须保留**：

- 统一 key、value 序列化
- 避免 Redis 里乱码、对象无法缓存

## ****六、Spring Cache 常用注解**



|注解|作用|
|---|---|
| `@Cacheable` |查询：有缓存读缓存，没缓存查数据库并放入缓存|
| `@CachePut` |更新 / 新增：**一定会执行方法**，并更新缓存|
| `@CacheEvict` |删除：清空指定缓存|
| `@Caching` |组合多个缓存注解|
| `@CacheConfig` |类上统一指定缓存名，简化写法|

---

## 七、实战用法（Service 层使用）

注意：不应该把注解加在控制层，那是及其不标准的

### 1. 单个根据 ID 查询缓存

Java

运行

```java
// value：缓存分区名，key：缓存唯一键
@Service
public class UserService {

    // 第一次查走数据库，之后直接走Redis缓存
    @Cacheable(value = "user", key = "#id")
    public User getUserById(Long id) {
        // 模拟查数据库
        System.out.println("走数据库查询id：" + id);
        return new User(id, "张三");
    }
}
```

- 第一次调用：打印日志、查库、结果存入 Redis
- 第二次同 id 调用：**不进方法**，直接从 Redis 返回

### 2. 新增 / 更新 `@CachePut`

Java

运行

```java
@CachePut(value = "user", key = "#user.id")
public User saveUser(User user) {
    // 新增/更新数据库
    return user;
}
```

- 一定会执行方法
- 执行完自动把结果更新到 Redis 缓存

### 3. 删除缓存 `@CacheEvict`

Java

运行

```java
// 删除单个缓存
@CacheEvict(value = "user", key = "#id")
public void deleteUser(Long id) {
    // 删除数据库数据
}

// 清空user下所有缓存
@CacheEvict(value = "user", allEntries = true)
public void clearUserCache() {
}
```

### 4. 类上统一配置 `@CacheConfig`

Java

运行

```java
@Service
@CacheConfig(cacheNames = "user") // 整个类默认缓存分区
public class UserService {

    @Cacheable(key = "#id")
    public User getUserById(Long id){...}
}
```

---

## 八、设置缓存过期时间

Spring Cache 默认**永久不过期**，需要手动配置 Redis 缓存管理器，统一设置过期时间。

### 改造 RedisConfig，加入 CacheManager

Java

运行

```java
@Configuration
public class RedisConfig {

    // 缓存管理器：统一设置过期时间
    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                // 设置默认缓存过期时间 30分钟
                .entryTtl(Duration.ofMinutes(30))
                // 关闭key前缀
                .disableKeyPrefix()
                // key序列化
                .serializeKeysWith(RedisSerializationContext.SerializationPair
                        .fromSerializer(new StringRedisSerializer()))
                // value序列化
                .serializeValuesWith(RedisSerializationContext.SerializationPair
                        .fromSerializer(new GenericJackson2JsonRedisSerializer()));

        return RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
    }

    // 你之前的RedisTemplate Bean 保留不变
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        // ... 之前代码不变
    }
}
```

---

## 九、关键点总结

1. 只要引入 `redis starter` + `@EnableCaching`，Spring Cache **自动底层适配 Redis**
2. 业务中**只用注解**，不用手动操作 RedisTemplate
3. `@Cacheable` 查缓存、`@CachePut` 改缓存、`@CacheEvict` 清缓存
4. 必须配序列化 + `CacheManager` 解决乱码和设置过期时间
5. 适合：**字典、用户信息、商品详情、接口列表** 等读多写少场景

## 十、和原生 RedisTemplate 对比

- Spring Cache：**开发快、注解式、无侵入**，适合通用业务缓存
- RedisTemplate：**灵活度高**，适合分布式锁、计数器、限流、复杂 Hash/List 操作

实际项目：**Spring Cache 做普通业务缓存 + RedisTemplate 做特殊高级操作** 搭配用。






# 精简标准 RedisConfig + springcache
给你一套**企业标准最简版**，直接复制就能用，适配 `RedisTemplate` + `SpringCache`，一键搞定序列化、乱码、缓存过期。


```java
import org.springframework.cache.CacheManager;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.time.Duration;

@Configuration
@EnableCaching
public class RedisConfig {

    // 1. 配置 RedisTemplate 序列化
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);

        StringRedisSerializer strSerializer = new StringRedisSerializer();
        GenericJackson2JsonRedisSerializer jsonSerializer = new GenericJackson2JsonRedisSerializer();

        // key、hashKey 用字符串序列化
        template.setKeySerializer(strSerializer);
        template.setHashKeySerializer(strSerializer);
        // value、hashValue 用JSON序列化
        template.setValueSerializer(jsonSerializer);
        template.setHashValueSerializer(jsonSerializer);

        template.afterPropertiesSet();
        return template;
    }

    // 2. 配置 SpringCache 缓存管理器，默认过期30分钟
    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisCacheConfiguration cfg = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofMinutes(30))       // 默认过期时间
                .serializeKeysWith(RedisCacheConfiguration
                        .SerializationPair.fromSerializer(new StringRedisSerializer()))
                .serializeValuesWith(RedisCacheConfiguration
                        .SerializationPair.fromSerializer(new GenericJackson2JsonRedisSerializer()))
                .disableCachingNullValues();            // 不缓存null值

        return RedisCacheManager.builder(factory)
                .cacheDefaults(cfg)
                .build();
    }
}
```

## 配套说明

1. 启动类不用再加 `@EnableCaching`，配置类已经加了；
2. 自动兼容：**普通 RedisTemplate 操作 + SpringCache 注解缓存**；
3. Key 正常字符串、Value 标准 JSON，可视化工具直接看懂；
4. 默认缓存 30 分钟过期，全局统一控制；
5. 禁止缓存 `null`，避免无用缓存占空间。

## 想用自定义过期时间怎么搞？

在注解上单独指定，覆盖全局默认