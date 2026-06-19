# 15 Skill Invocation Layer：Skill 调用分层

> V0.7.1 变化：本文件只负责"分层原则"——谁能调谁、谁不能套娃、谁是流程别名。
> 具体每类任务"按什么顺序启用哪些 reference"已统一归到 `16-task-routing-map.md`，本文件不再重复展开调用顺序，避免和 16 撞车。

## 一句话定位

本文件用于把 Kun Coding Router 从“万能管家”升级为“项目经理型调度器”。

核心目标：

> Router 不再亲自承担所有执行细节，而是判断当前任务应该调用哪些子 Skill / reference，以及调用顺序是什么。

---

## 为什么需要分层

如果所有规则都写进 `SKILL.md`，Router 会越来越胖：

- 新项目要问需求；
- 大功能要写规格；
- Bug 要复现；
- 改代码要安全施工；
- 涉及架构要验收；
- 做完要更新文档；
- 换窗口还要交接。

这些规则都对，但不应该全部由 Router 亲自展开。

V0.7 的原则是：

```text
Router 负责判断；
Flow 负责推进；
Skill 负责纪律；
Docs 负责记忆；
Handoff 负责接力；
Acceptance 负责验收。
```

---

## A. 用户主动调用型 Skill（流程别名，不是独立文件）

这些 Skill / Flow 由用户显式触发，主要用于流程编排。

**重要：除了 Kun Coding Router 本体，下面的 Flow 都不是 references 里的独立文件，而是 Router 内部的流程别名。** 用户说出这些名字时，不要去找同名文件，直接按对应路由走。

它们负责回答：

1. 现在处于哪个阶段？
2. 应该进入哪条流程？
3. 应该调用哪些子能力？
4. 最终应该产出什么？

### 包含

| 名称 | 实际指向 | 作用 |
|---|---|---|
| Kun Coding Router | SKILL.md（本体） | 总入口，判断阶段和路由 |
| New Project Flow | 16 表第 1 节 | 新项目从想法到 AI-SDD / Project Setup |
| Feature Planning Flow | 16 表第 2 节 | 大功能规格、范围锁定、任务拆解 |
| Handoff Flow | 18-handoff-protocol.md | 跨窗口交接、生成 HANDOFF.md |

### 规则

- 用户主动调用型 Flow 不应互相套娃调用。
- 它们可以调度 Router 调度型 Skill。
- 它们不应该重复展开所有子 Skill 的完整细则。
- 它们必须输出"启用什么、跳过什么、为什么"。

---

## B. Router 调度型 Skill

这些能力可以由 Router 根据任务类型自动触发。

它们负责具体执行纪律。

### 包含

| 名称 | 文件 | 作用 |
|---|---|---|
| Idea Pressure Test | 01-idea-pressure-test.md | 判断想法是否值得做 |
| Spec Start | 02-spec-start-qiaomu-inspired.md | 生成开工规格草案 |
| Product Brief / MVP | 04-product-brief-mvp.md | 锁定产品目标和 MVP 边界 |
| AI-SDD | 05-ai-sdd-template.md | 生成 AI 可执行规格 |
| Task Spec | 06-task-spec-template.md | 单轮任务规格 |
| Design Gate | 07-design-gate.md | 页面布局、视觉、交互基调 |
| Architecture Gate | 08-architecture-gate.md | 开工前架构蓝图 |
| Test First Gate | 09-test-first-gate.md | 写代码前定义验收 / 测试 |
| Codex Safe Construction | 10-codex-safe-construction.md | 小步修改、少动文件、可回滚 |
| Computer-Use E2E | 11-computer-use-e2e-gate.md | 真实用户路径验收 |
| Verification / Git / Report | 12-verification-git-report.md | 验收、Git、完成报告 |
| Project Cleanup Gate | 13-project-cleanup-gate.md | 项目文档和状态防腐 |
| Backend Acceptance | 14-backend-architecture-acceptance.md | 后端骨架写业务前验收 |
| Project Setup | 17-project-setup.md | 项目第一次接入建档 |
| Handoff Protocol | 18-handoff-protocol.md | 跨窗口交接 |

---

## C. 调用顺序原则

> V0.7.1：各类任务的具体调用顺序（新项目、大功能、Bug、后端验收、新窗口继续等）已统一收纳到 `16-task-routing-map.md`，本文件不再重复列出，避免两处维护、两处对不上。

需要知道"某类任务先做什么、再做什么"时，直接查 16 对应章节。本文件只保留一条总原则：

```text
Router 先判断任务类型与风险 → 再去 16 表取对应顺序 → 只取当前任务那一节，不整表塞入上下文。
```

---

## D. Router 不该做什么

Router 不应该：

- 把所有 references 的内容复制到主上下文；
- 明明是小改，却强行走完整 PRD / AI-SDD；
- 明明是 Bug，却开始重构架构；
- 明明是已有项目，却回到想法验证；
- 明明准备跨窗口，却只写一句“下次继续”；
- 明明后端未验收，却直接写业务；
- 明明用户要求保存，却假装已经 push。

---

## E. Router 应该输出什么

每次路由时，Router 必须输出：

```md
## 当前判断
任务类型：
项目阶段：
风险等级：

## 本轮启用
-

## 本轮跳过
-

## 调用顺序
1.
2.
3.

## 本轮禁止事项
-

## 预计产物
-

## 验收方式
-
```

---

## 最重要原则

Router 的价值不是“自己会所有事”。

Router 的价值是：

> 让用户不用记 Skill 名字，也能在正确阶段触发正确流程。 
