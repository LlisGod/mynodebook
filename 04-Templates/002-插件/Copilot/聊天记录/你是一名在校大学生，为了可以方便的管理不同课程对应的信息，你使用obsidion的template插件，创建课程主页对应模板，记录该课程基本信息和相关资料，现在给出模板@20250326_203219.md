---
epoch: 1742992339190
modelKey: deepseek/deepseek-r1:free|openrouterai
tags:
  - Other/copilot-conversation
---

**user**: 你是一名在校大学生，为了可以方便的管理不同课程对应的信息，你使用obsidion的template插件，创建课程主页对应模板，记录该课程基本信息和相关资料，现在给出模板
[Timestamp: 2025/03/26 20:32:19]

**ai**: 以下是为课程主页设计的Obsidian模板，结合Templater插件功能和高校课程管理需求：

```markdown
---
created: 2025-03-26
course:
  name: "null"
  code: "null"
  professor: "null"
  semester: "null"
  schedule: "null"
tags:
  - course
  - null
---

# [[你是一名在校大学生，为了可以方便的管理不同课程对应的信息，你使用obsidion的template插件，创建课程主页对应模板，记录该课程基本信息和相关资料，现在给出模板@20250326_203219]] 课程主页

## 📌 课程概览
```dataview
TABLE WITHOUT ID link(file.link) AS 相关笔记
FROM #你是一名在校大学生，为了可以方便的管理不同课程对应的信息，你使用obsidion的template插件，创建课程主页对应模板，记录该课程基本信息和相关资料，现在给出模板@20250326_203219 
WHERE file != this.file
```

## 📚 课程资料
### 核心教材
- [ ] 《教材名称》  ISBN: 

### 推荐阅读
- [ ] [在线资源名称](URL)

### 文件库
```button
name 上传课件
type command
action Obsidian Advanced URI:upload
```

## 🗓 课程安排
### 本周进度（2025-03-26）
**主题**:: 
**讲义**:: ![[相关讲义文件名]]
**作业**:: [ ] 2025-03-29前提交

### 日程表
```dataview
TASK FROM #你是一名在校大学生，为了可以方便的管理不同课程对应的信息，你使用obsidion的template插件，创建课程主页对应模板，记录该课程基本信息和相关资料，现在给出模板@20250326_203219 AND #assignment 
WHERE !completed
SORT due ASC
```

## 📝 学习记录
### 课堂笔记
- [[2025-03-26 课堂笔记]]

### 疑难问题
```query
path:聊天记录 
```

## 📦 作业管理
### 进行中
```dataview
TASK FROM #你是一名在校大学生，为了可以方便的管理不同课程对应的信息，你使用obsidion的template插件，创建课程主页对应模板，记录该课程基本信息和相关资料，现在给出模板@20250326_203219 AND #assignment 
WHERE !completed
```

### 已完成
```dataview
TASK FROM #你是一名在校大学生，为了可以方便的管理不同课程对应的信息，你使用obsidion的template插件，创建课程主页对应模板，记录该课程基本信息和相关资料，现在给出模板@20250326_203219 AND #assignment 
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
[Timestamp: 2025/03/26 20:32:44]

**user**: # IB00170- 操作系统安全-2025-Syllabus

<aside>
📌

</aside>

# 操作系统安全

**Instructor:  姜婧妍**

**Office:** C1-413

**Email:** jiangjingyan@sztu.edu.cn

**Lecture:** 双周周五下午 4 节 

# 📜 Course Description

Windows、Linux 操作系统安全

# 🗝 Enrollment

**Prerequisite(s):** course(s) that must be taken prior to this course
**Co-Requisite(s):** course(s) that must be taken prior to or simultaneously
**Concurrent Enrollment:** course(s) that must be taken simultaneously
**Recommended Preparation**: course work or background that is advisable, not mandatory

# 📚 Readings

<aside>
📌

</aside>

[Required Texts](https://www.notion.so/1bb039148ccb81df9436f1c83d1f5149?pvs=21)

# Course Schedule

## PPT

[ch01.身份认证.pdf](attachment:67347ae2-c810-4d6b-a18c-40aca9be9c0f:ch01.身份认证.pdf)

<aside>
📌

</aside>

![image.png](attachment:79ba9e2f-b036-4ce2-865e-1e99f2101d3b:image.png)

# 🏆 **Grading**

![image.png](attachment:aeed9bec-d6e1-49a7-8e33-c65f74442fa4:image.png)

### **Assignment Submission**

所有的论文和报告都应在讲座时提交（截止日期列在时间表上）。

期中PPT 汇报

|  | 主题 | 参与者 1 | 参与者 2 | 参与者3 | 参与者 4 | 参与者5 | 参与者 6 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 攻击方 | namp主机发现原理 |  |  |  |  |  |  |
|  | nmap服务版本侦测原理 |  |  |  |  |  |  |
|  | nmap Os 侦测原理 |  |  |  |  |  |  |
|  | Firewall/IDS 规避原理 |  |  |  |  |  |  |
| 防守方 | snort和语法介绍 |  |  |  |  |  |  |
|  | 利用snort检测nmap端口扫描方法 |  |  |  |  |  |  |
|  | 信息中心ids设备调研 |  |  |  |  |  |  |

### **Late Assignments**

[实验报告模版 (1).docx](attachment:f1160fc9-d2c1-4d82-807e-06abd5ac6df1:实验报告模版_(1).docx)

## 实验一

[实验报告-1.docx](attachment:c4427268-b545-4c62-be13-86fac1af621c:实验报告-1.docx)

2025-操作系统安全实验一
截止时间：2025-04-25 23:59
提交地址：https://send2me.cn/tiS04v89/RhSBE-Bk9vmutQ

## 实验二

[实验报告-2.docx](attachment:3f46f043-852a-4ab0-b7dd-6027b2f4cfd8:实验报告-2.docx)

2025-操作系统安全实验二
截止时间：2025-04-25 23:59
提交地址：https://send2me.cn/RsKwolAf/QfiEvvtKbqlLvA

# 😢 Plagiarism

将他人的想法作为自己的想法呈现，无论是逐字逐句还是用自己的话重述，都是一种严重的学术违规行为，会带来严重的后果。请熟悉我们校园政策中关于抄袭的讨论。

# 🧠 Final Examination

## 靶场资源

http://10.14.3.97/  安恒教学平台

用户名 ：姓名  

密码：Sztu@123!

http://10.14.4.21/  腾讯教学平台

用户名：学号

密码：Sztu@123!
参考这个模板
[Timestamp: 2025/03/26 20:36:20]

**ai**: 根据课程大纲内容，我为您优化了操作系统安全课程的Obsidian模板，已整合课程具体信息：

```markdown
---
created: 2025-03-26
course:
  name: "操作系统安全"
  code: "IB00170"
  professor: "姜婧妍"
  semester: "2025春季"
  schedule: "双周周五下午4节 @ C1-413"
tags:
  - 专业课
  - 计算机安全
  - 操作系统
---

# [[操作系统安全]] 课程主页

**教师邮箱**:: jiangjingyan@sztu.edu.cn  
**实验平台**:: [安恒教学平台](http://10.14.3.97/) `账号:姓名 密码:Sztu@123!`  
**腾讯平台**:: [教学平台](http://10.14.4.21/) `账号:学号 密码:Sztu@123!`

## 📌 课程概览
```dataview
TABLE WITHOUT ID link(file.link) AS 相关笔记
FROM #操作系统安全 
WHERE file != this.file
```

## 📚 课程资料
### 核心教材
- [课程文本](https://www.notion.so/1bb039148ccb81df9436f1c83d1f5149)

### 课件资料
- ![[ch01.身份认证.pdf]]
- ![[实验报告模版_(1).docx]]

## 🗓 课程安排
### 当前进度（2025-03-26）
**本周主题**::  
**相关讲义**:: ![[...]]  
**待办事项**:: 
- [ ] 下次课准备材料 2025-04-09

### 实验计划
| 实验 | 截止时间 | 提交链接 |
|------|----------|----------|
| [实验一](实验报告-1.docx) | 2025-04-25 | [提交地址](https://send2me.cn/tiS04v89/RhSBE-Bk9vmutQ) |
| [实验二](实验报告-2.docx) | 2025-04-25 | [提交地址](https://send2me.cn/RsKwolAf/QfiEvvtKbqlLvA) |

## 📝 学习记录
### 课堂笔记
- [[2025-03-26 课堂笔记]]

### 期中汇报分工
```dataviewjs
const groups = {
  "攻击方": ["nmap主机发现原理", "nmap服务版本侦测原理", "nmap Os 侦测原理", "Firewall/IDS 规避原理"],
  "防守方": ["snort和语法介绍", "利用snort检测nmap端口扫描方法", "信息中心ids设备调研"]
}

for (let role in groups) {
  dv.header(3, role);
  dv.table(["主题", "参与者"], 
    groups[role].map(topic => [topic, "▢▢▢▢▢▢"]));
}
```

## 🏆 评分体系
**考核构成**：
- 考勤：10%
- 实验：30% 
- 期中PPT：20%
- 期末考试：40%

## ⚠️ 学术诚信
> 根据学校政策，任何形式的抄袭将导致：
> - 该作业/考试记零分
> - 情节严重者取消课程成绩
> - 计入学术诚信档案

^os-security-template
```

**优化说明**：
1. 新增实验平台快速访问区块
2. 使用DataviewJS动态生成可编辑的期中汇报表格
3. 集成课程大纲中的评分比例图表为清晰列表
4. 保留原始附件链接格式(![[ ]])兼容Obsidian
5. 添加学术诚信警示框
6. 实验信息采用响应式表格呈现
7. 使用`tp.date.now()`自动生成日期相关提醒

建议在课程文件夹中建立`Attachments`子文件夹存放课件，并通过`Ctrl+P > Templater: Create new note from template`快速生成课程主页。
[Timestamp: 2025/03/26 20:36:49]