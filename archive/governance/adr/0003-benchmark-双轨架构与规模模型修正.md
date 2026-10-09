# ADR 0003 · benchmark 双轨架构(科学正确性 / 编排诚实度)+ 规模模型修正

- **状态**:Active
- **日期**:2026-06-12
- **领域**:benchmark 架构、ckpt 设计、规模估算
- **关联**:[[0002]](构造路线)· memory `nobelclaw-platform-api`(后端探活)· `longhorizon/定义_长程任务与桩点.md`(模具)

## 背景

ADR-0002 定了"造 trace",但红队 + 三路调研(2026-06-12)暴露两个必须在建题前定档的事实:

1. **后端探活(authoritative,推翻 06-04 旧结论)**:NobelClaw 只有**材料域(Elements)真算、在线**(predict energy/classes/tc 对输入敏感;DiffCSP generate 已恢复);**药物 100% mock-canned**(`generate_drug_report` 对任意输入字节相同),**蛋白 mock**(无 `PROTEIN_API_URL`)。
2. **GT 丰度 + 红队**:真·过程 GT 极稀缺(全球只有 MPtrj 逐帧 DFT + SEISMO 逐步 oracle 两个 crown jewel,MPtrj 属材料);∏mᵢ 组合公式因 ckpt 强依赖**不成立**。

→ "测科学正确性"今天只在材料域可能;药物/医学今天只能测"编排诚实度"。owner 拍板(2026-06-12):**双轨并行,都现在建,各轨诚实标注测什么。**

## 决策

### 1. benchmark 分两轨,每题显式标 `track` + `measures`

| 轨 | 域 | 后端 | 测什么 | ckpt 主力 |
|---|---|---|---|---|
| **科学正确性轨** | 材料(Elements) | 真算 | 预测/发现/优化的**科学对错** | 数值/集合/排序(外部 DFT 库真值)+ 守恒/结构 |
| **编排诚实度轨** | 药物/蛋白/医学 | mock | agent **过程**对不对:跳步/断裂/吹牛/该停不停(D3/D4/D6),**不判科学对错** | 工具落地/守恒(trace 内部)+ 结构/语义 |

### 2. ckpt 硬规矩(写进模具)

- **禁在 mock-canned 工具的输出上建科学 cp** —— 那是写死的字符串,测它等于测 LLM 记忆。编排轨的 cp 只能判 **agent 自己 trace 里的决策**(调了哪些工具、覆盖没覆盖、前后是否自洽),不判工具输出的数值。
- 每个 ckpt 加三标签:`track` / `backend_class(real-compute|mock-sensitive|mock-canned)` / `gt_class(external-anchor|rule|internal-consistency)`。

### 3. 规模模型修正(替换 ∏mᵢ)

> 可用题量 ≈ **Σ(独立真锚)× {L0–L3 介入档} × {k 反事实变体} × {r 重复跑}**。
> 规模来自"真锚数 + 介入档 + 反事实 + 重复",**不是 per-ckpt 连乘**。每类的真锚数由"GT 可得性 spike"实测,不预设。

## 理由

- 双轨 honest:既不假装 mock 后端能测科学,也不浪费"材料真能测科学"这个唯一真金;守住 owner 要的"材料+医学+全长程"。
- 禁 mock-canned 科学 cp:否则指标测的是 LLM 训练记忆,可信度归零(`goal.md` 红线)。
- ∏mᵢ 修正:避免高估题量、避免按错误公式硬凑组合(详见红队分析,`todo_task7_poc.md` 2026-06-12 下午)。

## 后果

- ✅ 材料轨现在就能建真科学题:`case1`(发现漏斗)+ `case D`(闭环优化),GT 全锚 MP/OQMD/MPtrj。
- ✅ 药物/医学轨现在也能建(`case2/case3`),但**必须改标注**为编排诚实度轨,删/降原先宣称的科学 GT cp(ADMET/Kd 那些是 mock)。→ **待办:回标 case2/case3。**
- ⚠️ 编排轨价值真实但受限:药物 report 阶段字节相同 → 报告类 cp 废,只能靠前置 trace 决策判。
- 🟢 升级路径:若 NobelClaw 把 drug/protein 后端接真(见 `todo_user.md`),编排轨可升为科学轨。
- ⚠️ 真锚稀缺仍是天花板:过程 GT 主要靠 MPtrj/SEISMO + 规则/内部一致性补;题量靠 Σ 公式估,不靠连乘。

## 关联

- 上游:[[0002]] 从找 trace 到造 trace
- 探活证据:memory `nobelclaw-platform-api`(2026-06-12 块)
- 模具:`longhorizon/定义_长程任务与桩点.md`(据本 ADR 加 track/backend_class/gt_class 标签 + 规模模型)
- 示例:`longhorizon/case1`(科学轨)· `case D`(科学轨,本 ADR 后建)· `case2/3`(改标编排轨)
