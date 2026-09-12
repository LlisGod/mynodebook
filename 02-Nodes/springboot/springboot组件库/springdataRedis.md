---
tags:
  - 计算机/后端/springboot
---
# 介绍

是在 springboot 应用中操作 redis 的组件

# 使用

1. 启动 redis
2. 引入redis 核心依赖
```xml
<!-- SpringBoot Redis 核心依赖 --> 
<dependency> 
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId> 
</dependency> 
<!-- 连接池依赖（必须加，否则会报错） --> 
<dependency> 
	<groupId>org.apache.commons</groupId> 
	<artifactId>commons-pool2</artifactId> 
</dependency>
```
3. 配置 redis 链接信息，类似链接数据库
4. 创建 redis 配置类
	- 可以加入自定义的 redis 序列化配置，让 key 不是乱码，方便在 redis可视化工具中查看，防止程序错误，反正是固定代码，别嫌麻烦
```java
@Configuration 
public class RedisConfig {
	@Bean 
	public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) { 
		RedisTemplate<String, Object> template = new RedisTemplate<>(); 
		template.setConnectionFactory(factory); 
		// 序列化配置 
		StringRedisSerializer stringSerializer = new StringRedisSerializer(); 
		Jackson2JsonRedisSerializer<Object> jacksonSerializer = new Jackson2JsonRedisSerializer<>(Object.class); 
		ObjectMapper om = new ObjectMapper(); 
		om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY); 
		om.activateDefaultTyping(om.getPolymorphicTypeValidator(), ObjectMapper.DefaultTyping.NON_FINAL); jacksonSerializer.setObjectMapper(om);
		 // key 采用 String 序列化 
		 template.setKeySerializer(stringSerializer);
		// hash key 采用 String 序列化 
		template.setHashKeySerializer(stringSerializer); 
		// value 采用 JSON 序列化 
		template.setValueSerializer(jacksonSerializer); 
		// hash value 采用 JSON 序列化 
		template.setHashValueSerializer(jacksonSerializer); 
		template.afterPropertiesSet(); return template; 
		} 
	}
```


## 五、第四步：实战使用（最常用操作）

直接注入 `RedisTemplate` 即可操作 Redis，我把**开发中 90% 的常用方法**整理好了。

### 1. 基础：注入 RedisTemplate



```java
@RestController
@RequestMapping("/redis")
public class RedisController {

    // 直接注入
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;
}
```

### 2. 字符串操作（最常用）


```java
// 1. 存值（永久有效）
redisTemplate.opsForValue().set("name", "张三");

// 2. 存值（设置过期时间：10分钟）
redisTemplate.opsForValue().set("phone", "13800138000", 10, TimeUnit.MINUTES);

// 3. 取值
String name = (String) redisTemplate.opsForValue().get("name");

// 4. 删除key
redisTemplate.delete("name");

// 5. 判断key是否存在
Boolean hasKey = redisTemplate.hasKey("name");
```

### 3. 存储 Java 对象（自动序列化）


```java
// 定义实体类
@Data
@AllArgsConstructor
public class User implements Serializable {
    private Long id;
    private String username;
}

// 存储对象
redisTemplate.opsForValue().set("user:1", new User(1L, "李四"));

// 获取对象
User user = (User) redisTemplate.opsForValue().get("user:1");
```

### 4. 哈希操作（Hash）

适合存储对象、配置信息：



```java
// 存
redisTemplate.opsForHash().put("user:2", "name", "王五");
redisTemplate.opsForHash().put("user:2", "age", 20);

// 取
String name = (String) redisTemplate.opsForHash().get("user:2", "name");

// 获取整个hash
Map<Object, Object> map = redisTemplate.opsForHash().entries("user:2");
```

### 5. 列表操作（List）



```java
// 左推
redisTemplate.opsForList().leftPush("list", "a");
// 右推
redisTemplate.opsForList().rightPush("list", "b");
// 获取列表
List<Object> list = redisTemplate.opsForList().range("list", 0, -1);
```

---

## 六、封装 Redis 工具类（推荐）

实际开发中，我们会封装工具类简化调用，不用每次写 `opsForValue()`：

### RedisUtil. Java（直接复制用）



```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import java.util.concurrent.TimeUnit;

@Component
public class RedisUtil {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    // 存值
    public void set(String key, Object value) {
        redisTemplate.opsForValue().set(key, value);
    }

    // 存值+过期时间
    public void set(String key, Object value, long time) {
        redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
    }

    // 取值
    public Object get(String key) {
        return redisTemplate.opsForValue().get(key);
    }

    // 删除
    public boolean delete(String key) {
        return Boolean.TRUE.equals(redisTemplate.delete(key));
    }

    // 判断是否存在
    public boolean hasKey(String key) {
        return Boolean.TRUE.equals(redisTemplate.hasKey(key));
    }
}
```

### 使用工具类


```java
@Autowired
private RedisUtil redisUtil;

redisUtil.set("name", "工具类测试");
String name = (String) redisUtil.get("name");
```

---

## 七、常见应用场景

1. **缓存数据**：减轻数据库压力（用户信息、商品信息）
2. **分布式锁**：解决集群下并发问题
3. **计数器**：点赞数、浏览量、订单数
4. **消息队列**：简单队列使用 List 结构
5. **Session 共享**：分布式系统登录状态同步
6. **限流**：接口访问频率限制

---

## 八、常见问题解决

1. **连接超时**：检查 Redis 服务是否启动、端口是否开放、防火墙是否拦截
2. **乱码**：必须配置我上面的 `RedisConfig`
3. **空指针**：确保 Redis 注入成功，不要手动 new 对象
4. **认证失败**：检查 Redis 密码配置是否正确