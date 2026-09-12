---
tags:
  - 计算机/后端/springboot
---

- **`MultipartFile` 接口**：这是Spring提供的用于接收上传文件的核心接口。它封装了上传文件的**文件名、大小、内容类型**以及文件数据本身。在Controller的方法参数中声明 `MultipartFile`，就能轻松获取客户端上传的文件内容