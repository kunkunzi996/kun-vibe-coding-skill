# Kun Coding Router V0.6：项目流程路由器

这是昆昆子的个人 Vibe Coding 总入口 Skill。

它的核心不是“写一个更大的万能 Skill”，而是做一个**路由器**：根据当前问题判断项目阶段，自动选择最合适的子流程，避免新手每次忘记该用哪个 Skill。

## V0.6 关键变化

相比 V0.5，本版本新增“项目洁癖 Gate”：

1. 新增 `references/12-project-cleanup-gate.md`，用于功能完成、阶段收尾、部署后或新开对话前的项目内部文档同步。
2. 将“收尾”拆成两层：`11-verification-git-report.md` 负责验收/Git/完成报告，`12-project-cleanup-gate.md` 负责 README、PROJECT_STATE、docs、AGENTS.md/CLAUDE.md 的必要同步。
3. 明确洁癖范围：只处理当前 Codex 代码项目，不默认处理 Obsidian 或个人知识库。
4. 增加“AGENTS.md 不是变更日志”规则，避免 AI 启动上下文被历史流水账污染。
5. 增加“项目状态防腐”思路：代码变了，项目状态、架构说明、运行方式、下一轮入口也要跟着更新。
6. 保留 V0.5 的轻量 Router 结构，不把洁癖流程塞进主 `SKILL.md`。

## V0.6.1 增强：后端验收官 + 强制确认哨兵

针对新手用户"AI 搭完后端骨架就直接堆业务、地基不稳后期返工"的高频问题，本次增量更新：

1. **新增 `references/13-backend-architecture-acceptance.md`（后端验收官）**：
   作为 07 架构门的下游验收门，在写业务前用 7 步验收（规则证据 / 目录责任 / 最小模块演练 / 接口响应 / 框架复用 / 运行证据包 / 收口报告）判断后端骨架能不能作为稳定起点。**不写业务代码，只验收并输出整改指令。**
2. **新增"后端验收哨兵规则"（强制确认模式）**：
   写进 `SKILL.md`。Router 在每次开工前会**主动扫描**项目：发现已有 `routes/`、`services/`、`controllers/` 等后端目录、或用户表达"加功能 / 写业务 / 做登录"等意图时，如果 `PROJECT_STATE.md` 没有"已验收 / 不适用"记录，就**强制暂停**，要求用户三选一确认（走验收 / 跳过 / 已验过）后再施工。**解决新手忘记主动说"帮我验收一下"导致漏检的问题。**
3. **`PROJECT_STATE.template.md` 新增"后端骨架验收状态"字段**：
   作为哨兵的唯一可信源，记录"未验收 / 已验收（日期）/ 不适用 / 跳过验收（日期，用户自担风险）"，避免每次都重复问。

三道保险一起堵：用户嘴说触发 + Router 主动扫描 + PROJECT_STATE 留档。

## 设计目标

用户不需要记住：

- qiaomu-ai-prd
- AI-SDD
- Architecture Gate
- Test First Gate
- Computer-Use E2E Gate

用户只要说：

- “帮我开工这个新项目”
- “继续开发当前项目”
- “修这个 bug”
- “帮我跑通验收”
- “保存一下并提交 Git”

Router 会自动判断当前应该走哪条流程。

## 推荐项目目录

```text
project-root/
├── PROJECT_STATE.md
├── docs/
│   ├── PRODUCT_BRIEF.md
│   ├── AI_SDD.md
│   ├── ARCHITECTURE_SNAPSHOT.md
│   ├── DESIGN_SPEC.md
│   ├── CHANGES.md
│   ├── TROUBLESHOOTING.md
│   └── TASKS/
│       └── TASK_001_xxx.md
└── src/
```

## 使用示例

### 新项目

```text
使用 Kun Coding Router，帮我开工这个新项目：我想做一个个人游戏市场情报中台。
```

Router 应该启用：

- 想法压力测试
- 开工规格
- Product Brief
- AI-SDD
- 必要时设计门 / 架构门

### 研发中期

```text
使用 Kun Coding Router，继续开发当前项目。本轮任务是增加买量测试信息录入功能。
```

Router 应该启用：

- PROJECT_STATE
- Task Spec
- Test First Gate（如涉及表单/数据）
- Safe Construction
- E2E Gate（如涉及多页面交互）
- Verification / Git / Report

### 阶段收尾 / 项目洁癖

```text
使用 Kun Coding Router，本轮功能已经验收通过。请做一次项目洁癖，更新项目状态、必要文档和下一轮 Codex 入口。
```

Router 应该启用：

- Verification / Git / Report
- Project Cleanup Gate

重点：只整理当前项目内部文档和状态，不默认处理 Obsidian。

### 后端骨架验收（V0.6.1 新增）

```text
使用 Kun Coding Router，帮我验收一下后端架构。
```

或者，你忘了说也没关系，**Router 会主动拦你**：

```text
使用 Kun Coding Router，帮我加一个登录功能。
```

如果项目里已经有后端目录，但 PROJECT_STATE 里"后端骨架验收状态"还是"未验收"，Router 会先暂停，弹出三选一确认（走验收 / 跳过 / 已验过），不会直接施工。

走验收时启用：

- Backend Architecture Acceptance Reviewer（后端验收官）
- 通过后再走 Verification / Git / Report 打稳定版本

### Bug 修复

```text
使用 Kun Coding Router，修这个 bug：新增记录后刷新页面数据丢失。
```

Router 应该启用：

- 复现路径
- Test First Gate
- Safe Construction
- Verification

## 文件说明

```text
kun-coding-router-v0.6/
├── SKILL.md
├── README.md
└── references/
    ├── 00-skill-index.md
    ├── 01-idea-pressure-test.md
    ├── 02-spec-start-qiaomu-inspired.md
    ├── 03-product-brief-mvp.md
    ├── 04-ai-sdd-template.md
    ├── 05-task-spec-template.md
    ├── 06-design-gate.md
    ├── 07-architecture-gate.md
    ├── 08-test-first-gate.md
    ├── 09-codex-safe-construction.md
    ├── 10-computer-use-e2e-gate.md
    ├── 11-verification-git-report.md
    ├── 12-project-cleanup-gate.md
    ├── 13-backend-architecture-acceptance.md
    └── PROJECT_STATE.template.md
```

## 核心原则

SOP 是地图，不是每次都必须从起点走到终点。

Skill 是导航，不是把整张地图塞进当前上下文。

Router 的价值是：让用户不用记 Skill 名字，也能在正确阶段使用正确流程。

V0.6 的新增价值是：让项目不只会开工和施工，也会在阶段完成后“防腐”。代码、文档、AI 规则、项目状态和下一轮入口保持一致，避免下一轮 Codex 基于过期信息继续施工。

V0.6.1 的新增价值是：在"开工 → 施工"之间加一道**主动哨兵**。即使新手忘记说"帮我验收一下后端"，Router 也会自己看见、自己拦下、强制三选一确认，把"后端骨架看着像、其实空"的高风险节点固定为一道必经的关口。
