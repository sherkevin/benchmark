# Round 1：Benchmark / 最近邻交叉审计

本轮读取 Round 0 的故事审查与方法审查，逐条检查它们是否过度收窄。依据 `评价标准/03-Benchmark与Baseline评价标准.md`。

## 1. 自建 routing layer 不等于 benchmark 无效

**POSITION**：支持“动态 JEV 不能当已证实算法”，但挑战“自建 routing layer 因为不是社区 benchmark 就不足以支撑 AAMAS”。

**TARGET**：故事审查对 custom routing protocol 的担忧；Benchmark B5/B7。

**EVIDENCE**：生效标准明确把公共 truth layer 与 provider routing layer 分开，后者可以是本项目新建协议，但不能冒称社区 benchmark。TDC truth 有公共来源；真正缺口是 routing contract、provider lineage、overlap/confirmation 和 independent recovery。

**REASONING**：AAMAS 没有固定 benchmark 数量。若双层 manifest、事件 schema、checker、provider 资格和一次独立回放闭合，可以写“该协议下的 feedback-adaptive routing”；不能写成公共 LLM-routing benchmark 或普遍动态工具适应。

**DECISION TEST**：公开 truth/routing 双层 manifest 和 hash；checker 从 raw JSONL 重建 visible history；在 BBB scaffold-disjoint stream 上预登记 d∈{0,1,4}、pending、duplicate、failure/restart，再做一次只读 confirmation。

**CONFIDENCE**：0.90。当前 Gate A 临时分：A1=3、A2=2、A3=2、A4=2，总 9/16，HOLD；不是因自建协议本身，而是证据未闭合。

## 2. 没有自然 drift 不等于 feedback-adaptive 方法不能成立

**POSITION**：支持方法审查的 selected-only 反事实不可识别和 selector/verifier 分离；挑战把“未做 arrival/retire/version”直接当成方法失败。

**TARGET**：方法 M10/M11/M15、Benchmark B7。

**EVIDENCE**：B7 区分 feedback-adaptive（d∈{0,1,4}、pending queue、selected-only release）与 environment-nonstationary（arrival/retire/version/shift）。当前固定菜单可以检验前者，不能检验后者；P/Q synthetic 反例仍必须做。

**REASONING**：应 kill environment-nonstationary 叙事，而不是在没有声明时 kill feedback-adaptive 候选。若 P/Q 反例、propensity 和 selector/verifier 分解失败，再 kill algorithmic claim。protocol/negative study 仍需 Gate A/B/C。

**DECISION TEST**：CPU synthetic P/Q：A 轨迹相同，B=0.95/0.05；比较无探索、固定 propensity、fit-only transfer；真实流分开跑 selector-only、verifier-only、组合和 matched escalation。只有 declared feedback-adaptive scope 有 d={0,1,4} 可重算日志，才保留该 scope。

**CONFIDENCE**：0.93。当前 B7 可给 2；明确 scope 并完成压力后可到 3。

## 3. baseline 不应无限堆，但最小 matched roster 必须闭合

**POSITION**：Round 0 要求补齐所有近期 baseline 过宽；但当前最小 roster 仍不足。

**TARGET**：故事/方法报告的 baseline 缺口；B9/B10/B11。

**EVIDENCE**：最小可辩护 roster 是 Random、Best Fixed、Frozen、matched Online/Replay、一个可复核 delayed/contextual nearest（LinUCB/Greedy/PILOT 之一）、Oracle；verifier/second-call control 只在对应主张出现时加入。GraphRouter/Laya 若无法保真，应降为 audit-only，而不是阻塞整篇论文。

**REASONING**：B9 要求角色完整，不要求复刻所有 router；B10/B11 更关注源码/commit/license、同合同和 family correlation。24 个 B3clf 变体必须做 family-macro、exposure 和 paired CI，否则 pooled headroom 可能虚高。

**DECISION TEST**：建立一页 role matrix，完成最小 roster 的 raw-run/recovery；GraphRouter/Laya 登记为 fidelity 通过、audit-only 或删除三态。若 full 只胜 Random/Frozen，或胜幅只出现在不匹配预算，停止动态 JEV 优越性主张。

**CONFIDENCE**：0.87。当前 Gate B 临时 9/16，B9/B10=2；完成角色、版本、适配和统一合同后才可能到 3。

## 4. 单一 go/no-go

**POSITION**：不在“算法已证实”和“benchmark 无效”之间二选一；按三门独立闸门裁定论文定位。

**TARGET**：Round 0 共同 HOLD、Gate A/B/C、B7/B9/B10/B14。

**EVIDENCE**：当前三门均 HOLD；标准要求每门 ≥12/16，且 A1/A3/A4、B1–B4、C1–C4 达到最低线；总分不能互相抵消。

**DECISION TEST**：一次性冻结 truth/lineage/family manifest；最小 baseline roster；E1–E5（任务有效性、主比较、消融、declared feedback-adaptive 压力、错误/成本/恢复）；保存 raw JSONL、paired molecule/scaffold/family CI 和 failure/unknown 分母；confirmation 只读一次。

成功条件：H1–H10 全 PASS；Gate A/B/C 各≥12/16；B7/B9/B10/B14≥3；`full−strongest_matched` 在预登记 ROPE 外、至少 2/3 delays 胜出，且没有额外标签或调用预算偷换。否则收窄为 feedback-adaptive protocol/negative diagnosis；若只有 verifier 过线，单独保留 post-call verifier 结果。

**CONFIDENCE**：0.96。
