# AI figure paper-scale QA — Figure 1 v29 / Figure 2 v26

日期：2026-10-08。

## 构建

- 源：`generated/figure1/v29_ai.png`、`generated/figure2/v26_ai.png`
- 页面：每张图以 7 inch 通栏宽度嵌入独立页面。
- PDF：`ai_structures_7inch_20261008.pdf`
- 编译：两次 `pdflatex -interaction=nonstopmode -halt-on-error`，无 warning。
- 渲染：`page-1.png` 为 Figure 1，`page-2.png` 为 Figure 2，180 dpi。

## 结果

独立复核见 `../../external_review/20261008_figure1_role_v39_7inch_followup.md`：

- Figure 1 的主链、公式、延迟边和水平 `Update`→高亮行内部写入端口在 7 inch 下仍清楚。
- Figure 2 的时间线和 Figure 1 的架构职责仍可区分。
- 次级字段（`output/runtime status`、`runtime error`、计数格细节）偏小，因此纸面可读性项保持 1/2；没有新增硬错误。

## 哈希

- PDF SHA-256：`059907f167ba3907872ba3fa1b03f0772f70af87b50f67103c3c7e4cda6debb8`
- `page-1.png` SHA-256：`bf08cd90e0b98739615135a518059d34c45c92c7172f4c4e0b3ea7ec139c1ace`
- `page-2.png` SHA-256：`814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa`

该 QA 只验证图稿纸面表现，不改变 Figure 1/2 主版，也不自动替换 `figures/qa/main.pdf`。
