


#### 原理
基于 cookie 实现

当建立访问后，服务器会创建拥有唯一标识的 session

使用 Set-Cookie：JSEEION=id
将标识传递给前端
请求头中 Cookie 值就对应了 session 唯一标识

#### 特性
优点：存储在服务器，安全
缺点：
- 在集群中，由于服务器之间数据不共享，无法使用
- Cookie 的所有缺点