# `build/main.pdf` 中文译稿审计回执

## 源文件

- 源 PDF：`/Users/jingwu/work/benchmark/paper/aamas2027/build/main.pdf`
- 源 PDF SHA-256：`8b078e87e659d7921618f1c9cae680891ed0c7f646b31408ec5425bed5427cdd`
- PDF 页数：9（正文 8 页，参考文献 1 页；以 `pdfinfo` 的 `Pages: 9` 为准）
- 页面尺寸：Letter，612 × 792 pt
- PDF 创建时间（源元数据）：2026-10-07 14:25:50 CST
- 译稿：`/Users/jingwu/work/benchmark/paper/aamas2027/build/main_zh_review.md`
- 译稿 SHA-256（本次写入后）：以 `sha256sum` 重新计算为准；内容变更后须同步更新本回执。

## 只读提取命令

在项目目录执行：

```bash
PDF=/Users/jingwu/work/benchmark/paper/aamas2027/build/main.pdf
sha256sum "$PDF"
pdfinfo "$PDF" | sed -n '1,25p'
pdftotext -layout "$PDF" /tmp/main_build_extract.txt
wc -l -w -c /tmp/main_build_extract.txt
```

本次只使用 `pdftotext -layout` 的文本作为翻译依据，并人工按双栏论文的章节顺序重排。没有从当前 `main.tex`、上层 `main.pdf`、其他迭代 PDF 或实验日志补入内容。

## 覆盖范围

- 摘要、CCS 概念、关键词和 ACM 参考文献格式。
- 第 1–13 节：引言；问题与任务契约；MIRA-Relay 方法规格；Benchmark 与 Baseline；实验；结果；错误与成本分析；机制解释；局限与讨论；有效性与可复现性检查；相关工作；Artifact 范围；结论。
- 全部 6 张表（表 1–表 6）。
- 全部 2 个图注（图 1、图 2）。
- 参考文献保留英文作者、题名和出版信息；不逐条翻译专名和书目信息。

## 翻译与版本说明

1. 主 PDF、主 TeX 和候选迭代文件均未修改；译稿和本回执是新增的审阅文件。
2. 公式用 `$`/`$$` 保留；任务契约中的 $r^{\mathrm{run}}$、$r^{\mathrm{chem}}$、$r^{\mathrm{sys}}$，历史 $H_t$ 与更新算子 $U$ 均按源 PDF 记录。释放更新保留 $n_{a_s,t}^{+}=n_{a_s,t}^{-}+1$、$s_{a_s,t}^{+}=s_{a_s,t}^{-}+r_s^{\mathrm{chem}}$ 的前后状态记号。
3. `agent` 统一译为“智能体”；provider score surface 统一译为“工具评分函数”或“离线拟合的评分函数”；correctness/runtime head 译为“正确性预测头/运行状态预测头”；prequential 译为“逐次先预测、后评估”；propensity 译为“动作选择概率”。
4. `fresh` 译作 `fresh（独立确认）`，避免把 fresh checkpoint/menu 误读为普通“新鲜数据”。
5. 源 PDF 的运行示例明确保留：两次调用的 $r^{\mathrm{run}}=1$，但化学标签不同。
6. 源 PDF 自身在第 6 节写作 “MIRA-Relay changes 164 actions”；译稿忠实保留“164 个动作”，没有用其他版本或其他实验的数字替换。
7. 图形像素本身没有复制到 Markdown；图注已完整翻译。若需要查看图形，应直接打开源 PDF。
8. 个别数学符号由 PDF 字体提取时可能发生视觉分栏或上下标错位；译稿依据同页上下文重排，并在回执中保留源文件哈希，未声称对 PDF 字形进行再渲染。

## 当前校验

```text
正文译稿应包含：19 个 Markdown 二级标题（含摘要/元信息、章节、图注、参考文献），6 个表标题，2 个图注。
```

以上是针对旧版 `build/main.pdf` 的中文审阅译稿，不代表其他版本 PDF 的内容，也不替代英文主稿。
