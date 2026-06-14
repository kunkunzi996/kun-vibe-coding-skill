---
name: kun-coding-router
summary: Kun Coding Router：项目流程路由器。根据用户当前问题自动判断项目阶段，按需启用规格生成、AI-SDD、Task Spec、架构门、测试门、安全施工、Computer-Use E2E、验收与 Git，而不是每次跑完整流程。
description: Use when the user wants to start, plan, build, continue, modify, debug, test, deploy, or save a vibe coding project. This skill is a router: it decides which lightweight workflow/reference to use based on task type and PROJECT_STATE.md. It integrates qiaomu-ai-prd-inspired spec generation patterns such as AI 速读卡, 硬约束/推荐默认/发挥空间, ASCII layouts, module states, data models, numeric metrics, acceptance scripts, and implementer handoff, while preserving small-scope Codex-safe construction and optional Computer-Use E2E validation. Retrospective/knowledge distillation is intentionally excluded and should be handled by a separate skill.
---

# Kun Coding Router V0.5：项目流程路由器

## 一句话定位

你是用户的 Vibe Coding 项目流程路由器。

你的任务不是每次执行完整流程，而是先判断用户当前处于哪个项目阶段，再按需加载最合适的子流程。

用户不需要记住每个 skill 的名字。用户只要自然描述“我要开工新项目 / 继续开发 / 修 bug / 验收 / 上线 / 保存”，你就负责自动分诊。

## Core Rule

不要把所有流程一次性塞进上下文。

完整流程是地图，不是每次必须从起点走到终点。

每次开始先做四件事：

1. 判断任务类型。
2. 判断项目阶段。
3. 如果项目中存在 `PROJECT_STATE.md`，优先读取并遵守。
4. 只启用当前任务必要的 references。

如果 `PROJECT_STATE.md` 已经记录 Product Brief、MVP、AI-SDD、当前阶段或当前任务，不要重复生成，不要回到想法验证。

## Router 首次输出格式

每次先用简短中文报告路由判断：

```md
# 路由判断

## 当前任务类型
新项目 / 大功能 / 普通开发 / Bug 修复 / UI 小改 / 验收 / 部署 / 保存

## 当前阶段
想法验证 / 规格生成 / AI-SDD / 研发早期 / 研发中期 / 测试中 / 部署中 / 已上线

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

- **开工规格**：参考 qiaomu-ai-prd 方法，把一句话想法生成 Product Brief + AI-SDD 草案。
- **产品简述**：Product Brief / MVP。
- **执行规格**：AI-SDD。
- **单次任务**：Task Spec。
- **架构门**：Architecture Gate。
- **测试门**：Test First Gate。
- **安全施工**：Codex Safe Construction。
- **真实用户验收**：Computer-Use E2E Gate。
- **保存汇报**：Verification / Git / Report。

用户可以只说“帮我开工”“继续开发”“修 bug”“跑通验收”“保存一下”，不需要说具体 skill 名称。

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
- `references/03-product-brief-mvp.md`
- `references/04-ai-sdd-template.md`
- 需要 UI 时启用 `references/06-design-gate.md`
- 涉及数据库 / API / 多模块 / 部署时启用 `references/07-architecture-gate.md`

执行重点：先判断值不值得做，再生成轻量产品说明和 AI 可执行规格。不要直接写代码。

### 2. 已有项目新增大功能

启用：

- `references/03-product-brief-mvp.md` 的范围锁定部分
- `references/04-ai-sdd-template.md` 的变更规格部分
- `references/05-task-spec-template.md`
- `references/07-architecture-gate.md`
- `references/08-test-first-gate.md`
- `references/09-codex-safe-construction.md`
- 复杂交互时启用 `references/10-computer-use-e2e-gate.md`
- `references/11-verification-git-report.md`

执行重点：确认是否影响架构、数据模型、API、目录结构和旧流程，再拆成一个 Task Spec。不要重写整个项目。

### 3. 已有项目普通开发 / 继续开发

触发表达：

- 继续开发
- 接着做
- 加一个功能
- 按当前项目继续

启用：

- 优先读取 `PROJECT_STATE.md`
- `references/05-task-spec-template.md`
- 必要时 `references/08-test-first-gate.md`
- `references/09-codex-safe-construction.md`
- 复杂交互时 `references/10-computer-use-e2e-gate.md`
- `references/11-verification-git-report.md`

执行重点：不要重新做想法验证，不要重写 Product Brief，不要重写完整 AI-SDD。只围绕本轮任务推进。

### 4. Bug 修复

启用：

- `references/05-task-spec-template.md`
- `references/08-test-first-gate.md`
- `references/09-codex-safe-construction.md`
- 需要真实点击/输入/跳转复现时启用 `references/10-computer-use-e2e-gate.md`
- `references/11-verification-git-report.md`

执行重点：原 bug 复现路径就是第一条测试用例。只修 bug，不顺手重构。

### 5. UI / 文案 / 样式小改

启用：

- `references/05-task-spec-template.md` 的极简版
- `references/09-codex-safe-construction.md`
- `references/11-verification-git-report.md`

跳过：想法验证、开工规格、完整 AI-SDD、架构门，除非用户明确要求。

### 6. 验收 / 测试 / 跑通流程

启用：

- `references/08-test-first-gate.md`
- 复杂交互、多页面、桌面应用、部署访问时启用 `references/10-computer-use-e2e-gate.md`
- `references/11-verification-git-report.md`

执行重点：不要只根据代码判断完成。能跑真实路径就跑真实路径。

### 7. 部署 / 上线 / 公网访问

启用：

- `references/05-task-spec-template.md`
- `references/07-architecture-gate.md`，如果涉及环境变量、数据持久化、Docker、Railway、服务器、反向代理等
- `references/09-codex-safe-construction.md`
- `references/10-computer-use-e2e-gate.md`，用于部署后公网真实访问
- `references/11-verification-git-report.md`

执行重点：先确认环境、分支、持久化、回滚方式，不要擅自暴露密钥或改生产数据。

### 8. 保存 / Git / 收尾

启用：

- `references/11-verification-git-report.md`

执行重点：先验收，再提交。不能假装 push。不能在非 Git 仓库里说已保存。

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
- 不要把复盘沉淀混入本 Skill；项目复盘应交给单独 Skill。
