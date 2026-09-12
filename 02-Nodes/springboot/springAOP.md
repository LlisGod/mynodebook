---
tags:
  - 计算机/后端/springboot
---


## 一、什么是 AOP？

AOP（Aspect Oriented Programming，面向切面编程）  
将**横切关注点**（如日志、事务、权限、性能监控）从业务逻辑中抽取出来，**无侵入地**织入到目标方法中。

> 通俗理解：在不改源码的情况下，给现有方法增加前后行为。

---

## 二、核心概念（关键词）

| 概念 | 说明 |
|------|------|
| **Aspect（切面）** | 横切关注点的模块化（一个普通类 + 切面注解） |
| **Join Point（连接点）** | 方法执行过程中的某个点（Spring AOP 中特指方法执行） |
| **Advice（通知）** | 切面在某个连接点执行的具体动作（如 `@Before`） |
| **Pointcut（切入点）** | 通过表达式或注解，匹配一组连接点 |
| **Target Object（目标对象）** | 被增强的业务对象 |
| **Weaving（织入）** | 将切面应用到目标对象的过程 |

---

## 三、常用注解（你已理解，这里精确定义）

| 注解                | 执行时机                        | 典型用途         |
| ----------------- | --------------------------- | ------------ |
| @Aspect           | 项目启动时                       | 定义切面类        |
| `@Before`         | 方法执行**之前**                  | 参数校验、权限检查    |
| `@After`          | 方法执行**结束**（finally，无论成功/异常） | 资源释放、清理日志    |
| `@AfterReturning` | 方法**正常返回后**                 | 记录返回值、修改返回值  |
| `@AfterThrowing`  | 方法**抛出异常后**                 | 异常监控、报警、事务回滚 |
| `@Around`         | 方法执行**前后**（最强大）             | 性能监控、缓存、事务控制 |

> ⚠️ `@After` 不等于 `@AfterReturning`：  
> - `@After`：一定执行（类似 finally）  
> - `@AfterReturning`：仅正常返回时执行

---

## 四、`@Around` 的核心（你特别提到的重点）

`@Around` 需要**主动调用** `proceed()`，否则目标方法不会执行。

```java
@Around("execution(* com.example.service.*.*(..))")
public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
    long start = System.currentTimeMillis();

    Object result = joinPoint.proceed(); // ⚠️ 必须调用

    long time = System.currentTimeMillis() - start;
    System.out.println("耗时：" + time + "ms");
    return result;
}
```

- 可以控制是否执行目标方法
- 可以修改返回值
- 可以替换异常处理

---

## 五、切入点表达式示例（匹配式）

```java
// 任意返回值，service包下所有类的所有方法
@Before("execution(* com.example.service.*.*(..))")

// 带注解的方法
@Before("@annotation(com.example.annotation.Log)")
```

---

## 六、一个完整的切面类例子

```java
@Aspect
@Component
public class LogAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logStart() {
        System.out.println("方法开始");
    }

    @AfterReturning(value = "execution(* com.example.service.*.*(..))", returning = "result")
    public void logReturn(Object result) {
        System.out.println("方法正常返回：" + result);
    }

    @AfterThrowing(value = "execution(* com.example.service.*.*(..))", throwing = "e")
    public void logException(Exception e) {
        System.out.println("方法异常：" + e.getMessage());
    }
}
```

---

## 七、AOP 的限制（Spring AOP vs AspectJ）

| 特性 | Spring AOP | AspectJ |
|------|------------|---------|
| 代理方式 | JDK 动态代理 / CGLIB | 字节码织入 |
| 是否支持 private 方法 | ❌ 不支持 | ✅ 支持 |
| 是否支持静态方法 | ❌ 不支持 | ✅ 支持 |
| 是否必须通过 Spring 容器代理 | ✅ 是 | ❌ 不必须 |

> 绝大多数业务场景 Spring AOP 足够。

---

## 八、一句话总结（方便记忆）

> **AOP = 切面类 + 切入点表达式 + 通知（@Before/@Around/@After...），无侵入增强方法行为。**  
> `@Around` 必须手动 `proceed()`，其余注解由 Spring 自动决定执行时机。

---

如果你想，我可以再帮你补充：
- **Spring AOP 代理机制选择（JDK vs CGLIB）**
- **基于注解的自定义切入点（@annotation）**
- **多个切面的执行顺序（@Order）**

或者直接生成 **Markdown / PDF 版本文档** 也行。