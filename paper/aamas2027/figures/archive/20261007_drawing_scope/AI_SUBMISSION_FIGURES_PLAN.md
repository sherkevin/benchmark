# AAMAS 2027 三张 AI 投稿图长期打磨计划

**当前唯一工作目标**：只处理三张主图；三张主图均有 AI 生成的正式候选，视觉风格接近 Top-Conf Figure Gallery 的高质量 architecture / pipeline / results 图；每一版都保留原始 PNG、完整 prompt、版本索引和审查记录。最终进入论文的图必须同时满足语义、数据流、审美和 PDF 缩放可读性。

## 范围边界

本目标不处理实验、正文重写、Benchmark、Baseline 或其他论文资产；只处理三张图的 prompt、AI 原始图、版本、审查和最终排版接入。

## 当前事实

- **Figure 1（overview）**：AI v05 是当前候选，路径 `generated/figure1/v05_ai.png`。它由 v11 完整 prompt 生成，修复了 v02/v03 的输入到反馈错误箭头和 v04 的 terminal outcome 断链；形式稿路径尚未切换，待三图统一通过最终 PDF 缩放检查后再切换。
- **Figure 2（lifecycle）**：AI 历史资产为 `generated/figure2/v07_ai.png`–`v22_ai.png`。v22 保留 v21 的紧凑 queue 拓扑，并改用当前 MIRA-Context 合同的 `provider residual + released counters` 标签；它仍需通过最终 PDF 缩放检查。正式稿目前仍引用 `figure2/v13/figure2_v13.png`，需要等语义对齐候选通过后再切换。
- **Figure 3（results / evidence）**：AI v05 是当前 receipt-bound 数据候选，路径 `generated/figure3/v05_ai.png`；v01 的 ProbeNoUpdate 比较口径、v02 的 “Other” 图例、v03 的空数据布局和 v04 的命名过渡版均已保留。v05 使用完整 AI prompt 将 `data_binding_v1.json` 中已核验的曲线、点、柱高、坐标和方法名放入 v03 布局，并把面板 (b) 的方法名统一为 `MIRA-Relay`；没有凭空补数字。尚未接入正文。

## 统一审查门

每张图按 16 分检查：

1. **语义合同 0–4**：模块与论文方法一致；无过时名称、无越界模块、无虚构组件。
2. **因果/数据流 0–4**：输入、选择、执行、判定、反馈、更新的方向和可见性无歧义；没有 shortcut 或信息泄漏。
3. **样例库风格 0–4**：模块优先、短标签、统一线宽、有限配色、适度留白、颜色有语义而非装饰。
4. **投稿可读性 0–4**：双栏/通栏最终尺寸下文字、箭头、公式和图例可读。

任何硬错误（错误箭头、旧方法标签、AI 虚构数字、未观察信息被画成已观察）直接淘汰该版本，不以总分抵消。

## 版本工作流

1. 先写自然语言结构和完整 prompt；不在 prompt 里依赖未保存的上下文。
2. 用 AI 生成原始图，保留生成文件和 SHA-256。
3. 按 16 分门逐张审查；上一版有硬错误时重写完整 prompt，不在原图上手工修补。
4. 通过后编译英文稿和中文阅读版，在最终尺寸目视检查。
5. 只把通过三份验收标准、图文一致性和 PDF 缩放检查的版本标为 current；旧版本全部保留。

## 当前顺序

1. Figure 1 v05 和 Figure 2 v22 已通过语义、因果、样例库风格和 7-inch 通栏 PDF QA；保留为当前候选。
2. Figure 3 v05 已通过 receipt 对齐检查和 7-inch 通栏 QA；v03 空布局、v04 数据候选和 v05 命名一致版都保留。
3. 三图 joint-lock 记录 `external_review/20261007_three_figure_joint_lock.md` 已完成：Figure 1 v05、Figure 2 v22、Figure 3 v05 各 16/16，未发现硬错误。正式稿仍未切换，当前不改 `main.tex`。
4. 将三图接入副本整稿的 QA 记录在 `external_review/20261007_three_figure_paper_integration_qa.md`：图像和编译通过，但总页数为 10 页，未通过当前 9 页总计预算；不在图稿任务中擅自压缩正文。
