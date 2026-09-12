---
tags:
  - 计算机/后端/springboot
---
# 介绍
HttpClient 就是 “程序里的浏览器”，用来发 HTTP 请求、收响应，常见有两个：

- **Apache HttpClient**（老牌、功能全，Java 生态常用）
	- 阿里云的 sdk 依赖了这个包，所以如果引入了阿里云可以直接用
- **Java 11+ 内置 HttpClient**（[java.net](https://java.net). http，轻量现代）

下面用最常用的 **Apache HttpClient 4. X** 来讲。

---

### 一、它能做什么

- 发 **GET/POST/PUT/DELETE** 等请求
- 自定义**请求头、Cookie、超时、代理、HTTPS**
- 支持**连接池、重试、异步、表单 / JSON 提交**
- 比原生 `HttpURLConnection` 好用太多

---

### 二、快速入门（Maven）

#### 1）引依赖

Xml

```
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.14</version>
</dependency>
```

#### 2）发 GET 请求

Java

运行

```
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

public class Demo {
    public static void main(String[] args) throws Exception {
        // 1. 创建客户端
        try (CloseableHttpClient httpClient = HttpClients.createDefault()) {
            // 2. 创建 GET 请求
            HttpGet get = new HttpGet("https://httpbin.org/get");
            // 3. 发请求、收响应
            try (var resp = httpClient.execute(get)) {
                // 4. 读响应体
                String body = EntityUtils.toString(resp.getEntity());
                System.out.println(body);
            }
        }
    }
}
```

#### 3）发 POST JSON（最常用）

Java

运行

```
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import com.alibaba.fastjson.JSON;

public class PostJsonDemo {
    public static void main(String[] args) throws Exception {
        try (CloseableHttpClient httpClient = HttpClients.createDefault()) {
            HttpPost post = new HttpPost("https://httpbin.org/post");
            // 1. 构造 JSON
            String json = JSON.toJSONString(new User("tom", 18));
            StringEntity entity = new StringEntity(json, "UTF-8");
            entity.setContentType("application/json");
            // 2. 设置请求体
            post.setEntity(entity);
            // 3. 执行并读取
            try (var resp = httpClient.execute(post)) {
                System.out.println(EntityUtils.toString(resp.getEntity()));
            }
        }
    }
    static class User { String name; int age; User(String name,int age){this.name=name;this.age=age;}}
}
```

---

### 三、核心组件（记这几个就行）

- **CloseableHttpClient**：客户端入口，负责发请求
- **HttpGet / HttpPost / HttpPut…**：请求对象
- **CloseableHttpResponse**：响应对象
- **HttpEntity**：请求体 / 响应体（字符串、文件、流）
- **EntityUtils**：工具类，把实体转字符串、字节数组

---

### 四、高频配置（实际项目必配）

#### 1）超时

Java

运行

```
RequestConfig config = RequestConfig.custom()
    .setConnectTimeout(5000)   // 连接超时 5s
    .setSocketTimeout(10000)  // 读取超时 10s
    .build();
HttpGet get = new HttpGet("https://httpbin.org/get");
get.setConfig(config);
```

#### 2）连接池（高并发用）

Java

运行

```
PoolingHttpClientConnectionManager cm = new PoolingHttpClientConnectionManager();
cm.setMaxTotal(200);              // 最大连接数
cm.setDefaultMaxPerRoute(20);    // 每个域名最大连接
CloseableHttpClient httpClient = HttpClients.custom()
    .setConnectionManager(cm)
    .build();
```


# HttpClientUtil
直接给你写一个**企业级可直接复制使用的 HttpClient 工具类**，特点：

- 全局单例连接池（高性能，不会频繁创建销毁）
- 封装 GET、POST-JSON、POST - 表单
- 自带超时、异常处理、自动释放资源
- 不用每次 new 客户端，线程安全

## 1. 先引入依赖（Maven）

Xml

```
<!-- Apache HttpClient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.14</version>
</dependency>
<!-- JSON 解析 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.83</version>
</dependency>
```

## 2. HttpClientUtil 工具类（直接复制）

Java

运行

```java
import com.alibaba.fastjson.JSON;
import org.apache.http.*;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.impl.conn.PoolingHttpClientConnectionManager;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.net.URI;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.Map;

/**
 * HttpClient 工具类（连接池版，全局单例）
 */
public class HttpClientUtil {

    // 全局唯一客户端（连接池）
    private static final CloseableHttpClient HTTP_CLIENT;

    // 超时配置：连接超时5秒，读取超时10秒，从连接池获取超时3秒
    private static final RequestConfig REQUEST_CONFIG = RequestConfig.custom()
            .setConnectTimeout(5000)
            .setSocketTimeout(10000)
            .setConnectionRequestTimeout(3000)
            .build();

    static {
        // 初始化连接池
        PoolingHttpClientConnectionManager poolManager = new PoolingHttpClientConnectionManager();
        poolManager.setMaxTotal(200);        // 最大总连接数
        poolManager.setDefaultMaxPerRoute(50);// 单个域名最大连接数

        HTTP_CLIENT = HttpClients.custom()
                .setConnectionManager(poolManager)
                .setDefaultRequestConfig(REQUEST_CONFIG)
                .build();
    }

    //==================== GET 请求 ====================
    public static String doGet(String url) throws Exception {
        return doGet(url, null);
    }

    public static String doGet(String url, Map<String, String> params) throws Exception {
        URIBuilder uriBuilder = new URIBuilder(url);
        if (params != null) {
            for (Map.Entry<String, String> entry : params.entrySet()) {
                uriBuilder.addParameter(entry.getKey(), entry.getValue());
            }
        }
        URI uri = uriBuilder.build();
        HttpGet httpGet = new HttpGet(uri);

        try (CloseableHttpResponse response = HTTP_CLIENT.execute(httpGet)) {
            return handleResponse(response);
        }
    }

    //==================== POST JSON 请求 ====================
    public static String doPostJson(String url, Object data) throws IOException {
        HttpPost httpPost = new HttpPost(url);
        String jsonStr = JSON.toJSONString(data);
        StringEntity entity = new StringEntity(jsonStr, StandardCharsets.UTF_8);
        entity.setContentType("application/json;charset=UTF-8");
        httpPost.setEntity(entity);

        try (CloseableHttpResponse response = HTTP_CLIENT.execute(httpPost)) {
            return handleResponse(response);
        }
    }

    //==================== POST 表单请求 ====================
    public static String doPostForm(String url, Map<String, String> params) throws IOException {
        HttpPost httpPost = new HttpPost(url);
        ArrayList<NameValuePair> pairs = new ArrayList<>();
        if (params != null) {
            for (Map.Entry<String, String> entry : params.entrySet()) {
                pairs.add(new BasicNameValuePair(entry.getKey(), entry.getValue()));
            }
        }
        httpPost.setEntity(new UrlEncodedFormEntity(pairs, StandardCharsets.UTF_8));

        try (CloseableHttpResponse response = HTTP_CLIENT.execute(httpPost)) {
            return handleResponse(response);
        }
    }

    // 统一处理响应、释放资源
    private static String handleResponse(CloseableHttpResponse response) throws IOException {
        HttpEntity entity = response.getEntity();
        String result = EntityUtils.toString(entity, StandardCharsets.UTF_8);
        EntityUtils.consume(entity); // 释放流资源
        return result;
    }
}
```

## 3. 使用示例（超级简单）

Java

运行

```
public class Test {
    public static void main(String[] args) throws Exception {
        // 1. GET
        String getResult = HttpClientUtil.doGet("https://httpbin.org/get");
        System.out.println(getResult);

        // 2. POST JSON
        Map<String,Object> map = Map.of("name","张三","age",20);
        String postJson = HttpClientUtil.doPostJson("https://httpbin.org/post", map);
        System.out.println(postJson);
    }
}
```

## 4. 对应你刚才理解的资源关闭

- **客户端只初始化一次**，全局复用，不用频繁关闭
- 每次请求的 **response 用 try-with-resources 自动关闭**
- `EntityUtils.consume(entity)` 手动释放请求体流
- 完全解决你担心的：**资源泄漏、连接不回收**