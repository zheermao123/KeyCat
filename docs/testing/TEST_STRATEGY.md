# TEST_STRATEGY — 测试策略

**状态：** DRAFT（Phase 0 期间充实；性能基线章节待 P0-09 产出后回填）
**维护责任：** QA 提案；负责人批准关键门禁

## 1. 测试分层

| 层 | 内容 | 工具/环境 | 状态 |
|---|---|---|---|
| 文档/配置检查 | 权威文档一致性、配置格式校验 | 人工 + 脚本 | 可用 |
| 组件构建 | Weasel/librime/Rime Ice 构建 | Phase 0-00 确定的工具链 | 待建 |
| 输入法功能 | PRD §10.1 清单 | 实机 Windows | 待建 |
| 输入法性能 | PRD §10.2 基线与回归 | 原版基线环境（P0-09） | 待建 |
| 角色生成 | PRD §10.3 | 8GB VRAM GPU 环境 | 待建 |
| 系统集成 | PRD §10.4 | Win10/11 实机 | 待建 |

## 2. 输入法性能基线（对应 D-C01–C04，Phase 0-09 交付）

- 基准对象：原版 Weasel + Rime Ice；对照对象：KeyCat Input。
- 语料：500 条（许可确认后固定版本；不允许再分发的语料仅本地保存于 `docs/testing/corpus/`（git 忽略））。
- 指标：首候选响应延迟、候选窗口弹出耗时、内存占用、首选候选命中率、长时间运行稳定性。
- 回归预算：延迟 ≤ 基线 +10%；首选命中率不下降；内存变化记录并解释。
- 固定变量：硬件、Windows 环境、输入方案、初始用户词频、语料版本。
- 交付物：`docs/testing/IME_BASELINE.md`。

## 3. 输入法健康判定（D-B05）

三项联合：TSF 注册有效 + 必要进程正常 + 真实编辑环境测试输入成功。
第三项必须在测试环境完成；禁止未经用户许可向前台应用自动输入。

## 4. 证据标准

遵循 `governance/INDEPENDENT_ACCEPTANCE.md` §4：PASS 须附真实命令、退出码或可核验输出；禁止"应该没问题"。QA 报告模板见 `templates/QA_REPORT_TEMPLATE.md`，报告归档于 `docs/testing/qa-reports/`。

## 5. 缺陷严重级别

P0–P3 定义见 `governance/INDEPENDENT_ACCEPTANCE.md` §3。P0 立即阻断；P1 合并前必须修复。

## TODO

- [ ] P0-09 后回填基线数值与工具命令。
- [ ] Phase 1 前确定安装/升级/卸载自动化测试方案。
- [ ] Phase 3 前确定角色生成质量验收样本集。
