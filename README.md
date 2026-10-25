# 绿洲 · OASIS LEGACY

> 一个纯前端（HTML / CSS / 原生 JS + Canvas）的像素风叙事游戏。本仓库把同一项目的**两个阶段**收在一起：
> **1.0 可运行原型**（`小学期网页开发/`）与 **2.0 剧情与设计文档**（`网页游戏开发2.0/`）。
> 原属 `grade-2-at-ruixin` 仓库，因后续要独立迭代而拆出。

- 题材：2135 年，叠楼区（现实）与绿洲（虚拟世界）双世界叙事，带平台跳跃 + 解谜 + 小游戏。
- 技术形态：无构建、无框架、无后端，所有存档在浏览器 `localStorage`，音效由 WebAudio 实时合成。
- 当前状态：1.0 原型缺 5 个核心脚本（见 [已知缺口](#4-已知缺口先读这一段)），补齐后即可运行；2.0 内容定稿基本完成，代码落码只完成 X 线样板。

---

## 1. 两个目录是什么关系

两个文件夹不是两套东西，而是**同一个项目的“实现基线”和“下一版规格”**：

| 阶段 | 目录 | 内容 | 状态 |
| --- | --- | --- | --- |
| 1.0 | `小学期网页开发/` | 小学期 vibe coding 出来的完整前端原型：登录/注册、菜单、设置、关于、帮助、开场、成就图鉴、主线关卡、档案室、索引场、竞速、四个像素小游戏 | 功能走通过，美术与交互粗糙，计划重做；本快照缺 5 个脚本，无法直接启动 |
| 2.0 | `网页游戏开发2.0/剧情和设计文档/` | 重做版的内容与工程规划：完整剧本（主线 + 七条扩展线 + 四结局）、第六剧情线方案、各扩展线的**数据契约**、回归测试清单、批次验收报告 | 剧本与契约已定稿；落码只完成 X 线，其余按批次排期 |

承接关系一句话：**1.0 决定“工程长什么样”，2.0 决定“往里写什么”**。

- 2.0 的落码目标就是 1.0 这套同构工程：`index.html` 按顺序加载脚本 → `js/engine/` 提供引擎 → `js/data/route-*.js` 提供关卡几何 → `js/story_config.js` 提供剧情/地图/对白真值。
- 2.0 文档中的工程路径写的是 `.../oasis/oasis-game-draft/`（另一处开发目录，另有约 137 MB zip 备份），**该工程本体不在本仓库**；本仓库的 1.0 目录是它的前身与结构参照。
- 因此改造时的一般做法：以 `小学期网页开发/` 的引擎与美术管线为骨架，按 2.0 的数据契约往 `story_config.js` / `i18n.js` 里填事件、flag 与中英对白，不推翻 `engine/`。

---

## 2. 目录结构

```
oasis-front-end-game/
├── 小学期网页开发/                     # 1.0 原型（工程本体）
│   ├── index.html                     # 单页入口：登录/注册/菜单/设置/关于/帮助/开场/成就/游戏
│   ├── css/                           # base / screens / world / style（4 个）
│   ├── js/
│   │   ├── main.js                    # 启动入口：new Game(STORY_CONFIG).boot()
│   │   ├── auth.js  intro.js  about.js  achievements.js  scoreboard.js
│   │   ├── audio.js                   # WebAudio 合成音效（无音频文件）
│   │   ├── fx.js                      # 粒子、扫描线等画面氛围
│   │   ├── i18n.js                    # zh / en 字典 + data-i18n 扫描
│   │   ├── core/registry.js           # Svc 注册中心（模块接口登记）
│   │   ├── data/                      # 静态数据：成就、开场、团队、7 条关卡路线几何
│   │   └── engine/                    # 引擎 44 个文件，加载顺序不可打乱
│   └── image/                         # 31 张背景/素材 + platform_parts_512/ 8 个平台部件
│
└── 网页游戏开发2.0/                    # 2.0 规格与内容（无代码工程本体）
    └── 剧情和设计文档/
        ├── THE-OASIS-完整游戏剧本-20261005.md              # 剧本总源（3060 行）
        ├── THE-OASIS-完整游戏剧本-纯剧情版-20261005.md      # 只保留剧情正文
        ├── THE-OASIS-剧情结局重构与第六剧情线方案-20261008.md
        └── 百度搭子新建的（待审查）/new-chat/               # 落码执行文档（批次制）
```

---

## 3. 运行（1.0 原型）

本项目是纯静态页面，但**建议用本地服务器**打开（中文路径、相对资源与后续 fetch 都更稳）：

```bash
cd 小学期网页开发
python -m http.server 8080
# 浏览器访问 http://localhost:8080/
```

或用 Node：

```bash
npx serve 小学期网页开发
```

注意事项：

- 直接双击 `index.html` 走 `file://` 也能加载大部分资源，但推荐 `http://`，避免个别浏览器对本地文件的限制。
- 存档只在本机浏览器里（`localStorage`），换浏览器 / 清缓存即丢进度；测试前先确认当前玩家代号。
- 音效为 WebAudio 合成，首次交互后才会有声音（浏览器自动播放策略）。
- 2.0 文档目录里的 `serve_oasis.js` 是旧开发目录（`oasis-game-draft`）的冒烟测试服务器，根路径硬编码，**不指向本仓库**，需要时改 `root` 再用。

---

## 4. 已知缺口（先读这一段）

1.0 快照**不完整**：`index.html` 引用了但仓库里没有下面 5 个脚本，因此现在打开会报错（`STORY_CONFIG` / `Game` 未定义、`Util` / `Store` / `Shell` / `Settings` 缺失）：

| 缺失文件 | 作用（依据引用与文档推断） | 影响 |
| --- | --- | --- |
| `js/util.js` | 通用工具（`Util.esc` 转义、DOM 便捷函数等） | 关于页、成就页渲染失败 |
| `js/store.js` | 存档层（`Save` / `Store`，每玩家 `save::<user>`、设置、语言） | 登录、设置、进度全挂 |
| `js/settings.js` | 设置界面逻辑（音量、语言、存档管理） | 设置页不可用 |
| `js/shell.js` | 界面外壳/路由（screen 切换、`App` 对象） | 界面无法跳转 |
| `js/story_config.js` | **剧情/地图/对白唯一真值**（`window.STORY_CONFIG`），约数千行 | 没有它游戏无法启动 |

补齐的两条路：

- **找回**：从原开发目录或旧备份（如 `oasis-game-draft.zip`）取回同名文件放进 `js/` 即可，其余文件与它们是配套的。
- **重写**：按 2.0 数据契约重新生成 `story_config.js`（`meta` / `maps` / `dialogues` / `flags`），`util/store/settings/shell` 按现有调用点补最小实现——这也是重做版本来就要做的事。

建议第一步先补齐 `util.js` / `store.js` / `shell.js` / `settings.js` 让外壳能跑，再逐步替换 `story_config.js`。

### 4.2 素材与代码不是同一代（不阻断，但画面会退化）

`image/` 目录与代码对不上：

| 项 | 数量 | 说明 |
| --- | ---: | --- |
| 代码引用到的 `image/` 路径（js 34 + css 14，去重） | 45 | — |
| 其中实际存在的 | 5 | 只有 `art3mis_pixel.png`、`archive_frame_1~3.png` 等少数几张 |
| 引用但缺失的 | **40** | 见下表 |
| 目录里现有位图 | 23 | 另有 `platform_parts_512/` 8 个部件 |
| 其中没被任何代码引用 | 18 | `bg_archive_*`、`bg_dawn`、`bg_cyberspace_oasis_v1`、各 `.webp` 版等 |

引用但缺失的 40 张（按用途）：

- **背景（12）**：`bg_menu`、`bg_room`、`bg_reality_city`、`bg_starfield`、`bg_race_start_light1~3`、`bg_race_to_archive_transition`、`ending_void_bg`、`data_vortex`、`opening_oasis_ad`、`opening_slum_night`
- **CG（3）**：`cg_ending_inherit.jpg`、`cg_halliday_sea.jpg`、`cg_roof_seed`
- **角色（9）**：`player`、`player_big_idle`、`player_big_walk_1/2`、`player_front_id`、`halliday`、`nolan`、`moro_avatar_front`、`monster`
- **地块 / 危险物（6）**：`floor_xxx`、`hazard_oasis_pit_1/2`、`hazard_red_fog_1_a/b/c`
- **平台（5）**：`platform_oasis_gantry_2/3`、`platform_oasis_road_2/3`、`platform_tileset_oasis_2048x1024`
- **小游戏（3）**：`mg_archive_validation_room`、`mg_data_reassembly`、`mg_gold_room`
- **道具（2）**：`sprite_index_book_closed/opened`

影响与处理：

- `js/engine/image-gate.js` 对每张图都挂了 `onerror`，失败只弹一次“资源加载失败”提示，不阻塞流程；绘制侧有 Canvas 程序化兜底（角色、地块、平台都能画），所以**能跑，只是缺立绘/背景/CG**。
- 结论：这批 `image/` 是较新的一代（档案室、记忆井、阳台等场景），而 js/css 里仍留着早期素材名——**代码与素材是两次快照的混合**，不是搬丢的。
- 补齐方式二选一：从旧备份取回同名素材；或改代码把引用指向现有新图（`bg_home_stacks_v2.webp`、`bg_corridor_oasis_v1.png` 等 18 张未使用的新图正好是候选）。

### 4.3 完整性核对结论

- `小学期网页开发/`：100 个文件（`index.html` + css 4 + js 64 + image 31），与迁出前逐一致。
- `网页游戏开发2.0/`：23 个文件（剧本 3 份 + 待审查的落码文档 20 份），第三方绘图技能包已移除（备份见 §8）。
- 缺的都是**原本就没提交进 git** 的脚本与素材，不是搬迁遗漏；可用 `git log`/`git show` 在 grade-2-at-ruixin 历史里追溯到 2026-10-25 的迁出前状态。

---

## 5. 代码组织约定（改造时别踩）

- **无构建、无模块化**：全部是经典 `<script>`，靠 `index.html` 里的顺序建立依赖；没有 import/export，也没有打包步骤。
- **引擎拆分靠 `Object.assign` 挂原型**：`class Game` 定义在 `js/engine/game.js`，其余 `game-*.js` 把方法组挂到 `Game.prototype` 上。`game.js` 必须排在最前，其余按 `index.html` 现有顺序，**不要重排**。
- **数据只注册全局**：`js/data/route-*.js` 仅写 `window.ROUTE_DATA.<name>`，无异步初始化、无 DOM、无网络请求；关卡几何、断口、危险物、锚点都在这层。
- **剧情真值集中在 `story_config.js`**：地图、路线、对白、flag/clue 都在里面；演出与解锁逻辑在 `engine/game-progress.js` 等文件里读它。
- **双语硬约束**：`js/i18n.js` 以 `data-i18n` / `data-i18n-ph` / `data-i18n-title` 驱动，zh / en 各约 309 键；新增文案必须**成对**添加，否则会回退中文或显示 key。
- **美术管线**：背景用 `image/` 位图，地块/平台用 Canvas 程序化绘制（`game-tile-art*.js`、`game-platform-art.js`，平台由 `image/platform_parts_512/` 8 个部件拼接），过场用 `game-archive-transition.js` 的图片门。
- **小游戏**统一走 `minigame-session.js` 会话 + `game-minigames.js` 入口注册，各自实现放在 `minigame-*.js`（扫雷 / 贪吃蛇 / 俄罗斯方块 / 接力防守）。
- **无自动化测试**：验收手段是 2.0 的《回归测试清单》+ 目录里的 `verify_*.js` 脚本（对着 `story_config.js` 做静态断言）。

---

## 6. 后续开发路线

来自 2.0《人类 README》《文档地图与缺口清单》的现状口径：

1. **X 线（三条已实现扩展线）** —— 已完成，作为落码范例。
2. **批次 1：K 线（基拉）+ M 线（莫罗）** —— 契约与对白定稿，交付验收报告已有。
3. **批次 2：D / N / F 线（含 X4 告别）** —— 契约定稿，有 Handoff 文档。
4. **批次 3：E 线** —— 契约定稿。
5. **第零线（第六协议 · 归还）** —— 方案裁定 + 64 组中英对白定稿。

2.0 文档里写死的**红线**（改代码前务必遵守）：

- 不触碰四协议解锁布尔：任何新 flag/clue 只进演出、对白与 reviews，**不得写进** `inherit` / `public_energy` / `charter` 的 `requirements`；`allAlmanac` / `art3misOnline` 等派生状态零改动。
- 事件只用白名单；`filter` 只收 flag，**clue 判定必须走 `ifClue`**。
- 中英双语必须成对写入（`dialogueTree` 与 `dialogueTree_en`）。
- 只改剧情数据面（`story_config.js`，必要时 `i18n.js`），不动 `store.js` 的版本逻辑与 `engine/`。

---

## 7. 文档索引（2.0）

**内容源文档（管“写什么”，一般不修改）**

| 文件 | 一句话 |
| --- | --- |
| `THE-OASIS-完整游戏剧本-20261005.md` | 剧本总源：主线 + 七条扩展线、四协议结局、状态标识、批次顺序 |
| `THE-OASIS-完整游戏剧本-纯剧情版-20261005.md` | 同上，剥离制作与数据内容，只看剧情与对白 |
| `THE-OASIS-剧情结局重构与第六剧情线方案-20261008.md` | 第零线（第六协议）方案：入口规则、Z1–Z4 节拍、结局正文 |

**落码执行文档（管“怎么落码”，批次推进依据）**

| 文件（在 `.../百度搭子新建的（待审查）/new-chat/` 下） | 一句话 |
| --- | --- |
| `THE-OASIS-人类README-20261017.md` | 给项目方的进度与文档使用说明 |
| `THE-OASIS-AI交接README-20261017.md` | 给接手 Agent 的执行手册（含三条铁律） |
| `THE-OASIS-文档地图与缺口清单-20261017.md` | 文档全貌、缺口与已定案口径 |
| `THE-OASIS-20261008方案采纳基线-20261017.md` | 拍板采纳的范围与红线 |
| `THE-OASIS-KM线数据契约-20261017.md` | 批次 1 落码规格 |
| `THE-OASIS-D线 / N线 / F线 / E线数据契约-20261018.md` | 批次 2、3 落码规格 |
| `THE-OASIS-第零线对白-20261017.md` | 第零线 64 组中英对白 |
| `THE-OASIS-第零线数据契约-20261017.md` | 第零线落码规格 |
| `THE-OASIS-回归测试清单-20261018.md` | 每批落码后的回归验收步骤 |
| `THE-OASIS-批次1交付验收报告-KM线-20261018.md` | 批次 1 验收结果 |
| `THE-OASIS-批次2-Handoff-DNF线-20261020.md` | 批次 2 交接说明 |
| `serve_oasis.js` / `verify_*.js` | 旧工程的本地服务器与静态校验脚本 |

---

## 8. 仓库来源与相关仓库

- 本仓库内容原位于 **[grade-2-at-ruixin](https://github.com/RG0104151433/grade-2-at-ruixin)** 的 `小学期网页开发/` 与 `网页游戏开发2.0/`；移出后，那边只保留指向本仓库的说明。
- 2.0 目录下原有的 `opencode若干前端绘图技能（原始）/`（前端绘图技能包，34 MB）与本项目代码无关，已归位到 **[my-skills-hub](https://github.com/RG0104151433/my-skills-hub)**，需要时去那里取；本仓库不再保留副本。
- 同一时期的其他项目：`python-football-project`（足球 Elo 预测）、`my-skills-hub`（Agent Skills 合集）。

## 9. 约定

- 中文资料用中文文件名；剧本/契约类文档保持 `THE-OASIS-<主题>-<日期>.md` 的命名。
- 文档是“定稿再落码”：先改文档、再改 `story_config.js`，避免代码与契约分叉。
- 大文件（背景图、备份 zip）不入库；`image/` 属于工程本体，必须保留。
- 提交信息写清批次与线名（如 `feat(story): 批次2 D线 落码`）。
