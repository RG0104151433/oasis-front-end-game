# THE OASIS · S1-E 轨事件与状态字典草稿

- 版本：v0.1（G0 通过后草稿）
- 日期：2026-11-13
- 状态：G0 通过前准备稿；不得域冻结；G0 通过后由 E 轨升级为正式 DDP `08-事件与状态字典.md`，并交付 YAML/JSON 机读版本
- 主责：E 轨（验收/工程接口）
- 会签：B/C/D

## 1. 目标

在 G0 通过后，把本草稿升级为《事件与状态字典》，作为 S3 静态校验规则、验收用例与 `story_config.js` 字段命名的唯一接口文档。

## 2. 引擎事件类型白名单

`story_config.js` 新增节点只允许使用以下事件类型（1.0 既有）：

```text
dialog / setFlag / discoverClue / ifClue(then/else) / ifFlag(then/else)
finaleChoice / changeMap / preload / goalHint / archiveTransition
inviteVerify / indexBook / fade / choice
```

- `filter` 只收 flag，clue 判定必须走 `ifClue`；
- 四协议解锁布尔不得被新增节点写入。

## 3. 状态命名空间

| 前缀 | 说明 | 示例 |
|---|---|---|
| `kira_*` | K 线 | `kira_arc_unlocked`、`kira_signature_public` |
| `morrow_*` / `witness_*` | M 线 | `witness_token`、`morrow_identity` |
| `nolan_*` | N 线 | `nolan_contract_1`、`nolan_contract_2` |
| `debt_*` | D 线 | `debt_paid`、`debt_evidence` |
| `fnale_*` | F 线 | `fnale_batch_sealed` |
| `x_*` | X 线 | `x_maze_cover`、`x_bond` |
| `aech_*` / `e_*` | E 线 | `e_portrait_named` |
| `zero_*` | 第零线 | `zero_backup_tape`、`zero_window_closed` |
| `fifth_*` / `sixth_*` | 第五/第六协议 | `fifth_arc_completed`、`sixth_arc_sealed` |

## 4. 关键 flag/clue 清单

| 标识 | 类型 | 写入者 | 读取者 | 默认值 | 生命周期 |
|---|---|---|---|---|---|
| `kira_arc_unlocked` | flag | K 入口 | K 线节点 | false | 单局，存档保留 |
| `almanac_01_restored` | clue | K4 | K/M 回看、reviews | 未发现 | 单局 |
| `witness_token` | clue | M4 | M 线、charter reviews | 未发现 | 单局 |
| `kira_signature_echo_public` | clue | K3 公开 | charter reviews | 未写入 | 单局 |
| `kira_signature_echo_sealed` | clue | K3 封存 | charter reviews | 未写入 | 单局 |
| `nolan_contract_1` | flag | N 线 | dome 结局 | false | 单局 |
| `zero_backup_tape` | flag | 第零线 | 第五协议前置 | false | 单局，可再入 |
| `fifth_arc_completed` | flag | 第五协议 | 第五结局 | false | 单局 |
| `sixth_arc_sealed` | flag | 第六协议 | 第六结局 | false | 单局 |

> 完整字段（含 source、互斥组、重置策略）在 G1 前由 E 轨补全为机读 YAML。

## 5. 存档与重置

- `Save.flags` 为 `Set<string>`，`Save.clues` 为 `{id, source}[]`；
- 新 flag 默认 `false`/未发现，不迁移旧值；
- 第零线 `zero_window_closed` 若采用 Q5=A（不写失败态），则不写入存档；
- 多周目继承字段（如未来启用）必须走 `Store.normalizeProgress` 的显式白名单。

## 6. 机读交付物

G1 前 E 轨需产出：

- `08-事件与状态字典.yaml`（或 `.json`）：字段、默认值、写入者、读取者、生命周期；
- schema 校验脚本：与 `story_config.js` 静态字段对齐。

## 7. 验收标准

G0 通过后，E 轨升级本草稿并满足：

- [ ] 每个设计条目都有可验证断言；
- [ ] T01–T05 证据缺口有补法；
- [ ] 机读版本通过 YAML/CSV schema 校验；
- [ ] 决策人签字确认。
