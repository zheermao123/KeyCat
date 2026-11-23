# TASKS — 任务索引与状态台账

**状态定义：** BACKLOG / READY / IN_PROGRESS / IMPLEMENTED / IN_REVIEW / IN_VERIFICATION / BLOCKED / CHANGES_REQUESTED / DONE / CANCELLED（见 governance/TASK_HANDOFF_PROTOCOL.md §3）
**维护责任：** 主 Agent

## 治理与文档任务

| ID | 任务 | 状态 | 分支 | 交付物 |
|---|---|---|---|---|
| T001 | OpenCode 多 AI 协同标准落地（治理入仓 + 权威文档基线） | IN_VERIFICATION | docs/T001-governance-adoption | 治理文件、PRD/DECISIONS/ARCHITECTURE 等权威文档、ADOPTION_AUDIT/VERIFICATION |

## Phase 0：项目审计与技术验证

| ID | 任务 | 状态 | 依赖 | 交付物 |
|---|---|---|---|---|
| P0-00 | 开发环境清点（VS/SDK/CMake/Rust/Node/Python/GPU/磁盘） | BACKLOG | — | 环境缺口清单 |
| P0-01 | 现有仓库审计 | BACKLOG | — | BASELINE.md、EXISTING_ARCHITECTURE.md、REUSE_AND_REFACTOR.md |
| P0-02 | Weasel 审计 | BACKLOG | P0-00 | WEASEL_INTEGRATION.md、WEASEL_BUILD.md |
| P0-03 | librime 审计 | BACKLOG | P0-00 | RIME_INTEGRATION.md |
| P0-04 | Rime Ice 审计 | BACKLOG | P0-00 | RIME_ICE_INTEGRATION.md |
| P0-05 | 许可证审查 | BACKLOG | P0-02~04 | THIRD_PARTY_LICENSES.md、REDISTRIBUTION_REVIEW.md |
| P0-06 | 安装与权限验证 | BACKLOG | P0-02 | INSTALLATION_DESIGN.md + 验证记录 |
| P0-07 | 统一更新机制验证 | BACKLOG | P0-02 | UPDATE_POLICY.md |
| P0-08 | Windows 输入法集成原型 | BACKLOG | P0-02~04, P0-00 | 可运行原型 + POC_RESULTS.md |
| P0-09 | 输入法原版性能基线 | BACKLOG | P0-08 | IME_BASELINE.md、IME_CORPUS.md |
| P0-10 | 角色生成并行预研 | BACKLOG | P0-00（GPU） | TECHNICAL_RESEARCH.md、VISUAL_GUIDELINES.md、CHARACTER_SCHEMA.md |

**Phase 0 顺序**：P0-00 → P0-01 →（P0-02/03/04 并行）→ P0-05 → P0-06/07 → P0-08 → P0-09；P0-10 穿插。
**Phase 0 门禁**：见 `docs/testing/ACCEPTANCE_CRITERIA.md` §2。许可证与输入法集成门禁未通过，不得进入 Phase 1 正式开发。

## Phase 1：KeyCat Input MVP

| ID | 任务 | 状态 | 依赖 |
|---|---|---|---|
| P1-01 | 组件版本锁定和构建 | BACKLOG | Phase 0 门禁 |
| P1-02 | KeyCat 品牌定制 | BACKLOG | P1-01 |
| P1-03 | 候选窗口主题 | BACKLOG | P1-01 |
| P1-04 | 输入方案和词库 | BACKLOG | P1-01 |
| P1-05 | 安装器 | BACKLOG | P1-01~04 |
| P1-06 | 卸载和修复 | BACKLOG | P1-05 |
| P1-07 | 统一更新管理 | BACKLOG | P1-05 |
| P1-08 | 本地备份与恢复 | BACKLOG | P1-04 |
| P1-09 | KeyCat 主程序集成 | BACKLOG | P1-05~07 |
| P1-10 | 性能回归测试 | BACKLOG | P1-01~04, P0-09 |
| P1-11 | 主程序更新通道重建（签名密钥 + 更新源） | BACKLOG | P1-01 |

## Phase 2–5

Phase 2（输入法管理完善）、Phase 3（2D 角色 MVP）、Phase 4（桌面猫整合）、Phase 5（整合稳定化）任务在对应阶段启动前拆解入册。macOS 已移出路线图（D-R01）。

## 阶段完成原则

每个任务必须有：需求编号关联、实际交付物、测试结果、截图或日志、风险与已知问题、对用户数据的影响说明。没有测试证据的任务不能标记完成；未过门禁的代码不得进入稳定发布分支。
