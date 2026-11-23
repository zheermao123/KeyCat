# API_CONTRACTS — 模块接口和数据契约

**状态：** 占位（TODO）— Phase 0/1 期间随实现逐步填充
**维护责任：** 对应模块负责人提案；跨模块变更需审查

本文件是 KeyCat 模块间接口与数据格式的唯一权威来源。修改接口必须同步更新本文件。

## 待定义契约清单（TODO）

| ID | 契约 | 定义阶段 | 说明 |
|---|---|---|---|
| AC-CHAR-001 | Character Bundle Schema（角色资源包格式） | Phase 3 前 | 元数据 + 各动作 Sprite Sheet + 帧率 + 帧尺寸 + 动画原点 + 朝向翻转规则 + 资源版本；冻结是 Phase 3 门禁（D-A04） |
| AC-CHAR-002 | 结构化外观特征字段与枚举 | Phase 0-10 | 毛色、花纹、眼睛颜色、脸型、耳朵形态、毛发长度（D-A03） |
| AC-IME-001 | 输入法健康状态查询接口 | Phase 1 | KeyCat 主程序查询 TSF 注册 / 进程 / 测试输入三项状态（D-B05） |
| AC-IME-002 | 组件版本清单格式 | Phase 1 | KeyCat 版本 ↔ Weasel/librime/Rime Ice 上游 commit 映射（D-A08） |
| AC-UPD-001 | 更新清单（latest.json 等价物） | Phase 1 | 自有更新源格式与签名校验方式 |
| AC-AI-001 | Character Studio 本地服务 API | Phase 3 | 主程序 ↔ AI Worker 本地接口，须支持进程隔离与失败降级（D-T05/D-B04） |
| AC-CFG-001 | KeyCat 配置文件格式 | Phase 2 | 配置 schema 与迁移说明 |

## 记录规则

- 每个契约定义后须包含：格式定义、版本化策略、兼容性承诺、迁移说明。
- 未实现的契约保持 TODO 状态，不得写成既定事实。
