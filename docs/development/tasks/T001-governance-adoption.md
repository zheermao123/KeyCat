# Task: T001 — OpenCode 多 AI 协同开发标准落地

## Basic Information
- ID: T001
- Priority: P0
- Status: IN_VERIFICATION
- Assigned Agent: 主 Agent（Developer 角色）
- Reviewer: reviewer（子代理，独立）
- QA: qa（子代理，独立）
- Target milestone: Phase 0 前置
- Branch / Worktree: docs/T001-governance-adoption

## Objective

将《OpenCode 多 AI 协同开发标准 V1.0》按方案 B（治理入仓）安全落地到本仓库：治理文件版本化、权威文档基线建立、OpenCode 配置适配，并通过独立审查与 QA。

## Background and authoritative references
- 来源任务书: `standard/OPENCODE_BOOTSTRAP_PROMPT.md`
- 负责人批准: 方案 B（治理入仓），2026-11-23
- PRD: `docs/product/PRD.md`（同批创建）
- Decision IDs: D-R01、D-A06（ARCHITECTURE 修订依据）；D-T07（个人使用定位）
- Architecture: `docs/architecture/ARCHITECTURE.md` v1.1（原方案稿修订）

## Scope
- 治理文件入仓：AGENTS.md、governance/×3、.opencode/agents/×4、templates/×8、git/×2、adoption/×2、standard/（README/manifest/bootstrap 任务书）
- opencode.jsonc 入仓（shell 通配符按 V2 规范修正）
- 权威文档创建：PRD、DECISIONS、ARCHITECTURE（原稿修订 E1/E2）、API_CONTRACTS、DEVELOPMENT_RULES、TASKS、HANDOFF_PROTOCOL、TEST_STRATEGY、ACCEPTANCE_CRITERIA
- PR 模板：.github/PULL_REQUEST_TEMPLATE.md
- 审计与验证报告：adoption/ADOPTION_AUDIT.md、adoption/ADOPTION_VERIFICATION.md
- 归档目录：docs/development/{tasks,handoffs,reviews}/、docs/testing/qa-reports/

## Out of Scope
- 不修改上游产品代码（win/、web/、mac/、site/、tools/）
- 不修改既有 CI workflow 文件
- 不处理工作区散件（install-windows.ps1、KeyCat-Windows-Setup.exe）
- 不执行合并、发布、tag

## Allowed files / directories
- 新增：AGENTS.md、governance/、.opencode/、templates/、git/、adoption/、standard/、docs/（新增子目录与文件）、.github/PULL_REQUEST_TEMPLATE.md、opencode.jsonc
- 修改：docs/architecture/ARCHITECTURE.md（E1/E2 修订，已批准）

## Dependencies
- 负责人对方案 B 的批准（已获得，2026-11-23）

## Acceptance Criteria
- [ ] AC-1: manifest.json 清单中 23 个标准文件全部存在于仓库对应位置，标准 README 未覆盖项目 README
- [ ] AC-2: opencode.jsonc 与 4 个 agent 文件符合 OpenCode V2 格式（permissions 数组、shell 通配 ` *` 后缀）
- [ ] AC-3: 9 份权威文档存在；未实现内容标 TODO/占位，无伪装为已确认规范的内容
- [ ] AC-4: ARCHITECTURE.md macOS/双拼条款与 D-R01/D-A06 一致，修订有记录
- [ ] AC-5: 提交遵循 COMMIT_CONVENTION 且关联 T001；未直接提交 main
- [ ] AC-6: Reviewer 审查完成（Gate 2A）；QA 验证完成（Gate 2B）
- [ ] AC-7: ADOPTION_VERIFICATION.md 记录真实命令与结果，未验证项如实标注

## Verification Plan
| Check | Command / Procedure | Required evidence |
|---|---|---|
| AC-1 | 逐文件存在性检查（脚本比对） | 文件清单输出 |
| AC-2 | 对照官方文档人工核查 + JSON 解析 | 核查记录 |
| AC-4 | Select-String 检查 macOS/双拼条款 | 匹配结果 |
| AC-5 | git log 检查 | 提交记录 |
| AC-6 | reviewer/qa 子代理独立执行 | 审查与 QA 报告 |
| AC-7 | 本文件 | ADOPTION_VERIFICATION.md |
