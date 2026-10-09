我先只做三件事：核对这份 `build/main.pdf` 的只读证据、检查中文译稿是否完整、把本次“冻结主版”的记录补到工作日志。不会改 PDF、正文源文件或其他版本。

已完成本轮收敛，当前只认这份主版：

- 主 PDF：`paper/aamas2027/build/main.pdf`
- SHA-256：`8b078e87e659d7921618f1c9cae680891ed0c7f646b31408ec5425bed5427cdd`
- 共 9 页：正文 8 页，参考文献 1 页
- PDF、`main.tex` 和其他候选版本均未修改。

中文译稿已完成，可直接查看：

[中文审阅译稿](</Users/jingwu/work/benchmark/paper/aamas2027/build/main_zh_review.md>)

:codex-file-citation{path="/Users/jingwu/work/benchmark/paper/aamas2027/build/main_zh_review.md" purpose="source"}

译稿覆盖摘要、第 1–13 节、6 张表、2 个图注和参考文献。核心内容是：

1. 科学智能体在多个工具之间选择；
2. 工具结果只能在调用结束并经过延迟后反馈；
3. MIRA-Relay 用冻结工具评分函数、选中工具的局部残差和释放账本进行在线适应；
4. 自然 fresh 菜单上与 Frozen 持平；
5. 在受控正确性漂移下，MIRA-Relay 相比 Frozen 提升 14.18 个百分点。

这份译稿严格对应指定的旧版 `build/main.pdf`，保留了其中的数字和实验结论，没有混入其他 PDF 版本的结果。

:codex-file-citation{path="/Users/jingwu/work/benchmark/paper/aamas2027/build/main.pdf" purpose="source"}