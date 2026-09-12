---
tags:
  - 计算机
  - cpp
---

## vector ：变长数组，自动扩容
 
  ```cpp
  vector<int> nums;
  /* 访问元素 */
  int num = nums[1];  // 访问索引 1 处的元素
  
  /* 更新元素 */
  nums[1] = 0;  // 将索引 1 处的元素更新为 0
  
  /*清空*/
  nums.clear();
  
  /* 在尾部添加元素 */
  nums.push_back(1);
   
  /* 在中间插入元素 */
  nums.insert(nums.begin() + 3, 6);  // 在索引 3 处插入数字 6
  //(nums.begin,3,6) 插入3个6
  
  /* 删除元素 */
  nums.erase(nums.begin() + 3);      // 删除索引 3 处的元素
  
  /* 通过索引遍历列表 */
  int count = 0;
  for (int i = 0; i < nums.size(); i++) {
      count += nums[i];
  }
  
  /* 直接遍历列表元素 */
  count = 0;
  for (int num : nums) {
      count += num;
  }
  
  ```

## stack ：栈实现
```cpp
/* 初始化栈 */
stack<int> stack;

/* 元素入栈 */
stack.push(1);
stack.push(3);
stack.push(2);
stack.push(5);
stack.push(4);

/* 访问栈顶元素 */
int top = stack.top();

/* 元素出栈 */
stack.pop(); // 无返回值

/* 获取栈的长度 */
int size = stack.size();

/* 判断是否为空 */
bool empty = stack.empty();
```
## queue ：队列实现
  ```cpp
  /* 初始化队列 */
  queue<int> queue;
  
  /* 元素入队 */
  queue.push(1);
  queue.push(3);
  queue.push(2);
  queue.push(5);
  queue.push(4);
  
  /* 访问队首元素 */
  int front = queue.front();
  
  /* 元素出队 */
  queue.pop();
  
  /* 获取队列的长度 */
  int size = queue.size();
  
  /* 判断队列是否为空 */
  bool empty = queue.empty();
  ```
## deque ：双向队列
```cpp
/* 初始化双向队列 */
deque<int> deque;

/* 元素入队 */
deque.push_back(2);   // 添加至队尾
deque.push_back(5);
deque.push_back(4);
deque.push_front(3);  // 添加至队首
deque.push_front(1);

/* 访问元素 */
int front = deque.front(); // 队首元素
int back = deque.back();   // 队尾元素

/* 元素出队 */
deque.pop_front();  // 队首元素出队
deque.pop_back();   // 队尾元素出队

/* 获取双向队列的长度 */
int size = deque.size();

/* 判断双向队列是否为空 */
bool empty = deque.empty();
```