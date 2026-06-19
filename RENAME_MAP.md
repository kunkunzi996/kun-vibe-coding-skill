# V0.6.1 → V0.6.2 文件编号变化

V0.6.2 按流程顺序重新整理 references。

| V0.6.1 | V0.6.2 | 说明 |
|---|---|---|
| `00-skill-index.md` | `00-skill-index.md` | 保持 |
| `01-idea-pressure-test.md` | `01-idea-pressure-test.md` | 保持 |
| `02-spec-start-qiaomu-inspired.md` | `02-spec-start-qiaomu-inspired.md` | 保持 |
| 新增 | `03-pre-coding-gate.md` | 新增开工前门禁 |
| `03-product-brief-mvp.md` | `04-product-brief-mvp.md` | 顺延 |
| `04-ai-sdd-template.md` | `05-ai-sdd-template.md` | 顺延 |
| `05-task-spec-template.md` | `06-task-spec-template.md` | 顺延 |
| `06-design-gate.md` | `07-design-gate.md` | 顺延 |
| `07-architecture-gate.md` | `08-architecture-gate.md` | 顺延 |
| `08-test-first-gate.md` | `09-test-first-gate.md` | 顺延 |
| `09-codex-safe-construction.md` | `10-codex-safe-construction.md` | 顺延 |
| `10-computer-use-e2e-gate.md` | `11-computer-use-e2e-gate.md` | 顺延 |
| `11-verification-git-report.md` | `12-verification-git-report.md` | 顺延 |
| `12-project-cleanup-gate.md` | `13-project-cleanup-gate.md` | 顺延 |
| `13-backend-architecture-acceptance.md` | `14-backend-architecture-acceptance.md` | 顺延 |
| `PROJECT_STATE.template.md` | `PROJECT_STATE.template.md` | 保持 |

## 迁移建议

如果你直接覆盖仓库：

1. 先备份旧 references。
2. 删除旧编号文件，放入本版本文件。
3. 检查 `SKILL.md`、`README.md`、`00-skill-index.md` 中的引用是否已全部更新。
4. 如果已有项目的 `PROJECT_STATE.md` 仍引用旧编号，不用强行改历史记录；只在下一轮项目洁癖时更新“当前入口”。
