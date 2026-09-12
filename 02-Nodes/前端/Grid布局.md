---
tags:
  - 计算机/前端
---
Grid 布局是 CSS3 推出的**二维网格布局系统**，擅长处理行列交错的复杂布局。核心特点：

![Grid布局示意图](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-template-areas.svg)

### 三大核心概念
1. **网格容器** (父元素)
   - 设置 `display: grid` 激活网格布局
2. **网格轨道** (行/列)
   - 通过 `grid-template-columns/rows` 定义列宽行高
3. **网格单元** (子元素)
   - 可自由跨越行列区域

### 常用容器属性
```css
.container {
  grid-template-columns: 100px 1fr 2fr; /* 列定义 */
  grid-template-rows: auto 200px;        /* 行定义 */
  gap: 10px;              /* 网格间距 */
  grid-template-areas:    /* 可视化布局区域 */
    "header header"
    "sidebar main";
}
```

### 常用项目属性
```css
.item {
  grid-column: 1 / 3;     /* 跨 1-2 列 */
  grid-row: span 2;       /* 跨 2 行 */
  grid-area: header;      /* 指定布局区域 */
}
```

**典型应用场景**：杂志式排版、仪表盘、表单布局、响应式网页结构  
（与 Flex 互补：Flex 适合微观排列，Grid 擅长宏观架构）

例如创建 3 列布局：
```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
```

#### [[grid和Flex布局的区别]] 