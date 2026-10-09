# 后端库配置报告 · literature_search

| 字段 | 值 |
|---|---|
| 工具规范名 | `literature_search` |
| alias | `fetch_bib_info`、`search_drug_literature`、`screen_literature`（旧名在录像/判分读取时按 tools_schema.json `alias_map` 归一到本规范名） |
| source_tier | A |
| 预算 | search_budget（计检索预算，不计查证预算） |
| 后端来源决策 | **自建**（调研报告 §3 决策表行 1：61 家 benchmark 无一家提供离线文献元信息查询后端） |
| 契约依据 | `后端库规格契约.md` v1.1 §3.1、§6.2 |
| 支撑 checkpoint | cp1 文献检索 |

## 1. 数据数量（实测）

| 侧 | 文件 | 记录数 | 体积 |
|---|---|---:|---|
| A 侧（无 B 侧——文献不是 A/B 分层对象） | `dag_poc/data/backend/literature_search/literature_search_A.jsonl` | **321**（契约下限 ≥300 ✓） | 200,246 bytes（du 200K） |

构成：造题期从 CrossRef API 采集的**真实 DOI 题录冻结快照**（缓存落盘 `src/data_build/_build_state/`，可离线重跑），覆盖药物线 + 材料线各 case 的 cp1 文献检索主题。

## 2. 功能

按自然语言查询检索学术文献，返回论文元信息（ID/标题/年份/引用数），支撑 cp1 文献检索 checkpoint。**只返回文献元信息，不返回任何分子/性质数值**（性质一律走 get_property）；无命中返 `EMPTY_RESULT`，不得编造论文。判分零联网：发布后查询完全命中冻结快照，不再触网。

## 3. 调用方式（契约 §6.2 读取接口签名）

```
load:   literature_search/literature_search_A.jsonl → dict[doc_id → record] + 关键词倒排索引
query(query: str, max_results: int) → {query, total_found, returned, papers[{doc_id, title, year, citation_count}]}
```

- 打开方式：进程启动时一次性全载（标准库 `json` 逐行解析），运行期只读不写；
- 检索匹配：`keywords` 字段倒排索引（构造期内存中建立，不落盘第二文件）；
- 无命中返错误码 `EMPTY_RESULT`；每次调用计 search_budget。

## 4. 出入参含义（对照 tools_schema.json）

**入参**：

| 参数 | 类型 | 必填 | 含义 |
|---|---|---|---|
| `query` | string | 是 | 自然语言检索词（与倒排索引的 keywords 匹配） |
| `max_results` | integer（1–20，默认 5） | 否 | 单页返回论文数上限 |

**出参**：

| 键 | 类型 | 含义 |
|---|---|---|
| `query` | string | 回显入参 |
| `total_found` | integer | 命中总数 |
| `returned` | integer | 本页返回数 |
| `papers[]` | array | 每篇含 `doc_id`（DOI/PMID，供 trace.docs 与 cp1 硬判）、`title`、`year`、`citation_count`（冻结时点引用数）四字段，`additionalProperties=false` |

**错误码**：`INVALID_ARGUMENT` / `EMPTY_RESULT` / `BUDGET_EXCEEDED`。

## 5. 子库记录字段与 provenance 写法

每条记录字段：`doc_id` / `title` / `year`（1900–2026）/ `citation_count`（≥0，采集时点写入 provenance）/ `keywords`（造题期标注）/ `provenance`。

实测示例（首行记录原文节选）：

```json
{"doc_id":"10.1002/047002142x","title":"The hERG Cardiac Potassium Channel: Structure, Function and Long QT Syndrome","year":2005,"citation_count":4,
 "keywords":["herg","potassium channel","cardiac","toxicity","qt prolongation"],
 "provenance":{"source_db":"CrossRef","source_url":"https://doi.org/10.1002/047002142x","source_paper_doi":"10.1002/047002142x",
  "note":"冻结快照采集自 CrossRef API，采集时点 2026-08-06T09:20:03Z；citation_count 为该时点 is-referenced-by-count；keywords 为造题期半自动标注"}}
```

## 6. 性能实测（引自 `性能与隔离实测报告.md` §3）

| 指标 | 实测值 |
|---|---|
| 全表扫描 | 0.002 s（3 轮最小值） |
| 点查均值 | 26 ns/次（建 dict 后抽样 200 次） |
| 全载内存峰值（tracemalloc） | 1.5 MB |

## 7. 已知限制

1. `citation_count` 是 **2026-08-06 冻结时点**的 CrossRef `is-referenced-by-count`，不随真实世界更新（冻结快照语义，非缺陷）；
2. 检索为关键词倒排匹配，非语义检索；query 与 keywords 无交集即 `EMPTY_RESULT`；
3. 无 B 侧文件：文献不是 A/B 分层对象，不存在评分侧复核需求。
