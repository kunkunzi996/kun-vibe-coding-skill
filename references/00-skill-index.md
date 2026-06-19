# 00 Skill Index：技能索引表

这个文件是 Kun Coding Router 的"分诊表"。

目标：用户不用记住每个 Skill 名字，Router 根据自然语言自动选择流程。

本索引还承担一层职责：帮助 Router 判断哪些能力是"用户主动调用型"，哪些是"Router 调度型"。详细分层见 `15-skill-invocation-layer.md`。

V0.7.1 新增"分组"列：把 references 按职责归为四组（Flows 流程 / Gates 关卡 / Meta 元规则 / Setup 接续），方便扩展时找位置。

---

## 分组速览

- **Flows 流程（01-06）**：从想法到单轮任务的规划链（含开工门禁）。
- **Gates 关卡（07-14）**：写代码前后的各种"门"，负责挡住风险。
- **Meta 元规则（15-16）**：Router 自己怎么分层、怎么路由。
- **Setup 接续（17-18）**：项目建档与跨窗口交接。
- **模板（templates）**：PROJECT_STATE / HANDOFF 各有 minimal 与完整两版。

---

## 技能索引

| 分组 | 短别名 | 文件 | 作用 | 典型触发 |
|---|---|---|---|---|
| Flows | 想法压力测试 | `01-idea-pressure-test.md` | 判断想法是否值得做 | 我有个新想法 / 这个项目值得做吗 |
| Flows | 开工规格 | `02-spec-start-qiaomu-inspired.md` | 参考 qiaomu-ai-prd 方法生成规格草案 | 帮我开工 / 生成规格 / 新项目 |
| Flows | 开工门禁 | `03-pre-coding-gate.md` | 正式写代码前检查目录、MVP、致命卡点、Spec、Git、会话分工 | 准备让 Codex 写代码 / 进入施工 / 设计图后不知道下一步 |
| Flows | 产品简述 | `04-product-brief-mvp.md` | 轻量 Product Brief 与 MVP 边界 | 第一版做什么 / 不做什么 |
| Flows | 执行规格 | `05-ai-sdd-template.md` | AI 可执行项目规格书 | 准备给 Codex 开发前 |
| Flows | 单次任务 | `06-task-spec-template.md` | 研发中期单轮施工规格 | 继续开发 / 本轮任务 |
| Gates | 设计门 | `07-design-gate.md` | 页面布局与视觉基调 | 做前端 / 页面不好看 / 仪表盘 |
| Gates | 架构门 | `08-architecture-gate.md` | 判断架构、数据、API、目录风险 | 大功能 / 数据库 / API / 部署 |
| Gates | 测试门 | `09-test-first-gate.md` | 写代码前先定义通过/失败标准 | Bug / 表单 / 数据 / 计算 |
| Gates | 安全施工 | `10-codex-safe-construction.md` | 控制 Codex 小步、少改、可回滚 | 所有改代码任务 |
| Gates | 真实用户验收 | `11-computer-use-e2e-gate.md` | 鼠标键盘/浏览器/桌面端到端验证 | 多页面 / 复杂交互 / 部署后验证 |
| Gates | 保存汇报 | `12-verification-git-report.md` | 验收、Git、完成报告 | 做完了 / 保存一下 / 提交 |
| Gates | 项目洁癖 | `13-project-cleanup-gate.md` | 阶段收尾时同步 README、PROJECT_STATE、docs、AGENTS/CLAUDE 规则和下一轮入口 | 收尾 / 同步文档 / 项目状态 / 新开对话前 |
| Gates | 后端验收官 | `14-backend-architecture-acceptance.md` | 后端骨架搭建完成后验收是否能进入业务开发 | 后端骨架搭完 / 后端越写越乱 / 准备写业务 |
| Meta | Skill 调用分层 | `15-skill-invocation-layer.md` | 定义用户主动调用型与 Router 调度型，避免 Router 越写越胖 | 升级 Router / 梳理 Skill 架构 |
| Meta | 任务路由表 | `16-task-routing-map.md` | 明确不同任务类型应该启用哪些流程、禁止什么、产物是什么 | Router 判断任务类型时 |
| Setup | Project Setup | `17-project-setup.md` | 项目第一次接入 Router 时建立 PROJECT_STATE、CONTEXT、ACCEPTANCE 等档案 | 新项目 / 旧项目首次接入 / 先分类文件夹 |
| Setup | Handoff 协议 | `18-handoff-protocol.md` | 跨窗口 / 跨会话接续，生成 HANDOFF.md | 下次继续 / 新窗口继续 / 先到这里 |
| 模板 | 项目状态模板 | `PROJECT_STATE.minimal / .template.md` | 项目户口本，分最小版与完整版 | Project Setup 时建档 |
| 模板 | 交接模板 | `HANDOFF.minimal / .template.md` | 下班纸条，分最小版与完整版 | 跨窗口交接时生成 |

---

## 自然语言触发规则

### 用户说"新项目 / 想法 / 开工"

优先启用：

1. 想法压力测试
2. 开工规格
3. 开工门禁，进入施工前启用
4. 产品简述
5. 执行规格
6. Project Setup（首次接入时）
7. 必要时设计门 / 架构门

### 用户说"我有设计图 / 高保真图 / 页面方案，下一步怎么开发"

优先启用：

1. 开工门禁
2. 若缺 MVP：产品简述
3. 若缺执行规格：执行规格
4. 若涉及页面：设计门
5. 若涉及数据 / API / 部署：架构门

重点：防止陷入高保真设计，先确认核心闭环能否跑通。

### 用户说"继续开发 / 接着做 / 加功能"

优先启用：

1. PROJECT_STATE
2. HANDOFF（如果存在或用户说新窗口继续）
3. 如果是大功能或首次进入施工：开工门禁
4. 单次任务
5. 测试门（按需）
6. 安全施工
7. 真实用户验收（按需）
8. 保存汇报

### 用户说"修 bug / 不对 / 报错"

优先启用：

1. 单次任务
2. 测试门
3. 安全施工
4. 真实用户验收（按需）
5. 保存汇报

### 用户说"上线 / 部署 / 公网 / 服务器 / Railway / Docker"

优先启用：

1. 单次任务
2. 架构门
3. 安全施工
4. 真实用户验收
5. 保存汇报

### 用户说"收尾 / 同步文档 / 项目状态 / 新开对话前"

优先启用：

1. 保存汇报
2. 项目洁癖
3. Handoff 协议（如准备跨窗口）

只处理当前代码项目内部的文档和状态同步，不默认处理 Obsidian 或个人知识库。

### 用户说"项目先分类 / 建档 / 新窗口要自动接上 / 下次项目开始前"

优先启用：

1. Project Setup
2. PROJECT_STATE.minimal.template.md（默认）/ PROJECT_STATE.template.md（项目大时）
3. Handoff 协议（如涉及跨窗口）

### 用户说"验收 / 跑通 / 像真实用户一样测"

优先启用：

1. 测试门
2. 真实用户验收
3. 保存汇报

### 用户说"后端架构 / 后端骨架 / 后端能不能继续写业务"

优先启用：

1. 架构门，若缺少蓝图
2. 后端验收官
3. 保存汇报，若验收通过

---

## 跳过规则

- 已进入研发中期：跳过想法压力测试、开工规格、完整 Product Brief。
- 只是小 UI / 文案：跳过开工规格、开工门禁、执行规格、架构门，走轻量路由。
- Bug 修复：跳过产品定位，先写复现路径。
- 只是保存：先检查验收和 Git 状态，不要重新规划。
- 只是小改：不强制项目洁癖，除非影响 README、PROJECT_STATE、架构、API、数据模型、运行方式、AI 施工规则或下一轮入口。
- 项目洁癖：只同步项目内部文档，不默认处理 Obsidian。
- 用户明确说"不要重写 PRD / 不要重做规格"：必须遵守。
- Router 不要重复展开子 Skill 的所有细则，只说明"调用谁、为什么、产物是什么"。
