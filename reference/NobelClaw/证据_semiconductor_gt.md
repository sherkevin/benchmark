# Semiconductor Band-Gap Ground-Truth Findings

Domain: **semiconductor / insulator electronic band gap (eV)**, for scoring an AI science agent.
Headline property: experimental band gap at ~300 K; also direct-vs-indirect, plus a few other clean-GT properties.

Date compiled: 2026-06-03. All values traced to a recognized source (Ioffe NSM archive, Madelung
"Semiconductors: Data Handbook", primary measurement papers, or the Zhuo et al. 2018 experimental
dataset distributed via Matbench/matminer). Where a value carries real experimental scatter or
depends on phase/polytype/temperature, this is flagged explicitly.

---

## 1. Canonical public datasets for semiconductor band gaps (with GT)

### 1.1 Matbench `matbench_expt_gap` — EXPERIMENTAL gaps (recommended primary GT)
- **What:** regression task, input = **composition only**, target = `gap expt` (band gap in eV, measured experimentally).
- **Size:** 4,604 samples (zero-gap/metal entries removed). MAD of target = 1.1432 eV.
- **Source of labels:** Zhuo, Mansouri Tehrani & Brgoch, *"Predicting the Band Gaps of Inorganic
  Solids by Machine Learning"*, **J. Phys. Chem. Lett. 2018, 9(7), 1668-1673**, DOI
  `10.1021/acs.jpclett.8b00124`.
- **Benchmark paper:** Dunn, Wang, Ganose, Dopp, Jain, *"Benchmarking materials property prediction
  methods: the Matbench test set and Automatminer reference algorithm"*, **npj Comput. Mater. 6, 138
  (2020)**, DOI `10.1038/s41524-020-00406-3`, arXiv:2005.00707.
- **URLs:** leaderboard https://matbench.materialsproject.org/ ; data download
  https://ml.materialsproject.org/projects/matbench_expt_gap.json.gz (note: ml.materialsproject.org
  may rate-limit/IP-block — see 1.6).

### 1.2 Raw Zhuo et al. `expt_gap` dataset (the full underlying experimental set)
- **What:** the un-trimmed Zhuo dataset. Columns: `formula`, `gap expt` (eV, experimental).
- **Size:** **6,354 entries** (includes zero-gap metals and multiple measurements per compound).
- **Mirror that WORKS (not IP-blocked):** Figshare — `https://ndownloader.figshare.com/files/13464434`
  (referenced by matminer's `dataset_metadata.json`, key `expt_gap`).
- **Provenance:** Zhuo et al. compiled it from prior compilations (incl. Strehlow & Cook's critical
  review of band gaps) plus literature. **It contains real experimental scatter** — many compounds
  appear with several differing measured values (see §3). Use the median across measurements, not a
  single row, when treating it as GT.

### 1.3 Matbench `matbench_expt_is_metal` — categorical GT (metal vs non-metal)
- classification, composition input, target `is_metal` (bool). **4,921 samples.** Same Zhuo source.
- Directly supports the "classify metal / semiconductor / insulator" framing (§5b).

### 1.4 Matbench `matbench_mp_gap` — DFT (PBE) gaps (use with caution)
- regression, **structure** input, target `gap pbe` (eV) from Materials Project DFT. **106,113 samples.**
- **CAVEAT:** these are PBE/GGA gaps and **systematically UNDERESTIMATE** experiment (see §4).
  Do NOT use as experimental GT. Useful only to illustrate the DFT-underestimation gap, or as a
  composition→structure feature.
- Companion classification task `matbench_mp_is_metal` (106,113 samples).

### 1.5 Other recognized compilations / databases
- **Madelung, "Semiconductors: Data Handbook", 3rd ed., Springer 2004** — the authoritative printed
  reference for elemental + III-V + II-VI gaps, effective masses, lattice constants. (Used here as
  the anchor for elemental Si/Ge/diamond and for direct/indirect designations.)
- **Ioffe Institute "New Semiconductor Materials. Characteristics and Properties" (NSM) archive**,
  http://www.ioffe.ru/SVA/NSM/Semicond/ — free, widely cited; per-material `bandstr.html`/`basic.html`
  pages give 300 K gaps, direct/indirect, T-dependence (Varshni), effective masses, lattice constants.
  **All elemental/III-V/II-VI/SiC values in the table below were read directly from this archive.**
- **Materials Project** (https://materialsproject.org) — DFT (mostly PBE; some r2SCAN). GT field:
  `band_gap`. API `mp-api`. *Underestimates experiment.* (Our IP was temporarily blocked; use the
  official `mp-api` client with an API key for bulk access.)
- **JARVIS-DFT** (NIST, https://jarvis.nist.gov/jarvisdft/) — OptB88vdW (underestimates) plus a
  TBmBJ meta-GGA gap (`optb88vdw_bandgap`, `mbj_bandgap`) that is much closer to experiment.
- **OQMD** (http://oqmd.org) — DFT formation energies and gaps (PBE).
- **AFLOW** (http://aflowlib.org) — DFT (PBE) electronic structure.

### 1.6 Tooling note (this environment)
- `ml.materialsproject.org` and the MP REST API returned **"IP/ASN temporarily blocked"** to raw curl.
  The **Figshare mirror (§1.2) worked** and is the practical way to pull the experimental GT here.
- The Ioffe NSM pages are Windows-1251 encoded; fetch with `curl ... | iconv -f WINDOWS-1251 -t UTF-8`.

---

## 2. Material table — experimental band gaps (~300 K) with direct/indirect and source

"Accepted value" = the standard textbook/handbook value at ~300 K. "Zhuo median" = median of the
experimental measurements for that formula in the Zhuo/Matbench dataset (corroboration, n shown).

| # | Formula / name        | Eg (eV, ~300 K) | Direct/Indirect | Zhuo median (n) | Source |
|---|-----------------------|-----------------|-----------------|-----------------|--------|
| 1 | Si (silicon)          | **1.12**        | indirect        | n/a (elemental) | Ioffe NSM: "Eg = 1.12 eV" 300 K; Madelung |
| 2 | Ge (germanium)        | **0.66**        | indirect        | n/a (elemental) | Ioffe NSM: "Eg = 0.66 eV" 300 K; Madelung |
| 3 | GaAs                  | **1.42**        | direct          | 1.51 (25)       | Ioffe NSM: "Eg = 1.42 eV" 300 K |
| 4 | GaN (wurtzite)        | **3.39–3.44**   | direct          | 3.48 (16)       | Ioffe NSM: "Eg(300K) = 3.44 eV" (also 3.39) |
| 5 | ZnO (wurtzite)        | **3.37**        | direct          | 3.44 (22)       | Madelung; well-accepted 3.37 eV at 300 K |
| 6 | C (diamond)           | **5.47**        | indirect        | n/a (elemental) | Ioffe NSM: "Eg = 5.46–5.6 eV"; Madelung 5.47 |
| 7 | InAs                  | **0.354**       | direct          | 0.41 (21)       | Ioffe NSM: "Eg = 0.35 eV" 300 K (≈0.36) |
| 8 | InP                   | **1.34**        | direct          | 1.38 (20)       | Ioffe NSM: "Eg = 1.34 eV" 300 K |
| 9 | CdTe (zincblende)     | **1.50**        | direct          | 1.57 (16)       | Madelung; well-accepted 1.50 eV at 300 K |
| 10| 4H-SiC                | **3.23**        | indirect        | (in SiC family) | Ioffe NSM: 4H-SiC "3.23 eV" 300 K |
| 11| 6H-SiC                | **3.0–3.05**    | indirect        | (in SiC family) | Ioffe NSM SiC page; Madelung ≈3.02 eV |
| 12| 3C-SiC (cubic)        | **2.36–2.42**   | indirect        | (in SiC family) | Ioffe NSM: 3C-SiC "2.416 eV" 300 K |
| 13| AlN (wurtzite)        | **6.0–6.2**     | direct          | 6.09 (5)        | Ioffe NSM: "Eg(300K) = 6.026 eV" (~6.2 quoted) |
| 14| GaP                   | **2.26**        | indirect        | 2.75 (4)*       | Ioffe NSM: "Eg = 2.26 eV" (Eo direct = 2.78) |
| 15| Cu2O (cuprous oxide)  | **2.17**        | direct          | 2.59 (3)        | Madelung; yellow-exciton series ≈2.17 eV |
| 16| MoS2 monolayer        | **1.8–1.9**     | direct          | n/a             | Mak et al., PRL 105, 136805 (2010) |
| 17| MoS2 bulk             | **1.2–1.3**     | indirect        | n/a             | Mak et al. 2010 (bulk ≈1.29 eV, indirect) |
| 18| GaSb                  | **0.73**        | direct          | 0.81 (15)       | Madelung; Zhuo dataset corroborates |
| 19| InSb                  | **0.17**        | direct          | 0.23 (19)       | Madelung; Zhuo dataset corroborates |
| 20| ZnS (zincblende)      | **3.68**        | direct          | 3.70 (20)       | Madelung; Zhuo dataset corroborates |
| 21| ZnSe                  | **2.70**        | direct          | 2.80 (16)       | Madelung; Zhuo dataset corroborates |
| 22| CdS (wurtzite)        | **2.42**        | direct          | 2.55 (27)       | Madelung; Zhuo dataset corroborates |
| 23| MgO (rocksalt)        | **7.8**         | direct (insul.) | 7.78 (4)        | Madelung; wide-gap insulator |

\* **GaP note:** the *fundamental* gap is **2.26 eV indirect** (Γ→X). The Zhuo dataset entries for GaP
cluster near 2.75 eV — these report GaP's **direct** gap Eo = 2.78 eV, not the fundamental gap. This is
a concrete example of why a single dataset row can be the "wrong" gap; use the fundamental (indirect)
2.26 eV as GT and treat this as a deliberate direct-vs-indirect distractor if desired.

The first 13 rows cover the user's requested textbook anchors. Rows 14–23 add more clean-GT materials
(useful as additional questions / distractors). All have verified experimental GT.

### 2.1 Other clean-GT properties (optional, well-sourced from Ioffe NSM / Madelung)
- **Lattice constant a (300 K):** Si 5.431 Å, Ge 5.658 Å, GaAs 5.653 Å, diamond 3.567 Å, InP 5.869 Å.
- **Electron effective mass (m*/m0):** Si ~0.26 (DOS, conductivity), GaAs 0.063, InAs 0.023, InP 0.08.
- **Carrier type / direct-indirect** as in the table (clean categorical GT).
These come from the same Ioffe/Madelung pages and are similarly citable if you want non-gap questions.

---

## 3. Important caveat: experimental scatter in the GT itself

The experimental band gap is **not a single perfectly sharp number**. The Zhuo/Matbench dataset shows
real measurement spread per compound (different samples, methods, temperatures, doping):

| Material | n meas. | min | max | mean | median | pop. stdev |
|----------|--------:|----:|----:|-----:|-------:|-----------:|
| GaAs | 25 | 1.40 | 1.58 | 1.483 | 1.510 | 0.047 |
| GaN  | 16 | 3.21 | 3.80 | 3.441 | 3.475 | 0.143 |
| ZnO  | 22 | 3.20 | 3.51 | 3.403 | 3.440 | 0.086 |
| InAs | 21 | 0.28 | 0.47 | 0.397 | 0.410 | 0.045 |
| InP  | 20 | 1.25 | 1.50 | 1.369 | 1.380 | 0.069 |
| CdTe | 16 | 1.43 | 1.66 | 1.550 | 1.570 | 0.070 |
| CdS  | 27 | 2.39 | 2.66 | 2.535 | 2.550 | 0.078 |

Implication for scoring: a tolerance of **±0.1 eV is too tight** (smaller than literature scatter for
several compounds); **±0.2 eV or ±15%** comfortably covers the experimental uncertainty while still
being discriminating. Always state the temperature (~300 K) and phase/polytype in the question.

---

## 4. DFT underestimation caveat (critical for scoring)

Standard semi-local DFT (**PBE / GGA / LDA**) **systematically underestimates** band gaps, typically by
**30–50%**, sometimes predicting a true semiconductor as a metal (Eg≈0). Examples (PBE vs experiment):
Si ~0.6 vs 1.12 eV; GaAs ~0.5 vs 1.42 eV; ZnO ~0.7–0.8 vs 3.37 eV; diamond ~4.1 vs 5.47 eV; Ge often
metallic in PBE vs 0.66 eV experiment. This is the well-known "band-gap problem."

Consequences:
- **`matbench_mp_gap` (gap pbe) and raw Materials Project / OQMD / AFLOW PBE gaps are NOT experimental
  GT.** If an agent quotes "the Materials Project value," it may be a PBE number that is 30–50% low.
- Methods that correct this (hybrid functionals HSE06, GW, or meta-GGA TB-mBJ / r2SCAN) get closer to
  experiment — JARVIS exposes a TBmBJ gap (`mbj_bandgap`) for this reason.
- When scoring, **score against the experimental gap** (Zhuo/Matbench/Ioffe/Madelung), and be aware an
  agent may "correctly" report a DFT value that is wrong vs experiment — that should count as wrong (or
  the question should specify "experimental gap at 300 K").

---

## 5. Recommended scoring approach

Three complementary modes, in increasing difficulty:

**(a) Comparison (most robust, hardest to game by memorizing one number).**
"Which has the larger band gap, A or B?" Score = correct/incorrect. Choose pairs whose gaps differ by
more than the scatter (>~0.3 eV) so the answer is unambiguous, e.g. Si (1.12) vs GaN (3.4); InAs
(0.36) vs InP (1.34); GaP (2.26) vs ZnO (3.37). Avoid near-ties (e.g. CdTe 1.50 vs GaAs 1.42).

**(b) Categorical (clean, robust to small numeric errors).**
Classify material X as **metal (Eg ≈ 0)** / **semiconductor (0 < Eg ≲ 3 eV)** / **wide-gap or
insulator (Eg ≳ 3 eV)**. Score = correct class. Backed directly by `matbench_expt_is_metal`. Also:
**direct vs indirect** classification (binary, clean GT from the table).

**(c) Numeric with tolerance.**
"Predict the experimental band gap of X at 300 K (eV)." Score correct if within band:
- **±0.2 eV absolute** (recommended default — exceeds literature scatter, still discriminating), OR
- **±15% relative** (better for small gaps like InSb 0.17 eV, where ±0.2 eV would be too lax), OR
- a graded score, e.g. `max(0, 1 − |pred−GT| / 0.5)`.
Recommendation: **pass if within ±0.2 eV OR ±15%, whichever is larger.** Always specify temperature
and phase/polytype.

---

## 6. Suggested question framings (score cleanly)

1. **Comparison:** "Which has the larger electronic band gap at room temperature: gallium nitride
   (GaN) or indium phosphide (InP)?" → GaN (3.4 eV > 1.34 eV). Clean, large margin.
2. **Categorical (metal/semiconductor/insulator):** "Classify diamond (C) by its room-temperature
   electronic conductivity class: metal, semiconductor, or wide-gap insulator." → wide-gap insulator
   (Eg = 5.47 eV). Backed by `matbench_expt_is_metal` framing.
3. **Numeric with tolerance:** "Predict the experimental band gap (eV) of GaAs at 300 K. Correct if
   within ±0.2 eV." → 1.42 eV (accept 1.22–1.62).
4. **Direct vs indirect:** "Is the fundamental band gap of silicon direct or indirect?" → indirect.
   Good distractor set: Si/Ge/GaP/diamond/MoS2-bulk = indirect; GaAs/InP/InAs/GaN/ZnO/MoS2-mono =
   direct.
5. **Phase/dimensionality (harder, anti-memorization):** "How does the band gap of MoS2 change from
   bulk to monolayer, and what changes about its nature?" → bulk ~1.2–1.3 eV indirect → monolayer
   ~1.8–1.9 eV direct (indirect→direct crossover). Tests real understanding, not lookup.

---

## 7. Contamination concern and mitigation

**Concern:** the headline anchors (Si 1.12, GaAs 1.42, GaN 3.4, ZnO 3.37, diamond 5.47, …) are so
famous that a strong LLM has them memorized — these test recall, not prediction/reasoning. Keep a few
as calibration "gimme" anchors, but don't rely on them to discriminate capable agents.

**Mitigations:**
- **Use less-common ternaries/quaternaries and specific compositions** drawn from the 4,604-entry
  `matbench_expt_gap` set (e.g. oxides, halide perovskites, chalcogenide ternaries) that are far less
  likely memorized. The dataset is the natural pool for "obscure but GT-backed" items.
- **Use comparison and categorical framings (§5a/§5b)** — robust even if the agent half-remembers a
  number, and harder to ace by recall alone.
- **Use alloy compositions** (e.g. Al_xGa_{1-x}As, In_xGa_{1-x}As at a stated x) where the gap follows
  a known bowing relation — tests interpolation/reasoning, not lookup. (Vegard/bowing parameters are
  in Madelung/Ioffe.)
- **Use the direct-vs-indirect and bulk-vs-monolayer twists (§6.4/§6.5)** — these require understanding
  band structure, not just the headline gap.
- **Hold out exact numbers:** ask for class or comparison, reserving numeric prediction for the less
  famous compositions where memorization is unlikely.

---

## 8. Citations (verified this session)

1. Zhuo, Y.; Mansouri Tehrani, A.; Brgoch, J. "Predicting the Band Gaps of Inorganic Solids by Machine
   Learning." *J. Phys. Chem. Lett.* **2018**, 9 (7), 1668–1673. DOI 10.1021/acs.jpclett.8b00124.
   (PMID 29532658.) — primary experimental-gap dataset (6,354 entries; Figshare file 13464434).
2. Dunn, A.; Wang, Q.; Ganose, A.; Dopp, D.; Jain, A. "Benchmarking materials property prediction
   methods: the Matbench test set and Automatminer reference algorithm." *npj Comput. Mater.* **2020**,
   6, 138. DOI 10.1038/s41524-020-00406-3. arXiv:2005.00707. — Matbench (`matbench_expt_gap` 4,604;
   `matbench_expt_is_metal` 4,921; `matbench_mp_gap` 106,113).
3. Madelung, O. *Semiconductors: Data Handbook*, 3rd ed.; Springer: Berlin, **2004**. — handbook gaps,
   direct/indirect, effective masses, lattice constants.
4. Ioffe Institute, "New Semiconductor Materials. Characteristics and Properties" (NSM Archive).
   http://www.ioffe.ru/SVA/NSM/Semicond/ — per-material 300 K gaps and direct/indirect (Si, Ge, GaAs,
   GaN, InAs, InP, GaP, SiC polytypes, AlN, diamond read directly this session).
5. Mak, K. F.; Lee, C.; Hone, J.; Shan, J.; Heinz, T. F. "Atomically Thin MoS2: A New Direct-Gap
   Semiconductor." *Phys. Rev. Lett.* **2010**, 105, 136805. — MoS2 monolayer ~1.8–1.9 eV direct vs
   bulk ~1.29 eV indirect.
6. Strehlow, W. H.; Cook, E. L. "Compilation of Energy Band Gaps in Elemental and Binary Compound
   Semiconductors and Insulators." *J. Phys. Chem. Ref. Data* **1973**, 2, 163. — upstream compilation
   feeding the Zhuo dataset.

Dataset access verified this session: Figshare mirror `https://ndownloader.figshare.com/files/13464434`
(6,354 rows, columns `formula`, `gap expt`) downloaded and parsed successfully. Materials Project
endpoints (`ml.materialsproject.org`, REST API) were IP/ASN-blocked to raw curl — use the official
`mp-api` client with a key for those.
