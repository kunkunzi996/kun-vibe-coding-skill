# CHANGELOG

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
