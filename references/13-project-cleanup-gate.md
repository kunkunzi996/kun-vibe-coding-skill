# 13 Project Cleanup Gate：项目洁癖门

## 一句话定位

项目洁癖门用于 Codex 项目阶段收尾。

它不负责复盘人生经验，不负责 Obsidian 归档，不负责个人知识库沉淀。

它只解决一个问题：

> 代码、功能、架构或部署状态已经变化后，项目内部文档、项目状态、AI 施工规则和下一轮入口是否也同步更新？

## 使用时机

当出现以下情况时启用：

- 一个功能完成并通过验收。
- 一个 Bug 修复完成并通过复现路径验证。
- 一个阶段完成，准备进入下一阶段。
- 部署完成或部署方式变化。
- 架构、API、数据模型、目录结构、环境变量、运行命令发生变化。
- 出现新的 Codex 施工禁区、踩坑、必须遵守的项目规则。
- 准备新开对话，需要下一轮 Codex 快速接手。
- 用户明确说：收尾、整理项目、同步文档、项目状态更新、做一次洁癖。

## 不使用时机

以下情况默认不强制启用：

- 纯文案小改。
- 纯样式小改。
- 临时实验，且未进入主线。
- 未验收通过的任务。
- 只是在讨论方案，还没有改项目。
- 用户明确说不要更新文档。

如果小改影响 README、运行方式、项目状态、AI 施工规则或下一轮入口，则仍可启用。

## 核心边界

### 默认只处理当前代码项目

应该处理：

- README.md。
- PROJECT_STATE.md。
- docs/。
- AGENTS.md / CLAUDE.md。
- TASKS / CHANGES / TROUBLESHOOTING 等项目内文档。
- 下一轮 Codex 入口。

不默认处理：

- Obsidian。
- 个人知识库。
- 长期人生复盘。
- 跨项目方法论文章。
- 非项目相关素材。

### 允许标记跨项目经验，但不直接沉淀

如果本轮出现明显可复用经验，只在报告中标记为：

```text
候选沉淀：可以未来整理为通用 SOP / Skill。
```

不要在本 Gate 中展开个人知识库归档。

## 清理对象分层

### 1. PROJECT_STATE.md

检查：

- 当前阶段是否准确。
- 当前任务是否完成。
- 最新已完成是否更新。
- 下一轮 Codex 入口是否明确。
- 本轮只做 / 本轮不做是否仍然有效。
- MVP 范围是否变化。
- 风险 / 待确认是否变化。
- 最近一次 Git 状态是否更新。
- Computer-Use E2E 状态是否更新。
- 开工门禁状态是否需要更新。

PROJECT_STATE 是下一轮 Codex 的接手卡，不是完整历史记录。

### 2. README.md

检查：

- 项目一句话说明是否仍然准确。
- 启动方式是否变化。
- 环境变量是否变化。
- 主要功能说明是否变化。
- 部署方式是否变化。
- 新人是否能根据 README 跑起来。

README 面向人类和第一次接手的 AI，应该短、准、能启动。

### 3. AGENTS.md / CLAUDE.md

如果项目中存在 AGENTS.md 或 CLAUDE.md，检查是否需要更新。

它们是 AI 施工规则手册，不是变更日志。

应该放：

- 禁止事项。
- 项目结构速查。
- 常用命令。
- 高风险提醒。
- 必须遵守的业务规则。
- 重要文档指针。
- 数据/接口/权限的硬边界。

不应该放：

- 某次开发流水账。
- 单次 Bug 复盘长文。
- 已完成任务叙事。
- docs 中已有的详细架构说明。
- 临时想法。
- “今天 / 最近 / 上周”这类相对时间描述。

如果没有 AGENTS.md / CLAUDE.md，不要强制创建；只有当项目已经频繁由 AI 施工，且存在稳定规则或禁区时，才建议创建。

### 4. docs/

按影响范围检查：

- PRODUCT_BRIEF.md：产品目标、MVP、第一版不做是否变化。
- AI_SDD.md：页面、功能、模块、验收剧本是否变化。
- ARCHITECTURE_SNAPSHOT.md：架构、数据流、API、目录结构是否变化。
- DESIGN_SPEC.md：页面布局、交互、视觉规则是否变化。
- TASKS/：本轮 Task Spec 是否需要归档或标记完成。
- CHANGES.md：是否需要记录阶段性变更。
- TROUBLESHOOTING.md：是否出现可复用的故障处理经验。

没有对应文件时，不要为了形式强行创建；只有后续开发确实会受益时才建议创建。

## AGENTS.md 不是变更日志规则

如果发现 AGENTS.md / CLAUDE.md 里已经堆了历史流水账，应建议精简：

- 历史变更移到 CHANGES.md。
- 故障复盘移到 TROUBLESHOOTING.md。
- 任务过程移到 TASKS/。
- 只把“以后必须遵守”的规则留在 AGENTS.md / CLAUDE.md。

原则：

```text
规则留下，历史移走。
硬边界留下，过程移走。
下次施工必须知道的留下，只是本次发生过的移走。
```

## 影响判断表

| 本轮变化 | 优先同步位置 |
|---|---|
| 新增/删除功能 | README、AI_SDD、PROJECT_STATE、CHANGES |
| 修改 MVP 边界 | PRODUCT_BRIEF、AI_SDD、PROJECT_STATE |
| 修改页面或交互 | DESIGN_SPEC、AI_SDD、E2E 路径 |
| 修改架构/目录 | ARCHITECTURE_SNAPSHOT、AGENTS/CLAUDE |
| 修改 API / 数据模型 | ARCHITECTURE_SNAPSHOT、AI_SDD、README（如影响使用） |
| 修改运行命令/环境变量 | README、AGENTS/CLAUDE、PROJECT_STATE |
| 修复关键 Bug | TROUBLESHOOTING、CHANGES、PROJECT_STATE |
| 新增 Codex 禁区 | AGENTS/CLAUDE、PROJECT_STATE |
| 准备新开对话 | PROJECT_STATE、下一轮入口、未处理事项 |

## 执行步骤

### Step 1：确认本轮已验收

先确认 `12-verification-git-report.md` 已完成：

- 本轮做了什么。
- 修改了哪些文件。
- 验收是否通过。
- Git 状态是否清楚。

未验收时，不要急着做项目洁癖。

### Step 2：判断影响范围

根据本轮改动判断需要检查哪些文件：

- 只改 UI 小文案：可能只更新 PROJECT_STATE。
- 改数据模型：必须检查 AI_SDD / ARCHITECTURE_SNAPSHOT。
- 改运行方式：必须检查 README / AGENTS。
- 修高风险 Bug：考虑 TROUBLESHOOTING / AGENTS。

### Step 3：同步项目内文档

优先顺序：

1. PROJECT_STATE.md。
2. README.md。
3. AGENTS.md / CLAUDE.md。
4. docs/ 中受影响文件。
5. TASKS / CHANGES / TROUBLESHOOTING。

先更新“下一轮会立刻用到”的文件，再处理详细文档。

### Step 4：生成下一轮入口

必须让下一轮 Codex 能快速接手：

```text
下一轮建议从这里开始：
- 当前状态：
- 已完成：
- 未完成：
- 继续文件：
- 验收命令：
- 风险提醒：
```

### Step 5：输出项目洁癖报告

使用下方模板。

## 项目洁癖报告模板

```md
# 项目洁癖报告

## 触发原因
- 功能完成 / Bug 修复 / 部署完成 / 新开对话前 / 用户要求 / 其他：

## 本轮影响范围
- 功能：有 / 无
- 架构：有 / 无
- API / 数据模型：有 / 无
- 运行方式 / 环境变量：有 / 无
- AI 施工规则：有 / 无
- 下一轮入口：有 / 无

## 已同步
- PROJECT_STATE：
- README：
- AGENTS.md / CLAUDE.md：
- docs/：
- TASKS / CHANGES / TROUBLESHOOTING：

## 未同步与原因
-

## 建议合并 / 标记过期
-

## 下一轮 Codex 入口
- 当前状态：
- 继续文件：
- 建议任务：
- 验收方式：
- 风险提醒：

## 候选沉淀
- 无 / 可未来整理为通用 SOP 或 Skill：

## 备注
- 默认未处理 Obsidian 或个人知识库。
```

## 最重要的原则

项目洁癖不是把文档写多，而是让项目不腐烂。

宁可少写，也要准确。

宁可标记“未同步原因”，也不要假装所有文档都已经更新。

当前项目内部闭环优先，个人知识库沉淀以后再说。
