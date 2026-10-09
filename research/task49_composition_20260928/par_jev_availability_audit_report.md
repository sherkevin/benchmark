# PAR-JEV availability 审计结果

日期：2026-09-28  
状态：开发期否证；不是最终 benchmark 结果。

## 做了什么

`audit_par_jev_availability.py` 只读取
`fingerprint_diagnostic_v1/inputs.json` 的 fit rows，按 5 个 scaffold fold、33 个
provider 和真值 $Y\in\{0,1\}$ 统计 `system_success`。没有读取 evaluation rows，
没有训练 selector，也没有使用 confirmation。

结果文件：

`experiments/logs/task49_composition_20260929/par_jev_availability_audit_v0/result.json`

输入 SHA-256 和结果 SHA-256 已写入结果/日志，不在报告中重复复制。

## 结果

| fold | fit rows | provider 间最大 $|\hat u_{a,1}-\hat u_{a,0}|$ | provider 平均绝对差异 |
|---:|---:|---:|---:|
| 0 | 444 | 0.4153 | 0.1819 |
| 1 | 483 | 0.4542 | 0.2057 |
| 2 | 492 | 0.4039 | 0.1925 |
| 3 | 483 | 0.4264 | 0.1924 |
| 4 | 480 | 0.4586 | 0.1774 |

这些差异说明，当前数据不能把有效返回 $V_a$ 与性质真值 $Y$ 的条件关系当作
可默认忽略的项。它不是 provider 彼此独立与否的问题，而是同一个 provider 的
availability 是否满足 $V_a\perp Y\mid v$ 的问题。

## 决定

停止 `par_jev_factorized_v0` 作为主方法候选。不是把这个结果扩大成“动态 JEV
不可行”，而是只否证该候选的因子化前提。已有 kernel 和 13 项合同测试保留为
失败候选的可复现实现场；不运行它的效果实验，不申请 A800，不把它写入生效方法。

下一候选使用 provider 的冻结**端到端成功概率** $P(S_a=1\mid v)$ 作为单一
joint-success score，只维护每个 provider 的低维 residual。它不拆出 $u$、$q$
和 $h$，因此不需要可用性与真值条件独立；仍须用同一 selected-only、延迟反馈和
matched direct system-success baseline 做 CPU 证伪。这个方向的贡献边界仍是候选，
不能提前写成已验证的实时 JEV。
