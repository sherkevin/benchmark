> **历史版本：v1 已被 v2 取代。** v1 的 verifier 没有发现 14 条同标签重复源行未进入任何 ledger；v1 的 manifest/CPU 数字只能作为失败历史，当前结果请看 [provider_fresh_menu_v2/report.md](../provider_fresh_menu_v2/report.md)。

# Provider-fresh BBB 菜单：资格清单 v1

日期：2026-09-30  
状态：**已生成并独立核验；尚未训练 provider，也没有打开 confirmation。**

## 目的

现有 33 个缓存 provider 的上游训练成员和 checkpoint 绑定不完整。它们可以继续做 development/operational routing，但不能直接支撑“model-fresh confirmation”。本任务把一条可执行的补齐路线固定下来：在公开 TDC ADMET Group 的 BBB `train_val` 上重新训练一个小菜单，并把训练成员、验证成员和 157 条确认候选分开。

## 固定输入

| 输入 | SHA-256 |
|---|---|
| TDC BBB `train_val` | `5035139072311f4cb0a1a54cffe11bc727004886ace4629b98f6db20c431e4af` |
| 已读过的 selector development panel | `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf` |
| scaffold-disjoint confirmation 候选 | `c0af602bbc4ec49df448b916fb1e54dfce183926965d61c184cb736e68402805` |

机器清单与逐行文件在 `experiments/logs/task49_provider_fresh_menu_20260930/manifest_v1/`。

## 预先固定的排除与切分规则

1. 排除 canonical 分子标签冲突的整组，不取第一行、不多数投票。
2. 排除已被 selector development panel 读取过的 canonical 分子。
3. 再排除 157 条 confirmation 候选，防止未来改动确认清单时静默泄漏。
4. 同 canonical 且标签一致的重复测量只保留 source row 最小的一行，其他行进入 `excluded_rows.csv`。
5. 对剩余分子按不含手性的 Bemis--Murcko scaffold 做 `GroupShuffleSplit`，seed=`20260930`，validation fraction=`0.2`。

这个规则只决定 provider 的训练/验证成员，不改变 selector development panel，也不把确认标签提供给 selector。

## 结果

| 项目 | 数量 |
|---|---:|
| `train_val` 原始行 | 1,624 |
| canonical 分子 | 1,581 |
| 冲突 canonical group | 8 |
| 被排除行 | 585 |
| provider 候选 canonical 分子 | 1,025 |
| provider train | **845** |
| provider validation | **180** |
| train scaffold | 505 |
| validation scaffold | 127 |
| train/validation scaffold overlap | **0** |
| confirmation 候选（只记录数量） | 157 |

训练集和验证集均无 canonical 重复、无标签冲突、无 development overlap、无 confirmation overlap。独立 verifier 检查了源行恢复、输入 hash、文件 hash、canonical/scaffold 隔离和计数，结果为 `verified`：

`experiments/logs/task49_provider_fresh_menu_20260930/verification_v1.json`

同一命令在两个临时目录逐字节复现，四个输出文件 hash 完全一致。

## CPU provider training v1

在固定 manifest 上完成了一次本地 CPU 训练，代码 commit=`6962ac6c2607470bb8919ad3a1cf42df079a72ed`。这次运行只读取 845 个 provider-train 分子和 180 个 provider-validation 分子，没有读取 confirmation 文件；完整 checkpoint、逐行验证预测、配置和环境收据在：

`experiments/logs/task49_provider_fresh_menu_20260930/cpu_v1/`

四个 checkpoint 和 720 条验证预测都能由独立 verifier 重算，最大概率误差小于 `5.0×10^-11`，收据为 `verified`：

`experiments/logs/task49_provider_fresh_menu_20260930/cpu_verification_v1.json`

验证集上的 sanity metrics（不是 selector 结果，也不是 confirmation 结果）如下：

| provider | accuracy | ROC-AUC | BCE |
|---|---:|---:|---:|
| ECFP-LogReg | 0.9333 | 0.9759 | 0.1639 |
| ECFP-MLP | 0.9167 | 0.9355 | 0.2681 |
| ECFP-RandomForest | 0.9111 | 0.9638 | 0.2538 |
| Graph message passing | 0.7611 | 0.8384 | 0.4655 |

这些数字只说明训练器和验证输出正常，不能用来证明 provider 在 confirmation 上有效，更不能提前证明 MIRA 优于 baseline。Graph 模型的验证表现较弱也保留在记录中；后续若要进入 confirmation，必须按同一合同决定是否保留它，不能因为结果不好而换 split 或删样本。

## 候选 provider 菜单

暂定四个互补 family，名字只表示实现候选，不表示已经通过 baseline 或 confirmation 资格：

1. ECFP + Logistic Regression：线性指纹基线；
2. ECFP + MLP：非线性指纹模型；
3. ECFP + Random Forest：树模型；
4. Molecular Graph Message Passing：图模型。

四个模型必须只读取 `provider_train.csv`；`provider_validation.csv` 只能用于选停止轮或 checkpoint。每个模型最终都要记录源码 commit、特征生成版本、训练行 manifest、验证行 manifest、checkpoint hash、环境锁定和失败日志。

## 这一步解决了什么

- 把“公开 benchmark 有真值”和“provider 是否看过确认分子”拆开，形成可执行的 provider 资格合同；
- 把旧缓存模型从 model-fresh 主张中隔离出来，不再把 33 个相关 provider 当独立来源；
- 给后续一次性 confirmation 提供了一个固定、可复核的 provider 训练输入。

## 还没有解决什么

- 四个 provider 仅完成一次 CPU 训练/验证，不能报告 confirmation 预测；
- 图模型实现、训练预算和最终超参尚未冻结；
- selector 的 backbone、MIRA 更新式、baseline 公平合同和 confirmation 运行闸门仍未完成；
- 因此本文件不会打开 Gate A--C，也不改变现有 development 结果。

下一步是先为四个 provider 写统一训练合同和最小 CPU smoke；只有训练成员、checkpoint 绑定和前向输出全部可审计，才决定是否用 A800 训练最终菜单。
