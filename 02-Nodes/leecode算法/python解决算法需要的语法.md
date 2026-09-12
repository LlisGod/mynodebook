---
tags:
  - python
---

# 循环输入

```python
import sys 
# 1. 无限循环 
while True: 
	try: 
	
		# 2. 尝试读取一行并分割 
		line = input().split() 
		
		# 3. 【可选】处理空行情况（防止本地测试死循环）
		 
		if not line: 
			break
		
	 	# 4. 解析数据 
		a = int(line[0]) b = int(line[1]) 
		
		# --- 在这里写你的核心逻辑 --- 
		# solve(a, b) 
		# -------------------------- 
		
	except EOFError: 
		# 5. 捕获文件结束信号，安全退出 
		break
```

```python
import sys 
# 1. 直接遍历标准输入流 
for line in sys.stdin:
 	# 2. 去除首尾空白符并分割 
 	line = line.strip().split() 
 	# 3. 如果该行不为空，则处理 
 	if line: 
 	a = int(line[0]) 
 	b = int(line[1]) 
 	# --- 核心逻辑 --- 
 	# solve(a, b) 
 	# ------------
```

## 数字分割
使用 `line.split( )` 可以做到分割“1 2 23 11”这样被空格分开的数字，
对“1，2，3，4”则使用 `line.split(',')`

## 文本/字符串
可以直接遍历，可以使用 `enumerate(items)` 返回下标和元素


# 数学计算


## 基础运算符

| 符号  | 操作  |
| --- | --- |
| %   | 取余  |
| //  | 整除  |
| **  | 次方  |
|     |     |

# 列表

A=[]
就是一个空列表
可以用 list(items) 强行转换列表

使用
.append( ) 插入
.pop() 删除

生成 3 x 3 二维列表
`Grid = [[0] * 3 for _ in range (3)]`


# 数据结构表达

## 栈
使用 list 就可以
Append 就是压栈
Pop 就是出栈

## 队列
使用
From collections import deque

Popleft 就是出队

或者直接使用 list 的 pop(0) 也可以，不过效率比较低

## 堆
使用
import heapq
默认最小堆
```python
import heapq

heap = []
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 2)

print(heap[0])       # 偷看堆顶（最小值） -> 1
print(heapq.heappop(heap)) # 弹出最小值 -> 1
```

或者自己定义堆类


## 树和图

自己定义，使用列表或者定义节点类