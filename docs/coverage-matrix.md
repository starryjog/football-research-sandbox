# 覆盖矩阵

最近维护日期：2026-06-27。

本表说明当前覆盖边界。`已覆盖` 只表示站内某个明确口径阶段性完成，不代表官方全量。

| 领域 | 当前状态 | 覆盖说明 | 主要入口 |
| --- | --- | --- | --- |
| AFC U17 Asian Cup Saudi Arabia 2026 | 已覆盖重点口径 | 中国、日本、韩国 U17 以 AFC 最终报名表为基础，补到赛事结果和中国队决赛摘要；乌兹别克斯坦保留关键对照样本。 | `data/raw/tournaments.json`、`data/raw/players/*.json`、`data/raw/tournament-archive.json` |
| AFC U23 Asian Cup Saudi Arabia 2026 | 已覆盖重点口径 | 中国、日本、韩国 U23 已按 2026 年 1 月赛事终报名名单建档；当前保留 AFC 注册拼写和基础路径。 | `data/raw/players/china-u23-2026.json`、`japan-u23-2026.json`、`korea-republic-u23-2026.json` |
| 亚洲 U 系列赛事档案 | 部分覆盖 | 已覆盖 2020 年后 AFC U23 / U20 / U17 主干赛事，并补入若干历史锚点；逐场明细和阵容完整度按赛事不同而不同。 | `data/raw/tournament-archive.json` |
| 中国 U17 / U23 球员追踪 | 部分覆盖 | 重点围绕 AFC 赛事名单、足球小将相关样本、现有青训路径和外部链接；不是所有同年龄段中国球员全量库。 | `data/raw/players/*.json`、`data/raw/dossiers.json` |
| 日本 U17 / U23 球员追踪 | 部分覆盖 | 赛事报名名单基础较完整，俱乐部路径、留洋路径和后续职业发展仍需继续补。 | `data/raw/players/japan-*.json` |
| 韩国 U17 / U23 球员追踪 | 部分覆盖 | 赛事报名名单基础较完整，海外注册、大学/高中路径和后续职业发展仍需继续补。 | `data/raw/players/korea-republic-*.json` |
| 中国 2026 中超青年样本 | 样本覆盖 | 记录当前一线队 U21 和具备留洋经历的 U23 重点样本；不是中超所有青年球员名单。 | `data/raw/players/china-csl-2026-youth.json` |
| 当前中国球员海外注册样本 | 样本覆盖 | 保留公开可核的当前海外体系样本；签约未生效、试训和传闻不计入当前注册。 | `data/raw/players/china-overseas-current.json`、`data/raw/overseas-history.json` |
| 中日韩留洋历史 | 精选样本 | 按五大联赛、五大联赛国家青训/低级别、欧洲其他、亚洲其他、美洲其他拆 bucket。不是官方全量人数统计。 | `data/raw/overseas-history.json` |
| 五大联赛亚洲教练 | 口径化样本 | 主口径统计执教时所属足协为 AFC 成员协会的教练，广义口径另列土耳其、以色列等边界项。 | `data/raw/big-five-asian-coaches.json` |
| 董路足球小将与 2034 杯专题 | 部分覆盖 | 已拆公开可核批次主干、09 核心样本、荷兰杯名单和 2034 杯赛事平台；逐名档案仍在补。 | `data/raw/projects.json`、`data/raw/dossiers.json` |
| 中国男足青年队教练周期 | 部分覆盖 | 记录可公开核验的青年队周期、主教练、集训和来源；不是全部历史教练库。 | `data/raw/china-men-youth-coaches.json` |
| Transfermarkt 身价 | 部分覆盖 | 仅覆盖已关联 Transfermarkt 个人页且同步到快照的球员；用于展示外部口径，不是官方身价。 | `data/raw/player-market-values.json` |
| SQLite 本地库 | 生成覆盖 | 按当前 JSON 数据生成，供本地查询和后续分析使用；不作为独立维护源。 | `storage/youth-football.sqlite` |

## 明确未覆盖

当前未覆盖或未承诺全量覆盖：

- 所有 AFC 成员协会的 U17 / U20 / U23 完整球员库。
- 所有中国、日本、韩国青训球员和各级青年联赛。
- 所有中国球员历史留洋人数、当前海外注册人数和试训人数。
- 所有亚洲教练在欧洲各级联赛的完整执教史。
- 实时转会、实时出场、实时身价和合同信息。
- 非公开注册资料、未成年人私人信息和内部训练名单。

## 覆盖状态定义

- `已覆盖重点口径`：明确赛事或名单版本已阶段性完成。
- `部分覆盖`：有稳定框架和部分样本，仍有明显缺口。
- `样本覆盖`：只维护代表性或当前重点样本。
- `精选样本`：用于研究入口，不能推导全量人数。
- `生成覆盖`：由脚本生成，不手工维护。
