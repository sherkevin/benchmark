# 执行进度

2026-10-07。单线任务已提交：`01a1153c076660894f3390124224a569`。执行commit `84c61207d7cccece592d0f5879e8122ef4ed2b7d`；输入SHA `8109405b121472e7628f987ccae72ee90d7442f64f36301efca65c733ce58447`。现已success/FINISHED，回收40文件且独立验证通过；20fits/4000epochs/2888OOF齐全，结果见report.md。

## 已完成

- 冻结实验卡、数学审查和实现P0审查；15项定向测试通过。
- CPU准备首次成功，722行64重建与旧seal一致、1024→64 OR全通过，10份扩展initial封存；root独立特征/继承行/hash检查通过。
- 原输入34,587,591bytes；Git存6,699,626bytes无损gzip，原文留本地并用于OSS。下载回验SHA一致。
- source/输入准备提交84c61207已推远程；doctor及dry-run通过。1worker×1A800、15CPU/240GB，原四工具不再forward。

## 传输问题与处理

首次任务列表查询超时，重试显示无运行任务。OSS区域入口可达；bucket DNS解析到39.98.14.70时TLS失败，使用区域入口实际解析的47.92.17.55并保留bucket原始TLS域名可达。path-style请求被OSS要求三级域名而拒绝，未继续使用。

本地仅绑定127.0.0.1的短时CONNECT转发将该bucket:443连到已核IP；拒绝其他目标，不解密TLS，不改证书校验，不写系统DNS，不经过Clash。官方ossutil通过这个临时转发完成上传/下载；输入及输出前缀为空，上传后回验原文件SHA。脚本和探测日志保存在remote_v1，20分钟自动结束；后续下载须重新确认地址，不将IP当永久配置。

## 证据与下一步

`experiments/logs/task49_fresh_representation_20261007/`保存config、输入manifest、sourcehash、命令、失败、OSS校验和taskID。独立核验器已准备，并经worker只读对照schema未发现P0。

上述回收与独立核验已完成，未过双对照go，停止该固定构造；查询失败期间未重复提交。三份科学标准仍HOLD，正文保留已验8页。
