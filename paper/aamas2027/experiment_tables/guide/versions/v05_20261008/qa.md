# Guide v05 QA

根代理实际检查3页render：水印/页眉/页脚齐全；M/T显式区分；蓝色目标行清晰；最后source段落完整，无孤立尾页、裁切或重叠。表字9.5/11.5pt。

本地两次顺序编译，最终无Warning/Overfull/Underfull/undefined/fatal；3页。v01科学独立审查PASS，v05机器核对所有目标、实测锚点和10来源hash不变。v05视觉独立闭环PASS，可提升；实际看三页，最后Sources完整。

v01警告版、v02段落错误版、v03/v04孤立尾页版全部保留。没有新实验/API/GPU/provider调用。
