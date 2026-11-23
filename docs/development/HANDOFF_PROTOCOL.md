# HANDOFF_PROTOCOL — 任务交接协议入口

**状态：** ACCEPTED
**权威来源：** 本文件是入口索引；协议正文以 `governance/TASK_HANDOFF_PROTOCOL.md` 为唯一权威（单一事实来源，此处不复制内容）。

## 流程速览

```
任务卡（templates/TASK_TEMPLATE.md）
  → Developer 实现 + 自检（Gate 1）
  → 交接报告（templates/HANDOFF_TEMPLATE.md）→ 状态 IMPLEMENTED
  → Reviewer 独立审查（templates/REVIEW_TEMPLATE.md）→ Gate 2A
  → QA 独立验证（templates/QA_REPORT_TEMPLATE.md）→ Gate 2B
  → 主 Agent 汇总证据 → 负责人批准 → DONE（Gate 3）
```

## 归档位置

| 产物 | 位置 |
|---|---|
| 任务卡 | `docs/development/tasks/<TASK-ID>.md` |
| 交接报告 | `docs/development/handoffs/<TASK-ID>-handoff.md` |
| 审查报告 | `docs/development/reviews/<TASK-ID>-review.md` |
| QA 报告 | `docs/testing/qa-reports/<TASK-ID>-qa.md` |
| 任务状态 | `docs/development/TASKS.md` |

## 强制要求

- 交接报告缺失、测试无证据、diff 不清或工作区状态不明 → 不得进入独立验收。
- IMPLEMENTED / IN_REVIEW / IN_VERIFICATION 均不等于 DONE。
- 例外跳过任何关卡须记录批准人、理由、风险与复查日期（governance/INDEPENDENT_ACCEPTANCE.md §7）。
