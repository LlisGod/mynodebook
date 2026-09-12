---
tags:
  - AT/Dashboard
---

````tabs
--- 最近编辑
```dataview
TABLE file.mtime as "最后修改"
FROM ""
SORT file.mtime DESC
LIMIT 15
```
--- 任务管理

```dataview
TASK
FROM ""
GROUP BY completed
limit 10

```

--- 未完成MOC

```dataview
TABLE file.ctime AS "创建时间"
FROM #BASE/MOC/undone
SORT file.ctime DESC

```


--- 即将到期

```dataview
TABLE 
    title AS "标题",
    priority AS "优先级",
    dueDate AS "截止日期",
    (date(dueDate) - date(today)) AS "剩余天数",
    tags AS "标签"
FROM "/"
WHERE status = "进行中" AND date(dueDate) - date(today) <= 30
SORT dueDate ASC

```

````

