---
tags:
  - 计算机/前端
---

# 标签化语言

html、css、JavaScript为前端三剑客
html是网站的骨架，css决定网站的颜值，JavaScript为网站注入灵魂

html是让计算机阅读的语言，通过标签让计算机将纯文本内容渲染成功能丰富，格式多样的网页，类似markdown语言

标签的一般形式为``<div>内容</div>``
其中``<div>``为头， ``</div>``为尾，成对出现
头可以添加属性，诸如标签，id，类，颜色字体等

部分特殊标签单独出现如``<input>``代表输入框

# 常用的标签

```tabs
--- 基础

| 作用  | 标签头     | 标签尾      |
| --- | ------- | -------- |
| 标题  | <h1~h6> | </h1~h6> |
| 段落  | \<p>    | \</p>    |
| 块   | \<div>  | \</div>  |
| 链接  | \<a>    | \</a>    |

--- 特殊
| 作用  | 标签头     | 标签尾      |
| --- | ------- | -------- |
| 图片 | \<img> | （无） | 
| 换行 | \<br> | （无） |
```


# 一个简单的使用实例

```html
<h1>这是一个标题</h1>
<p>这是一段话，巴拉巴拉</p>
<h2>在这个二级标题下插入些好东西</h2>
<a href="https://www.runoob.com/html/html-links.html">菜鸟教程的网站</a>
<h3>插入一张图片</h3>
<img src="_url_" alt="替代文本（图片无法加载时显示）">
```

# 版本和基本格式

正宗的html当然不会像上面一样直接写，而是像cpp中``#include<iostrem> int main(){}``一样有一段固定的开场白

即

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>网站标题</title>
</head>
<body>
 <!-- 注释 --->
网页内容
 
</body>
</html>
```

- **<!DOCTYPE html>** 声明为 HTML5 文档
- **\<html>** 元素是 HTML 页面的根元素
- **\<head>** 元素包含了文档的元（meta）数据，如 <meta charset="utf-8"> 定义网页编码格式为 **utf-8**。
- **\<title>** 元素描述了文档的标题
- **\<body>** 元素包含了可见的页面内容

