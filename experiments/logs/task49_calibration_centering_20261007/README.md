# 六格中心化诊断证据

对应固定卡与科学报告：`task/task49-pharma-benchmark-baseline-freeze/calibration_centering_v1/`。

- `prepare_v1/`：runner准备、6项合成测试、固定源码与schema。
- `verifier_prepare_v1/`：独立checker准备、40项合成/篡改测试、封存缓存核对。
- `launch_v1.jsonl` 与 `run_stdout_v1.txt`：root发起的唯一真实矩阵运行，180流、25,992决策，8.895秒。
- `verify_launch_v1.jsonl` 与 `verify_stdout_v1.txt`：root发起的独立核验；`v1/independent_verification.json`为最终验证回执，verified、0 issues。
- `v1/`：原config、summary、逐流/配对/OOF结果与77,976条原始事件。

## 事件归档

本地原始 `v1/events.jsonl` 保留不动（257,853,031字节），Git跟踪逐字节可还原的 `v1/events.jsonl.gz`（13,114,722字节）。解压后SHA-256必须为 `2658cc7c01231263ce68fccd2ce1bfa55daf6fb4a790665e8ba02c86e39015c7`。gzip SHA-256为 `2fec71734585dfbd36629cdca8423d3625a9d5a18a4b68137c3730a0ceb736e6`；完整归档前后校验见 `archive_events_v1.jsonl`。

在仓库根目录恢复，已有raw时不运行：

```python
from pathlib import Path
import gzip, shutil, hashlib
p = Path('experiments/logs/task49_calibration_centering_20261007/v1')
with gzip.open(p/'events.jsonl.gz', 'rb') as src, (p/'events.jsonl').open('xb') as dst:
    shutil.copyfileobj(src, dst)
assert hashlib.sha256((p/'events.jsonl').read_bytes()).hexdigest() == '2658cc7c01231263ce68fccd2ce1bfa55daf6fb4a790665e8ba02c86e39015c7'
```

然后使用独立checker，输出到新的文件以保留原验证：

```bash
.srcprobe/beir_venv/bin/python -B -m src.probe_headless.verify_calibration_centering_v1 --run-dir experiments/logs/task49_calibration_centering_20261007/v1 --output /tmp/centering_verification_new.json
```

运行时Git HEAD是 `35293ad6`，两个新增脚本当时尚未提交；因此复现以config中的完整source/input hash及归档脚本为准，不能仅checkout旧HEAD。runner SHA-256=`9c1deb6e05d2e632eaefa7033568472c56485fa5cf1fc963a79c2b821614719a`；checker SHA-256=`e5dcb249ad6f76da4ff2b1fb5c556dc5289e55204ae15cbd1b5cde7697c3e497`。

原summary中的independent_verification=pending是生成时状态，保持不改。之后的独立验证结果以独立回执为准。所有数据只用于development诊断；未运行新API或checkpoint forward，未读取旧confirmation选参数。
