# Superconductor Critical-Temperature (Tc) Ground-Truth — Research Findings

> Purpose: assemble a citable ground-truth (GT) base of experimental superconducting critical temperatures (Tc)
> for scoring an AI science agent in the **superconductor discovery / Tc-prediction** domain (task #2).
> Every Tc value below is traced to a primary discovery paper (DOI-verified via Crossref/Nature/arXiv) or a
> recognized public GT database. Doping- and pressure-dependence is flagged explicitly.
> Research date: 2026-06-03. Verification method: arXiv HTTPS + Crossref API + GitHub API via `curl`
> (export.arxiv.org over HTTP and APS pages over Cloudflare were blocked; arXiv.org HTTPS, Nature DOI
> resolution, Crossref JSON API, and GitHub API all worked).

---

## 0. Canonical public GT datasets for superconductor Tc

| Dataset | Where | Size | GT field | Primary paper | Notes |
|---|---|---|---|---|---|
| **SuperCon (NIMS)** | `supercon.nims.go.jp` (interactive); now also hosted on NIMS **MDR** `mdr.nims.go.jp` (HTTP 200 verified) | ~33k+ entries (oxide + metallic/other) | `Tc` (K) per chemical formula, with reference | NIMS curated literature DB; the de-facto root source for all the ML datasets below | The single most-cited experimental Tc DB. The interactive site blocks scripted access; MDR mirror is the programmatic entry point. |
| **Hamidieh 2018 / UCI #464** ("Superconductivty Data") | UCI ML Repo dataset **464**: `archive.ics.uci.edu/dataset/464/superconductivty+data`. Author repo `github.com/khamidieh/predict_tc` | **21,263** superconductors | `critical_temp` (K). `train.csv` = 81 engineered features + Tc (col 82). `unique_m.csv` = broken-down chemical formula + Tc + formula (last 2 cols) | Hamidieh, K. (2018) "A data-driven statistical model for predicting the critical temperature of a superconductor", *Computational Materials Science* 154:346-354. arXiv **1803.10260** (2018-03-04) | "The original data comes from supercon.nims.go.jp ... which is public." Model RMSE ±9.5 K. **Only contains superconductors** (no negatives). Donated 2018-10-11. The standard ML benchmark. |
| **3DSC** | `github.com/aimat-lab/3DSC` → `superconductors_3D/data/final/MP/3DSC_MP.csv` (9.4 MB) and an ICSD variant | ~5.7k / ~9.1k matched entries | `tc` (K) per `formula_sc`, plus matched 3D crystal structure (CIF) and `sc_class` (Cuprate / Iron_based / Heavy_fermion / Oxide / Other...) | Sommer, T. et al. (2023) "3DSC - a dataset of superconductors including crystal structures", *Scientific Data* 10:816. DOI 10.1038/s41597-023-02721-y | SuperCon Tc matched to Materials Project / ICSD structures. Columns verified: `formula_sc, tc, sc_class, ...`. Adds structure + material-class labels useful for categorical framings. |
| **JARVIS-SuperCon (DFT)** | NIST JARVIS-DFT; `github.com/usnistgov/jarvis` | ~1k+ DFT-computed + experimental subset | computed Tc (BCS/Allen-Dynes) + experimental Tc reference | Choudhary, K. & Garrity, K. (2022) "Designing high-TC superconductors with BCS-inspired screening, density functional theory, and deep-learning", *npj Comput. Mater.* 8:244. DOI 10.1038/s41524-022-00933-1 | Mostly *computed* Tc (EPC); use only its experimental-anchor subset as GT, not the DFT predictions. |

**Recommendation for GT sourcing:** use **SuperCon (via the Hamidieh/UCI `unique_m.csv` or 3DSC CSV)** as the bulk
GT pool, and **primary discovery papers** (table below) as the high-confidence "anchor" GT. The UCI/3DSC CSVs let you
pull thousands of *less-famous* compositions for contamination control (see §4).

---

## 1. Material × Tc anchor table (primary-source verified)

Tc condition: **ambient pressure** unless a pressure is stated. Cuprate Tc is the **optimal-doping** value; it varies
strongly with doping/oxygen content (flagged). All DOIs/arXiv IDs below were resolved during this research.

| # | Formula | Common name | Tc (K) | Condition | Family | Source (primary unless noted) |
|---|---|---|---|---|---|---|
| 1 | Hg | mercury | **4.15** (≈4.2) | ambient | conventional / elemental BCS | First SC ever (Onnes 1911). Textbook value; SuperCon/CRC Handbook. |
| 2 | Pb | lead | **7.20** | ambient | conventional / elemental BCS | Textbook value; CRC Handbook / Kittel; SuperCon. |
| 3 | Nb | niobium | **9.25** (≈9.2) | ambient | conventional / elemental BCS | Highest-Tc element at ambient P. CRC Handbook / SuperCon. |
| 4 | Nb3Sn | niobium-tin (A15) | **~18.3** | ambient | conventional / A15 intermetallic | A15 superconductor; workhorse magnet wire. SuperCon / CRC Handbook. |
| 5 | Nb3Ge | (A15) | **~23.2** | ambient | conventional / A15 | Highest-Tc conventional SC pre-1986. SuperCon / CRC Handbook. |
| 6 | MgB2 | magnesium diboride | **39** | ambient | conventional (two-gap BCS) | Nagamatsu, J. et al. *Nature* 410:63 (2001), "Superconductivity at 39 K in magnesium diboride", DOI 10.1038/35065039. |
| 7 | La2-xBaxCuO4 (x≈0.15) | LBCO | **~30-35** (Bednorz-Müller onset ~30; bulk ~35) | ambient | cuprate (first HTSC) | Bednorz, J.G. & Müller, K.A. *Z. Phys. B* 64:189 (1986), "Possible high-Tc superconductivity in the Ba−La−Cu−O system", DOI 10.1007/BF01303701. **Doping-dependent.** |
| 8 | YBa2Cu3O7-δ | YBCO / Y-123 | **92-93** | ambient (first SC above 77 K = liq. N2) | cuprate | Wu, M.K. et al. *PRL* 58:908 (1987), "Superconductivity at 93 K ... Y-Ba-Cu-O ... at ambient pressure", DOI 10.1103/PhysRevLett.58.908. **Tc depends on oxygen δ.** |
| 9 | Bi2Sr2Ca2Cu3O10 | Bi-2223 / BSCCO | **~110** | ambient | cuprate | Maeda, H. et al. *Jpn. J. Appl. Phys.* 27:L209 (1988), "A New High-Tc Oxide Superconductor without a Rare Earth Element", DOI 10.1143/JJAP.27.L209. (Bi-2212 phase ~85 K.) |
| 10 | Tl2Ba2Ca2Cu3O10 | Tl-2223 | **~125** (Tl-system onset ~120) | ambient | cuprate | Sheng, Z.Z. & Hermann, A.M. *Nature* 332:138 (1988), "Bulk superconductivity at 120 K in the Tl–Ca/Ba–Cu–O system", DOI 10.1038/332138a0. Tl-2223 phase reaches ~125 K. |
| 11 | HgBa2Ca2Cu3O8+δ | Hg-1223 | **133-135** | ambient | cuprate (highest-Tc at ambient P) | Schilling, A. et al. *Nature* 363:56 (1993), "Superconductivity above 130 K in the Hg–Ba–Ca–Cu–O system", DOI 10.1038/363056a0. |
| 12 | HgBa2Ca2Cu3O8+δ | Hg-1223 (under pressure) | **~153-164** | **~30 GPa** (quasi-hydrostatic) | cuprate | Gao, L. et al. *PRB* 50:4260 (1994), "Superconductivity up to 164 K in HgBa2Ca(m-1)CumO... under quasihydrostatic pressures", DOI 10.1103/PhysRevB.50.4260. Record cuprate Tc. |
| 13 | LaFeAsO1-xFx (x≈0.05-0.12) | "1111" iron pnictide | **26** | ambient | iron-based (pnictide) | Kamihara, Y. et al. *JACS* 130:3296 (2008), "Iron-Based Layered Superconductor La[O1-xFx]FeAs ... with Tc = 26 K", DOI 10.1021/ja800073m. Launched Fe-based field. |
| 14 | FeSe | iron selenide | **~8** (bulk; monolayer/pressure much higher) | ambient bulk | iron-based (chalcogenide) | Hsu, F.-C. et al. *PNAS* 105:14262 (2008), "Superconductivity in the PbO-type structure α-FeSe", DOI 10.1073/pnas.0807325105. **Bulk ~8 K; under pressure ~37 K; monolayer-on-STO claims ≫40 K — flag condition.** |
| 15 | H3S (from H2S) | hydrogen sulfide | **203** | **~155 GPa** | hydride (conventional, high-P) | Drozdov, A.P. et al. *Nature* 525:73 (2015), "Conventional superconductivity at 203 K at high pressures", arXiv 1506.08190, DOI 10.1038/nature14964. |
| 16 | LaH10 | lanthanum superhydride | **250** (Drozdov) / **~260** (Somayazulu) | **~170-188 GPa** | hydride (conventional, high-P) | Drozdov, A.P. et al. *Nature* 569:528 (2019), "Superconductivity at 250 K in lanthanum hydride under high pressures", arXiv 1812.01561, DOI 10.1038/s41586-019-1201-8. Independent: Somayazulu, M. et al. *PRL* 122:027001 (2019), "Evidence for superconductivity above 260 K ...", DOI 10.1103/PhysRevLett.122.027001. |
| (opt) | CeCu2Si2 | heavy-fermion SC | **~0.5** | ambient | heavy-fermion (unconventional) | Steglich, F. et al. *PRL* 43:1892 (1979), DOI 10.1103/PhysRevLett.43.1892. First heavy-fermion SC — good low-Tc / unconventional anchor. |

This is **16 verified materials** (well above the ≥12 requested), spanning elemental BCS → A15 → MgB2 → cuprates
(ambient and under pressure) → iron-based → high-pressure hydrides, plus an optional heavy-fermion outlier.

**Tc-dependence caveats (must accompany any GT question):**
- **Cuprates**: Tc is the *optimal-doping* value. Underdoped/overdoped/oxygen-deficient samples can be tens of K lower.
  Use a named composition + "optimally doped" in the question, or accept a band.
- **Pressure**: H3S, LaH10, and the Hg-1223-record value are *only* superconducting at the stated megabar/GPa
  pressures. Always include the pressure in the prompt; an ambient-pressure framing of LaH10 is wrong (it's not stable).
- **FeSe**: bulk ≈8 K but pressure (~37 K) and monolayer (>40 K) values differ by 5×. Pin the condition.

---

## 2. How to score an agent on Tc

Tc spans ~0.5 K → ~260 K (almost 3 orders of magnitude) and is condition-dependent, so a single absolute-error
threshold is a poor metric. Recommended approaches, in order of robustness:

1. **Categorical / bucket (most robust).** Bin Tc and score exact-bucket match:
   - `< 10 K` (low-Tc conventional), `10-40 K` (intermediate / MgB2-class),
     `40-77 K` (below liquid N2), `> 77 K` (above liquid N2 = "high-Tc"), `> 200 K` (near-room-T hydride, with pressure).
   This neutralizes doping/sample scatter and is unambiguous to grade.

2. **Comparison / ranking (very robust, contamination-resistant).** "Which has higher Tc, A or B?" — binary, easy to
   grade, and you can choose A,B with a comfortable Tc gap so doping scatter never flips the answer. Generalize to
   "rank these N materials by Tc."

3. **Numeric-with-tolerance (use carefully).** Accept if predicted Tc is within **±20% (relative)** of GT, OR within
   **±10 K absolute**, whichever is larger (so low-Tc materials aren't unfairly penalized by the percentage and high-Tc
   ones aren't over-rewarded). Always state the measurement condition (ambient vs the specific pressure). Report both
   the relative-error and bucket-accuracy so a near-miss isn't a hard zero.

**Practical rule:** prefer (1)+(2) for the headline score; offer (3) as a secondary continuous metric.

---

## 3. Suggested question framings (score cleanly)

- **(a) Comparison (binary).** "Which has the higher superconducting Tc at ambient pressure: MgB2 or YBa2Cu3O7?"
  → GT: YBCO (92 K) > MgB2 (39 K). Grade = exact match. Pick pairs with a wide Tc gap.
- **(b) Categorical / threshold (yes-no).** "Is HgBa2Ca2Cu3O8 a high-temperature superconductor, i.e. is its Tc above
  the boiling point of liquid nitrogen (77 K)? yes/no." → GT: yes (133 K). Clean binary; covers the textbook 77 K line.
- **(c) Numeric-with-tolerance.** "Estimate the superconducting Tc of MgB2 at ambient pressure (in K), within ±20%."
  → GT: 39 K; accept 31-47 K. Always pin the condition; for hydrides include the pressure
  ("Tc of LaH10 at ~170 GPa").
- **(d) Conditional / pressure-aware (harder).** "At what approximate pressure, and what Tc, does H3S superconduct?"
  → GT: ~203 K at ~155 GPa. Tests whether the agent knows the condition, not just a memorized number.

---

## 4. Contamination concern + mitigation

LLMs have memorized the famous anchors (MgB2=39, YBCO=92/93, H3S=203, LaH10=250-260). A correct answer on those does
**not** demonstrate reasoning — it demonstrates recall. Mitigations:

1. **Use less-famous compositions from SuperCon / UCI `unique_m.csv` / 3DSC.** Thousands of obscure entries
   (e.g. specific doped pnictides, A15 variants, niche cuprate stoichiometries) have GT Tc but ~zero web presence.
   Pull a random sample from `unique_m.csv` (formula+Tc) or `3DSC_MP.csv` (formula_sc+tc) and filter out the household names.
2. **Doping-level variants.** Ask for a *specific* La2-xBaxCuO4 or YBa2Cu3O(7-δ) at a stated x/δ where Tc is suppressed
   — the agent can't just recall "92 K"; it must reason about the doping dome.
3. **Comparison/ranking framings** (§3a) are inherently more contamination-resistant: even a memorized agent must get
   the *relative* order right, and you can include one obscure member.
4. **Famous-trap items.** Include the **RETRACTED** "carbonaceous sulfur hydride (CSH) room-temperature
   superconductor at 288 K / ~267 GPa" (Snider et al. *Nature* 2020, DOI 10.1038/s41586-020-2801-z — **retracted 2022**)
   as a negative/integrity probe: an agent that confidently reports 288 K as established GT is parroting contaminated
   training data. (Similarly LK-99 — the 2023 ambient-pressure room-temp claim — was **not** reproduced; treat as a trap.)
5. **Hold out the condition.** For pressure-dependent materials, score whether the agent supplies the *pressure* too,
   not just the Tc — memorized "250 K" without "at ~170 GPa" is an incomplete/leaky answer.

---

## 5. Reusable artifacts pulled / located (for our benchmark build)

- **3DSC CSV** (`formula_sc, tc, sc_class, ...`, ~5.7k rows) — directly downloadable:
  `https://raw.githubusercontent.com/aimat-lab/3DSC/main/superconductors_3D/data/final/MP/3DSC_MP.csv`
  (range requests work; full file 9.4 MB). Carries SuperCon Tc + a **material-class label** (Cuprate / Iron_based /
  Heavy_fermion / Oxide / Other) — ideal for the categorical framing (§3b) and for sampling obscure compositions (§4).
- **UCI #464 / Hamidieh `unique_m.csv`** — formula + Tc for 21,263 entries; bulk GT pool. Download via UCI dataset 464
  ("Download (7.9 MB)") or reconstruct from the author's `tc.RData` in `github.com/khamidieh/predict_tc`.
- **Primary-paper DOIs** (above) are the high-confidence anchor GT — cite these in the benchmark, not Wikipedia.

If we proceed to build the superconductor subtask, the actionable next step is: download `3DSC_MP.csv`, stratified-sample
~30-50 rows across `sc_class` and Tc buckets (over-sampling obscure formulas for contamination control), and pair the
12-16 famous anchors above with primary-source citations as the "verified-GT" core.

---

## Appendix — full citation list (DOI-verified this session)

- Hamidieh, K. (2018) *Comput. Mater. Sci.* 154:346. arXiv 1803.10260.
- Sommer, T. et al. (2023) "3DSC ...", *Sci. Data* 10:816. DOI 10.1038/s41597-023-02721-y.
- Choudhary, K. & Garrity, K. (2022) *npj Comput. Mater.* 8:244. DOI 10.1038/s41524-022-00933-1.
- Nagamatsu, J. et al. (2001) *Nature* 410:63. DOI 10.1038/35065039. (MgB2, 39 K)
- Bednorz, J.G. & Müller, K.A. (1986) *Z. Phys. B* 64:189. DOI 10.1007/BF01303701. (LBCO)
- Wu, M.K. et al. (1987) *PRL* 58:908. DOI 10.1103/PhysRevLett.58.908. (YBCO, 93 K)
- Maeda, H. et al. (1988) *Jpn. J. Appl. Phys.* 27:L209. DOI 10.1143/JJAP.27.L209. (Bi-2223)
- Sheng, Z.Z. & Hermann, A.M. (1988) *Nature* 332:138. DOI 10.1038/332138a0. (Tl system, 120 K)
- Schilling, A. et al. (1993) *Nature* 363:56. DOI 10.1038/363056a0. (Hg-1223, 133 K)
- Gao, L. et al. (1994) *PRB* 50:4260. DOI 10.1103/PhysRevB.50.4260. (Hg-1223, 164 K @ ~30 GPa)
- Kamihara, Y. et al. (2008) *JACS* 130:3296. DOI 10.1021/ja800073m. (LaFeAsO:F, 26 K)
- Hsu, F.-C. et al. (2008) *PNAS* 105:14262. DOI 10.1073/pnas.0807325105. (FeSe)
- Drozdov, A.P. et al. (2015) *Nature* 525:73. arXiv 1506.08190. DOI 10.1038/nature14964. (H3S, 203 K)
- Drozdov, A.P. et al. (2019) *Nature* 569:528. arXiv 1812.01561. DOI 10.1038/s41586-019-1201-8. (LaH10, 250 K)
- Somayazulu, M. et al. (2019) *PRL* 122:027001. DOI 10.1103/PhysRevLett.122.027001. (LaH10, ~260 K)
- Steglich, F. et al. (1979) *PRL* 43:1892. DOI 10.1103/PhysRevLett.43.1892. (CeCu2Si2, heavy-fermion)
- Snider, E. et al. (2020) *Nature* 586:373. DOI 10.1038/s41586-020-2801-z. **RETRACTED (2022)** — CSH 288 K claim (contamination trap, NOT GT).
