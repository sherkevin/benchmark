---
kind: frontend_style
name: PhysicianBench 站点：Tailwind v4 + CSS 变量设计系统
category: frontend_style
scope:
    - '**'
source_files:
    - task7/poc3_physicianbench/PhysicianBench/website/package.json
    - task7/poc3_physicianbench/PhysicianBench/website/postcss.config.mjs
    - task7/poc3_physicianbench/PhysicianBench/website/src/app/globals.css
    - task7/poc3_physicianbench/PhysicianBench/website/src/components/Hero.tsx
    - task7/poc3_physicianbench/PhysicianBench/website/src/components/Navbar.tsx
---

本仓库仅在 `task7/poc3_physicianbench/PhysicianBench/website` 子目录中包含前端代码，其余部分为纯 Python 评测内核与 Markdown 文档，无全局样式体系。该站点的风格系统如下：

1. **技术栈**
- Next.js 16 + React 19（App Router）
- Tailwind CSS v4，通过 `@tailwindcss/postcss` 在 PostCSS 中启用
- TypeScript 5，组件以 `.tsx` 单文件组织
- 图表使用 Recharts 3

2. **核心文件**
- `src/app/globals.css`：集中声明全部设计令牌（颜色、阴影、圆角、字体）、动画 keyframes 以及通用 UI 类（`.chip-*`、`.clinical-card`、`.chart-card`、`.stat-card`、`.btn-primary`、`.btn-ghost` 等），并通过 `@import "tailwindcss"` 引入 Tailwind
- `postcss.config.mjs`：仅注册 `@tailwindcss/postcss` 插件，零自定义配置
- `package.json`：锁定 tailwindcss ^4、@tailwindcss/postcss ^4、next 16.2.4、react 19.2.4、recharts ^3.8.1
- `src/components/*.tsx`：页面级组件（Hero、Navbar、Leaderboard、Breakdowns、FailureAnalysis、Methodology、TrajectoryEmbed 等）以 Tailwind 原子类为主，辅以 globals.css 中的语义化类

3. **架构与约定**
- **设计令牌层**：所有品牌色、中性色、阴影、圆角均通过 `:root` CSS 自定义属性暴露（如 `--accent`、`--coral`、`--paper`、`--ink`、`--shadow-accent`、`--r-lg`），组件内通过 `var(--xxx)` 引用，避免硬编码色值
- **双轨样式策略**：布局与间距优先用 Tailwind 原子类（`pt-20 pb-24 px-6 grid md:grid-cols-[1.05fr_1fr] gap-x-10`），视觉语义块（卡片、按钮、标签、徽章）则封装为全局 CSS 类，保证跨组件一致性与可维护性
- **字体系统**：标题使用 Fraunces（带 ops/SOFT/WONK 变体），正文 Inter，代码 Mono，通过 CSS 变量注入到 `font-family`；提供 `.font-display`、`.font-emph`、`.font-mono` 等辅助类
- **动效规范**：统一使用 `fade-in` / `score-bar` / `float-gentle` / `pulse-dot` / `steth-swing` / `data-flow` 等 keyframes，配合 `cubic-bezier(0.22, 1, 0.36, 1)` 缓动曲线，保持微交互一致性
- **响应式策略**：基于 Tailwind 断点（`md:`、`lg:`）与 CSS Grid 列宽比例，未引入独立媒体查询
- **主题色板**：主色 teal（`#0d9488`）+ 强调 coral（`#f97316`），类别色（dr/cr/ae/doc）采用低饱和版本，整体偏向“临床/学术”气质

4. **开发者应遵循的规则**
- 新增颜色必须先在 `:root` 中以 `--xxx` 形式声明，再在组件中通过 `var(--xxx)` 引用，禁止直接写十六进制色值
- 布局与间距优先使用 Tailwind 原子类；需要复用的视觉块（卡片、按钮、标签、徽章、统计卡）应提取为 globals.css 中的语义类
- 动画统一复用已有的 keyframes 命名，不随意新建；缓动曲线固定为 `cubic-bezier(0.22, 1, 0.36, 1)` 或 `ease-out`
- 字体选择遵循：标题 → Fraunces，正文 → Inter，代码 → Mono，必要时通过 `.font-display` / `.font-emph` / `.font-mono` 覆盖
- 组件文件保持单一职责，样式尽量外置到 `globals.css`，不在组件内嵌 `<style>` 或 CSS-in-JS