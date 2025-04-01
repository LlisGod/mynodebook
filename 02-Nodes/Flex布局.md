---
tags:
  - Base/Note/WHAT
  - Other/前端
---

Flex 布局是 CSS3 提供的**弹性盒子布局模型**，通过简单的属性设置就能实现灵活的一维布局（行或列排列）。核心特点：
![[Pasted image 20250328214121.png]]

### 三个核心概念
1. **容器** (父元素)
   - 设置 `display: flex` 即成为弹性容器
2. **项目** (子元素)
   - 自动成为弹性项目，自动继承父元素属性
1. **双轴系统**
   - 主轴（main axis）：通过 `flex-direction` 定义方向
   - 交叉轴（cross axis）：始终垂直于主轴

### 常用容器属性
```css
.container {
  justify-content: /* 主轴对齐方式 */;
  align-items:     /* 交叉轴对齐方式 */;
  flex-direction:  /* 主轴方向 row|column */;
  flex-wrap:       /* 换行控制 */;
  gap:             /* 项目间距 */;
}
```

### 常用项目属性
```css
.item {
  flex: 1;           /* 弹性比例 */
  order: 2;          /* 排列顺序 */
  align-self: center;/* 单独对齐 */
}
```

**典型应用场景**：导航栏、等分布局、垂直居中、响应式排列  
（与传统 float/position 布局相比，代码更简洁易维护）