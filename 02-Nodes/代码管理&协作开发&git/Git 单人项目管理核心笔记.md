---
tags:
  - 计算机/git
---


# 一、核心分支结构（固定规范）

核心分支：2个固定分支，分工明确，避免混乱

- **master 分支**：主干分支，保持稳定、可发布状态，不直接在上面开发，仅通过 PR 合并功能代码

- **dev 分支**：开发分支，所有功能开发、bug修复都在本地 dev 进行，阶段性 push 到远端 dev（作为备份）

分支使用原则：每次开发新功能，都从最新 master 切出新 dev 分支；功能完成合并后，删除本地+远端 dev 分支，下次开发重复此流程。

# 二、核心开发流程（完整闭环）

#### 1 . 准备阶段：
删除旧 dev 分支（若存在），同步最新 master，新建 dev 分支
```cmd
# 1. 删除远端 dev 分支
git push origin --delete dev
# 2. 切回 master 分支
git checkout master
# 3. 删除本地 dev 分支（-d 安全删除，需确认已合并）
git branch -d dev
# 4. 拉取远端 master 最新代码（关键：保证基线最新）
git pull origin master
# 5. 基于最新 master 新建 dev 分支
git checkout -b dev
```

#### 2 . 开发阶段：
在本地 dev 分支开发，小步提交（按规范写 commit），阶段性 push 到远端 dev（备份）
```cmd
# 开发中提交（按 Conventional Commits 规范）
git add .
git commit -m "feat: 新增员工查询接口"  # 功能新增用 feat
git commit -m "fix: 修复查询接口参数错误" # bug修复用 fix
# 阶段性推送到远端 dev（备份）
git push origin dev
```
#### 3 . 合并准备：
功能开发完成后，本地 dev 先 rebase 最新 master，解决冲突（核心避坑步骤）
单人开发可以省略
```cmd
# 1. 切回 master，拉取最新代码
git checkout master
git pull origin master
# 2. 切回 dev，执行 rebase（将 dev 嫁接至最新 master）
git checkout dev
git rebase master
# 3. 解决冲突（若有），解决后继续 rebase
git add .
git rebase --continue
# 4. 强制推送 rebase 后的 dev 到远端（单人场景安全）
git push -f origin dev
```
#### 4 . 合并阶段：
远端发起 PR（dev → master），选择合适的合并策略，合并完成后删除 dev 分支

# 三、PR（Pull Request）核心作用

PR 本质：代码合并的审核/确认流程，单人开发也需使用，核心价值如下：

- 功能确认：作为“功能开发完成”的里程碑，PR 标题/描述记录功能核心内容，形成可追溯记录

- 代码自查：PR 页面直观展示 dev 与 master 的代码差异（diff），比本地 git diff 更清晰，避免低级错误

- 记录留存：合并后留存 PR 编号、功能说明、代码差异，后续回溯时可快速定位功能背景

- 协作适配：提前适应团队流程，后续有合作者时，直接用 PR 做代码审核，无需调整流程

PR 描述建议（结构化，方便回溯）：
      
标题：feat: 员工管理模块CRUD功能交付
      
描述：1. 实现员工查询新增/编辑/删除接口；2. 支持分页/参数校验；3. 本地测试通过，无报错。

# 四、三种 PR 合并策略（按需选择）

核心前提：无论选哪种策略，**本地 dev 先 rebase 最新 master + 解决冲突** 都是必须的（提前避坑）。

|合并策略|核心行为|master 记录形态|适用场景|优缺点|
|---|---|---|---|---|
|Create a merge commit（创建合并提交）|保留 dev 所有 commit，新增一条合并提交|非线性历史（类似“P”形，有分叉）|教学、开源项目（需展示完整开发过程）|优点：保留全部细节；缺点：master 历史杂乱，分叉多|
|Squash and merge（压缩合并）✅ 单人常用|将 dev 所有 commit 压缩成 1 个新 commit|线性历史，1个 commit = 1个完整功能（“P”弧压成一个点）|小模块、简单功能（追求主干整洁）|优点：master 极度整洁，易回溯；缺点：丢失开发过程细节|
|Rebase and merge（变基合并）|将 dev 所有 commit 原样搬运到 master 顶端|线性历史，保留所有小 commit（“P”弧抽至 I 顶端）|复杂模块、核心功能（需复现过程、二次开发）|优点：保留过程细节，历史线性；缺点：commit 较多，需规范提交信息|
灵活运用建议：小模块用 Squash（一笔带过），复杂模块用 Rebase（保留过程）；同一功能全程用一种策略，避免混乱。

# 五、Commit 规范（Conventional Commits）

非 Git 强制要求，但强烈推荐，单人开发成本低、收益高，方便回溯和后续协作。

## 核心前缀（3类足够用）

- **feat**：新增功能（如：feat: 新增员工分页查询接口）

- **fix**：修复 bug（如：fix: 修复删除接口未校验ID的问题）

- **docs**：仅修改文档/注释（如：docs: 补充接口注释）

使用技巧：dev 分支的小 commit 简洁规范即可；PR 合并（尤其是 Squash）时，用规范格式写最终 commit（汇总功能内容）。

# 六、关键辅助模块（可选增强）

## 1. Issues（任务/问题清单）

本质：可跟踪、可关闭的任务卡片，替代零散备忘录，核心作用：

- 记录待办：新增功能、bug、优化点，避免遗忘

- 进度管理：按 Issue 逐个开发，避免开发混乱

- 可追溯：关联 PR，PR 合并后自动关闭 Issue，形成“任务→开发→合并”闭环

## 2. Actions（CI/CD 自动化工具）

核心：代码提交/PR/合并后，自动执行重复工作（机器人），对应 CI/CD 概念：

- **CI（持续集成）**：PR 提交后，自动代码检查、跑测试，避免烂代码合并到 master

- **CD（持续部署）**：合并到 master 后，自动打包、部署，省去手动操作

单人常用场景：PR 自动代码检查、合并后自动生成 CHANGELOG，5分钟可配置，降低犯错概率、节省时间。

## 3. 其他辅助模块

- Projects：可视化任务看板，关联 Issues，管理“待做/开发中/已完成”进度

- Wiki：项目文档库，存放接口文档、部署步骤，替代本地笔记

- Releases：版本发布记录，合并 master 后打版本，附上 CHANGELOG，记录版本交付内容

# 七、核心注意事项（避坑重点）

1. 删除 dev 分支：先删远端 dev，再切到 master 删本地 dev，避免残留

2. rebase 时机：合并前必须先拉远端 master 最新代码，再本地 dev rebase，避免冲突

3. 强制推送：rebase 后需用 git push -f origin dev（单人场景安全，多人协作需谨慎）

4. 策略选择：不追求“统一”，按模块复杂度灵活选 Squash/Rebase，核心是 master 整洁、可追溯

# 八、总结（核心逻辑）

单人 Git 管理的核心：用规范的分支结构（master+dev）、前置 rebase 避坑、灵活的 PR 合并策略，实现“开发高效、历史清晰、可追溯、可扩展”。当前流程（dev开发→rebase→PR→合并→删分支），既是单人最优实践，也是团队协作的标准流程，提前养成习惯，后续协作零成本切换。