---
created: 2025-03-26
course:
  name: 大学物理B2重修
  code: 略
  professor: 唐永波
  semester: 2025上半年
  schedule: 周二早上十点及周四晚上10点
tags:
  - Other/通识课
  - Base/Project/Course
---

# [[大学物理B2重修README]] 课程主页

## 📌 课程概览
```dataview
TABLE WITHOUT ID link(file.link) AS 相关笔记
FROM #大学物理B2重修 
WHERE file != this.file
```

## 📚 课程资料
### 核心教材
- [x] 《教材名称》  ISBN:

### 推荐阅读
- [x] [在线资源名称](URL)

### 文件库
```button
name 上传课件
type command
action Obsidian Advanced URI:upload
```

## 🗓 课程安排

### 实验课安排 

| 周数   | 实验名称               | 地点     |
| ---- | ------------------ | ------ |
| 第5周  | 分光计的调整 与玻璃三棱镜折射率测量 | B1-724 |
| 第6周  | 透镜焦距、偏振光及光敏电阻      | B1-723 |
| 第7周  | 霍尔效应实验             | B1-713 |
| 第8周  | 单多缝衍射的光强分布         | B1-721 |
| 第9周  | 干涉法测线膨胀系数          | B1-719 |
| 第10周 | 动态磁滞回线的观测          | B1-702 |
| 第11周 | 牛顿环测平凸透镜的曲率半径      | B1-724 |
| 第12周 | 落球法变温粘滞系数实验        | B1-715 |
| 第13周 | 多普勒效应测量声速          | B1-711 |
| 第14周 | 光栅衍射及原子光栅光谱测量      | B1-721 |
| 第15周 | 导热系数的测量            | B1-719 |
| 第16周 | 考试                 |        |

### 日程表
```dataview
TASK FROM #大学物理B2重修 AND #assignment 
WHERE !completed
SORT due ASC
```

## 📝 学习记录
### 课堂笔记
- 

### 疑难问题
```query
path:Courses 
```

## 📦 作业管理
### 进行中
```dataview
TASK FROM #大学物理B2重修 AND #assignment 
WHERE !completed
```

### 已完成
```dataview
TASK FROM #大学物理B2重修 AND #assignment 
WHERE completed
```

## 🎯 考试信息
### 期中考试
- 日期:: 
- 范围:: 
- 复习材料:: 

### 期末考试
- 日期:: 
- 占比:: 
- 备考计划:: 

^template-course  // 模板锚点
```

**使用说明**：
1. 需安装Templater和Dataview插件
2. 创建新笔记时选择此模板，会自动弹出课程信息输入框
3. 双链符号`[[ ]]`用于连接相关笔记
4. `![[ ]]`用于嵌入本地附件
5. Dataview区块会自动聚合课程相关任务和笔记
6. 通过按钮可直接上传课件到当前笔记所在文件夹

建议在Templater设置中将此模板绑定到「课程」类笔记的默认模板，并配合QuickAdd插件实现快速创建课程主页。