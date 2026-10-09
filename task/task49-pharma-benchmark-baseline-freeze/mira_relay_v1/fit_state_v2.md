# MIRA-Relay fit-state v2

状态：fold0/seed0 封存器与独立 verifier 已实现；尚未进入生效 preregistration 或完整效果 replay。

`seal_mira_relay_fit_state_v2.py` 先要求 trust-exact candidate 通过独立 candidate verifier，然后只在 fit cells 上重算候选点 Hessian。它封存 `theta`、冻结 direct prior、family residual/provider bias、Hessian 对角 precision/variance、provider-to-family 顺序以及 fit 区域的 runtime `ok/failure` counts。

`verify_mira_relay_fit_state_v2.py` 不导入 serializer 的状态构造逻辑。它独立检查 candidate/config/input/source identity、NPZ 精确 dtype、provider/family 顺序、direct prior、precision 与 Hessian 对角线、variance reciprocal、runtime counts/status hash 和 canonical state hash。任何 archive、counts、direct prior 或状态字段篡改都应 fail closed。

runtime counts 来自 `inputs.rows[fold0.fit].output_statuses`：`ok` 是 valid return，`error` 或 `failure` 是 runtime failure（回放边界会把 `error` 归一为 `failure`）。valid-but-wrong 的 chemistry label 不会被误算成 runtime failure；unknown/refusal 目前直接拒绝，因为冻结输入的 fit status 合同只允许 `ok/error/failure`。

这是 fit-state 完整性门，不是效果结果。通过后仍需在新合同下决定是否更新 preregistration，再跑完整 CPU 矩阵。

最终 fold0/seed0 收据：`experiments/logs/task49_mira_relay_fit_state_20261003/v7/`，
`state_hash=fb05a340a5ba71286592241b9c2b283f84bed41dc6faac6e872a9e0395f622c7`，
`state_sha256=c8b5828427701ee8647e8653a5ee67750e565cf50d09f8855248db6a8d63f787`，
objective=`3340.106321841232`，gradient∞=`2.3267e-9`。archive、counts 和 direct prior
篡改均由 verifier 拒绝；篡改副本保存在对应实验日志目录，不覆盖 sealed state。
