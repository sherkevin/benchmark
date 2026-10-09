# Vault 笔记对动态 JEV 的启发

**日期：** 2026-09-25  
**定位：** 辅助阅读，不改变动态 JEV 主线

## 1. 读了什么

主要读了：

- `/Users/jingwu/work/vault/loop attention.md`
- `/Users/jingwu/work/vault/kv cache.md`
- `/Users/jingwu/work/vault/Linear Attention.md`
- `/Users/jingwu/work/vault/01-Methods/inference-KV-Cache.md`
- `/Users/jingwu/work/vault/01-Methods/inference-kv-cache-management.md`

这几份笔记里最有用的不是某个可以直接搬过来的模型，而是对“状态怎么存、什么时候更新、什么时候还能复用”的区分。

## 2. 最值得借用的三个原则

### 2.1 把查询状态和被查询的状态分开

`loop attention.md` 里最清楚的一点是：$Q/K$ 主要决定去哪里找，$V$ 主要决定拿回什么。如果每一轮都直接改同一个 residual，寻址和内容会一起变化；一个更干净的结构是固定或慢更新 $K,V$，只让 $Q$ 和当前推理状态继续变化。

对应到动态 JEV：

- 已冻结的 Laya/AnyJev 表示、工具身份和任务特征，是相对稳定的“被查询状态”；
- 当前已经看到的工具结果、预算和 pending feedback，是当前 episode 的“查询状态”；
- 在线更新只改小的决策头和统计状态，不改 frozen encoder。

这里的迁移是状态语义上的，不是把 Transformer 的 Q/K/V 直接塞进 JEV。

### 2.2 在线状态应该是追加事件，不是反复重写历史

KV cache 的正确性依赖一个简单前提：历史前缀不变，缓存就可以复用；一旦前缀变了，从变化点开始的缓存就失效。`inference-KV-Cache.md` 还把缓存和原始 token 区分开：缓存是可复用的派生状态，原始内容仍然是事实来源。

对应到动态 JEV：

- 每次 CALL、反馈到达、状态更新都写成不可变事件；
- `state_version` 只在事件被 exactly-once 消费后递增；
- 选择时读取一个明确的 state snapshot；
- 原始工具返回和实验真值保留在 replay/audit 记录里，统计状态只是派生缓存。

这正好支持延迟和乱序反馈。对 $ρ=1$ 的 M0，反馈更新是加法，事件到达顺序不影响最终统计量；对 $ρ<1$，衰减时钟必须显式记录，不能把到达顺序悄悄当成环境时间。

### 2.3 状态压缩必须保留任务需要的充分信息

`loop attention.md` 对 summary 的判断很重要：摘要不需要恢复完整 CoT，而应该保留后续任务所需的 sufficient state、约束和 retrieval handle。缓存、摘要和原始内容不是同一层。

对应到动态 JEV：

- M0 的 $S/Z$ 或 Beta 统计量是 provider 正确率的有限状态摘要；
- M1 的 $G/Z^g$ 是条件复核收益的有限状态摘要；
- 它们不能被解释成完整历史，也不能在没有记录原始事件的情况下用于审计；
- 一旦 encoder、特征映射、provider 菜单或标签合同变化，旧统计量的解释可能失效，必须通过版本号隔离或重建。

## 3. 这给动态 JEV 带来的具体设计约束

建议把在线状态写成下面六部分，而不是只保存一组可变向量：

$$
\mathcal S_n=(v_e,v_m,v_f,\;Q_n,\;R_n,\;\mathcal E_n)
$$

其中：

- $v_e$：encoder 版本；
- $v_m$：provider 菜单和工具合同版本；
- $v_f$：反馈标签与损失合同版本；
- $Q_n$：截至事件步 $n$ 已确认的在线统计状态；
- $R_n$：尚未到达的 delayed feedback 队列；
- $\mathcal E_n$：追加式事件日志的引用或 checkpoint 边界。

真实例子：2026-09-25 的 M0 smoke 中，`q_exec` 在 CALL 返回后更新，`q_corr` 要等 `delay=1` 的 scored feedback release 后才更新；下一次选择看到的是 release 后的 learner version。这个顺序属于状态合同，不能由实现细节推断。

## 4. “前缀复用”对本项目的安全迁移

KV cache 的前缀复用可以转成一个工程要求：相同的输入表示和合同版本才允许复用特征或在线状态。

可以复用：

- 相同分子、性质问题、provider 菜单和 frozen encoder 版本下的 $\phi(h,p)$；
- 相同历史快照下的候选分数和 propensity 计算；
- 已经 release 的事件到 checkpoint 的统计状态。

不能直接复用：

- encoder fine-tune 之后由旧 encoder 产生的特征；
- 改过标签定义、损失、答案规则后的 correctness/gain 统计；
- 换过 provider 版本或工具输出 schema 后的历史状态；
- 只看起来相同、但已有工具结果不同的 episode history。

这也是为什么当前主线应该先冻结 Laya/AnyJev 表示，只训练在线头。若以后要实时 fine-tune backbone，必须先解决旧特征的失效和状态重建，不能只给在线 head 加一个梯度更新。

## 5. “loop”能启发什么，不能声称什么

能借用的部分：

- 用一个小的 recurrent state 做多次 cheap read/update；
- 把当前查询（当前 history）和跨 episode 的可复用统计分成两个时间尺度；
- 让追加调用成为一次有条件的 read，而不是每次都重算全历史。

如果后面要处理 provider drift，`Linear Attention.md` 里的 Decay–Erase–Write 可以作为一个候选更新结构：旧状态先按环境时间衰减，再沿当前特征方向擦除旧关联，最后写入新反馈。它的价值是把“遗忘”拆成可解释的三个动作；但第一版不应直接引入，先用 $ρ<1$ 的时间衰减做基线，并按环境时间戳而不是反馈到达顺序计算衰减。

不能直接声称的部分：

- 线性递推本身不是新颖性来源；
- “冻结 KV、递归 Q”不是我们动态 JEV 的算法贡献；
- KV cache 的前缀复用也不等价于 provider 之间的条件独立或可交换；
- 普通 causal KV 依赖原始 prefix，不能据此假设工具结果可以任意重排或组合。

因此它只能帮助我们约束实现和解释状态生命周期，不能替代 M1 的 verification-gain 识别问题。

## 6. 对主线的最终结论

这次阅读没有改变主方法。它把主线的实现原则进一步压实为：

1. frozen encoder；
2. episode history 只在当前 episode 内变化；
3. 跨 episode 状态采用 append-only event + exactly-once release；
4. 每次选择读取带版本号的 snapshot；
5. encoder、菜单或标签合同变化时，旧状态必须隔离或重建；
6. M1 继续研究随机 probe 下的 conditional verification gain，不能把 KV/loop 类比当成方法创新。

下一步仍然是 M1：在 M0 的 selected-only runner 上加入随机二次 probe、propensity 日志和 $G/Z^g$ 更新。Vault 笔记对这一步的帮助主要是保证状态读写顺序和失效条件可审计。
