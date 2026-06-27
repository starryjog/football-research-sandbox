# 数据字典

本文解释核心 JSON 文件、生成文件和常用字段。实际校验规则以 `scripts/validate-data.mjs` 为准。

## 数据流

```text
data/raw/**  ->  scripts/validate-data.mjs  ->  scripts/build-site-data.mjs  ->  data/site/**
                                \->  scripts/sync-sqlite.mjs  ->  storage/youth-football.sqlite
```

`data/raw/**` 是维护入口。`data/site/**` 和 `storage/**` 是生成结果，不应手工编辑。

## `data/raw/players/*.json`

按年龄段、国家或专题拆分的球员原始数据。加载时所有 JSON 文件会被合并。

核心字段：

- `id`：全库唯一 ID。
- `name`：英文、注册或数据库常用拼写。
- `local_name`：中文、日文、韩文或本地常用名。
- `country`：国家或协会口径，例如 `China PR`、`Japan`、`Korea Republic`。
- `birth_date`：出生日期，格式 `YYYY-MM-DD`。
- `age_band`：年龄段，例如 `U17`、`U21`、`U23`。
- `primary_position`：主要位置。
- `height_cm` / `weight_kg`：可选身体数据。
- `registration_club`：对象，至少包含 `name` 和 `country`。
- `league_system_override`：可选，用于覆盖站点自动判断的联赛体系。
- `overseas_bucket_override`：可选，值必须来自 `overseas-history.json` 的 `bucket_definition`。
- `training_pathway`：数组，记录学校、青训、俱乐部或项目路径。
- `focus_tags`：用于专题筛选。
- `tournament_participation`：数组，记录赛事报名、出场、进球和备注。
- `external_links`：数组，公开来源链接。
- `verification`：核验状态、核验日期和备注。

加载器会根据 `player-name-overrides.json` 自动生成 `names.zh`、`names.en`、`names.native`、`names.ja`、`names.ko`，供站点展示和 SQLite 同步使用。

## `data/raw/tournaments.json`

当前重点赛事卡。用于首页、赛事页和球员参赛关联。

常用字段：

- `id`：赛事 ID，球员 `competition_id` 会引用它。
- `name` / `short_name`：完整名称和短名称。
- `focus_level`：站内关注级别。
- `status`：赛事或维护状态。
- `last_checked`：最近核验日期。
- `date_range.start` / `date_range.end`：赛事日期。
- `focus_teams`：重点跟踪球队。
- `headline`：赛事摘要。
- `notes`：维护备注和口径说明。
- `sources`：来源链接。

## `data/raw/tournament-archive.json`

历史赛事归档，重点记录中国队相关结果和可扩展明细。

常用字段：

- `id`、`confederation`、`competition_name`、`level`、`edition_label`。
- `host`、`date_range`、`status`、`champion`、`runner_up`。
- `china_status`、`china_summary`、`china_detail_scope`。
- `china_matches`：中国队逐场结果。
- `china_key_players`：关键球员或贡献。
- `china_squad`：可选阵容名单。
- `regional_history`：可选区域历史对照。
- `source_links`：来源链接。

## `data/raw/overseas-history.json`

中日韩等国家球员留洋历史和当前海外路径样本。

顶层字段：

- `bucket_definition`：允许的留洋 bucket。
- `countries`：按国家维护留洋样本、备注和专题列表。

国家字段：

- `country`：国家口径。
- `status`：维护状态。
- `verified_records`：当前已核代表样本数量，不等于官方总人数。
- `bucket_focus`：各 bucket 的维护说明。
- `seed_examples`：待追踪或代表性名字。
- `notes`：整体口径说明。
- `special_lists`：可选专题名单。
- `big_five_appearance_checklist`：可选五大联赛出场榜核对清单。
- `featured_records`：精选留洋记录。

`featured_records` 不是全量库。常用字段包括 `id`、`name`、`local_name`、`bucket`、`league`、`club`、`season`、`status`、`appearances`、`appearance_label`、`competitive_debut`、`active_abroad`、`summary`、`notes`、`source_links`。

## `data/raw/projects.json`

项目或专题入口卡，适合记录仍在推进的研究线索。

常用字段：

- `id`、`name`、`status`、`priority`。
- `focus_tags`：与球员或专题关联的标签。
- `summary`、`goal`、`completed`、`next_step`。
- `watch_items`：后续观察点。

## `data/raw/dossiers.json`

深度专题档案。比 `projects.json` 更适合承载长文本、时间线、名单视图和争议边界。

常用字段：

- `id`、`name`、`status`、`last_reviewed`。
- `focus_tags`、`summary`、`scope_note`。
- `source_document`、`supporting_documents`。
- `role_model`、`timeline`、`roster_views`。
- `link_audit`、`search_disambiguation`、`controversies`、`open_questions`。

## `data/raw/china-men-youth-coaches.json`

中国男足青年队教练周期档案。

常用字段：

- `id`、`name`、`last_checked`。
- `team_cycles`：按队伍和周期记录主教练、阶段、集训和来源。
- `football_boys_alignment`：与足球小将等专题的关联说明。

## `data/raw/big-five-asian-coaches.json`

五大联赛亚洲教练样本。它区分 AFC 成员协会主口径和广义地理边界口径。

常用字段：

- `id`、`last_checked`、`scope_counts`。
- `primary_scope_record`：主口径汇总战绩。
- `coaches`：教练样本。
- `source_links`：整体来源。

教练记录里的 `top_flight_record` 必须等于 `club_records` 的联赛战绩汇总。

## 覆盖和覆盖修正文件

- `data/raw/player-name-overrides.json`：手动覆盖球员多语言姓名。
- `data/raw/club-name-overrides.json`：俱乐部展示名、国家或归类修正。
- `data/raw/player-market-values.json`：Transfermarkt 身价快照和来源信息。

## 生成文件

- `data/site/players.json`：合并、补名、补身价后的球员列表。
- `data/site/overview.json`：站点首页和各专题页使用的聚合数据。
- `storage/youth-football.sqlite`：本地 SQLite 输出，不提交。

生成文件里的字段可能比 `data/raw/**` 多，因为加载器会补 `names`、`market_value` 和聚合统计。
