---
tags:
  - Base/Note
  - "#Other/插件"
  - Other/AI
---

1. 打开 Inline AI 的设置页面，你会看到以下选项
	![[202502272795.png]]
2. 首先你需要申请一个自己的 API Key，申请方法可以看下面这两篇文章
	- [自由使用任意大模型 API：OpenRouter 注册、付费与 API Key 设置教程](https://eryinote.com/post/1695)
	- 硅基流动 API 申请教程：[点我](https://docs.siliconflow.cn/cn/usercases/use-siliconcloud-in-obsidian)（用我的邀请码注册 `Bx0usMBl` 可以额外获得 2000 万的 token 额度）
3. 拥有自己的 API Key 之后，需要将这个 Key 填入「API Key」这一项设置里
4. 在 Model 位置填入你想要使用的模型，例如 `deepseek-ai/DeepSeek-V3`
5. 在 Custom endpoint 这个位置填入：`https://api.siliconflow.cn/v1`

都设置完毕后，在正文中选中段落，然后使用快捷键 `Ctrl+shift+I` 唤出命令窗口。（如果唤出失败的话，到【设置 - 快捷键】中搜索 `inline` 然后找到 `show cursor tooltip`，然后重新设置一下快捷键。）