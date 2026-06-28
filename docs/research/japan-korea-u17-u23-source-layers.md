# 日韩 U17/U23 青训路径来源层补充

更新时间：2026-06-28

本文件对应 [issue #16](https://github.com/starryjog/football-research-sandbox/issues/16)，用于记录日本、韩国 U17/U23 球员的 `source_layers` 首批补充范围。目标不是一次性补完 92 名球员，而是先建立可校验字段和样本口径，后续按同一结构继续扩展。

## 字段口径

`source_layers` 是球员级可选字段。每条来源层至少说明：

- `type`：来源层类型。
- `label`、`url`、`checked_at`：来源标题、链接和核验日期。
- `confidence`：`high`、`medium`、`low`。
- `fields`：该来源支撑的字段。
- `claim`：该来源能证明的事实边界。

当前允许类型：

| 类型 | 说明 |
| --- | --- |
| `afc-registration` | AFC final registration 或 final squad PDF，优先支撑赛事报名、报名俱乐部、出生日期、位置。 |
| `national-fa-profile` | JFA/KFA 队伍页、名单公告或国家队 profile。 |
| `club-academy-profile` | 俱乐部、青训梯队、军队球队官网或俱乐部公告。 |
| `school-profile` | 高中、大学、校园足球队资料；只有 AFC 报名字段时需标为 `medium` 并写明后续补个体页。 |
| `league-registration` | J.League、K League 当前注册页或转会公告。 |

## 首批样本

| 队伍 | 球员 | 补充层 | 说明 |
| --- | --- | --- | --- |
| Japan U17 | Rei Ono | AFC、JFA、club-academy | 海外梯队报名归属：Bayer 04 Leverkusen U17。 |
| Japan U17 | Aran Sato | AFC、JFA、club-academy | 海外梯队报名归属：RC Strasbourg Alsace。 |
| Japan U17 | Takaya Sekine | AFC、JFA、school | 学校路径样本：Shizuoka Gakuen High School。 |
| Japan U23 | Masataka Kobayashi | J.League、JFA、FC Tokyo、AFC | 职业体系 + 国家队节点，区分当前联赛注册和赛事报名。 |
| Japan U23 | Kaito Tsuchiya | J.League、JFA、AFC | 职业体系 + U21 国家队节点。 |
| Japan U23 | Kosei Ogura | AFC、school | 大学路径样本：Hosei University；个体大学页待补。 |
| Korea Republic U17 | Seung Min Lee | AFC、KFA、school | 高中路径样本：Boin High School。 |
| Korea Republic U17 | Geon Woo Park | AFC、KFA、club-academy | K League 俱乐部 U18 梯队样本：Ulsan HD FC U18。 |
| Korea Republic U23 | Moon Hyunho | AFC、Gimcheon、Portimonense | Gimcheon Sangmu 与 Portimonense 来源分层保留，状态改为 `mixed-source` 待复核。 |
| Korea Republic U23 | Bae Hyunseo | K League、AFC | 当前 K League 注册 + U23 赛事报名。 |
| Korea Republic U23 | Lee Chanouk | AFC、Gimcheon | 军队球队报名归属样本：Gimcheon Sangmu；个体页待补。 |
| Korea Republic U23 | Kim Taewon | J.League、AFC | 日本联赛注册的韩国 U23 留洋样本。 |
| Korea Republic U23 | Kim Yonghak | Portimonense、AFC | 葡萄牙注册的韩国 U23 留洋样本。 |

## 后续可做

- 为所有 U17/U23 AFC 终报名球员补至少一条 `afc-registration`。
- 给日韩 U17 的海外梯队样本补个体俱乐部 profile，而不是只依赖 AFC club 字段。
- 给日本 U23 大学样本补大学队个体页或全队 roster 页。
- 给韩国 U23 Gimcheon Sangmu 样本补 K League 或俱乐部个体页，降低 organization-level source 的 `low` confidence。
- 对 Moon Hyunho 的 Gimcheon/Portimonense 双来源做同名、注册时点和 URL 误配复核。
- 后续前端如需要展示来源层，可在 player detail 的来源区域聚合 `source_layers`，但当前数据已经进入 `data/site/players.json` 和 SQLite `player_source_layers`。
