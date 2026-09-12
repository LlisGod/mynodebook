---
tags:
  - 计算机/前端
---

# Css 引入的三种方式

1. 行内样式 (直接在 HTML 上定义)
	 `<span style="color: gary;">内容</span>`
2. 内部样式(在文件中指定位置统一定义)
	```html
	<style>
		span {
			color: gary;
		}
	</style>
	```
3. 外部样式(定义在单独的文件中，链接引入)
	`<link rel="stylesheet" href="css/news.css">`


# Css 选择器
元素选择器
```css
h1 {
	color:gary;
}
/*所有h1元素*/
```

类选择器
```css
.class {
	color:gary;
}
/*类名为class的元素*/
```

Id 选择器
```css
#find{
	color:gary;
}
/*id为find的元素*/ 
```