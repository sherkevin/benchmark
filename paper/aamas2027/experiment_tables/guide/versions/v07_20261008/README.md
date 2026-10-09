# Guide v07 / 2026-10-08

6页独立水印目标与分布参照。v06的数值与条件要求保留；新增128条实测到达顺序的共同方法分布、配对误差、协方差、实际分位数与分别标注的区间。

- [PDF](guide.pdf)、[LaTeX](guide.tex)、[manifest](manifest.json)。
- 原始预算：[conditional_requirements.json](conditional_requirements.json)，只用于原manifest顺序。
- 来源与说明：[根目录README](../../README.md)、[v07修订说明](../../v07_修订说明.md)。
- 审查：[科学](../../reviews/v07_arrival_scientific_review.md)、[视觉](../../reviews/v07_arrival_visual_review.md)。
- 现有固定过程参照22/24；未知未来候选仍UNESTABLISHED。审查分不是概率。

新页只读已有结果，未运行新的训练/推理/回放。保留natural负收益及3个受控负收益顺序。已有分布不自动赋给未来候选。主版的成本/拒答label变为Table8/7，两个heading已对应同步；旧导出和审查在 `before_current_master_label_sync/` 完整保留。

内置编译和本地导出、源/PDF/审查hash均记录在manifest。本地两个尾页Underfull hbox已记录，实际目视无裁切/碰撞。Guide不把目标值写入正文。
