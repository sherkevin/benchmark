# 07 · W 档：9 个 web 工具取值与证据存档

Type: task
Status: open
Blocked by: 04

## 为什么单独一档

web 工具无法得知服务端权重版本，且状态实测会漂移。同日两次测量：ADMETlab 3.0 从 200 变 403、pkCSM 从 200 变 000、graphB3 web 从 200 变 000；LightBBB 唯一入口已 404 永久下线。这类工具的数值**不可复现**，故单独成列，不进 L3 判分列。

## 9 个目标

ADMETlab 3.0、admetSAR 3.0、pkCSM、SwissADME、DeepB3 server、LogBB_Pred、BBBper、VEGA、PreADMET

## 每次调用必须存的证据（缺一不可）

```
data/bbb_census_v3/web_evidence/<tool_id>/<timestamp>/
  request.json      # URL、method、headers（去敏）、payload
  response.raw      # 原始响应体全文（HTML/JSON 不做裁剪）
  response.meta     # HTTP 状态码、响应头、耗时
  parsed.json       # 解析出的数值 + 解析器版本 + 解析规则标识
```

理由：只存解析后的数值等于把证据扔了。ADMETlab3 半天内状态翻转已实测发生，将来对不上账时无法自证。

## 重试与漂移

- 单次调用失败：3 次指数退避（1s / 4s / 16s），仍失败记 `timeout` 并继续，不中断整批
- **隔日全量重取一次**，与首日结果比对，把差异记进 `web_drift.json`。这一步是区分"工具真下线"与"当时网络抖"的唯一手段（pkCSM 在相隔数小时的两次测量里 200 与 000 各出现一次，无法排除抖动）

## 规模控制

730 个分子 × 9 个 web 工具 = 6570 次请求，且多数站点无批量接口。**先跑 A 级 10 个分子**（既有 `candidate_pool.json` 里 3 对全冲突的分子）验证解析器正确，再决定是否全量——全量前须评估是否会触发对方限流或被封。这是本 ticket 的第一个决策点，跑完 10 个分子后停下来汇报。

## 验收

- 每个工具至少一次成功取值，或明确的失败状态 + 证据
- `web_evidence/` 目录结构完整，可离线复查
- `web_drift.json` 记录隔日重取差异
- 数值写入宽表的 **W 档专属列**（列名带 `w__` 前缀），与 L3 列物理隔离

## Comments
