---
tags:
  - 计算机/后端/springboot
---
# RabbitMQ 完整学习笔记（基于费曼学习法对话整理）

> 本笔记整理自一次深入的技术对话，从零开始讲解消息队列，涵盖基础概念、Spring Boot 集成、可靠性保障、高可用架构、工程实践及秒杀场景设计。

---

## 一、消息队列是什么？

**一句话**：生产者把消息放进队列，消费者从队列里取，双方不需要同时在线。

**三大价值**：
- **异步**：发完消息立即返回，不用等处理结果
- **解耦**：生产者和消费者不直接依赖，可独立变化
- **削峰填谷**：突发流量先暂存队列，消费者平稳处理，保护后端系统

**常见产品**：RabbitMQ、Kafka、RocketMQ、ActiveMQ

---

## 二、RabbitMQ 核心概念（费曼式理解）

| 概念 | 比喻 |
|------|------|
| 生产者 | 厨师（做菜） |
| 队列 | 出菜窗口（暂存菜） |
| 消费者 | 客人（取菜） |
| 交换机 | 分菜员（决定菜送到哪个窗口） |

**工作流程**：
```
生产者 → 交换机 → 队列 → 消费者
```

---

## 三、交换机类型

| 类型 | 行为 | 适用场景 |
|------|------|----------|
| Direct | 点对点，按 routing key 精确匹配 | 单播任务 |
| Fanout | 广播，发给所有绑定的队列 | 多系统同时需要同一消息 |
| Topic | 按通配符匹配（如 `order.*`） | 灵活路由 |
| Headers | 按消息头属性匹配 | 少见 |

---

## 四、Spring Boot 集成 RabbitMQ

### 4.1 依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

### 4.2 配置（application. Yml）
```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
```

### 4.3 配置队列与交换机（示例：Fanout）
```java
@Configuration
public class RabbitConfig {
    @Bean
    public FanoutExchange fanoutExchange() {
        return new FanoutExchange("my.fanout");
    }
    @Bean
    public Queue smsQueue() {
        return new Queue("sms.queue");
    }
    @Bean
    public Queue emailQueue() {
        return new Queue("email.queue");
    }
    @Bean
    public Binding smsBinding(Queue smsQueue, FanoutExchange fanoutExchange) {
        return BindingBuilder.bind(smsQueue).to(fanoutExchange);
    }
    @Bean
    public Binding emailBinding(Queue emailQueue, FanoutExchange fanoutExchange) {
        return BindingBuilder.bind(emailQueue).to(fanoutExchange);
    }
}
```

### 4.4 生产者（放在 Service 层）
```java
@Service
public class OrderService {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    @Transactional
    public void createOrder(OrderDTO dto) {
        // 1. 写数据库
        orderMapper.insert(dto);
        // 2. 成功后发 MQ
        rabbitTemplate.convertAndSend("my.fanout", "", dto);
    }
}
```

### 4.5 消费者（放在 listener 包，调用 Service）
```java
@Component
public class SmsListener {
    @Autowired
    private SmsService smsService;

    @RabbitListener(queues = "sms.queue")
    public void handle(OrderDTO dto) {
        smsService.send(dto.getUserPhone());
    }
}
```

**架构分层总结**：
- **Controller**：HTTP 入口，不碰 MQ
- **Service**：业务逻辑，生产消息或消费消息后调用
- **Listener**：独立包，专门消费 MQ 消息，调用 Service
- **DAO**：只做数据库操作

---

## 五、可靠性保障（消息不丢、不重复）

### 5.1 手动 ACK（防丢失）
```java
@RabbitListener(queues = "sms.queue")
public void handle(String msg, Channel channel, 
                   @Header(AmqpHeaders.DELIVERY_TAG) long tag) {
    try {
        doBusiness(msg);
        channel.basicAck(tag, false);   // 成功确认
    } catch (Exception e) {
        channel.basicNack(tag, false, true);  // 失败并重新入队
    }
}
```

### 5.2 重试 + 死信队列（防无限重试）
配置队列：
```java
@Bean
public Queue normalQueue() {
    return QueueBuilder.durable("normal.queue")
            .deadLetterExchange("dead.exchange")
            .deadLetterRoutingKey("dead.routing")
            .build();
}
```
消费者中判断重试次数（利用 `x-delivery-count` 头），超过次数则 `basicNack(tag, false, false)` 送入死信队列。

### 5.3 幂等性（防重复消费）

**方案一：唯一消息 ID + Redis**
```java
String messageId = msg.getMessageProperties().getMessageId();
if (redisTemplate.hasKey("processed:" + messageId)) return;
// 处理业务
redisTemplate.opsForValue().set("processed:" + messageId, "1", 1, TimeUnit.HOURS);
```

**方案二：数据库唯一键**
```sql
CREATE TABLE message_consumer_log (
    message_id VARCHAR(64) PRIMARY KEY,
    ...
);
```

**方案三：业务状态检查**（如订单状态已是“已支付”则跳过）

---

## 六、延时队列（TTL + 死信）

**场景**：下单 30 分钟未支付自动取消。

**原理**：消息进入一个设置了 TTL（存活时间）的队列，过期后自动转入死信队列，消费者监听死信队列。

```java
@Bean
public Queue waitingQueue() {
    return QueueBuilder.durable("waiting.queue")
            .ttl(30 * 60 * 1000)      // 30分钟
            .deadLetterExchange("real.exchange")
            .deadLetterRoutingKey("real.key")
            .build();
}
@Bean
public Queue realQueue() { return new Queue("real.queue"); }
```
生产者发消息到 `waiting.queue`，消费者监听 `real.queue`。

---

## 七、高可用：镜像集群

**问题**：单机 RabbitMQ 挂掉，全部服务不可用。

**解决方案**：镜像集群（数据在多个节点复制）。

```bash
# 设置策略：所有队列在所有节点上镜像
rabbitmqctl set_policy ha-all "^.*" '{"ha-mode":"all"}'
```

Spring Boot 客户端配置多个地址：
```yaml
spring:
  rabbitmq:
    addresses: 192.168.1.1:5672,192.168.1.2:5672,192.168.1.3:5672
```
自动故障转移。

**注意**：镜像集群是复制（备份），不是负载均衡（分摊存储）。

---

## 八、生产者与消费者的返回值问题

**核心**：MQ 是单向异步，生产者不等待消费者结果。

- **不需要返回值**：发短信、记日志、更新缓存等（80%场景）
- **需要返回值**（如减库存结果）：  
  - 方案 A：使用 **RPC over MQ**（请求-响应模式，不推荐）  
  - 方案 B：**消费者回调 HTTP 接口**（最终一致性 + 补偿查询）  
  - 方案 C：**定期对账**（凌晨扫表修复）

**原则**：如果必须同步等待结果，不要用 MQ，改用 HTTP/RPC。

---

## 九、失败处理策略

**原则**：生产者只保证消息成功送达 MQ，不保证消费成功。消费失败通过内部重试、死信、监控、人工处理解决，一般不反向通知生产者。

**消费者内部重试配置**（application. Yml）：
```yaml
spring:
  rabbitmq:
    listener:
      simple:
        retry:
          enabled: true
          max-attempts: 3
          initial-interval: 1000
          multiplier: 2
```

**代码中区分临时失败与永久失败**：
```java
try {
    smsService.send(msg);
} catch (TemporaryException e) {
    throw new AmqpRejectAndDontRequeueException(false); // 触发重试
} catch (PermanentException e) {
    throw new AmqpRejectAndDontRequeueException(true); // 直接死信
}
```

---

## 十、秒杀场景：Redis + MQ 联动

### 10.1 流程
```
用户秒杀 → Redis 预减库存（原子 decrement）
   ├─ 失败 → 返回“已抢光”
   └─ 成功 → 发送 MQ 消息 → 立即返回“排队中”
                  ↓
            MQ 消费者（异步）
                  ├─ 扣数据库库存
                  ├─ 创建订单
                  └─ 结果通知用户（WebSocket/轮询）
```

### 10.2 关键问题：Redis 预减成功但数据库扣减失败怎么办？

**方案一：失败补偿表（最常用）**  
消费者失败时写入补偿表，定时任务重试，凌晨对账修复。  
*结果*：可能少卖几件（Redis 多扣了但数据库没扣），可接受。

**方案二：回滚消息**  
消费者失败后发一条回滚消息到另一个队列，回滚 Redis 库存。  
需处理回滚失败的情况。

**方案三：不用 Redis 预减**  
所有请求直接进 MQ 排队，消费者串行写库（适合库存极少场景，但体验差）。

### 10.3 善后总结
- 每个组件（Redis、MQ、DB）都有备份或降级方案
- 最终一致性通过补偿 + 对账保证
- 用户只看到“排队中” → 最终成功/失败

---

## 十一、何时使用 MQ？

| 场景        | 是否需要 MQ | 原因              |
| --------- | ------- | --------------- |
| 注册后发短信    | ✅ 需要    | 解耦 + 异步，用户不等待   |
| 订单支付后更新积分 | ✅ 需要    | 异步执行，不阻塞支付完成    |
| 用户修改昵称    | ❌ 不需要   | 简单同步写库即可，无附属长任务 |
| 秒杀下单      | ✅ 需要    | 削峰填谷，保护数据库      |
| 上传头像生成缩略图 | ✅ 需要    | 异步处理，避免用户等待     |

**结论**：MQ 不仅用于高并发削峰，更常用于常规异步解耦。

---

## 十二、架构分层速查

| 层 | 职责 | 是否发 MQ | 是否消费 MQ |
|----|------|----------|------------|
| Controller | 接收 HTTP，参数校验 | ❌ | ❌ |
| Service | 业务逻辑，事务管理 | ✅（先写库再发） | ❌（但可被 Listener 调用） |
| Listener | 专门消费 MQ 消息 | ❌ | ✅（调用 Service） |
| DAO | 数据库 CRUD | ❌ | ❌ |

**重要**：核心业务（增删改）**必须先写数据库，事务提交后再发 MQ**，不能先发 MQ。

---

## 十三、学习总结

通过本次对话，你已经掌握了：
- ✅ 消息队列的核心价值（异步、解耦、削峰填谷）
- ✅ RabbitMQ 的交换机、队列、死信、延时、镜像集群
- ✅ Spring Boot 完整集成与分层设计
- ✅ 手动 ACK、重试、幂等、死信队列保证可靠性
- ✅ 生产者不等待消费者返回结果的设计哲学
- ✅ 秒杀场景下 Redis+MQ 协同削峰及补偿方案
- ✅ 何时使用 MQ 的判断标准

**下一步建议**：
1. 动手写一个秒杀小项目（Redis + MQ + MySQL）
2. 学习 Kafka 对比（顺序消息、吞吐量、日志场景）
3. 研究 RabbitMQ 镜像集群的复制协议（选读）

---

*文档整理自真实技术对话，采用费曼学习法逐层深入，适合从零到生产级别的学习路径。*