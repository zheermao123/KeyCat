# OpenCode 多 AI 协同开发标准 V1.0

**用途：** 为使用 OpenCode 的软件项目建立可复用的 AI 协作、交接、审查和验收机制。  
**语言：** 制度文档以中文为主；Agent 指令采用中英混合、以明确可执行为准。  
**适用范围：** Windows 桌面软件、Web、移动端、服务端及跨平台项目。

## 1. 本包解决什么问题

本标准将 AI 辅助开发组织为可审计的工程流程：

1. 单一事实来源（Single Source of Truth）
2. 标准任务卡与 Agent 交接协议（Task & Handoff）
3. 独立代码审查、QA 验证和发布门禁（Independent Acceptance）

原则：**一个主 Agent 统筹，专业 Agent 分工，Git 是变更事实记录，CI 和测试证据决定是否通过，人类项目负责人保留关键决策权。**

## 2. 目录说明

```text
OpenCode-Multi-Agent-Development-Standard/
├── README.md
├── OPENCODE_BOOTSTRAP_PROMPT.md
├── AGENTS.md
├── opencode.jsonc
├── .opencode/
│   └── agents/
│       ├── architect.md
│       ├── developer.md
│       ├── qa.md
│       └── reviewer.md
├── governance/
│   ├── SINGLE_SOURCE_OF_TRUTH.md
│   ├── TASK_HANDOFF_PROTOCOL.md
│   └── INDEPENDENT_ACCEPTANCE.md
├── templates/
│   ├── PRD_TEMPLATE.md
│   ├── DECISIONS_TEMPLATE.md
│   ├── ARCHITECTURE_TEMPLATE.md
│   ├── TASK_TEMPLATE.md
│   ├── HANDOFF_TEMPLATE.md
│   ├── REVIEW_TEMPLATE.md
│   ├── QA_REPORT_TEMPLATE.md
│   └── PULL_REQUEST_TEMPLATE.md
├── git/
│   ├── BRANCHING_STRATEGY.md
│   └── COMMIT_CONVENTION.md
└── adoption/
    ├── PROJECT_ADOPTION_CHECKLIST.md
    └── PROJECT_STRUCTURE.md
```

## 3. 如何使用

### 方式 A：把整套标准交给 OpenCode（推荐）

1. 解压本包。
2. 将包内文件复制到目标项目根目录，保留 `.opencode/` 隐藏目录。
3. 如果目标项目已经有 `AGENTS.md` 或 `opencode.jsonc`，**不要覆盖**。先备份，再将本标准内容合并进去。
4. 在 OpenCode 中打开目标项目根目录。
5. 将 `OPENCODE_BOOTSTRAP_PROMPT.md` 的内容作为第一条任务交给 OpenCode。
6. 让 OpenCode 先审计现有仓库并提出适配计划；确认后再让它创建项目文档和治理配置。

### 方式 B：先只试运行

如果不想一次性启用所有规则，可以先复制：
- `AGENTS.md`
- `opencode.jsonc`
- `.opencode/agents/`
- `governance/`
- `templates/`

然后先用一个低风险任务走完整流程。

## 4. 关键约定

- `AGENTS.md` 是项目规则入口，不代替 PRD、架构文档或任务卡。
- `docs/` 下的项目文档是项目事实来源；本包的 `governance/` 是治理制度来源。
- 任务状态 `IMPLEMENTED` 不等于 `DONE`。
- Developer 不得批准自己的代码；Reviewer 必须独立。
- Reviewer 只读；QA 默认不改产品代码。
- OpenCode Agent 使用当前文档中的 `permissions` 有序规则（action/resource/effect）；工具权限不是操作系统沙箱，也不能替代 GitHub 分支保护、CI 或人工审批。
- 任何工具权限配置都应在当前 OpenCode 版本中实际验证。权限匹配规则按 OpenCode 文档定义，避免仅凭提示词实现安全边界。
- 多 Agent 并行时使用独立 Git worktree 或独立克隆；禁止多个 Agent 同时写入同一工作目录。

## 5. 安全与适配提醒

本包提供的是**保守起点**，不是对所有项目都适用的最终权限策略：
- 不在模板中指定模型供应商或模型 ID；由项目负责人按实际可用模型配置。
- 不预设构建、测试命令；必须先由 OpenCode 检查项目实际脚本。
- 不自动允许 `git push`、`git merge`、发布、删除分支或破坏性命令。
- 不覆盖已有项目规范、CI、分支保护或许可证文件。
- Windows 项目应额外检查 PowerShell、路径空格、权限/UAC、签名和安装升级测试。

## 6. 官方文档

配置格式可能随 OpenCode 版本演进。实施前应以官方文档为准：
- https://opencode.ai/docs/agents
- https://opencode.ai/docs/config/
- https://opencode.ai/docs/permissions
- https://opencode.ai/docs/

本包配置依据 2026-11-23 可访问的 OpenCode 官方文档整理。若本机版本字段或权限行为不同，先验证并修正配置，再投入实际开发。
