# 执行前实现核对

2026-10-07；独立只读审查者 /root/headroom_diagnostic_math。对照已冻结实验卡审查 prepare_fresh_representation_v1、fresh_representation_v1、nebula_fresh_representation_v1。

结论：科学合同P0通过。相同完整初始化、BCE+CLS和validation规则；补零列保持0；64复现与OR折叠严格检查；旧initial及来源绑定；evaluation在best冻结后构造；双对照、双seed、1pp和全部LOO判据正确；不完整批次禁止go。

边界：有效容量未匹配，不能把正结果归因于碰撞；高维表示不构成新算法。审查未改文件、未运行实验、未访问confirmation。

worker已完成15项必要合成测试（新7与复用旧8）；CPU prepare首次成功，输入hash与raw位索引、10个initial见input_v1/manifest.json。root须完成独立输入核对、source pin、OSS回验及dry-run后才启动。
