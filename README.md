# Kun Coding Router V0.6.2：项目流程路由器

这是昆昆子的个人 Vibe Coding 总入口 Skill。

它的核心不是“写一个更大的万能 Skill”，而是做一个**路由器**：根据当前问题判断项目阶段，自动选择最合适的子流程，避免新手每次忘记该用哪个 Skill。

## V0.6.2 关键变化

相比 V0.6.1，本版本做两件事：

1. **轻量融合 Pre-Coding Gate**：新增 `references/03-pre-coding-gate.md`，在正式写代码前检查目录、MVP、致命卡点、Spec、Git 和会话分工。
2. **按真实流程重排编号**：把“开工门禁”放到前期流程中，而不是追加到最后；后续文件顺延，便于新手按目录理解项目阶段。

本版本遵守一个原则：

> Pre-Coding Gate 只负责拦截和路由，不重复 Product Brief、AI-SDD、Git、设计门、架构门和项目洁癖的完整内容。

## 设计目标

用户不需要记住：

- qiaomu-ai-prd
- AI-SDD
- Pre-Coding Gate
- Architecture Gate
- Test First Gate
- Computer-Use E2E Gate
- Project Cleanup Gate
- Backend Architecture Acceptance

用户只要说：

- “帮我开工这个新项目”
- “准备让 Codex 写代码”
- “继续开发当前项目”
- “修这个 bug”
- “帮我跑通验收”
- “保存一下并提交 Git”
- “项目洁癖，准备新开对话”

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
├── assets/                  # 可选：设计图、截图、素材
└── src/ 或 app/              # 代码目录
```

## 使用示例

### 新项目

```text
使用 Kun Coding Router，帮我开工这个新项目：我想做一个个人游戏市场情报中台。
```

Router 应该启用：

- 想法压力测试
- 开工规格
- 开工门禁
- Product Brief
- AI-SDD
- 必要时设计门 / 架构门

### 从设计 / 文档进入 Codex 施工

```text
我已经有设计图和想法了，准备让 Codex 开始写代码。
```

Router 应该启用：

- 开工门禁
- 缺什么就路由到对应 reference
- 通过后再进入 Task Spec / 安全施工

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
kun-coding-router-v0.6.2/
├── SKILL.md
├── README.md
├── CHANGELOG.md
├── RENAME_MAP.md
└── references/
    ├── 00-skill-index.md
    ├── 01-idea-pressure-test.md
    ├── 02-spec-start-qiaomu-inspired.md
    ├── 03-pre-coding-gate.md
    ├── 04-product-brief-mvp.md
    ├── 05-ai-sdd-template.md
    ├── 06-task-spec-template.md
    ├── 07-design-gate.md
    ├── 08-architecture-gate.md
    ├── 09-test-first-gate.md
    ├── 10-codex-safe-construction.md
    ├── 11-computer-use-e2e-gate.md
    ├── 12-verification-git-report.md
    ├── 13-project-cleanup-gate.md
    ├── 14-backend-architecture-acceptance.md
    └── PROJECT_STATE.template.md
```

## 核心原则

SOP 是地图，不是每次都必须从起点走到终点。

Skill 是导航，不是把整张地图塞进当前上下文。

Router 的价值是：让用户不用记 Skill 名字，也能在正确阶段使用正确流程。

V0.6.2 的新增价值是：让项目在“想法 / 设计 / 规格 → 正式施工”之间多一道轻量门禁，避免新手没目录、没 MVP、没 Spec、没 Git 存档就让 Codex 开始堆代码。
