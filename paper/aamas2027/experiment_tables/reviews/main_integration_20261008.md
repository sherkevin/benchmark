# v05 结果表合入 AAMAS 正式主稿

日期：2026-10-08。状态：最终构建、视觉审查、页数检查通过；主代理已提升唯一全文主版。

## 范围与来源

本轮把 `experiment_tables/versions/v05_20261008/main.tex` 的六组结果与 PGFPlots 延迟图合入官方匿名 AAMAS `main.tex`。参考样例和实测版式见 `experiment_tables/reference_style_report.md`；独立预览通过不代替全文检查，按决议 0016 重新构建全文。

表格首先支持同条件的逐列比较，再交代不同估计对象。采用自然宽度、booktabs、真实的两层条件表头、相邻比较行、短 caption 和明确表注。结果体 9pt，表注 8.5pt；没有 resizebox、缩小官方版心或更改 class。原 Figure 1 资产选择及其 submission 条件、Figure 2 引用均保持原样。

## 合入内容

| 实验 | 正式标签 | 修改 |
|---|---|---|
| E1 | `tab:main-results` / `tab:mechanism-results` | 两张拥挤单栏表合为一张自然/受控条件主表；保留 deployable ties、retrospective 边界和受控 paired CI |
| E2 | `tab:credit-ablation` | 新增等权 scaffold-mean 三组对照表；与 E1 pooled-call estimand 分开 |
| E3 | `tab:learned-prior` | 单列 accuracy；paired seeds 使用 `/`，确定性行不重复数字；增加 evaluator-only oracle |
| E4 | `tab:centering-diagnostic` | Raw/Corrected 分组与 multirow，保留本尺度内比较和 intervention exposure 限制 |
| E5 | `tab:cost-validity` | 新增 selection/update 数值表；仅减少列间距以适配单栏，字体不变 |
| E6 | `tab:postcall` / `tab:postcall-verification` | 绑定 renderer 生成新版 refusal 表，表注保留 W/R/C/N 定义、secondary/evaluator-only 身份及每行 785 records/calls |

延迟图使用预览中的记录值与自然零基线；保留描述性点估计、未显示 uncertainty 的说明。实验地图保持 E1–E6，开发诊断先于成本与 post-call，正式结果表顺序随之对齐。baseline-purpose 表改成 9pt 两列角色表；重复的 setup、scope 和 artifact 说明被压缩，唯一的科学限定及可复核数据数量仍在正文。

`bind_postcall_results.py` 仅修改 `render()`。证据 SHA、bootstrap draws SHA、3140/3140/785 行数、rho/kappa、strict receipt verification 和整个 `main()` 未改。CI 仍为 random contrast `[+2.77,+9.29]` pp 与 utility versus accept `[+0.0088,+0.0609]`。

## 科学边界与审查

源码差异核对确认保留：selected fit-best 在 tick 78 后的 evaluator-side label flip、自然 null 与 82/82 rescues/harms、E1 pooled/E2 scaffold 等权的区别、E2 90 streams 和两组 deterministic schedule replicas、E6 reused secondary cohort/fit-only threshold/evaluator-only truth/conditional random/124-cluster bootstrap、mean of stream-p95s、计时排除项、development 与泛化限制。最后将 bootstrap summary 的 `molecule cluster` 明确为 `scaffold cluster`。快照及可重查证据为 `build/review_table_integration_20261008/before_main.tex`、`build/review_table_integration_20261008/source_layout_audit_v1.json` 与 `build/review_table_integration_20261008/source_layout_audit_final.json`。

另一个 reviewer 实际查看第二候选 p5–p8 的视觉审查为 `experiment_tables/reviews/formal_tables_visual_review_20261008.md`：结果为 PASS，无必须修正项；E3/E4 在 p7 左栏纵向排列，E5 在右栏，未强行改成横向配对。binder 校验部分字节不变的独立记录为 `experiment_tables/reviews/postcall_binding_presentation_check_20261008.json`。

没有新实验、GPU、API 或 provider inference。`make submission` 中现有 natural/stress receipt audit 为只读验证。声明范围 gate 与 broader scientific HOLD 的界限没有改变。

## 留痕与构建

合入前源码、binder、provenance 快照和 hash 清单：`build/review_table_integration_20261008/`。各 submission 目录自带 `source_snapshot_main.tex`；失败现场完整保留。

| 构建 | 页数 / 结果 | 处理 |
|---|---|---|
| `build/submission_20261008_091621/` | 10 页，References p10；两处 run-in paragraph overfull | 保存原始 PDF/log/source/binder；压缩重复说明，修正过长段首 |
| `build/submission_20261008_092630/` | 10 页，References p10；无 overfull | p9 只剩结论最后六行，明确页因后压缩重复 Discussion/Artifact 文字 |
| `build/submission_20261008_093454/` | **PASS**：9 页，正文 8 页，References p9 | manifest 状态 complete；不改变字号、版心或 gate |

构建控制台在 `build/review_table_integration_20261008/build_attempt_0{1,2,3}.console.log`；page geometry 摘要和 source-layout JSON audit 同目录。

最终 source SHA-256：`831149e1f05b17a6943dafa8d309ddb98c4643eef5083a1a82e820fc54f47a72`。
Binder SHA-256：`02b0f7019628877f01cd97c00205655881ce6ecfc6e56fb01b9313f1078ed315`。
最终 PDF：`build/submission_20261008_093454/main.pdf`。
PDF SHA-256：`67fcf08e3d4eae094c072428eb3e98a17f5f833d90d2b87a183d6fed76141eec`。
Log SHA-256：`f15483b5a56876e1389d283f5b3701be9b88079f939238b0863f56db3fa7446b`。
Source manifest SHA-256：`b81db7da2a2cfac102d78fdaddd063e9517dd168502c87067c2b19c5d00eb759`。

最终 gate：

| Gate | 结果 / 证据 |
|---|---|
| 投稿声明范围 | `PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE`；未扩大 broader scientific gate |
| Natural / stress receipt audit | verified：270 / 150 streams；`natural_audit.json` / `stress_audit.json` |
| E6 evidence binding | bound：构建前后均通过；`postcall_binding.json` |
| 源码与 PDF 绑定 | complete；构建前后 tex hash 与 snapshot 一致；`source_manifest.json` |
| 页数与 References 位置 | 9 页、References 唯一从 p9 开始；manifest 的 rendered page binding 通过 |
| LaTeX 阻断项 | 无 Overfull、Undefined、Citation、missing math、Fatal、Emergency；原 Makefile gate 通过 |
| 最终目视检查 / 主版提升 | 主 Agent 待执行；本轮没有覆盖 stable `main.pdf` |

剩余日志诊断为 4 个 Underfull vbox、1 个 Underfull hbox（references 长 URL）以及已有模板环境的 `ifx` incomplete conditional；没有声称 zero-warning。第二候选的实际图表检查没有观察到对应裁切或越界。最终 renderer 快照为 `build/submission_20261008_093454/binding_snapshot.py`。

## 可复用交付与下一步

六表和延迟图的宏已经 inline 到被 manifest 绑定的正式源码中；E6 的生成器继续直接绑定审计证据，可复用到后续正文重建。E5 单栏列间距、E3/E4 短表、E1/E2/E6 宽表构成官方双栏稿可继续使用的组合。

主 Agent 下一步查看完整候选 PDF 的受影响页面，再按现有 `promote-main` 流程提升、同步 README/provenance 与工作日志。本子任务不覆盖 stable `main.pdf`。

## 主代理最终检查与提升

根代理实际查看最终构建的 p5–p9；独立 reviewer 的 final closure 已关闭候选2的页数阻断。2026-10-08 09:47:58 UTC 通过 promote-main 提升全文主版，PDF SHA 为 `67fcf08e3d4eae094c072428eb3e98a17f5f833d90d2b87a183d6fed76141eec`。旧版保存在 `archive/promotions/main_before_20261008T094758Z.pdf`，旧 provenance 同目录。

唯一主版、README、main_provenance 和实验表预览均已同步。渲染 QA 在 `build/submission_20261008_093454/visual_qa.json`；这次只完成展示整改，不改变 broader scientific HOLD 或声称引用补全完成。
