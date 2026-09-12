---
tags:
  - 计算机/后端/springboot
---

我来给你一个完整的 Spring Boot 统一响应体实现方案。

## 1. 统一响应体类

```java
import com.fasterxml.jackson.annotation.JsonInclude;
import lombok.Data;
import lombok.experimental.Accessors;

@Data
@Accessors(chain = true)
@JsonInclude(JsonInclude.Include.NON_NULL)
public class Result<T> {
    
    /**
     * 状态码
     */
    private Integer code;
    
    /**
     * 提示信息
     */
    private String message;
    
    /**
     * 数据
     */
    private T data;
    
    /**
     * 时间戳
     */
    private Long timestamp;
    
    /**
     * 成功响应（无数据）
     */
    public static <T> Result<T> success() {
        return new Result<T>()
                .setCode(ResultCode.SUCCESS.getCode())
                .setMessage(ResultCode.SUCCESS.getMessage())
                .setTimestamp(System.currentTimeMillis());
    }
    
    /**
     * 成功响应（带数据）
     */
    public static <T> Result<T> success(T data) {
        return new Result<T>()
                .setCode(ResultCode.SUCCESS.getCode())
                .setMessage(ResultCode.SUCCESS.getMessage())
                .setData(data)
                .setTimestamp(System.currentTimeMillis());
    }
    
    /**
     * 成功响应（自定义消息）
     */
    public static <T> Result<T> success(String message, T data) {
        return new Result<T>()
                .setCode(ResultCode.SUCCESS.getCode())
                .setMessage(message)
                .setData(data)
                .setTimestamp(System.currentTimeMillis());
    }
    
    /**
     * 失败响应
     */
    public static <T> Result<T> error(String message) {
        return new Result<T>()
                .setCode(ResultCode.ERROR.getCode())
                .setMessage(message)
                .setTimestamp(System.currentTimeMillis());
    }
    
    /**
     * 失败响应（自定义状态码和消息）
     */
    public static <T> Result<T> error(Integer code, String message) {
        return new Result<T>()
                .setCode(code)
                .setMessage(message)
                .setTimestamp(System.currentTimeMillis());
    }
    
    /**
     * 失败响应（使用ResultCode枚举）
     */
    public static <T> Result<T> error(IResultCode resultCode) {
        return new Result<T>()
                .setCode(resultCode.getCode())
                .setMessage(resultCode.getMessage())
                .setTimestamp(System.currentTimeMillis());
    }
}
```

## 2. 响应状态码枚举

```java
public interface IResultCode {
    Integer getCode();
    String getMessage();
}
```

```java
import lombok.AllArgsConstructor;
import lombok.Getter;

@Getter
@AllArgsConstructor
public enum ResultCode implements IResultCode {
    
    /**
     * 成功
     */
    SUCCESS(200, "操作成功"),
    
    /**
     * 失败
     */
    ERROR(500, "操作失败"),
    
    /**
     * 参数错误
     */
    PARAM_ERROR(400, "参数错误"),
    
    /**
     * 未登录/未认证
     */
    UNAUTHORIZED(401, "未登录或token已过期"),
    
    /**
     * 无权限
     */
    FORBIDDEN(403, "没有权限访问该资源"),
    
    /**
     * 资源不存在
     */
    NOT_FOUND(404, "资源不存在"),
    
    /**
     * 业务异常
     */
    BUSINESS_ERROR(1001, "业务处理失败"),
    
    /**
     * 数据重复
     */
    DATA_DUPLICATE(1002, "数据已存在"),
    
    /**
     * 数据不存在
     */
    DATA_NOT_EXIST(1003, "数据不存在");
    
    private final Integer code;
    private final String message;
}
```

## 3. 全局异常处理

```java
import lombok.extern.slf4j.Slf4j;
import org.springframework.validation.BindException;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import javax.validation.ConstraintViolation;
import javax.validation.ConstraintViolationException;
import java.util.stream.Collectors;

@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    /**
     * 业务异常处理
     */
    @ExceptionHandler(BusinessException.class)
    public Result<Void> handleBusinessException(BusinessException e) {
        log.error("业务异常：{}", e.getMessage());
        return Result.error(e.getCode(), e.getMessage());
    }
    
    /**
     * 参数校验异常 - Bean validation
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public Result<Void> handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {
        String message = e.getBindingResult().getFieldErrors().stream()
                .map(FieldError::getDefaultMessage)
                .collect(Collectors.joining(", "));
        log.error("参数校验异常：{}", message);
        return Result.error(ResultCode.PARAM_ERROR.getCode(), message);
    }
    
    /**
     * 参数绑定异常
     */
    @ExceptionHandler(BindException.class)
    public Result<Void> handleBindException(BindException e) {
        String message = e.getFieldErrors().stream()
                .map(FieldError::getDefaultMessage)
                .collect(Collectors.joining(", "));
        log.error("参数绑定异常：{}", message);
        return Result.error(ResultCode.PARAM_ERROR.getCode(), message);
    }
    
    /**
     * 参数校验异常 - 方法参数校验
     */
    @ExceptionHandler(ConstraintViolationException.class)
    public Result<Void> handleConstraintViolationException(ConstraintViolationException e) {
        String message = e.getConstraintViolations().stream()
                .map(ConstraintViolation::getMessage)
                .collect(Collectors.joining(", "));
        log.error("参数校验异常：{}", message);
        return Result.error(ResultCode.PARAM_ERROR.getCode(), message);
    }
    
    /**
     * 系统异常处理
     */
    @ExceptionHandler(Exception.class)
    public Result<Void> handleException(Exception e) {
        log.error("系统异常：", e);
        return Result.error("系统繁忙，请稍后重试");
    }
}
```

## 4. 自定义业务异常

```java
import lombok.Data;
import lombok.EqualsAndHashCode;

@Data
@EqualsAndHashCode(callSuper = true)
public class BusinessException extends RuntimeException {
    
    private Integer code;
    private String message;
    
    public BusinessException(String message) {
        super(message);
        this.code = ResultCode.BUSINESS_ERROR.getCode();
        this.message = message;
    }
    
    public BusinessException(Integer code, String message) {
        super(message);
        this.code = code;
        this.message = message;
    }
    
    public BusinessException(IResultCode resultCode) {
        super(resultCode.getMessage());
        this.code = resultCode.getCode();
        this.message = resultCode.getMessage();
    }
}
```

## 5. 使用示例

### Controller 层使用

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public Result<User> getUserById(@PathVariable Long id) {
        User user = userService.getById(id);
        return Result.success(user);
    }
    
    @PostMapping
    public Result<User> createUser(@Valid @RequestBody User user) {
        User created = userService.create(user);
        return Result.success("用户创建成功", created);
    }
    
    @DeleteMapping("/{id}")
    public Result<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return Result.success();
    }
    
    @GetMapping("/error-test")
    public Result<Void> testError() {
        if (true) {
            throw new BusinessException(ResultCode.DATA_NOT_EXIST);
        }
        return Result.success();
    }
}
```

### 实体类使用校验

```java
import lombok.Data;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;

@Data
public class User {
    
    private Long id;
    
    @NotBlank(message = "用户名不能为空")
    private String username;
    
    @NotNull(message = "年龄不能为空")
    private Integer age;
    
    @NotBlank(message = "邮箱不能为空")
    private String email;
}
```

### 响应示例

成功响应：
```json
{
    "code": 200,
    "message": "操作成功",
    "data": {
        "id": 1,
        "username": "张三",
        "age": 25,
        "email": "zhangsan@example.com"
    },
    "timestamp": 1703123456789
}
```

失败响应：
```json
{
    "code": 500,
    "message": "操作失败",
    "timestamp": 1703123456789
}
```

参数错误响应：
```json
{
    "code": 400,
    "message": "用户名不能为空, 年龄不能为空",
    "timestamp": 1703123456789
}
```

这个统一响应体方案包含了：
- 统一的响应格式
- 标准的状态码管理
- 全局异常处理
- 参数校验支持
- 业务异常支持

你可以根据项目需求调整状态码和响应格式。