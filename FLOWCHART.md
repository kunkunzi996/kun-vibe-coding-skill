# Kun Coding Router 流程图（V0.7.4）

> 这份文档用图带你看懂整个 skill 怎么运转。GitHub 会自动把下面的 Mermaid 代码渲染成图形。
> 看不懂文字版没关系，先看图，每张图下面都有大白话说明。

---

## 图 1：主流程（Router 收到任务后干什么）

Router 不是「上来就写代码」，而是先判断、再分流、过关卡、最后收口。

```mermaid
flowchart TD
    A[用户说一句话<br/>例如：帮我加个登录功能] --> B{先路由，再执行<br/>判断 6 件事}
    B --> B1[1.任务类型<br/>2.项目阶段<br/>3.有没有 PROJECT_STATE<br/>4.是不是新窗口接续<br/>5.是否触发哨兵/门禁<br/>6.只启用必要 references]

    B1 --> C{已有项目？<br/>有档案文件吗}
    C -- 有 --> C1[先读 PROJECT_STATE.md<br/>HANDOFF.md / AGENTS.md / README]
    C -- 没有/新项目 --> D
    C1 --> D{触发安全关卡了吗}

    D -- 触发强制确认哨兵 --> E[⛔ 暂停<br/>弹三选一，用户不选不动]
    D -- 触发开工门禁 --> F[🚧 过开工门禁<br/>查目录/MVP/Git/分支/会话分工]
    D -- 都没触发 --> G

    E -- 用户确认后 --> G
    F -- 门禁通过 --> G

    G{风险等级？}
    G -- 低风险小改 --> H1[输出『轻量路由报告』<br/>3 块就够]
    G -- 中高风险 --> H2[输出『完整路由报告』<br/>含启用/跳过/产物/验收方式]

    H1 --> I[按 16 表对应任务分流执行]
    H2 --> I
    I --> J[小步施工]
    J --> K[✅ 完成后必给『用户手动验收指引』<br/>打开哪→点什么→看到什么算成功→刷新查什么]
    K --> L{需要收尾/交接吗}
    L -- 阶段完成 --> M[项目洁癖：同步 PROJECT_STATE]
    L -- 换窗口继续 --> N[写 HANDOFF.md 交接纸条]
    L -- 都不需要 --> Z[结束]
    M --> Z
    N --> Z
```

**大白话**：
- 任何任务，Router 都先「判断 6 件事」，不会一上来就写代码。
- 已有项目先「读档」（看 PROJECT_STATE / HANDOFF），避免凭印象瞎猜。
- 中间有两道闸门：**强制确认哨兵**（高风险先暂停）和**开工门禁**（正式施工前体检）。
- 小改走轻量，大改走完整，最后**一定告诉你怎么验收**，需要时再交接。

---

## 图 2：12 类任务分流（每种任务走哪几步）

Router 判断出任务类型后，按 16 表跳到对应那一节，只走需要的文件。

```mermaid
flowchart LR
    R[判断任务类型] --> T1
    R --> T2
    R --> T3
    R --> T4
    R --> T5
    R --> T6
    R --> T7
    R --> T8
    R --> T9
    R --> T10
    R --> T11
    R --> T12

    T1[1.新项目] --> T1x[想法测试→开工规格→<br/>Product Brief→AI-SDD→<br/>Project Setup→开工门禁→设计/架构门]
    T2[2.大功能] --> T2x[读档→范围锁定→变更规格→<br/>Task Spec→开工门禁→架构门→<br/>测试门→安全施工→E2E→验收→洁癖→交接]
    T3[3.普通/继续开发] --> T3x[读档→Task Spec→<br/>测试门→安全施工→验收]
    T4[4.Bug修复] --> T4x[读报错→复现路径=第一条测试→<br/>测试门→安全施工→验收]
    T5[5.UI/文案小改] --> T5x[轻量：极简Spec→<br/>安全施工→最小验收]
    T6[6.验收/测试] --> T6x[测试门→E2E→验收报告→洁癖]
    T7[7.部署上线] --> T7x[读档→架构门→安全施工→<br/>E2E→验收→洁癖]
    T8[8.保存Git] --> T8x[先验收→commit→<br/>push或说明无法push]
    T9[9.项目收尾/洁癖] --> T9x[验收→项目洁癖→<br/>必要时交接]
    T10[10.后端骨架验收] --> T10x[架构门→后端验收官7步→<br/>验收报告。不写业务！]
    T11[11.Project Setup] --> T11x[建最小档案<br/>PROJECT_STATE.minimal]
    T12[12.Handoff交接] --> T12x[写HANDOFF.md：<br/>下一轮入口/必读/风险/禁区]
```

**大白话**：
- 同样是「写东西」，新项目要走一长串规划，改个按钮颜色只走 3 步——**这就是 Router 的价值：该重的重，该轻的轻**。
- 越往下（小改、保存）越短，越往上（新项目、大功能）越完整。

---

## 图 3：三道安全关卡（什么时候会拦住你）

这三道是 skill 的「保险」，专门防止 AI 在不稳的地基上乱写。

```mermaid
flowchart TD
    S[准备动手] --> Q1{强制确认哨兵<br/>有没有触发？}
    Q1 -- 命中任一条 --> STOP1[⛔ 暂停三选一<br/>① 后端没验收就写业务<br/>② 没档案就写业务<br/>③ 大功能没边界<br/>④ 多模块没回归方式<br/>⑤ 高风险破坏操作<br/>⑥ 改高风险面没测试<br/>⑦ 新窗口没读档]
    STOP1 --> WAIT[用户选 1/2/3 前不施工<br/>★ 这道不可降级]
    Q1 -- 没触发 --> Q2

    Q2{开工门禁<br/>要不要过？} -- 新项目首次施工/<br/>大功能/没档案 --> GATE[🚧 6 项体检<br/>1.目录清楚<br/>2.MVP锁定<br/>3.致命卡点<br/>4.有Spec<br/>5.Git初始化<br/>6.分支策略确认]
    GATE --> GATEOK{全过了吗}
    GATEOK -- 缺啥补啥 --> ROUTE[按缺口路由到对应文件]
    GATEOK -- 通过 --> Q3
    Q2 -- 小bug/小文案/单文件小修 --> Q3

    Q3[施工 → 完成] --> ACC[✅ 验收收口<br/>必给『你可以这样验收』<br/>前端给点击路径/Bug给复现+验证/<br/>API给请求示例/部署给公网地址<br/>跑不动要标『未实际验证』]
```

**大白话**：
- **第一道·哨兵**：最硬，碰到 7 种高风险情况直接暂停问你，哪怕你嫌烦也得先确认（唯一不能跳过的）。
- **第二道·开工门禁**：正式写代码前的「体检」，6 项缺哪补哪；小修小补免检。
- **第三道·验收收口**：干完活必须告诉你怎么亲自验，不准只说「已完成」。

---

## 一句话总结整套 skill

> **先判断（什么任务/什么阶段）→ 读档（别瞎猜）→ 过关卡（高风险先暂停）→ 按任务分流（该重则重该轻则轻）→ 小步施工 → 必给验收指引 → 需要时交接。**

这就是「项目经理 + 验收官」式的调度器：它不替你写所有代码，但保证每一步都不跑偏、能验收、能接续。

---

## 文件速查（这些步骤分别在哪个文件）

| 环节 | 对应文件 |
|---|---|
| 总调度 / 判断 | `SKILL.md` |
| 文件清单 | `references/00-skill-index.md` |
| 想法压力测试 | `references/01-idea-pressure-test.md` |
| 开工规格 | `references/02-spec-start-qiaomu-inspired.md` |
| **开工门禁（关卡 2）** | `references/03-pre-coding-gate.md` |
| Product Brief / MVP | `references/04-product-brief-mvp.md` |
| AI-SDD 规格 | `references/05-ai-sdd-template.md` |
| Task Spec | `references/06-task-spec-template.md` |
| 设计门 / 架构门 | `references/07-design-gate.md` / `08-architecture-gate.md` |
| 测试门 | `references/09-test-first-gate.md` |
| 安全施工 | `references/10-codex-safe-construction.md` |
| Computer-Use E2E | `references/11-computer-use-e2e-gate.md` |
| **验收 / Git / 完成报告（关卡 3）** | `references/12-verification-git-report.md` |
| 项目洁癖 | `references/13-project-cleanup-gate.md` |
| 后端验收官 | `references/14-backend-architecture-acceptance.md` |
| Skill 调用分层 | `references/15-skill-invocation-layer.md` |
| **任务路由表（图 2 来源）** | `references/16-task-routing-map.md` |
| Project Setup | `references/17-project-setup.md` |
| Handoff 协议 | `references/18-handoff-protocol.md` |
