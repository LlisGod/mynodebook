---
epoch: 1743498635829
modelKey: deepseek/deepseek-r1:free|openrouterai
tags:
  - copilot-conversation
---

**user**: 用dataview语法查找所有带有Base/MOC标签的文件
[Timestamp: 2025/04/01 17:10:35]

**ai**: 以下是使用Dataview语法查找所有带有 `#Base/MOC` 标签文件的查询语句：

```dataview
LIST
FROM #Base/MOC
```

或者更严谨的写法：
```dataview
LIST
WHERE contains(tags, "Base/MOC")
```

这两个查询都会：
1. 列出所有包含 `#Base/MOC` 标签的笔记
2. 以简洁的列表形式呈现
3. 自动更新（当新增符合要求的笔记时会实时显示）

如果需要显示更多元数据，可以尝试：
```dataview
TABLE file.mtime AS "最后修改时间"
FROM #Base/MOC
SORT file.mtime DESC
```

**注意：**
1. 确保你的笔记中确实存在 `#Base/MOC` 这个标签
2. 标签名称区分大小写
3. 斜杠`/`表示标签层级，在Obsidian中会显示为嵌套标签结构

你可以在[[Dataview基本语法]]笔记中找到更多查询示例，或在[[Obsidian标签管理]]中查看标签使用规范。
[Timestamp: 2025/04/01 17:10:52]