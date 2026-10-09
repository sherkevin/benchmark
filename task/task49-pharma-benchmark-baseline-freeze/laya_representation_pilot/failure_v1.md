# Laya A800 v1 失败记录

日期：2026-09-27

任务 `01a0de6f07a6e2892d3250d34029961a` 在 A800-SXM4-80GB 上成功加载了 Laya 权重，并通过输入合同、运行时版本记录和 scorer reconstruction。第一批表示导出时失败，错误为：

```text
OSError: [Errno 95] Operation not supported
np.savez_compressed(args.out / filename, ...)
```

`args.out` 位于 `/data/oss_bucket_0/`。`np.savez_compressed` 最后要回写 ZIP 的目录，OSS FUSE 挂载不支持这个随机定位操作，因此任务退出。这个失败不代表模型 forward 或表示质量失败；本轮没有得到可用表示，也没有进入任何选择器训练或效果评估。

完整 worker/scheduler 日志和下载清单在 `experiments/logs/task49_laya_representation_20260926/remote_v2/`。

修复方式是先把每个 `.npz` 写到容器本地临时目录，再用顺序流复制到 OSS，并检查远端文件大小和 SHA-256。修复后的入口增加了本地回归测试；下一轮使用新的输出前缀，保留本轮失败前缀和证据。
