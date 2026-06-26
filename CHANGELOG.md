# CHANGELOG

## V0.7.6

分层读档 / 按需写档规则。把已经定义好的 `CONTEXT.md` / `ACCEPTANCE.md` / `DECISIONS.md` 正式纳入稳定的"何时读、何时写"规则，避免 AI 推翻旧决策、误解业务口径或跳过验收，同时不把小改拖成文档工程。不新增模块、不接 Obsidian、不加 `NEXT_ACTIONS.md`。

### 新增

- `SKILL.md`：「Router 必须优先读取的项目文件」改为分层读档——`PROJECT_STATE` / `HANDOFF` 为默认读档；`DECISIONS` / `CONTEXT` / `ACCEPTANCE` 按任务触发读。新增**全 Skill 唯一一份「辅助上下文触发表」**作为单一事实源，其余文件引用它而不复制。低风险小改不强制读这三个文件。
- `references/17-project-setup.md`：新增「辅助上下文文件创建原则」（不默认创建三件套、宁缺空壳）与「辅助上下文写入规则」（DECISIONS / CONTEXT / ACCEPTANCE 各自的写 / 不写清单 + 写档总判据）。新增「写档时机：即时 vs 收尾」一节，划清与 `13-project-cleanup-gate.md` 的边界：重大稳定信息可即时写，零散文档同步仍归洁癖门。

### 调整

- `references/16-task-routing-map.md`：「路由判断总原则」由四件事扩为五件事，新增"按 SKILL.md 触发表判断是否读辅助上下文"，并声明各小节不重复对应关系。大功能 / 普通开发 / Bug 修复 / 验收测试各小节补一行「按需读」指引（验收节优先读 `ACCEPTANCE.md`，Bug 节修复后可回写回归路径到 `ACCEPTANCE.md`）；Project Setup 节补「不默认创建空壳」提示。
- `SKILL.md`：版本号、标题、`short-description`、`description` 升级到 V0.7.6。

## V0.7.5

Git 保存确认小补丁：防止普通开发、Bug 修复或小改完成后，AI 未经用户确认就自动 `commit` / `push`。

### 调整

- `references/12-verification-git-report.md`：Git 保存规则改为默认只输出保存建议，不实际执行 `git add` / `git commit` / `git push`；只有用户明确要求、提前授权，或当前任务本身就是“保存 / Git”时才允许执行。
- `SKILL.md`：「永远遵守」新增 Git commit / push 最高约束：版本保存动作必须先获得用户明确要求或提前授权。

## V0.7.4

强制「用户手动验收指引」，让每轮完成后都告诉用户怎么亲自验收。

### 新增

- `references/12-verification-git-report.md`：完成报告模板新增「你可以这样验收（必填）」块（启动/操作步骤/应该看到/刷新确认/旧功能回归/失败发回什么）；新增「用户手动验收指引规则」分场景填法（前端给点击路径、Bug 给复现+验证、API 给请求示例、部署给公网地址；轻量小改给一两步、跑不动标注「未实际验证」）。
- `SKILL.md`：「永远遵守」新增一条——每轮完成必须给用户手动验收指引，不许只说「已完成 / build 通过」。

## V0.7.3

开工门禁新增「分支策略」强制检查。

### 新增

- `references/03-pre-coding-gate.md`：新增第 6 项检查「本轮是否需要新建分支」。规则：新功能 / 大改开 `feature/xxx` 分支，小修 / 小 bug 走当前分支；强制三选一，未确认不进入施工。同步加入「通过标准」与输出模板的检查项。
- `SKILL.md`：开工门禁说明补充「会强制确认本轮分支策略」。

## V0.7.2

一致性修订，不加新功能。修复 V0.7.1 中路由表与 SKILL/索引之间的几处不一致。

### 修复

- `references/16-task-routing-map.md`：新项目路由补回 `03-pre-coding-gate.md`（进入施工前），与 SKILL.md「进施工前必须过开工门禁」对齐。
- `references/16-task-routing-map.md`：已有项目大功能路由补上 `03-pre-coding-gate.md`（影响页面/数据/API/架构/目录时）。
- `references/16-task-routing-map.md`：Project Setup 默认模板改为 `PROJECT_STATE.minimal.template.md`，与 `00-skill-index.md`、`17-project-setup.md` 一致（新手默认走最小版）。
- `README.md`：从文件树中移除已删除的 `RENAME_MAP.md`。
- `README.md` / `SKILL.md`：清除「设计目标」「回归验收方式」处的编码坏字符。

## V0.7.1

去重与减负，不加新功能。在 V0.6.2 完整底座（references 01-14）之上，把 V0.7 的 4 个新文件接为 15-18，并保留开工门禁。

### 新增

- `references/15-skill-invocation-layer.md`：Skill 调用分层（用户主动调用型 vs Router 调度型）。
- `references/16-task-routing-map.md`：12 类任务的显式路由表（启用/禁止/产物）。
- `references/17-project-setup.md`：项目第一次接入时建立最小上下文档案。
- `references/18-handoff-protocol.md`：跨窗口 / 跨会话交接协议。
- `references/PROJECT_STATE.minimal.template.md`：项目状态最小版（新手默认）。
- `references/HANDOFF.minimal.template.md`：交接最小版（4 项核心）。
- `references/HANDOFF.template.md`：交接完整版（11 项）。

### 调整

- `SKILL.md`：瘦身——删掉重复的完整任务清单（详表归 16）；合并"强制确认哨兵 + 后端验收哨兵"为一个哨兵 + 7 条硬触发；高风险触发面收窄为具体对象；新增轻量路由模式、Router 自降级规则、反面示例；澄清 New Project / Feature Planning / Handoff Flow 是流程别名而非独立文件；保留并接回开工门禁。
- `00-skill-index.md`：新增"分组"列（Flows/Gates/Meta/Setup/模板），补入 15-18 与模板，补 Project Setup / Handoff 触发规则。
- `PROJECT_STATE.template.md`：补入 Project Setup / Handoff 字段，保留开工门禁状态，加版本元信息头。
- `README.md`：更新版本脉络、文件树（18 个 reference）、使用示例（含轻量路由 / 建档 / 交接）。
- 四个模板顶部统一加"生成于 v0.7.1 / 日期 / 模板类型"注释，便于识别老文档。

### 设计原则

- 编号在 V0.6.2 基础上顺延，不打乱已有 01-14。
- 补丁离开底座就残废——本版本是完整可用版，不是补丁包。
- 低风险小改走轻量路由，用户嫌重时自降级，不为流程完整制造负担。

---

## V0.7

调度器化改造：把 Router 从"万能管家"升级为"项目经理型调度器"。

### 新增

- Skill 调用分层、任务路由表、Project Setup、Handoff Protocol 四个能力。

### 设计原则

- Router 负责判断、选择、排序、强制确认、要求产物；具体执行纪律交给对应 references。
- 注：V0.7 当时以补丁包形式存在，编号从 V0.6.1 接续，未含 03 开工门禁；V0.7.1 已修正并整合进 V0.6.2 完整底座。

---

## V0.6.2

### 新增

- 新增 `references/03-pre-coding-gate.md`。
- 在 `PROJECT_STATE.template.md` 中新增“开工门禁状态”。
- 在 Product Brief 模板中新增“MVP 致命卡点”。
- 在 Design Gate 中新增“反高保真沉迷规则”。
- 新增 `RENAME_MAP.md`，说明 v0.6.1 到 v0.6.2 的文件编号变化。

### 调整

- references 编号从“追加顺序”调整为“流程顺序”。
- `SKILL.md` 路由规则更新为 v0.6.2。
- `README.md` 更新文件树和使用示例。
- `00-skill-index.md` 更新自然语言触发规则。

### 设计原则

- 轻量融合，不把视频里的内容写成第二套大 SOP。
- `03-pre-coding-gate.md` 只负责检查与路由，不重复已有 references 的完整内容。
- 小 bug、小文案、小样式调整默认不触发开工门禁。
