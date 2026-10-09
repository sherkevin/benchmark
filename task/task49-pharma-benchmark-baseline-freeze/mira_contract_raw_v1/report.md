# MIRA raw-run contract v1

日期：2026-09-30
状态：contract-only evidence；不产生效果排名，不打开 confirmation，不申请 A800。

## 目的

baseline raw schema 审计发现，旧 MIRA 日志缺少原生菜单、decision read-set、state hash 和显式 terminal correctness，不能和 Greedy 的强合同放在同一证据等级。本轮新增一个只使用冻结缓存的 MIRA contract smoke，先验证这些字段能否在真实 cache replay 中原生落盘和独立恢复。

## 固定合同

- 64 个冻结 BBB development molecules、33 个 provider、Morgan-2 64-bit normalized feature；fixture prior 只用于接口 smoke，不是训练结果。
- MIRA relative-reliability，delay `{0,1,4}`，每个 delay 一条 stream；没有新预测、没有 GPU、没有 confirmation。
- decision 记录菜单、选择前可见的 released feedback IDs、decision read-set、feature hash、版本和 state hash；terminal 记录 selected output、status、correctness 和 due tick；feedback 只记录 reward/status、释放时间、版本转换、state hash 和 MIRA state，不包含 evaluator 的 chemical correctness 字段。

## 收据

权威目录：`experiments/logs/task49_mira_contract_raw_20260930/run_v5/`。

- runner：`src/probe_headless/run_mira_contract_raw_v1.py`
- independent verifier：`src/probe_headless/verify_mira_contract_raw_v1.py`
- 3 streams；decision/terminal/feedback 各 192。
- verifier 逐条检查 provider/menu binding、selected-only read-set、释放时钟、版本守恒、terminal 输出与 frozen cache 一致、state hash 前后链和显式 correctness；状态 `verified`。
- 篡改第一条 decision menu 或 feedback state hash，verifier 均以 exit code 1 拒绝。
- `verify_commit_provenance_v1.py` 从 `config.code_identity.declared_commit` 的提交树逐文件读取 source，9/9 source hash 匹配；把 commit 改成全零 hash 的篡改收据以 exit code 1 拒绝。此前 `run_v4` 使用错误的 declared commit，保留为 provenance engineering failure，不作为权威收据。

该收据把 MIRA 的日志证据提升到与合同 smoke 相同的字段层级，但它仍然不是效果实验，也不等于所有 C0–C8 已经统一重放。

## 下一步

将同一 writer/replayer 合同迁移到 Random、Matched Online-SGD、PILOT、BayesianRouter 和 Greedy 的统一 development replay。MIRA 的 commit provenance 已闭合，但只有 Gate A/B/C 的其他缺口也收口后，才考虑 A800 full matrix。
