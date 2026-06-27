# 贡献指南

本项目是静态足球研究站，核心数据源在 `data/raw/`，站点消费数据在 `data/site/` 由脚本生成。贡献数据时请先确认项目范围、来源政策和字段含义：

- [项目范围](docs/scope.md)
- [来源政策](docs/source-policy.md)
- [数据字典](docs/data-dictionary.md)
- [覆盖矩阵](docs/coverage-matrix.md)

## 基本流程

1. 确认本次修改属于项目范围。
2. 先改 `data/raw/**`，不要手工改 `data/site/**` 或 `storage/**`。
3. 为每条新增事实补来源链接、核验日期和必要的备注。
4. 运行校验和生成脚本。
5. 检查 diff，确认没有把无关生成物、临时报告或本地数据库提交进去。

常用命令：

```bash
npm run validate-data
npm run build-data
npm run sync-sqlite
```

或一次性执行：

```bash
npm run prepare-data
```

`sync-sqlite` 会写入 `storage/youth-football.sqlite`，该文件是本地输出，不提交。

## 新增或修改球员

球员原始数据按主题或年龄段放在 `data/raw/players/*.json`。新增球员前先全库搜索姓名、中文名、英文拼写和外部链接，避免重复建档。

每条球员记录至少维护：

- `id`：稳定、唯一、可读，通常使用国家前缀、姓名和出生年。
- `name` / `local_name`：英文或注册拼写、当地常用名。
- `country`、`birth_date`、`age_band`、`primary_position`。
- `registration_club`：当前或该数据口径下的注册俱乐部与国家。
- `training_pathway`：青训、学校、俱乐部或项目路径，按时间顺序写。
- `tournament_participation`：赛事报名、参赛、出场、进球、备注。
- `external_links`：公开可访问来源，不把搜索结果页当作唯一来源。
- `verification`：至少包含 `status`、`last_checked`、`notes`。

姓名多语言展示由 `scripts/lib/data-loader.mjs` 自动生成；确需覆盖时改 `data/raw/player-name-overrides.json`。

如果球员已经在库内存在，优先补字段，不要因为进入新的专题、赛事或留洋样本就再建第二条。

## 修改赛事与赛事归档

短期重点赛事卡维护在 `data/raw/tournaments.json`，历史赛事归档维护在 `data/raw/tournament-archive.json`。

赛事修改时请注意：

- `date_range.start` 和 `date_range.end` 使用 `YYYY-MM-DD`。
- `last_checked` 写实际核验日期。
- 官方赛程、报名表、比赛报告优先于二手汇总页。
- 中国队比赛明细、关键球员和阵容必须写清楚统计口径。
- 若只补摘要，不要暗示已经有完整逐场或全队数据。

球员的 `tournament_participation[].competition_id` 必须能对应 `data/raw/tournaments.json` 里的赛事 `id`。

## 修改留洋记录

留洋历史维护在 `data/raw/overseas-history.json`。这里是精选样本库，不是官方全量人数统计。

新增或修改 `featured_records` 时请明确：

- `bucket`：必须来自 `bucket_definition`。
- `league`、`club`、`season`：写清联赛层级和时间段。
- `appearances` 与 `appearance_label`：说明是联赛、各项赛事、梯队还是注册状态。
- `competitive_debut`：是否已经有正式比赛出场。
- `active_abroad`：是否当前仍在海外注册或海外体系内。
- `summary` 和 `notes`：解释为什么纳入该 bucket，特别是五大联赛、五大联赛国家青训、低级别联赛之间的边界。

签约已公布但尚未生效、试训、关注池和媒体传闻不能直接计入当前留洋样本；可先写在备注或专题文档中，等注册和出场来源稳定后再入库。

## 来源与冲突

优先使用官方报名表、协会公告、俱乐部官网、赛事报告和可核验数据库。Transfermarkt、Wikipedia、媒体稿可以作为交叉验证来源，但不要把单一二手来源当作最终事实。

资料冲突时不要直接覆盖。请在相关记录里保留较稳口径，并在 `verification.notes`、`notes`、`scope_note` 或来源备注中说明：

- 冲突字段是什么。
- 哪个来源更接近官方或原始记录。
- 本次选择的口径是什么。
- 后续需要回查什么。

## 提交前检查

提交前至少运行：

```bash
npm run validate-data
```

如果修改了 `data/raw/**`，还应运行：

```bash
npm run build-data
```

如果需要确认 SQLite 输出：

```bash
npm run sync-sqlite
```

最后检查：

```bash
git status --short
git diff
```

只提交本次任务相关文件。不要提交 `storage/*.sqlite`、`outputs/**`、临时研究报告、截图产物或本地缓存。
