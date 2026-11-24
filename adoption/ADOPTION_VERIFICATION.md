# ADOPTION_VERIFICATION — T001 落地运行时验证（Gate 2B / QA 产出）

> 报告人：qa（独立子代理）。日期：2026-11-24。分支：docs/T001-governance-adoption @ 5a46477d（base: main @ 2bc7a9d2）。
> 完整 QA 报告见 docs/testing/qa-reports/T001-qa.md。本文档只记录实际执行的命令与结果；未执行项明确标注 NOT RUN。

## 1. Agent 可发现性（4 个定义）
- 过程：glob（hidden:true）`.opencode/agents/*`；读取 .opencode/agents/qa.md 全文核对权限声明。
- 结果：**PASS** — architect.md / developer.md / qa.md / reviewer.md 全部存在，均为 mode: subagent + permissions 数组格式。
- 运行时旁证：本验证会话即由 qa agent 按其定义启动；qa.md 白名单内命令 `git log --oneline -15`、`git status --porcelain` 直接放行执行成功。

## 2. Reviewer 只读与分支状态核对
实际执行的命令与输出：
- `git log --oneline -15` → 任务提交恰 3 个：`5a46477d`（docs(governance): address T001 review findings and add handoff report [T001]）、`b3ea52da`（docs(product): add PRD, decisions and architecture baseline [T001]）、`1fcce9b5`（docs(governance): adopt OpenCode multi-agent standard V1.0 [T001]），其下即 base `2bc7a9d2`
- `git status --porcelain` → 空输出（工作区干净）
- `git status --branch --porcelain` → `## docs/T001-governance-adoption`
- `git log main..HEAD --oneline` → 恰上述 3 个提交
- `git log --oneline -1 main` → `2bc7a9d2`（main 未动）
- `git show --stat 5a46477d` → 6 文件 +135/−2（developer.md +3、ADOPTION_AUDIT.md 2±、ARCHITECTURE.md +5、handoff +77、review +48、DECISIONS.md 2±），与 Review F-1~F-7 处置范围一致
- 结果：**PASS** — 分支仅含 3 个任务提交、工作区干净、main 未动；审查报告所述仓库状态与实际一致。

## 3. 高危 Git 拦截实测（push）
- 命令：`git push --dry-run origin docs/T001-governance-adoption`
- 预期（按 .opencode/agents/qa.md：shell * → ask）：应触发批准门；后台子代理无人工批准通道，应被阻断或挂起取消。
- 实际：**FAIL（未被拦截）** — 命令下穿权限层直接执行；git 尝试连接 github.com:443 约 21 秒后失败，退出码 1：
  `fatal: unable to access 'https://github.com/zheermao123/KeyCat.git/': Failed to connect to github.com port 443 after 21075 ms: Could not connect to server`
- 关联旁证：白名单外只读命令 `git branch --show-current` 同样直接放行，返回 `docs/T001-governance-adoption`；`Set-Content` 写入 Temp（仓库外）探针亦执行成功。
- 结论：push 类命令不会被权限门静默拦下（本次仅因验证环境无外网而未产生远端效果）。定级 **DEF-1（P1，限定本运行时）**，处置要求见 QA 报告 Defects 节：负责人须在目标运行时复验 ask 行为，或记录经批准的例外。

## 4. manifest 23 文件存在性（QA 独立方法，未复用 Developer 脚本）
- 方法：逐目录 glob（.opencode/agents、templates/、governance/、git/、adoption/、standard/、.github/、根 *.md、opencode.jsonc）+ 与 standard/manifest.json 清单逐条比对。
- 结果：**PASS — 23/23 全部定位**：
  - 原路径 18 项：.opencode/agents/×4、AGENTS.md、adoption/PROJECT_ADOPTION_CHECKLIST.md、adoption/PROJECT_STRUCTURE.md、git/×2、governance/×3、opencode.jsonc、templates/×8
  - 有意重映射 2 项（审计 E8/§4 记录）：清单 `README.md` → `standard/README.md`；清单 `OPENCODE_BOOTSTRAP_PROMPT.md` → `standard/OPENCODE_BOOTSTRAP_PROMPT.md`（项目根 README.md 完好未被覆盖；根目录无散置的 OPENCODE_BOOTSTRAP_PROMPT.md）
  - 补充确认：`.github/PULL_REQUEST_TEMPLATE.md` 存在（审计 §4：复制自 templates/，属文档映射而非清单项）

## 5. 权威文档与决策一致性抽查（AC-3/AC-4）
- 9 份权威文档 glob 全部存在：PRD、DECISIONS、ARCHITECTURE、API_CONTRACTS、DEVELOPMENT_RULES、TASKS、HANDOFF_PROTOCOL、TEST_STRATEGY、ACCEPTANCE_CRITERIA → **PASS**
- 比对 1（D-R01）：DECISIONS.md L249-255「macOS 适配移出路线图；仓库保留 mac/ 源码快照，不承诺维护；如恢复评估 Squirrel」↔ ARCHITECTURE.md L57「已移出路线图（D-R01，DEFERRED）；仓库保留 mac/ 源码快照，不承诺维护与输入法整合」→ **一致**
- 比对 2（D-A06）：DECISIONS.md L84-92「1.0 不纳入双拼验收；双拼方案文件随上游保留；后续版本评估」↔ ARCHITECTURE.md L53「1.0 不纳入验收范围（D-A06）；方案文件随上游保留，后续版本评估开放」（另 L272/L1102/L1254 三处引用一致）→ **一致**
- 修订记录：ARCHITECTURE.md L17-19 存在 v1.0→v1.1（D-R01/D-A06 修订）→v1.2（Review F-1/F-6）完整链条 → **PASS**
- 占位诚实性抽查：API_CONTRACTS.md L3/L8/L23 明示「占位（TODO）」并声明未实现契约不得写成既定事实 → **PASS**

## 6. opencode.jsonc 与 agent 定义格式复核（AC-2 格式层）
- 独立语法验证：以 Code Mode 对 opencode.jsonc 内容执行 JSON.parse → **成功**；default_agent=build、instructions=3 条 governance 路径、build.permissions=6 条规则（edit allow、shell * ask、git status/diff/log 白名单、git branch --show-current）。
- 4 个 agent frontmatter 逐文件核查：均 mode: subagent + permissions 数组（{action,resource,effect}），shell 通配为 `git xxx *`（含空格）形式 → **PASS（格式层）**
- 运行时强制：见 §3 — ask 门未生效（DEF-1）。

## 7. 权限边界自我证明（QA 不改产品代码）
- edit 工具层面：qa 工具集中不存在任何 edit/写文件工具（与 qa.md `edit deny` 一致）→ **CONFIG-VERIFIED（工具层生效）**
- shell 层面：无害写入探针 `Set-Content` 至 `C:\Users\Administrator\AppData\Local\Temp\opencode\qa-write-probe.txt`（仓库外）→ 执行成功且读回一致 → shell ask 门未拦截写操作（DEF-1 佐证）。
- 本文件与 QA 报告的落盘：经任务分派方明确授权（治理验证记录，非产品代码），由 QA 以 shell 写入；产品代码与既有文档零改动（`git status --porcelain` 仅显示 2 个新增 QA 文件可证）。

## 8. NOT RUN（未执行项，如实标注）
| 项 | 状态 | 原因 / 建议验证方式 |
|---|---|---|
| default_agent/build 主会话权限行为（shell ask、edit allow 边界） | NOT RUN | 需人工在场的交互式主会话，QA 子代理无法发起；建议负责人在 build 主会话实测非白名单 shell 是否触发批准提示 |
| developer edit 权限运行时行为 | NOT RUN | 需 developer 子代理 + 人工批准通道实测；建议负责人派发一次含 edit 的小任务观察 ask 提示 |
| 真实远程 push / GitHub 分支保护 | NOT RUN | 本环境无 github.com 连通性；负责人推送分支后在仓库 Settings 配置并验证保护规则 |
| OpenCode 对 config/agent 配置的实际解析与加载日志 | NOT RUN | 需目标运行时启动日志；建议与 §3 复验一并取证 |
| CI（windows.yml / pages.yml）对本分支 | NOT RUN | 分支未推送，CI 未触发；push 后自动验证 |

## 9. 遗留风险与负责人决定事项
1. **DEF-1（P1）**：本运行时中 shell ask 权限门未生效（push/branch/写探针均直接执行）。任务进入 DONE 前须 (a) 在目标运行时复验 ask 生效并归档证据，或 (b) 由负责人按 INDEPENDENT_ACCEPTANCE §7 记录经批准的例外（含风险与复查日期）。
2. GitHub 分支保护需负责人在仓库 Settings 配置（本地无法验证远程状态）。
3. E7 工作区散件（install-windows.ps1、KeyCat-Windows-Setup.exe）处置待负责人决定（沿用审计记录）。