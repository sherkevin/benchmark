# Figure 2 v11 redraw review

日期：2026-10-05

## Purpose

将用户确认的 v09 紧凑几何重绘为可编辑 SVG/PDF。v10 作为中间渲染版本保留；v11 删除了会靠近反馈回路而产生误读的 unselected-output ghost，并恢复了简短的 `event hidden until τ_t` 说明。

## Checks

- [x] `state → selector → one selected provider → call → terminal record → frozen judge → chemical correctness` 为单一上层主线。
- [x] `system failure` 与 chemical correctness 分开，并在 `terminal outcome` 汇合。
- [x] `terminal outcome → selected outcome → selected-only queue → released outcome → relay update → S_τt → first action` 无跳过箭头。
- [x] `τ_t=t+1+D`、valid-return-only 和 event-hidden 语义可见。
- [x] 长回路只表示 future selector 读取 released state，不进入当前 action。
- [x] SVG、HTML、PNG、PDF 和渲染脚本均保留；SVG 有 title/desc。

## Remaining layout decision

v11 是 1800×620 的宽幅内部 redraw，适合做 `figure*` 几何和语义检查。根据 ADR 0010，它永远不替代 AI 生成的正式视觉资产；当前 `main.tex` 仍使用历史 v9，占位排版尚未替换为 AI v09。
