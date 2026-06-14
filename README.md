# Kun Coding Router V0.5：项目流程路由器

这是昆昆子的个人 Vibe Coding 总入口 Skill。

它的核心不是“写一个更大的万能 Skill”，而是做一个**路由器**：根据当前问题判断项目阶段，自动选择最合适的子流程，避免新手每次忘记该用哪个 Skill。

## V0.5 关键变化

相比 V0.4，本版本新增：

1. 正式改名为 **Kun Coding Router：项目流程路由器**。
2. 新增 `references/00-skill-index.md`，作为技能索引和触发表。
3. 新增 `references/02-spec-start-qiaomu-inspired.md`，吸收 qiaomu-ai-prd 的规格生成方法。
4. AI-SDD 模板加入：AI 速读卡、硬约束/推荐默认/发挥空间、ASCII 页面布局、模块状态、数字化性能指标、验收剧本、开发者交接说明。
5. 增加中文短别名：开工规格、执行规格、单次任务、测试门、真实用户验收等，降低记忆成本。
6. 保持 V0.4 的 Computer-Use E2E Gate。
7. 继续排除“复盘沉淀”，复盘应作为单独 Skill。

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
kun-coding-router-v0.5/
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
    └── PROJECT_STATE.template.md
```

## 核心原则

SOP 是地图，不是每次都必须从起点走到终点。

Skill 是导航，不是把整张地图塞进当前上下文。

Router 的价值是：让用户不用记 Skill 名字，也能在正确阶段使用正确流程。
