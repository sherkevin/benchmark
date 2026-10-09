# Figure 1 v2: 自然语言结构说明

## 一句话功能
用一个从左到右的单次决策循环，说明 MIRA-Relay 如何在只看到已释放的历史和当前工具菜单时选择一个 provider，接收该 provider 的终止结果，在延迟后只释放被选工具的反馈，并用它改变下一次选择。

## 读图顺序
1. **决策时可见输入**：分子 `x_t`、端点 `e_t`、当前菜单 `A_t` 和已释放历史 `H_t` 进入选择器；菜单约束由 A_t 标签给出，旁边明确写出未选输出不可见。
2. **选择器**：灰色的 fit-only direct head 是冷启动分数和不可变 fallback；蓝色的 MIRA-Relay 状态包含 family residual 和 provider bias；橙色的小标签表示 probe / exploit 分支。模块只输出一个 action `a_t`。
3. **一次真实调用**：只调用被选中的 provider `a_t`，产生 output `o_t` 和 runtime status `s_t`。未选 provider 不产生可见结果。
4. **调用后判定**：verifier 在 provider 返回后决定 accept、escalate 或 refuse；冻结 judge 产生 selected-only 的 end-to-end label `y_t = system_success`。
5. **延迟释放**：`y_t` 进入 delay queue，在 release tick `τ_t` 才对 learner 可见；队列之前用虚线反馈表示“尚未可读”。
6. **更新和下一步**：MIRA-Relay 只用已经释放的 selected event 更新 family residual、provider bias 和 hazard ledger，然后状态回到下一个决策。

## 语义边界
- **蓝色**：本文提出的可更新 MIRA-Relay 状态。
- **灰色**：fit-only 或运行时公共组件，不随线上事件改变（fallback、菜单、冻结 judge）。
- **橙色**：决策分支或关键反馈事件，表示 probe/exploit、selected-only label。
- **紫色**：时间延迟队列，强调反馈不是立即可见。
- 当前结果、准确率和提升值不放在 Figure 1 中；这些放在结果表和曲线中。

## 版式约束
- 画布 `1800 × 560`，宽高比约 `3.21:1`；放入 AAMAS `figure*` 后约 2.1 英寸高。
- 上方横向带标明 information boundary：`before action` 与 `after selected call`。
- 主数据流只有一个方向；延迟反馈使用一条回到 state 的虚线箭头，避免箭头交叉。
- 盒内每个模块最多三行文字，正文用 22–30 px，次要标签用 18–21 px，legend 保持 14 px。
- 不放公式推导，不放结果数字，不用红绿区分，不使用阴影或装饰性渐变。


## v3 semantic revision

The post-call panel distinguishes runtime verification from the independent frozen endpoint judge. It labels the correctness outcome as hidden until the release tick, states that runtime failure yields `y_t=0` with a null chemical label, names the shared MIRA-Relay state, and records the fallback ledger-only exception. The feedback loop now says “next visible decision” instead of assuming `t+1`.
