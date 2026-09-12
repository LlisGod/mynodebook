---
tags:
  - 计算机/前端
---
[[Flex布局]]与 [[Grid布局]]是 CSS 两大现代布局方案，主要区别如下：




### 核心差异对比表
| 特性               | Flex 布局                  | Grid 布局                  |
|--------------------|---------------------------|---------------------------|
| **维度**           | 一维（行 **或** 列）       | 二维（行 **和** 列）       |
| **控制方向**       | 单轴布局                  | 行列双向控制              |
| **子项尺寸**       | 基于内容动态调整          | 可严格定义轨道尺寸        |
| **重叠控制**       | 不支持元素重叠            | 支持行列交叉区域重叠      |
| **适用场景**       | 微观元素排列              | 宏观页面架构              |

### 典型场景示例
**Flex 更适合**：
```css
/* 导航栏 */
.nav {
  display: flex;
  justify-content: space-between;
}

/* 卡片内容垂直居中 */
.card {
  display: flex;
  align-items: center;
}
```

**Grid 更适合**：
```css
/* 杂志式布局 */
.page {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-areas: "sidebar main";
}

/* 响应式图片墙 */
.gallery {
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```

### 最佳实践
1. **组合使用**：用 Grid 搭建整体框架，内部用 Flex 排列细节
2. **移动优先**：Flex 处理简单流动布局，Grid 处理复杂响应式
3. **重叠需求**：需要元素层叠时必选 Grid

>  💡 现代网站常同时使用两者：比如用 Grid 创建 12 列页面骨架，用 Flex 实现导航栏和卡片内容布局，当然一般flex直接也能搞定