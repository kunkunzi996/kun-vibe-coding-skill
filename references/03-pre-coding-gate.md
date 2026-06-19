# 03 Pre-Coding Gate：写代码前门禁

## 一句话定位

Pre-Coding Gate 用于正式写代码前，检查项目是否具备开工条件。

它不是传统 PRD，不负责长篇规划。

它只负责判断：**现在能不能进入 Codex 施工；如果不能，缺什么，应该路由到哪个 reference。**

## 核心原则

> 开工门禁只负责拦截和路由，不替代 Product Brief、AI-SDD、Git、设计门、架构门和项目洁癖。

## 使用时机

当用户出现以下表达时启用：

- 帮我开工这个项目。
- 可以开始写代码了。
- 让 Codex 开发。
- 准备进入施工。
- 这个设计稿能不能开始做了。
- 我已经有想法 / 页面 / 原型了，下一步怎么做。
- 新项目第一次进入开发。
- 已有项目新增大功能，且可能影响目录、数据、接口或架构。

## 不使用时机

以下情况默认跳过：

- 小 bug 修复。
- 小样式调整。
- 文案修改。
- 已有明确 Task Spec 的小任务。
- 单文件小修。
- 用户当前只想讨论想法，不准备进入施工。

## 必查 6 项

### 1. 项目目录是否清楚

至少确认：

```text
project-root/
├── PROJECT_STATE.md
├── docs/
│   ├── PRODUCT_BRIEF.md
│   ├── AI_SDD.md
│   ├── ARCHITECTURE_SNAPSHOT.md
│   └── TASKS/
├── src/ 或 app/
└── assets/ 可选
```

要求：

- 文档放 `docs/`。
- 代码放 `src/` 或 `app/`。
- 设计图、截图、素材放 `assets/`，可选。
- 不允许产品文档、设计图、代码全糊在一个目录里。

### 2. MVP 是否锁定

必须回答：

- 第一版只验证什么？
- 第一版必须做什么？
- 第一版明确不做什么？
- 如果这个功能做不通，项目是否还成立？

### 3. 致命卡点是否优先验证

必须找出：

- 本项目最可能失败在哪里？
- 哪个能力一旦做不通，整个项目就不成立？
- MVP 是否优先验证这个卡点？

原则：

> MVP 不是先做一个丑版本，而是先验证最危险的核心闭环。

### 4. 当前版本是否有 Spec

进入施工前，至少应该能找到或准备生成：

- `docs/PRODUCT_BRIEF.md`
- `docs/AI_SDD.md`
- `docs/TASKS/TASK_001_xxx.md`

如果没有，不允许直接写代码，应先回到：

- `04-product-brief-mvp.md`
- `05-ai-sdd-template.md`
- `06-task-spec-template.md`

### 5. Git 是否初始化

如果已经有代码目录，必须检查：

```bash
git status
```

如果不是 Git 仓库，提醒用户先初始化：

```bash
git init
git add .
git commit -m "init project baseline"
```

对新手解释：

> Git commit 就是代码存档。正式施工前先存一个干净档，后面 AI 改坏了才能回退。

### 6. AI 会话角色是否分工

正式开发前，建议至少拆分两个 AI 会话：

1. 产品 / Router 会话  
   负责 MVP、里程碑、Spec、验收标准。

2. 研发 / Codex 会话  
   只负责按 Spec 小步施工，不重新发散需求。

复杂项目可增加：

3. UI 会话  
   负责视觉风格、页面布局、设计基调。

4. QA 验收会话  
   负责对照 Spec 检查功能是否真的完成。

## 设计沉迷拦截

如果用户持续打磨高保真 UI、logo、颜色、动效，但核心功能尚未验证，必须提醒：

> 当前可能陷入设计细节。建议先回到 MVP，验证核心功能和致命卡点，视觉细节放到 v0.2 / v0.3。

## 通过标准

只有满足以下条件，才允许进入施工：

- 目录结构清楚。
- MVP 已锁定。
- 致命卡点已识别。
- 当前 Spec 已存在或正在生成。
- Git 状态清楚。
- 本轮只做 / 不做已写明。

## 不通过时的路由

不要直接施工。

按缺口路由：

| 缺口 | 路由到 |
|---|---|
| 想法是否值得做不清楚 | `01-idea-pressure-test.md` |
| 没有开工规格 | `02-spec-start-qiaomu-inspired.md` |
| 缺 MVP / 第一版边界 | `04-product-brief-mvp.md` |
| 缺 AI-SDD | `05-ai-sdd-template.md` |
| 缺本轮任务规格 | `06-task-spec-template.md` |
| 页面视觉没基调 | `07-design-gate.md` |
| 涉及架构 / API / 数据 / 部署 | `08-architecture-gate.md` |
| 缺测试或验收标准 | `09-test-first-gate.md` |
| 准备让 Codex 改代码 | `10-codex-safe-construction.md` |
| 缺 Git / 验收 / 保存方式 | `12-verification-git-report.md` |

## 输出模板

```md
# Pre-Coding Gate 检查结果

## 是否允许进入施工
允许 / 暂不允许 / 补齐后允许

## 检查项
- 目录结构：通过 / 缺失 / 不适用
- MVP：通过 / 缺失 / 不适用
- 致命卡点：通过 / 缺失 / 不适用
- 当前 Spec：通过 / 缺失 / 不适用
- Git：通过 / 缺失 / 不适用
- AI 会话分工：已建议 / 不适用

## 当前缺口
-

## 下一步路由
-

## 给 Codex 的开工前提醒
-
```

## 禁止行为

- 不允许没有 MVP 就直接开发大功能。
- 不允许没有 Spec 就让 Codex 大改。
- 不允许文档、代码、素材混在一个目录。
- 不允许先沉迷高保真 UI，而不验证核心功能。
- 不允许为了走流程而让小 bug、小文案、小样式改动变重。
