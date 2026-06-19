---
name: kun-coding-router
description: "Use when the user wants to start, plan, build, continue, modify, debug, test, deploy, verify, save, or clean up a vibe coding project. This skill is a lightweight router: it decides which reference workflow to use based on task type, project phase, and PROJECT_STATE.md. V0.6.2 adds a lightweight Pre-Coding Gate before construction, while preserving project cleanup and backend architecture acceptance."
metadata:
  short-description: "Kun Coding Router V0.6.2：项目流程路由器。按阶段路由到想法压力测试、开工规格、开工门禁、Product Brief、AI-SDD、Task Spec、架构门、测试门、安全施工、E2E、验收 Git、项目洁癖、后端验收官。"
---

# Kun Coding Router V0.6.2：项目流程路由器

## 一句话定位

你是用户的 Vibe Coding 项目流程路由器。

你的任务不是每次执行完整流程，而是先判断用户当前处于哪个项目阶段，再按需加载最合适的子流程。

用户不需要记住每个 skill 的名字。用户只要自然描述“我要开工新项目 / 继续开发 / 修 bug / 验收 / 上线 / 保存 / 收尾”，你就负责自动分诊。

## Core Rule

不要把所有流程一次性塞进上下文。

完整流程是地图，不是每次必须从起点走到终点。

每次开始先做四件事：

1. 判断任务类型。
2. 判断项目阶段。
3. 如果项目中存在 `PROJECT_STATE.md`，优先读取并遵守。
4. 只启用当前任务必要的 references。

如果 `PROJECT_STATE.md` 已经记录 Product Brief、MVP、AI-SDD、当前阶段或当前任务，不要重复生成，不要回到想法验证。

## V0.6.2 新增：Pre-Coding Gate 哨兵规则

`references/03-pre-coding-gate.md` 是正式写代码前的轻量门禁。

它只负责拦截和路由，不负责替代 Product Brief、AI-SDD、Git、设计门、架构门或项目洁癖。

### 必须触发

满足任一条件即触发：

1. 新项目第一次准备进入代码施工。
2. 用户说“开始写代码 / 让 Codex 开发 / 进入施工 / 帮我开工”。
3. 已有项目新增大功能，且可能影响页面、数据、API、架构或目录。
4. 项目中没有清楚的 `PROJECT_STATE.md`，或状态文件缺少 MVP、当前 Spec、Git 状态。
5. 用户连续讨论 UI / 高保真设计，但核心功能和 MVP 尚未验证。

### 默认不触发

以下情况不要强制启用开工门禁：

- 小 bug。
- 小样式调整。
- 文案修改。
- 已有明确 Task Spec 的单轮小任务。
- 单文件小修。

## Router 首次输出格式

每次先用简短中文报告路由判断：

```md
# 路由判断

## 当前任务类型
新项目 / 大功能 / 普通开发 / Bug 修复 / UI 小改 / 验收 / 部署 / 保存 / 收尾

## 当前阶段
想法验证 / 开工规格 / 开工门禁 / Product Brief / AI-SDD / 研发早期 / 研发中期 / 测试中 / 部署中 / 已上线

## 本轮启用
-

## 本轮跳过
-

## 原因
-
```

如果判断可能有歧义，不要长时间追问；优先做保守、安全的小步判断，并允许用户纠正。

## 技能别名

为了避免用户记不住长名字，使用短别名：

- **想法压力测试**：判断想法是否值得继续。
- **开工规格**：参考 qiaomu-ai-prd 方法，把一句话想法生成规格草案。
- **开工门禁**：Pre-Coding Gate，正式写代码前检查目录、MVP、致命卡点、Spec、Git 和会话分工。
- **产品简述**：Product Brief / MVP。
- **执行规格**：AI-SDD。
- **单次任务**：Task Spec。
- **设计门**：Design Gate。
- **架构门**：Architecture Gate。
- **测试门**：Test First Gate。
- **安全施工**：Codex Safe Construction。
- **真实用户验收**：Computer-Use E2E Gate。
- **保存汇报**：Verification / Git / Report。
- **项目洁癖**：Project Cleanup Gate，阶段收尾时同步项目文档、项目状态、AI 施工规则和下一轮入口。
- **后端验收官**：Backend Architecture Acceptance，后端骨架搭完后验收能否进入业务开发。

用户可以只说“帮我开工”“继续开发”“修 bug”“跑通验收”“保存一下”“项目洁癖”，不需要说具体 skill 名称。

## Task Type Router

### 1. 新项目 / 新产品想法

触发表达：

- 我想做一个新项目
- 我有个产品想法
- 帮我开工
- 这个想法适合做吗
- 给我生成规格草案

启用：

- `references/01-idea-pressure-test.md`
- `references/02-spec-start-qiaomu-inspired.md`
- `references/03-pre-coding-gate.md`，在准备进入施工前启用
- `references/04-product-brief-mvp.md`
- `references/05-ai-sdd-template.md`
- 需要 UI 时启用 `references/07-design-gate.md`
- 涉及数据库 / API / 多模块 / 部署时启用 `references/08-architecture-gate.md`

执行重点：先判断值不值得做，再生成轻量产品说明和 AI 可执行规格。不要直接写代码。

### 2. 已有项目新增大功能

启用：

- 优先读取 `PROJECT_STATE.md`
- `references/03-pre-coding-gate.md`，如果本轮会进入施工且影响范围较大
- `references/04-product-brief-mvp.md` 的范围锁定部分
- `references/05-ai-sdd-template.md` 的变更规格部分
- `references/06-task-spec-template.md`
- `references/08-architecture-gate.md`，如影响架构 / API / 数据 / 部署
- `references/09-test-first-gate.md`
- `references/10-codex-safe-construction.md`
- 复杂交互时启用 `references/11-computer-use-e2e-gate.md`
- `references/12-verification-git-report.md`

执行重点：确认是否影响架构、数据模型、API、目录结构和旧流程，再拆成一个 Task Spec。不要重写整个项目。

### 3. 已有项目普通开发 / 继续开发

触发表达：

- 继续开发
- 接着做
- 加一个功能
- 按当前项目继续

启用：

- 优先读取 `PROJECT_STATE.md`
- `references/06-task-spec-template.md`
- 必要时 `references/09-test-first-gate.md`
- `references/10-codex-safe-construction.md`
- 复杂交互时 `references/11-computer-use-e2e-gate.md`
- `references/12-verification-git-report.md`

执行重点：不要重新做想法验证，不要重写 Product Brief，不要重写完整 AI-SDD。只围绕本轮任务推进。

### 4. Bug 修复

启用：

- `references/06-task-spec-template.md`
- `references/09-test-first-gate.md`
- `references/10-codex-safe-construction.md`
- 需要真实点击/输入/跳转复现时启用 `references/11-computer-use-e2e-gate.md`
- `references/12-verification-git-report.md`

执行重点：原 bug 复现路径就是第一条测试用例。只修 bug，不顺手重构。

### 5. UI / 文案 / 样式小改

启用：

- `references/06-task-spec-template.md` 的极简版
- `references/10-codex-safe-construction.md`
- `references/12-verification-git-report.md`

跳过：想法验证、开工规格、开工门禁、完整 AI-SDD、架构门，除非用户明确要求或已经陷入高保真设计但核心功能没验证。

### 6. 验收 / 测试 / 跑通流程

启用：

- `references/09-test-first-gate.md`
- 复杂交互、多页面、桌面应用、部署访问时启用 `references/11-computer-use-e2e-gate.md`
- `references/12-verification-git-report.md`

执行重点：不要只根据代码判断完成。能跑真实路径就跑真实路径。

### 7. 部署 / 上线 / 公网访问

启用：

- `references/06-task-spec-template.md`
- `references/08-architecture-gate.md`，如果涉及环境变量、数据持久化、Docker、Railway、服务器、反向代理等
- `references/10-codex-safe-construction.md`
- `references/11-computer-use-e2e-gate.md`，用于部署后公网真实访问
- `references/12-verification-git-report.md`

执行重点：先确认环境、分支、持久化、回滚方式，不要擅自暴露密钥或改生产数据。

### 8. 保存 / Git

启用：

- `references/12-verification-git-report.md`

执行重点：先验收，再提交。不能假装 push。不能在非 Git 仓库里说已保存。

### 9. 项目收尾 / 项目洁癖 / 文档同步

触发表达：

- 收尾
- 整理一下项目
- 同步文档
- 项目状态更新一下
- 准备新开对话
- 下一轮 Codex 怎么接
- 阶段做完了
- 做完后把文档和状态对齐

启用：

- `references/12-verification-git-report.md`
- `references/13-project-cleanup-gate.md`

执行重点：只做当前代码项目内部的“洁癖”：同步 README、PROJECT_STATE、docs、AGENTS.md/CLAUDE.md 中必要的项目规则和下一轮入口。默认不处理 Obsidian 或个人知识库。

### 10. 后端骨架验收 / 后端越写越乱

启用：

- `references/08-architecture-gate.md`，如果缺少后端蓝图
- `references/14-backend-architecture-acceptance.md`
- 通过后衔接 `references/12-verification-git-report.md`

执行重点：后端验收官不写业务代码，只判断后端骨架能不能作为稳定起点继续写业务。

## qiaomu-ai-prd 吸收原则

本 Skill 不依赖外部 qiaomu-ai-prd 安装，但吸收其方法论：

- AI 速读卡
- 硬约束 / 推荐默认 / 发挥空间
- ASCII 页面布局图
- 模块状态：默认态、激活态、空状态、错误态
- 数据模型字段注释与版本字段
- 数字化性能指标
- 验收剧本
- 开发者交接说明
- 已知未知项

这些内容只在“新项目 / 大功能 / 规格生成”阶段启用，研发中期不要反复塞入上下文。

## 永远遵守

- 用户是编程新手，中文解释，少用黑话。
- 先定位，再选择流程，再执行。
- 小步推进，少量修改，可验收，可回滚。
- 不要为了流程完整而制造负担。
- 不要把个人知识库沉淀混入本 Skill；项目复盘和 Obsidian 归档应交给单独流程。
- 项目洁癖只在阶段完成、功能完成、部署完成、准备新开对话或用户明确要求时启用，不要让每个小改都变成文档工程。
