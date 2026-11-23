# DEVELOPMENT_RULES — 开发规范

**状态：** ACCEPTED（随项目演进维护）
**维护责任：** 主 Agent 维护；负责人批准规则变更

## 1. 治理规则入口

本文件不重复治理制度。以下文件的规则具有同等约束力：

- `AGENTS.md`（仓库根）：项目级 AI 规则入口
- `governance/SINGLE_SOURCE_OF_TRUTH.md`：单一事实来源
- `governance/TASK_HANDOFF_PROTOCOL.md`：任务分配与交接
- `governance/INDEPENDENT_ACCEPTANCE.md`：独立审查与验收

## 2. Git 规范

权威来源：`git/BRANCHING_STRATEGY.md`、`git/COMMIT_CONVENTION.md`。

要点：
- 不直接在 `main` 开发；每任务专属分支，命名 `feature|fix|docs|chore/<TASK-ID>-短描述`。
- 提交格式 `<type>(<scope>): <summary> [<TASK-ID>]`。
- 未经负责人批准：禁止 force push、reset --hard、clean -fd、历史重写、删远程分支、直接推受保护分支、合并、发布。
- tag 推送会触发 `windows.yml` 发布 Release——tag 创建属发布操作，须负责人批准。

## 3. 构建与测试命令

> 以下命令以仓库实际脚本为准（AGENTS.md §3.5：不得假设，先验证）。

| 对象 | 命令 | 备注 |
|---|---|---|
| Windows 调试 | `cd win && npm ci && npm start` | 需 Node.js 22+、Rust |
| Windows 安装包 | `cd win && npm run dist` | 产物 `win/target/release/bundle/nsis/` |
| CI | `.github/workflows/windows.yml`（构建）、`pages.yml`（Pages） | tag 推送会发布 |

Phase 0-00 环境清点完成后补充：Weasel/librime 构建命令、AI 组件环境、性能基线工具。

## 4. 代码与文档规则

- 只修改任务范围内必要文件；不做未经批准的重构。
- 不改变公开接口、配置格式、数据结构而不更新 `docs/architecture/API_CONTRACTS.md` 与迁移说明。
- 不将密钥、令牌、证书私钥、个人数据写入 Git、日志或测试报告。更新签名私钥置于仓库外（参考上游 `signing/` 惯例，`.gitignore` 已排除）。
- 不记录用户完整输入内容。
- 权威文档变更遵循 `governance/SINGLE_SOURCE_OF_TRUTH.md` §4 冲突处理流程。

## 5. 完成定义

- 代码写完 = `IMPLEMENTED`，不是 `DONE`。
- `DONE` 需：Reviewer 无未解决 P0/P1 + QA 对强制范围 PASS + 门禁满足 + 交接/审查/QA 报告归档（见 `governance/INDEPENDENT_ACCEPTANCE.md`）。
- 报告必须区分 PASS / FAIL / BLOCKED / NOT RUN，禁止以推断替代证据。
