# THE OASIS · S1-B 轨六结局规则真值表草稿

- 版本：v0.1（G0 通过后草稿）
- 日期：2026-11-13
- 状态：G0 通过前准备稿；不得域冻结；G0 通过后由 B 轨升级为正式 DDP `05-六结局规则真值表.md`
- 主责：B 轨（结局/规则/状态）
- 会签：A/C/E

## 1. 目标

在 G0 通过后，把本草稿升级为正式的六结局真值表，作为 `story_config.js` 与验收用例的唯一结局规则输入。

## 2. 六结局定义

| 结局 id | 名称 | 前提 | 排除项 | 兼得 | 优先级 |
|---|---|---|---|---|---|
| inherit | 个人继承 | 三钥 + `palace_memory` 选择继承 | 无 | 可兼得 seen/done | 1 |
| dome | 穹顶托管 | 三钥 + `palace_memory` 选择穹顶 | 无 | 可兼得 seen/done | 2 |
| public_energy | 公共能源 | 三钥 + `palace_memory` 选择公共能源 + 现实证据（D/F） | 无 | 可兼得 seen/done | 3 |
| charter | 玩家公约 | 三钥 + `palace_memory` 选择公约 + K 线署名 echo clue | 无 | 可兼得 seen/done | 4 |
| fifth_protocol | 第五协议 | 首发不实现；G1 预留 `fifth_arc_completed` | 四协议 | — | 5 |
| sixth_protocol | 第六协议 | 首发不实现；G1 预留 `sixth_arc_sealed` | 四协议 | — | 6 |

> 第零线（zero_line）不是结局，是第五协议的前置维护窗口；Q5 推荐“同周目可再入、不写失败态”。

## 3. 状态字段

每个结局在存档中的状态：

```text
entered / chosen / completed / eligible / seen / done
```

- `entered`：进入宫殿并看到该结局选项；
- `chosen`：点击该结局；
- `completed`：播放完结局演出并写入 reviews；
- `eligible`：满足 requirements；
- `seen`：已看过结局文本（回看）；
- `done`：该结局已达成（不可再作为新达成记录）。

## 4. 旧档补算

| 字段缺失 | 默认行为 |
|---|---|
| `fifth_arc_completed` | 视为 false；第五协议不可达成 |
| `sixth_arc_sealed` | 视为 false；第六协议不可达成 |
| `zero_backup_tape` | 视为缺失；第零线维护窗口未关闭 |
| `kira_signature_echo_public/sealed` | 视为未选择；charter reviews 缺一条实体条款 |
| `morrow_witness_token` | 视为未获得；M 线徽记缺失 |

## 5. 红线

- 第五/第六/第零线只能新增 conditions，不改四协议 `requirements`；
- 不向 `inherit`/`dome`/`public_energy`/`charter` 的 requirements 写入任何新字段；
- 旧档补算必须在 `Save.normalizeProgress` 层完成，引擎不直接读缺失字段。

## 6. 验收标准

G0 通过后，B 轨升级本草稿并满足：

- [ ] 六结局公式可判定：任意 flag/clue 组合映射到唯一结局或“未达成”；
- [ ] C01–C03、C06 关闭或明确移出首发；
- [ ] 旧档补算规则与 `Store.normalizeProgress` 对齐；
- [ ] 决策人签字确认。
