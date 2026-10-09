# Menu sensitivity audit v1

日期：2026-09-27

## 目的

33 个具体 provider 中有 24 个是同一 B3clf 家族的配置变体。这里检查这种菜单粒度是否把 benchmark 的难度和可学习性扭曲了。审计没有改动主输入，也没有新增 provider 调用。

## 固定的 7-provider 代表菜单

`admet_ai_1_4_0`、`deeppurpose_bbb_molnet_morgan`、`graphb3`、`lgbm_bbb`、`b3clf_dtree_common`、`fpadmet`、`admetlab_3_0`。代表清单按公开 provider 语义在运行前固定；不是看标签挑出的赢家，也不是把一个家族的多个输出取最大值。

## 结果

5-fold scaffold split、Morgan 1024-bit、fit-only provider-wise logistic correctness head：

| 菜单 | Best Fixed | 线性 head | Oracle | 线性 − Fixed | Oracle − Fixed |
| --- | ---: | ---: | ---: | ---: | ---: |
| 33 providers | 0.91557 | 0.89618 | 0.99723 | -0.01940 | 0.08166 |
| 7-provider representatives | 0.91557 | 0.90308 | 0.99030 | -0.01249 | 0.07472 |

33-provider 和 7-provider 的 Best Fixed 都是 `admetlab_3_0`。7-provider 线性 head 相比 33-provider 线性 head 上升约 0.00691，但仍在五个 outer fold 中全部低于 Best Fixed；其 oracle headroom 反而较小。

机器结果：`experiments/logs/task49_menu_sensitivity_20260927/results_v1.json`。脚本：`src/probe_headless/audit_pharma_menu_sensitivity.py`。重复运行后结果文件逐字节比较通过。

## 裁定

不把 7-provider 代表菜单替换成主 benchmark，也不因为配置变体数量调整已有结果。保留 33 个具体 provider 作为主任务；论文中可以把 7-provider 代表菜单作为菜单粒度敏感性附录。这个审计没有产生动态 JEV 效果，也不申请 A800。

它排除了一个简单解释：当前线性选择 head 的负结果主要由 24 个 B3clf 变体造成。缩小菜单能减轻困难，但没有让静态选择超过强固定 provider。
