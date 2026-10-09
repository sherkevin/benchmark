# SafeSwitch smoke verifier review（P0）

日期：2026-10-02。范围仅限 SafeSwitch receipt/verifier；没有重跑 policy，也没有修改 ContextShield。

## 审查过程

- 对照候选 `proposal.md`/`preregistration.md`、runner/stream、当前 verifier 及 v2/v3/v4/v5 receipts。
- 先用临时 receipt copy 做 fail-open 检查：篡改 summary `n_correct`/`n_events`、summary event hash、config 的输入/manifest/registry/source/commit/cache 声明、全部 envelope 元数据、due/release tick、Frozen provider，旧 verifier 仍可返回 verified。
- 进一步对照 sealed input 发现 terminal payload 带有 chemical/system/recovered 字段；若 verifier 只信 payload，就会把未来/篡改 truth 当作 evaluator 结果。
- 对照候选语义确认 v2/v3 固定 kappa/gamma、v4 向量 kappa、v5 gamma=0/向量 kappa；verifier 必须按 receipt 的显式 kappa contract 重放。

## 语义漂移记录（不在本次 verifier 修复中扩大范围）

- `MIRAIncumbentSafeSwitchPolicy._score` 复用 FamilyGuard/RelativeReliability 后再做 SafeSwitch 修正；因此 raw base 已含 selected-feedback soft penalty，和候选文档的 frozen raw `p_a(x)` 定义不完全一致。
- runner 与 verifier 都在每次 decision 后更新 `current_incumbent`；它不是 fit-only immutable incumbent。v5 首次切换发生在 challenger 尚无在线反馈时，静态 fit prior 主导该动作。
- runner state hash 当前包含权重、在线计数、family gate、版本和 incumbent，但没有 release queue 内容；候选文档曾要求 queue hash。当前 verifier 只验证 runner 实际写出的 canonical state hash，未把缺失字段悄悄补入。

## 旧 verifier 的 P0 缺口

旧版本（commit `7054df51` 的 `verify` 213–303 行）只按 stream 聚合并检查 decision/terminal/feedback 数量，未绑定 `summary.config`、输入/manifest/cache/registry/source commit/hash；事件 hash 只作为输出而不与 summary 比对。它没有严格 envelope roster、canonical event id/order、decision-terminal 邻接、delay timing、summary final state/计数校验，也没有重放 FrozenDirect。terminal/feedback 的 evaluator 字段和选中输出未从 sealed rows 重算，provider 也未绑定到同一 decision。

## 修复

`src/probe_headless/verify_mira_safe_switch_smoke_v1.py` 现在（行号以当前工作树为准）：

1. `safe_repo_path`/`verify_sources`（42、75 行）校验完整 declared commit/blob SHA、`local_head` 绑定；`verify` 还绑定 config provider menu、registry/cache manifest；`verify_manifest`（103 行）校验 inputs/manifest/checkpoint/snapshot/direct prior/split index hash。manifest 接受全量 sealed folds，但要求 run 选中的 `(fold, seed)` 覆盖。
2. `verify` 的 summary/config 与 event SHA 绑定（432、483–486 行）；拒绝未知 event、缺失/错误 stream envelope、非法 state hash/bytes、stream roster/count 不一致（499–511、541 行）。
3. 约束 decision id、tick/entity/query 顺序、decision→terminal 邻接、terminal/feedback 顺序（565–578 行）、`due_tick = release_tick = decision_tick + 1 + delay`（646 行）。
4. `FrozenReplay`（354 行）独立重放 FrozenDirect；Safe kappa contract 解析在 228–246 行，逐事件比较 provider/scores/version/state hash/bytes。terminal provider 必须等于 decision provider（616 行）。
5. 从 sealed input 重算 selected output、valid/chemical/system/recovered truth、failure/scored evaluator（617–655 行）；回收 pending、版本、状态和 summary 聚合字段（658–660 行）。

## 验证结果

命令：

```bash
python3 -m src.probe_headless.verify_mira_safe_switch_smoke_v1 \
  --run experiments/logs/task49_mira_safe_switch_smoke_20261002/v5_fold0_seed0_delay0_full \
  --output experiments/logs/task49_mira_safe_switch_smoke_20261002/v5_fold0_seed0_delay0_full/verification_v2.json
python3 -m pytest -q src/probe_headless/tests/test_mira_incumbent_safe_switch.py
```

结果：8 passed；v2/v3/v4/v5 receipts 均通过新版独立 verifier。v5 新验证结果保存在 `verification_v2.json`：2 streams（Safe 1、Frozen 1），290 decisions，290 feedback，events SHA256 `5b283e56133cae8ba399f18f7d2ce434bdac1f55ba257500c29b77b228c4d98a`，无 effectiveness inference。

新增临时-copy tamper tests 均拒绝：

- source SHA 或 input SHA 篡改（并同步 summary/config）；
- Frozen decision provider 篡改（并重绑 summary event hash）；
- terminal chemical/system truth 篡改（并重绑 summary event hash）；
- summary aggregate 与 feedback release timing 篡改。

旧 `verification.json` 保留未覆盖；本次新版输出为 `verification_v2.json`。
