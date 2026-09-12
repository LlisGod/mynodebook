---
tags:
  - 计算机/后端/springboot
---
# 介绍

一个十分好用的组件，可以根据注解自动生成高可用的接口文档，十分利于接口的测试以及测试人员的代码审查

# 使用前

在 springboot 中，我们建议使用 knife4j 框架来调用 swagger，更加方便美观

需要在项目中引入 knife 4 j 的 maven 坐标
设置 knife 4 j 的配置类
配置接口文档的静态资源映射

新版和旧版略有不同，新版不需要手动指定扫描包。对于多模块，可以手动指定

这里的代码可以参考官网和 ai



# Swagger / OpenAPI 3 常用注解全套精讲

适配 **SpringBoot 3 + Knife 4 j 4**，都是现在项目**必用**注解，分场景给你讲，直接能上手用。

> 旧版 Swagger 2：`@Api、@ApiOperation、@ApiParam`
> 
> 新版 OpenAPI 3 全部换成 **io. Swagger. V 3. Oas. Annotations** 这套，下面全是新标准。

---

## 一、作用在控制器类上

### 1. @Tag

**给整个控制器打分组、描述**

Java

运行

```
@Tag(name = "用户管理模块", description = "用户增删改查相关接口")
@RestController
@RequestMapping("/user")
public class UserController {
}
```

- Name：页面显示的分组名称
- Description：模块备注说明

---

## 二、作用在接口方法上

### 1. @Operation

**描述单个接口**

Java

运行

```
@Operation(summary = "根据ID查询用户", description = "传入用户ID，返回用户详情信息")
@GetMapping("/get/{id}")
public Result<User> getUser(){}
```

- Summary：接口简短标题（必填）
- Description：详细描述（可选）

### 2. @Hidden

**隐藏某个接口，不在文档显示**

Java

运行

```
@Hidden
@PostMapping("/inner")
public void innerApi(){}
```

用于内部接口、调试接口，不想暴露在文档里。

---

## 三、作用在接口参数上

### 1. @Parameter

**描述路径参数、请求参数**

Java

运行

```
@GetMapping("/get/{id}")
public Result<User> getUser(
    @Parameter(description = "用户唯一ID", required = true) 
    @PathVariable Long id
){}
```

常用属性：

- Description：参数说明
- Required：是否必传 true/false
- Example：示例值

### 2. @Parameters + @Parameter 多参数

多个参数统一说明

Java

运行

```
@Parameters({
    @Parameter(name = "pageNum", description = "页码", example = "1"),
    @Parameter(name = "pageSize", description = "每页条数", example = "10")
})
@GetMapping("/list")
```

---

## 四、作用在实体类 / DTO 上（最常用）

### 1. @Schema

**描述实体类和字段**

#### 类上

Java

运行

```
@Schema(description = "用户实体DTO")
public class UserDTO {
}
```

#### 字段上

Java

运行

```
@Schema(description = "用户ID", example = "10001")
private Long id;

@Schema(description = "用户名", requiredMode = Schema.RequiredMode.REQUIRED)
private String username;

@Schema(description = "手机号", nullable = true)
private String phone;
```

常用属性：

- Description：字段说明
- Example：示例值
- RequiredMode：是否必填
- Nullable：是否可空

---

## 五、作用在请求体 @RequestBody

不需要额外注解，**只要 DTO 上加了 @Schema**，自动解析字段注释、必填、示例。

Java

运行

```
@PostMapping("/add")
public Result add(@RequestBody @Parameter(description = "用户新增参数") UserDTO dto){
}
```

---

## 六、响应结果统一说明

直接在返回的 **Result 统一返回类** 上加 `@Schema`

Java

运行

```
@Schema(description = "统一返回结果")
public class Result<T> {
    @Schema(description = "响应码：200成功，500失败")
    private int code;

    @Schema(description = "提示信息")
    private String msg;

    @Schema(description = "业务数据")
    private T data;
}
```

所有接口返回 Result，文档自动识别结构。

---

## 七、新旧注解对照表（必记）

表格

|旧版 Swagger 2|新版 OpenAPI 3|作用|
|---|---|---|
|@Api|@Tag|控制器分组|
|@ApiOperation|@Operation|接口说明|
|@ApiParam|@Parameter|参数说明|
|@ApiModel|@Schema|实体类说明|
|@ApiModelProperty|@Schema|实体字段说明|

---

## 八、最简使用套路（你以后照着写就行）

1. 控制器类：`@Tag`
2. 每个接口：`@Operation`
3. 路径参数：`@Parameter`
4. 实体 / DTO 类 + 字段：全用 `@Schema`
5. 内部不想暴露接口：加 `@Hidden`