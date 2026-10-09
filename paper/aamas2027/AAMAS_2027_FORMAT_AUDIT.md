# AAMAS 2027 主轨格式核验

> 状态：内部格式审计，2026-10-06。本文档记录已核对的官方要求和当前 PDF 证据，不替代投稿时对官网最新版本的复核。

## 1. 官方依据

- [Instructions for Authors](https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/instructions/)
- [Call for Main Track Papers](https://warwick.ac.uk/fac/sci/dcs/aamas2027/calls/call-for-main-track/)
- [Reviewer Guidelines](https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/reviewer-guidelines/)
- [Official Q&A](https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/qa/)
- [AAMAS 2027 template](https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/instructions/)

核对到的时间点：摘要截止 2026-10-01 AoE，全文截止 2026-10-08 AoE。最终提交前必须重新打开上述页面核对是否有更新。

## 2. 硬性格式门槛

| 项目 | 当前规则 | 当前稿检查 |
|---|---|---|
| 文件 | 英文 PDF，使用官方 LaTeX 模板 | 通过：`reference/template/aamas.cls` |
| 匿名 | 主轨双盲；正文不能泄露作者和机构 | 通过：`sigconf,anonymous`，正文未放作者信息 |
| 长度 | 主文最多 8 页；参考文献页另计 | 当前图表集成验证 PDF 8 页；达到上限但未超页 |
| 附录 | 最新 Q&A 将附录计入 8 页上限 | 当前没有附录；若以后加入，仍必须计入主文页数 |
| 补充材料 | 可选，单个 zip 不超过 25 MB；审稿人不保证查看 | 不能把关键方法、数据定义或核心结果只放补充材料 |
| 投稿信息 | 模板中的 submission ID、submission type、版权区块和关键词保留 | 已保留 |
| 图表 | 图题在图下，表题在表上；非装饰图需要 `\\Description{}` | 当前两幅正式矢量图均已提供 caption 和 Description |

“正文最好正好 7 页”不是官网硬性要求。当前版本自然排到 8 页，仍符合官方上限；不通过空白、缩字号或无关文字凑页。最终应检查的是不超过 8 页且信息完整。

## 3. 当前 PDF 证据

构建命令在独立输出目录中顺序执行了三轮 `pdflatex`，中间用官方模板的 BibTeX 样式完成 BibTeX，再执行最后一轮 LaTeX。没有并发构建。

- 源文件：`main.tex`
- 验证目录：`build/review_20261006_v16/`
- 当前 PDF：[build/review_20261006_v16/main.pdf](build/review_20261006_v16/main.pdf)
- 当前图表集成版页数：8。
- `pdfinfo` 标题：`After the Decision: Learning to Adapt from Delayed Outcomes`
- 当前 PDF 大小：1065342 bytes；SHA-256：`6de7eb6673c2899f03fe44a95b87f8313384107ad1c8ffe5cd18be1e6f5a50f3`
- `pdftotext` 未发现 `Temporary page`；8 页均为有效页面。
- `CCS` 已写入 metadata；最终 `main.log` 没有 undefined citation/reference、temporary page 或 overfull box；矩阵表仍有 underfull 断行提示。

当前主稿已填入声明范围内的真实结果，不再有 `TBD` 或内部 pre-results banner；`make submission` 现在执行声明范围内的本地 preflight，仍不执行上传。当前正文 8 页由方法、结果、图表和 artifact scope 共同构成。

## 4. 非致命警告与处理顺序

1. 当前日志没有 overfull box；矩阵表存在 underfull hbox/vbox 提示，属于窄列自动断行，需在结果填回后再做一次最终排版复核。
2. `\\balance` 环境在本机仍产生模板条件警告 `end occurred when \\ifx ...`，未修改官方 class；投稿前用完整 TeX 环境再次核验。
3. BibTeX 对少数条目的 publisher/address/page 给出元数据警告；不影响编译，后续逐条补齐。

任何一项不能用改字体、缩页边距、删掉方法信息来“修复”。

## 5. 投稿前门禁

- [ ] 官方页面再次核对日期、页数、匿名和补充材料限制。
- [ ] 用干净目录从提交 commit 重建 PDF；不能依赖已有 `.aux/.bbl`。
- [ ] 主文不超过 8 页；参考文献另计；附录若有则计入 8 页。
- [x] 删除内部 pre-results banner、所有 `TBD`、占位图和内部路径。
- [x] 真实结果、数据划分、失败处理、成本和统计不只存在于补充材料。
- [ ] 所有图有可读的线条/文字冗余和 `\\Description{}`；黑白打印仍可区分。
- [ ] 最终匿名性、引用自引和源码元数据复核。

当前结论：**格式构建已通过；声明范围内的科学结果和图表已回填，仍需作者最终双盲与官网复核。**


## 2026-10-06 最终排版与结果回填复核

- 当前构建：`build/review_20261006_v16/main.pdf`。
- `pdfinfo`：9 页总计；正文正好 8 页，参考文献从第 9 页开始。
- SHA-256：`6de7eb6673c2899f03fe44a95b87f8313384107ad1c8ffe5cd18be1e6f5a50f3`。
- 三轮 LaTeX + BibTeX + LaTeX 完成；最终日志没有 undefined citation/reference、overfull hbox/vbox、Missing `$`、fatal 或 emergency。
- 第 8 页为完整的 Artifact Scope 双栏内容，不是空白页或无关占位。
- 正文结果已绑定 fresh natural 和 controlled stress 收据；MIRA-Context 的自然 null 结果与 controlled drift 的正结果分开。
- 仍需投稿前重新打开官方说明页核对日期、匿名信息和补充材料；`make submission` 只生成本地候选 PDF，不替代作者最终提交。


## 2026-10-06 v16 final evidence/layout audit

- `build/review_20261006_v16/main.pdf`：9 页总计，正文正好 8 页，参考文献第 9 页；SHA-256=`6de7eb6673c2899f03fe44a95b87f8313384107ad1c8ffe5cd18be1e6f5a50f3`。
- 最终 `main.log` 未发现 `Overfull`、`Undefined`、`Missing $`、fatal 或 emergency；第 2 页新 Figure 1 v08 已与 provider-local residual 语义一致。
- 图版本：Figure 1 v08，Figure 2 v13；历史 v07/v12 保留，不覆盖。
- 结果证据：natural 270/270、controlled stress 150/150 contract/replay verification；其中新增 30 条 BayesianRouter stress comparator；MIRA-Context 的计数器已进入序列化状态/hash。

## 2026-10-06 clean-tree submission rebuild

- 来源：提交 `c2656260` 的 Git archive；不使用工作区 `.aux`、`.bbl` 或历史构建缓存。
- PDF：[clean_submission_20261006_main.pdf](clean_submission_20261006_main.pdf)
- `pdfinfo`：9 页总计；正文正好 8 页，参考文献第 9 页。
- 文件大小：1065349 bytes；SHA-256：`543d845970325577dc458dbdc73f46d331c644a243d732b21fc38b58d423cf79`。
- 干净 `main.log` 未发现 `Overfull`、`Undefined`、`Missing $`、fatal 或 emergency；第 2 页架构图和第 8 页 Artifact Scope 已目视检查。

## 2026-10-06 Bayesian stress rebuild

- 来源：提交 `07d88e20` 的 Git archive。
- PDF：[clean_submission_20261006_bayesian.pdf](clean_submission_20261006_bayesian.pdf)
- `pdfinfo`：9 页总计；正文正好 8 页，参考文献第 9 页。
- SHA-256：`458f804f8750209e9377429b8cf14ad8deba2c94214759eba6ee4a7bf554c04a`。
- 干净日志未发现 `Overfull`、`Undefined`、`Missing $`、fatal 或 emergency；第 2、5、6、8 页已目视检查。

## 2026-10-06 matrix rewrite build

- 当前源文件 `main.tex` SHA-256=`55eb99725c03b1b87b5b32056b663787ba65d5a3a85f82636ac07d2564fff513`。
- 当前 PDF：`build/clean_submission_20261006_matrix_rewrite.pdf`；总页数 9，正文正好 8 页，参考文献第 9 页。
- PDF SHA-256=`05a5a4173e14417f83ae43ef4e5069b4e09736a09c58be1ad34d7f3d9b2af346`；日志 SHA-256=`ec74473182d9e0f077d33471c7a67c0f6b18b358daaa39dff72d6bc50cdaa9ac`。
- 这版把实验地图压成四行，并把 natural fresh / controlled drift 结果分别放入主表；第 5、6、7、8 页已目视检查。
- 正式图仍为 Figure 1 v08 与程序绘制 Figure 2 v13。生成式 AI 候选不进入提交构建。
- 日志无 `Overfull`、`Undefined`、`Missing $`、`Fatal` 或 `Emergency`；仅保留官方模板常见的 `Underfull \vbox` 提示。

## 2026-10-07 投稿 preflight

- `make submission` 已执行成功，检查了声明范围科学状态、双盲 class、残留占位词、BibTeX/LaTeX 日志和页数；该目标只生成本地候选 PDF，不执行上传。
- 当前候选：`build/submission_20261006_181518/main.pdf`，9 页总计（正文 8 页、参考文献第 9 页）；PDF SHA-256=`abf88071fe9e829d1207d354513396533ec8c1379e2fa9eb9001193aac097394`。
- 独立回执合同审计已通过 420 条 fresh streams，覆盖 event identity/order、decision-terminal-feedback binding、due/release clock、decision allowlist、summary counts 和跨事件 opaque hash/version continuity；旧回执的 learned-state hash 独立重算仍明确为缺口。

### 2026-10-07 clean-tree final build

- 来源：当前提交 `dff33da9` 的 Git archive；不使用工作区缓存。
- `build/clean_submission_final/main.pdf`：9 页总计，正文正好 8 页；PDF SHA-256=`663d9fcdf779c6043b8c9b7883bf1887f2bba8fff6a556714e57f0fb0c08205f`。
- 源码 SHA-256=`ee5264e8819f1f3f2e6c06292105cc3d1c3837ef0f46fe3958d2d59ce81c3503`；日志 SHA-256=`c03a6d0645317fc663fa42d7eec4b4a9b3aaaa5b4163edf76a1694bef376e212`。
- 第 2 页架构图和第 3、5、8 页正文已目视检查；日志没有 overfull、undefined citation、missing math、fatal 或 emergency。
