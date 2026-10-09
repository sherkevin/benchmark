# Context residual experiment evidence

The actual run is `v1/`; `prepare_v1/` contains synthetic numerical tests only.
`launch_v1.jsonl` records the exact execution command and source/card hashes.
The independent verification lives in `../task49_contextual_residual_verification_20261007/v1/`.
Scientific interpretation is in `task/task49-pharma-benchmark-baseline-freeze/contextual_residual_v1/report.md` from repository root.

The raw `v1/events.jsonl` remains local. Git stores `v1/events.jsonl.gz` to avoid a large text blob. To restore a fresh clone from repository root, without rerunning the experiment:

```bash
python3 -B - <<'PY'
from pathlib import Path
import gzip, hashlib, shutil
p = Path('experiments/logs/task49_contextual_residual_20261007/v1')
target = p / 'events.jsonl'
expected = '05b8e80b2dbd9a8545b4bdbf5d60ff6870416d469d3d1bca439567347bfb8c54'
if not target.exists():
    with gzip.open(p / 'events.jsonl.gz', 'rb') as src, target.open('xb') as dst:
        shutil.copyfileobj(src, dst)
assert hashlib.sha256(target.read_bytes()).hexdigest() == expected
PY
```

Raw bytes: 88,163,544. Gzip bytes: 11,860,353. Gzip SHA-256: `6bad1545e44ceed2416da0a2eacc53340f49e6bc7b275e3b593ef1776ca69f37`.
`archive_events_v1.jsonl` verifies lossless restoration. Old six-cell inputs may also need restoration using their own log README.
Use a new output directory for any independent recheck; never overwrite the saved receipt or rerun science merely to recover logs.
