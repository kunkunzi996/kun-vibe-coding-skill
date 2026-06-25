# Kun Coding Router V0.7.5：项目流程调度器

这是昆昆子的个人 Vibe Coding 总入口 Skill。

它的核心不是"写一个更大的万能 Skill"，而是做一个**项目经理型调度器**：判断阶段、选择子 Skill、安排顺序、强制确认、要求产物，具体执行纪律交给对应 references。

---

## 版本脉络

- **V0.6.2**：引入开工门禁（Pre-Coding Gate），按真实流程重排编号。
- **V0.7**：调度器化改造，新增 Skill 调用分层、任务路由表、Project Setup、Handoff Protocol。
- **V0.7.1**：去重减负——SKILL.md 继续瘦身、合并安全哨兵、新增轻量路由与自降级、模板拆 minimal/完整、补反面示例。
- **V0.7.2**：一致性修订——路由表补回开工门禁、Project Setup 默认改最小模板、README 去掉已删的 RENAME_MAP、清编码坏字符。
- **V0.7.3**：开工门禁新增「分支策略」强制检查——新功能/大改开 `feature/xxx` 分支，小修/小 bug 走当前分支，未确认不进入施工。
- **V0.7.4**：强制「用户手动验收指引」——每轮完成后必须告诉用户怎么亲自验收（打开哪、点什么、看到什么算成功、刷新查什么、旧功能回归）。
- **V0.7.5（本版）**：新增「Git 保存确认护栏」——完成修改后默认只输出保存建议，不自动 `commit` / `push`；只有用户明确要求、提前授权，或当前任务本身就是“保存 / Git”时才执行。完整清单见 `CHANGELOG.md`。

V0.7.1 在 V0.6.2 完整底座（01-14）之上，把 V0.7 的 4 个新文件接为 15-18，并保留开工门禁。

---

## 核心设计（自 V0.7.1 起）

1. **SKILL.md 再瘦身**：删掉重复的完整任务清单，详表只留在 `references/16-task-routing-map.md`。
2. **两个哨兵合并**：强制确认哨兵 + 后端验收哨兵 → 一个哨兵 + 7 条硬触发；高风险面收窄为具体对象。
3. **轻量路由模式**：UI/文案小改只输出 3 块路由报告。
4. **Router 自降级规则**：用户嫌重时重新输出降级路由，不固执。
5. **反面示例**：SKILL.md 末尾加"错误 vs 正确路由"对照。
6. **澄清幽灵 Skill**：New Project / Feature Planning / Handoff Flow 是流程别名，不是独立文件。
7. **模板拆 minimal/完整**：PROJECT_STATE 与 HANDOFF 各两版，新手默认用最小版。
8. **保留开工门禁**：V0.6.2 的 `03-pre-coding-gate.md` 继续作为正式施工前的轻量门禁。
9. **Git 保存确认护栏**：普通开发完成后只输出 Git 保存建议，不自动 `commit` / `push`；版本保存必须由用户明确要求或提前授权。

> 开工门禁只负责拦截和路由，不重复 Product Brief、AI-SDD、Git、设计门、架构门和项目洁癖的完整内容。

---

## 设计目标

用户不需要记住 qiaomu-ai-prd / AI-SDD / Pre-Coding Gate / Architecture Gate / Test First Gate / Computer-Use E2E / Project Cleanup / Backend Acceptance / Project Setup / Handoff 这些名字。

用户只要说：

- "帮我开工这个新项目"
- "准备让 Codex 写代码"
- "继续开发当前项目"
- "修这个 bug"
- "帮我跑通验收"
- "保存一下并提交 Git"
- "项目洁癖，准备新开对话"
- "下次新窗口怎么接着做"
- "这个项目先帮我分类建档"

Router 会自动判断当前应该走哪条流程。

---

## 推荐项目目录

```text
project-root/
├── PROJECT_STATE.md
├── HANDOFF.md                 # 可选：跨窗口接续时生成
├── CONTEXT.md                 # 可选：项目术语 / 目录责任
├── ACCEPTANCE.md              # 可选：启动、测试、验收路径
├── DECISIONS.md               # 可选：关键设计决策
├── AGENTS.md / CLAUDE.md      # 可选：AI 施工规则
├── docs/
│   ├── PRODUCT_BRIEF.md
│   ├── AI_SDD.md
│   ├── ARCHITECTURE_SNAPSHOT.md
│   ├── DESIGN_SPEC.md
│   ├── CHANGES.md
│   ├── TROUBLESHOOTING.md
│   └── TASKS/
│       └── TASK_001_xxx.md
├── assets/                    # 可选：设计图、截图、素材
└── src/ 或 app/                # 代码目录
```

---

## 使用示例

### 新项目

```text
使用 Kun Coding Router，帮我开工这个新项目：我想做一个个人游戏市场情报中台。
```

Router 应该启用：想法压力测试 → 开工规格 → 开工门禁 → Product Brief → AI-SDD → Project Setup → 必要时设计门 / 架构门。

### 项目第一次接入 / 文件夹分类

```text
使用 Kun Coding Router，这个项目我准备开始长期开发，先帮我把项目状态文件和文档结构分类好。
```

Router 应该启用：Project Setup + PROJECT_STATE 最小模板 + 必要时 CONTEXT / ACCEPTANCE。重点：先建"户口本"，再写业务。

### 研发中期

```text
使用 Kun Coding Router，继续开发当前项目。本轮任务是增加买量测试信息录入功能。
```

Router 应该启用：PROJECT_STATE → 如有 HANDOFF 先读 → Task Spec → Test First Gate（如涉及表单/数据）→ 安全施工 → E2E Gate（如多页面）→ 保存汇报。

### UI 小改（走轻量路由）

```text
帮我把登录按钮颜色改成绿色。
```

Router 应该走轻量路由：只启用 安全施工 + 最小验收，不走规格、门禁、架构门。

### 阶段收尾 / 项目洁癖

```text
使用 Kun Coding Router，本轮功能已经验收通过。请做一次项目洁癖，更新项目状态、必要文档和下一轮入口。
```

Router 应该启用：保存汇报 → 项目洁癖 → 如准备新窗口，Handoff Protocol。

### 跨窗口交接

```text
使用 Kun Coding Router，帮我生成下一轮接手用的 HANDOFF.md。
```

Router 应该启用：Handoff Protocol（默认最小版，复杂任务用完整版）。

### Bug 修复

```text
使用 Kun Coding Router，修这个 bug：新增记录后刷新页面数据丢失。
```

Router 应该启用：复现路径 → Test First Gate → 安全施工 → 保存汇报。

### 后端骨架验收

```text
使用 Kun Coding Router，帮我加一个登录功能。
```

如果项目里已有后端目录但 PROJECT_STATE 里"后端骨架验收状态"还是"未验收"，Router 会先暂停，弹出三选一确认（走验收 / 跳过 / 已验过），不会直接施工。

---

## 文件说明

```text
kun-vibe-coding-skill/
├── SKILL.md
├── README.md
├── CHANGELOG.md
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
    ├── 15-skill-invocation-layer.md
    ├── 16-task-routing-map.md
    ├── 17-project-setup.md
    ├── 18-handoff-protocol.md
    ├── PROJECT_STATE.minimal.template.md
    ├── PROJECT_STATE.template.md
    ├── HANDOFF.minimal.template.md
    └── HANDOFF.template.md
```

---

## 核心原则

SOP 是地图，不是每次都必须从起点走到终点。

Skill 是导航，不是把整张地图塞进当前上下文。

```text
Router 负责判断；
Flow 负责推进；
Skill 负责纪律；
Docs 负责记忆；
Handoff 负责接力；
Acceptance 负责验收。
```

Router 的价值是：让用户不用记 Skill 名字，也能在正确阶段使用正确流程。
