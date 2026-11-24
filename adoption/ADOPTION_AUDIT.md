# ADOPTION_AUDIT —《OpenCode 多 AI 协同开发标准 V1.0》落地审计报告

**日期：** 2026-11-23
**审计人：** 主 Agent（OpenCode）
**任务：** T001（OPENCODE_BOOTSTRAP_PROMPT.md 第一阶段）
**性质：** 只审计，未修改任何既有项目文件（审计产出的本文件为新增）

## 1. 现状记录

| 检查项 | 结果 |
|---|---|
| 工作区根 `E:\Keycat` | 非 Git 仓库；标准包 23 个文件全部就位（与 standard/manifest.json 清单一致） |
| KeyCat 仓库 `E:\Keycat\KeyCat` | `main` @ `2bc7a9d2`，工作区干净，与 origin/main 同步，无未提交改动 |
| 仓库内既有 AGENTS.md / OpenCode 配置 | 无 → 无覆盖冲突源 |
| 治理加载实证 | 会话已注入 AGENTS.md + 3 份 governance 文件 → `instructions` 配置实际生效 |
| KeyCat CI | `pages.yml`（Pages，main push + 定时）、`windows.yml`（Tauri 构建，main push / tag v* / PR；tag 推送发布 Release） |
| .gitignore | 仓库自带，已排除 `signing/`（签名私钥）、`dist/`、`win/target/` 等 |
| 敏感文件 | 治理层无密钥 |

## 2. OpenCode V2 兼容性核查（对照官方 agents/permissions 文档，2026-11-23）

| 配置项 | 结论 |
|---|---|
| `.opencode/agents/` 复数目录 | 正确（V2 官方位置） |
| `permissions` 数组 `{action,resource,effect}` | V2 正确格式 |
| 规则顺序（宽 deny 在前、git 读例外在后） | 符合「最后匹配胜出、宽规则在前」语义 |
| `default_agent` + `agents.build(mode: primary)` 同 ID 覆盖 | 官方支持 |
| `mode: subagent` ×4 | 正确 |
| 默认基础策略语义 | 核实无误：reviewer 实际只读、developer shell 默认 ask、architect 无 shell |
| 轻微问题（已修正） | `git status*` → `git status *`（官方推荐形式，避免误匹配且兼容无参数调用） |
| 待运行时验证 | Reviewer 只读实测、QA 不改产品代码实测、高风险 Git 拦截实测、4 agent 可发现性 → 见 ADOPTION_VERIFICATION.md |

结论：标准包配置与 OpenCode V2 兼容，仅一处通配符微调，无结构性不兼容。

## 3. 核心结构问题与决策

治理文件原在工作区根 `E:\Keycat`（无版本控制），代码在 `E:\Keycat\KeyCat`。

| | 方案 A：维持分离 | 方案 B：治理入仓 |
|---|---|---|
| 版本控制 | 治理文件无历史 | 全部可追溯 |
| AGENTS.md 路径语义 | 需映射说明 | 与标准包「目标项目根目录」一致 |
| 代价 | 无 | 一次性移动 + 会话目录迁移 |

**负责人决定：方案 B（2026-11-23 批准）。** 治理、模板、配置全部移入仓库，OpenCode 会话工作目录迁至 `E:\Keycat\KeyCat`。

## 4. 文档映射表

| 标准角色 | 落地 | 说明 |
|---|---|---|
| PRD | `docs/product/PRD.md` v1.1.1 | 含已批准修订（D-R01 macOS、帧尺寸、模型校验、DoR 门禁） |
| DECISIONS | `docs/product/DECISIONS.md` v1.1.0 | 含 D-R01 + 5 项补充决策 + 理由字段 + ID 前缀规则 |
| ARCHITECTURE | `docs/architecture/ARCHITECTURE.md` v1.1 | 原 50KB 技术方案稿改造；E1/E2 冲突条款按 D-R01/D-A06 修订 |
| API_CONTRACTS | `docs/architecture/API_CONTRACTS.md` | 占位 TODO，7 个待定义契约 |
| DEVELOPMENT_RULES | `docs/development/DEVELOPMENT_RULES.md` | 引用 git/ 两文件为权威，不复制 |
| TASKS | `docs/development/TASKS.md` | 五态台账 + Phase 0/1 任务索引 |
| HANDOFF_PROTOCOL | `docs/development/HANDOFF_PROTOCOL.md` | 入口文件，权威在 governance/（不复制） |
| TEST_STRATEGY | `docs/testing/TEST_STRATEGY.md` | DRAFT，P0-09 后回填 |
| ACCEPTANCE_CRITERIA | `docs/testing/ACCEPTANCE_CRITERIA.md` | 从 PRD §10 + 门禁抽取 |
| PR 模板 | `.github/PULL_REQUEST_TEMPLATE.md` | 复制自 templates/ |
| 归档目录 | `docs/development/{tasks,handoffs,reviews}/`、`docs/testing/qa-reports/` | Git 不跟踪空目录，随首次归档文件按需创建（tasks/ 已随 T001 任务卡建立） |

## 5. 冲突与差异处理记录

| # | 冲突 | 处置 | 状态 |
|---|---|---|---|
| E1 | 方案稿「macOS 继续维护桌面猫」 vs D-R01 | 以 DECISIONS 为准，ARCHITECTURE.md 修订并记录依据 | 已修订 |
| E2 | 方案稿「双拼第一版验证后开放」 vs D-A06 | 同上 | 已修订 |
| E3 | governance 决策 ID 示例 `D-IME-001` vs 现有 `D-A/D-B/D-C/D-T/D-R` 前缀 | 保留现有体系，DECISIONS.md 头部写明前缀规则 | 已处理 |
| E4 | 分支纪律：此前直接提交 main | 治理生效后任务走分支 + PR；本任务（T001）为首个实践 | 已执行 |
| E5 | 提交格式 | 新提交遵守 `type(scope): summary [TASK-ID]` | 已执行 |
| E6 | tag 推送自动发布 Release | 与 AGENTS.md 发布审批一致，无需修改；P1-11 时再审 | 无需动作 |
| E7 | 工作区散件 `install-windows.ps1`、`KeyCat-Windows-Setup.exe` | 与治理无关，留工作区根，待负责人后续处置 | 待负责人决定 |
| E8 | 标准包 README 与项目 README 同名 | 标准 README 移入 `standard/`，项目 README 未动 | 已处理 |

## 6. 可直接采用 / 需合并 / 需创建

- **可直接采用：** AGENTS.md、governance/×3、agents×4、git/×2、templates/×8、adoption/×2、README（→standard/）、manifest（→standard/）、opencode.jsonc（微调一处）。
- **需合并：** 无——仓库内无既有项目规则或配置，首次落地零覆盖风险。
- **需创建：** 权威文档集（见 §4）+ ADOPTION_VERIFICATION.md（第四阶段）。

## 7. 遗留事项

1. GitHub 分支保护设置需负责人在仓库 Settings 确认/配置（本地无法验证远程状态）。
2. E7 散件处置待定。
3. 权限运行时验证结果见 ADOPTION_VERIFICATION.md。
