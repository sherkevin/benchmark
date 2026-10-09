# MIRA-Relay context-relative reliability v1 report

- 开发面板（722 BBB）上，MIRA-Context 的平均 accuracy 为 79.19%，Frozen 为 79.38%，差 -0.19 pp；这是开发诊断，不用于 confirmation 调参。
- 157 分子 fresh natural confirmation 上，MIRA-Relative（旧全局因子）与 Frozen 都是 89.81%，差 0；独立回放 240/240 通过。
- 同一 fresh menu 的预登记 controlled correctness drift（中点后翻转 fit-best selected label）上，MIRA-Context 87.56%，Frozen 73.38%，差 +14.18 pp，scaffold bootstrap 95% CI [+9.61,+19.03] pp；120/120 streams 独立回放通过。

结论：只乘全局 reliability factor 的版本不会改变动作，已停止；context-relative residual 在明确的 provider correctness drift 下能利用 selected-only delayed feedback 切换，同时自然 fresh menu 不损失。该正结果是受控非平稳机制证据，不写成自然漂移泛化。
