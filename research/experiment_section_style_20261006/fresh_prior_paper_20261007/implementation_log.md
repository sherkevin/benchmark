# 学习型prior结果回填

目的：把当前四工具的匹配BCE/BT真实开发结果写成紧凑数表，压缩重复审计描述；保留正文恰好8页，不改原方法和157分子确认结果。指标：5行10数与独立结果一致、无未定义引用/溢出、References从第9页开始、页面目视合规。

已保留原main.tex/references.bib副本。检测到协作者新增MIRAOverviewAsset双版本图开关和Makefile提交模式后，root未覆盖，先在main_fresh_prior_candidate_20261007.tex整合；仅停止本线程误启动的未编辑稿062120编译，协作者阅读稿编译不动。待副本通过再合回。

加入静态开发表，列Best Fixed/Raw Frozen/Corrected Frozen/BCE+CLS/BT+CLS；两seed分别报，不把重复分子合为独立样本。删除重复audit contract表及重复回放段，证据边界仍保留在Validity与Artifact。

补两条原本缺少的baseline引用：Wu与Lu的ICLR2026 Reward Model Routing in Alignment，以及Poon等人的AAAI2026 Online Multi-LLM Selection。元数据按2026-10-07官方会议页核对，URLs见revision.json。两者仍标任务适配，不冒充无改动复现。

编译首次因BibTeX绝对输出路径被openout_any=p拒绝；首遍LaTeX成功。保持失败日志，改用仓内相对路径重试BibTeX与后两遍；不改安全配置、模板、字号或边距。结果绑定见table_bindings.json，10个单元格均已通过独立结果映射。

最终构建与页面检查完成后追加到revision.json。三份完整科学标准保持HOLD。

最终：三轮LaTeX与BibTeX完成，9页总计、正文8页，References第9页。第7/8页目视表格清楚、无裁切；所有数字绑定通过。候选已合回main.tex，保留协作者图开关。此轮为多文件候选编译，不冒称新一轮make submission或clean-tree构建。PDF与源码hash见revision.json。脚本初次把References大小写误写为全大写而停止于修改前；按实际标题修正后通过，PDF本身未变。
