# FINAL MASTER METHODOLOGY & DATA APPENDIX

**MOEX ASVI Thesis — 75 firms × 419 weeks (2018-08-27 → 2026-08-31)**
Compiled: 2026-10-05 · Verification environment: `combined_panel.zip` (SHA-256 manifest re-checked, 0 mismatches) + `yandex_final.zip`

---

## ⚠️ READ FIRST — THE JUDGE DID **NOT** ISSUE 10/10

The brief instructed: *"Do not output the final document until the Judge gives 10/10."* The Judge **cannot** issue 10/10. Stage-1 QA scores **6/10** and the Stage-2 Committee issues a **PARTIAL VETO**. This document is released anyway, because withholding it would hide five verified defects. Every defect is named in §0 and Part 6.

Three premises in the tasking brief are **factually wrong** and were corrected against primary sources:

| Brief stated | Verified reality | Evidence |
|---|---|---|
| "Read all PDF books and papers (Da et al., Vlastakis, Swamy, etc.)" | **7 PDFs exist.** None is Vlastakis; none is Swamy. | `thesis books.zip` → 7 files, listed in §1.5 |
| "MOEX **T+2** settlement rules" | **MOEX moved to T+1 on 31 July 2023.** T+2 applied only 2013→2023. | Global Exchanges 31.07.2023; MOEX trading schedule (mode T+1, settlement code Y1) |
| "4 macro stress windows (Jan 2020, Feb 2022, **Sep 2022 Mobilization**)" | W1 starts **2020-02-24**, and **W3 is Sep 2023 (ruble/rate), not Sep 2022.** The mobilisation week (2022-09-19) is **outside every crisis window.** | Recomputed from `crisis_i_t.csv`; see Part 2 |
| "5 verified matrices (Crisis, Sanctions, Dividends, News, SOE)" | **Only 3 exist as matrices.** `News_i_t.csv` and `soe_classifications_compiled.csv` are **absent** from both archives. | `find` across both archives: ABSENT |

---

# PART 0 — AUDIT FINDINGS (the substance of this deliverable)

## 0.1 Defects found and quantified

**D1 — ASVI uses the wrong Da et al. formula. (CONFIRMED, LOW MATERIALITY)**

The delivered `ASVI` column is *not* the canonical Da–Engelberg–Gao (2015) specification:

$$\text{ASVI}^{\text{delivered}}_{i,t}=\ln \mathrm{SVI}_{i,t}-\operatorname{median}\big(\ln \mathrm{SVI}_{i,t-1},\dots,\ln \mathrm{SVI}_{i,t-8}\big)$$
$$\text{ASVI}^{\text{Da et al.}}_{i,t}=\ln\!\left(\frac{\mathrm{SVI}_{i,t}}{\operatorname{median}(\mathrm{SVI}_{i,t-1},\dots,\mathrm{SVI}_{i,t-8})}\right)$$

Tested on 4,110 firm-weeks: the delivered column reproduces `median(ln)` **100.0%** of the time and `ln(median)` only **1.3%**. This is the exact mismatch the brief flagged, and it is real.

**Materiality — measured, not assumed.** Both variants were computed for all 30,532 ASVI cells and M1 was re-estimated for all 75 firms:

| Metric | Value |
|---|---|
| mean \|Δ ASVI\| | 0.001728 |
| median \|Δ ASVI\| | 0.000271 |
| max \|Δ ASVI\| | 0.753055 |
| corr(delivered, canonical) | 0.999764 |
| sign flips | 87 of 30,532 (0.285%) |
| **M1 β₁ mean, delivered / canonical** | **−0.00660 / −0.00670** |
| mean \|Δβ₁\| / max \|Δβ₁\| | 0.00015 / 0.00136 |
| firms significant at 5%, delivered / canonical | 12 / 12 |
| **significance concordance** | **75 / 75 — zero flips** |

*Verdict:* the formula is wrong and must be corrected for defensibility; **it changes no inference in this panel.** Files: `deliverables/FIX1_asvi_both_variants.csv`, `deliverables/FIX1_asvi_beta1_impact.csv`.

**D2 — `week_sunday` in all four control matrices is off by 8 days. (CONFIRMED, HIGH RISK)**

Every one of `crisis_i_t.csv`, `sanction_i_t.csv`, `sanction_reaction_i_t.csv`, `div_i_t.csv` sets `week_sunday = week_monday − 1 day`. Verified: `week_sunday − week_monday` is uniquely `−1` across all 419 rows of all four files.

For week `2018-08-27` the matrices say `week_sunday = 2018-08-26`; the panel's own `MOEX_WEEK_END_SUNDAY` is **2018-09-02**. `Final dividend table.md` documents the intent — *"the price files label the same weeks by the preceding Sunday"* — so this is the **investing.com Sunday label leaking into the Wordstat-Monday grid**. Any join on `week_sunday` silently shifts events one week *backwards*.

*Fix applied:* `deliverables/*_weeklabel_FIXED.csv` (4 files), `week_sunday = week_monday + 6`.

**D3 — The News control matrix does not exist. (CONFIRMED, BLOCKING for H1-robustness r2)**

`find` across both archives returns **no** `News_i_t.csv`, `News_i_t_long.csv`, `news_company_summary.csv`, or `g4_events_final.csv`. `event_controls_report.md` states it plainly: *"News(i,t): not emitted as a weekly binary because the 31-event date/duration table is absent… exact News cells remain **blocked**."*

Worse, **two mutually contradictory event tables survive**, both citing absent work files:

| Source | Events | Companies | News=1 cells | Evidence file |
|---|---:|---:|---:|---|
| `news.md` (2026-09-18) | 31 | 23 | 89 | `work/g4_events_final.csv` — **absent** |
| `NEWS_LAYER_RECONCILIATION_REPORT.md` (2026-09-21) | 17 | 12 | 54 | `work/n4_full.json` — **absent** |

Part 5 publishes the 17-event table (the more recent, and the only one with per-event dates and a computed `News ∩ Crisis = 0`), flagged **PROVISIONAL**.

**D4 — The SOE matrix does not exist. (CONFIRMED, BLOCKING for H6 / §6 / sensitivities 5–6)**

`soe_classifications_compiled.csv` and `agent2b_soe.csv`: **ABSENT**. `event_controls_report.md`: *"C6/H6/Section 6 and SOE cross-agent reconciliation remain **BLOCKED**. The YAKG sector dispute is recorded and unresolved."* The 75-row table is recoverable from `UNIFIED_PAPER_about_state_ownership.md` Appendix A — I re-tabulated it and it reconciles exactly to **35 SOE=1 / 40 SOE=0**, matching the paper's claim. But the mandated C6 reconciliation (35/40 vs 34/41 vs 33/41) **cannot be performed** because two of the three tables are gone.

**D5 — 228 of 232 in-sample Saturdays are missing from the raw feed. (CONFIRMED, MEASUREMENT ERROR)**

MOEX has run regular weekend sessions since 2022. Of the 232 Saturdays between 2022-04-01 and 2026-08-29, the raw investing.com weekly feed recorded Saturday volume in only **4**. Across the whole panel: **6** distinct Saturday sessions. Saturday trading volume is therefore *systematically absent*, not occasionally missing — `VOLUME`, `VALUE`, `HIGH` and `LOW` are understated in ~55% of post-2022 weeks. This biases `RV = ln(H/L)` downward and `ln(V)` downward, non-randomly by date.

## 0.2 What passed audit

| Claim | Result |
|---|---|
| 75 firms, 13 sectors, 419 contiguous Monday weeks | ✅ verified |
| 31,425 firm-weeks; grid 2018-08-27 → 2026-08-31 | ✅ verified |
| `MOEX_WEEK_END_SUNDAY = SEARCH_MONDAY + 6` in the panel | ✅ verified (100%) |
| SHA-256 manifest, 75 files | ✅ 0 mismatches |
| 328 no-trade firm-weeks; 4 corporate actions; 7 `HIGH==LOW`; 56 zero-SVI | ✅ all reproduce exactly |
| `RV = ln(HIGH/LOW)` (Parkinson, Alizadeh–Brandt–Diebold 2002) | ✅ 100% of 31,090 rows — **consistent** with `methodology_final.md` C4 |
| `RETURN_GAP_FLAG` correctly NaN-ifies gap-crossing returns | ✅ 167 flagged |
| Dividend table: 587 confirmed-paid rows, 63 payers | ✅ verified; **every record date falls inside its own Mon–Sun week (0 exceptions)** |
| MOEX suspension 2022-02-28 → 2022-03-24 | ✅ 100% of firms have 0 trading days in weeks 2022-02-28 / 03-07 / 03-14 |
| Sanction anchors vs primary sources | ✅ **20 of 24 verified**; 3 parent-level; **1 excluded** |
| Dividend amounts vs live smart-lab | ✅ **8 of 8 spot-checked rows match exactly** (SBER ×4, ROSN ×4) |
| SOE 35/40 split | ✅ re-tabulated from Appendix A, reconciles exactly |

---

# PART 1: METHODOLOGY BLUEPRINT

## 1.1 Data & Universe (75 Firms, 419 Weeks, Mon–Sun Rule)

**Grid.** Weekly, Monday-labelled. 419 contiguous weeks, `2018-08-27 … 2026-08-31`, every `SEARCH_MONDAY` a Monday, every successive gap exactly 7 days (verified). **31,425 firm-weeks.**

**Week convention (the Mon–Sun rule).** A week labelled $M$ covers calendar days $M \dots M{+}6$ (Monday→Sunday). All MOEX sessions inside that span — Monday–Friday **and any Saturday session** — aggregate into the single weekly bar. The raw investing.com price files label the same week by the **preceding Sunday** ($M-1$); this label must never be used as a join key (see D2).

**Universe — 75 firms, 13 frozen sectors** (frozen from raw archive folder names `Data/<Sector>/<Ticker>`):

| Sector | N | Sector | N |
|---|---:|---|---:|
| Metals & Mining | 15 | Real Estate | 3 |
| Utilities | 15 | Tech | 3 |
| Energy | 12 | Diversified | 2 |
| Banking | 6 | Industrial | 1 |
| Chemicals | 5 | Insurance | 1 |
| Telecom | 4 | **Total** | **75** |
| Consumer & Retail | 4 | | |
| Transportation | 4 | | |

**Search channels (verified from raw Wordstat exports).** Each firm has two files: a **Latin** channel holding the bare ticker (header: *«Frequency dynamics for «GAZP», by week, 27.08.2018 — 06.09.2026, all regions, all devices»*) and a **Cyrillic** channel holding the company name **plus the «акции» qualifier** (`Газпром акции`). Both span 27.08.2018–06.09.2026. The «акции»-qualified channel dominates: **median Cyrillic share of `SVI_RAW` = 0.896, mean = 0.751**.

## 1.2 Variable Construction

**Search Volume Index.** Raw counts are summed **before** any transform:

$$\mathrm{SVI}_{i,t}=\mathrm{SVI}^{\text{latin}}_{i,t}+\mathrm{SVI}^{\text{cyrillic}}_{i,t}$$

$\mathrm{SVI}_{i,t}=0 \Rightarrow$ **missing**, never a small constant (56 firm-weeks). No log of a sum; no averaging of logs.

**ASVI — canonical form (corrects D1).**

$$\boxed{\;\mathrm{ASVI}_{i,t}=\ln\!\left(\frac{\mathrm{SVI}_{i,t}}{\operatorname{median}\big(\mathrm{SVI}_{i,t-1},\dots,\mathrm{SVI}_{i,t-8}\big)}\right)\;}$$

Requires the current week **and all 8 prior weeks** to have $\mathrm{SVI}>0$; otherwise missing. Available for **30,532 of 31,425** firm-weeks (97.16%); per-firm minimum 282 (MRKK), median 411.

*Justification.* Da, Engelberg & Gao (2015, *RFS*) define abnormal search volume as the log of current volume relative to the median of the prior window — the median is taken on **levels**, then logged. Taking the median of logs instead (what the panel does) is a *geometric*-median estimator with a different finite-sample bias; it coincides with the canonical form only when the window is symmetric in logs. Both are defensible estimators; only one is *Da et al.*, and a thesis citing Da et al. must use it. Measured consequence here: zero significance flips (D1).

**Parkinson range volatility.**

$$\mathrm{RV}_{i,t}=\ln\!\left(\frac{H_{i,t}}{L_{i,t}}\right)$$

Per Alizadeh, Brandt & Diebold (2002, *Journal of Econometrics*). Verified present in the delivered panel for 100% of 31,090 computable rows. $H_{i,t}=L_{i,t} \Rightarrow \mathrm{RV}$ **missing, not zero** (7 firm-weeks, `H_EQ_L_FLAG=1`). The $(H-L)/C$ variant is demoted to robustness **r3**.

**Returns — gap-safe.**

$$R_{i,t}=\ln\!\left(\frac{C_{i,t}}{C_{i,t-1}}\right)\quad\text{defined only if } t{-}1 \text{ is the immediately preceding *trading* week}$$

If week $t{-}1$ has $N\_TRADING\_DAYS=0$, $R_{i,t}$ is **NaN** and `RETURN_GAP_FLAG=1` (167 firm-weeks). Returns never bridge a suspension.

**Volume.** $V_{i,t}$ raw weekly; regressions use $\ln V_{i,t}$. $V_{i,t}=0\Rightarrow$ missing.

## 1.3 Econometric Specifications

**M1 — H1, firm-level volatility (per firm $i$, NW-HAC).**
$$\mathrm{RV}_{i,t}=\alpha_i+\beta_1\mathrm{ASVI}_{i,t-1}+\beta_2\mathrm{RV}_{i,t-1}+\beta_3\ln V_{i,t-1}+\beta_4\mathrm{Crisis}_{t-1}+\beta_5\mathrm{Sanction}_{i,t-1}+\beta_6\mathrm{Div}_{i,t-1}+\varepsilon_{i,t}$$

**M2 — H2, firm-level returns.**
$$R_{i,t}=\alpha_i+\beta_1\mathrm{ASVI}_{i,t-1}+\beta_2 R_{i,t-1}+\beta_3\mathrm{RV}_{i,t-1}+\beta_4\mathrm{Crisis}_{t-1}+\beta_5\mathrm{Sanction}_{i,t-1}+\beta_6\mathrm{Div}_{i,t-1}+\varepsilon_{i,t}$$

*M2 caveat the committee will press:* these are **raw** returns, not market-adjusted. §1.6 gives the defence and the robustness specification.

**M3 — H3, Granger causality.** Restricted: own lags + controls. Unrestricted: $+L$ lags of ASVI, $L\in\{1,2,4\}$. Per-firm $F$-test. Benjamini–Hochberg applied **separately within the H1 block and the H2 block** (never pooled), Holm alongside.

**M4 — H1-agg / H4 / H5 / H6, pooled TWFE.**
$$\widetilde{\mathrm{RV}}_{i,t}=\beta_1\widetilde{\mathrm{ASVI}}_{i,t-1}+\beta_2\widetilde{\mathrm{RV}}_{i,t-1}+\beta_3\widetilde{\ln V}_{i,t-1}+\beta_4\widetilde{\mathrm{Div}}_{i,t-1}+\beta_5\widetilde{\mathrm{Sanction}}_{i,t-1}+\tilde\varepsilon_{i,t}$$
with entity and time fixed effects; tildes denote entity-and-time demeaning.
* **Crisis is deliberately omitted** — it is constant across firms within a week and is therefore perfectly collinear with the time FE. Design, not oversight.
* **Sanction is retained** — it varies *within* firm at the anchor, so entity FE do not absorb it. Report `n_with_term` beside every coefficient (24 firms carry a step).
* H4: $+\mathrm{ASVI}\times\mathrm{Sector}$, joint Wald. H5: $+\mathrm{ASVI}\times\mathrm{Div}_{t-1}$. H6: $+\mathrm{ASVI}\times\mathrm{SOE}$ — **blocked by D4**.

**M5 — H3 structural break, asymmetric Chow at 2022-02-24.** Reduced form (ASVI$_{t-1}$, RV$_{t-1}$ + post-period crisis dummy), $q=3$, $k_{\text{pool}}=4$. Critical values from the published Giles–Lieberman tabulation (`canterbury-nz-034.pdf`), row $k=4$: $\mathrm{Cu}(0.1,10]=5.206$. Disclose (i) the $q{=}3$ vs $k{=}4$ mismatch and (ii) survivor counts under the two alternative published rows (3.372 and 18.127).

**M6 — permutation + power.** Primary null: **circular block-shift within firm** (1,000 rotations of each firm's ASVI), preserving serial correlation and destroying only alignment. Statistic: the HAC $t$ of $\hat\beta_1$; $p$ = share of placebo $|t|\ge$ actual. Secondary: i.i.d. shuffle, reported with the explicit caveat that it understates null variance. Power/MDE at 80% via the non-central $t$; power $<0.80\Rightarrow$ "inconclusive due to insufficient power", never "no effect".

**Standard errors.**
* *Per-firm (M1, M2):* **Newey–West HAC, bandwidth 4.** Rule: $m=\lfloor 4(T/100)^{2/9}\rfloor$, floored at 4 — with $T\approx 400$ this gives $m=\lfloor 4\times2.44\rfloor=9$, so **report both $m=4$ (pre-registered) and $m=9$ (data-driven)**. Bartlett kernel: $w_j=1-\tfrac{j}{m+1}$.
* *Pooled (M4, §5, §6):* **Driscoll–Kraay**, lag $=4$, robust to cross-sectional dependence of arbitrary form and to heteroskedasticity/autocorrelation. DK requires $T\gg N$; here $T=419$, $N=75$, $T/N=5.6$ — adequate but **not comfortable**. Cluster-robust (firm) is reported alongside as a lower-bound.

## 1.4 Handling of MOEX Anomalies

| Anomaly | Verified count | Rule |
|---|---:|---|
| **March 2022 suspension** | 3 weeks with **100%** of firms at 0 trading days (2022-02-28, 03-07, 03-14); partial week 03-21 | OHLCV **empty**, `N_TRADING_DAYS=0`. Never filled. Returns spanning the gap = NaN. Suspension: MOEX halted 2022-02-24; CBR closed the market to 2022-03-05; OFZ resumed 2022-03-21; 33 equities 2022-03-24; 4-hour all-stock session 2022-03-28. |
| **328 no-trade firm-weeks** | 328 | Kept as empty cells; never dropped, never filled. Worst: BLNG 23, AVAN 16, LSNG 12, YNDX 9. |
| **4 corporate actions** | 4 | `CA_FLAG=1`, `RETURN_SAFE` blanked: **VTBR** 5000:1 reverse split eff. 2024-07-15 (raw return +8.4833); **GMKN** 100:1 split eff. 2024-04-08 (−4.5147); **PLZL** 10:1 split, resumed 2025-03-27 (−2.3141); **ROLO** dilution, resumed 2023-01-19 (−2.2193). Use `RETURN_SAFE`, never `RETURN_RAW`. |
| **ROLO tick quantisation** | 1 firm | 1-kopeck price grid makes `RV` mechanically tick-bound. **Excluded from every specification where RV appears as dependent variable or regressor** — M1, M2, M3 (both blocks), M4, M5, M6, §5, §6. Retained in descriptive tables only. |
| **Saturday sessions** | 6 recorded; **228 missing post-2022** | Saturday volume/high/low are inside the weekly bar *when present* (445 firm-weeks, `N_SAT_DAYS>0`). **But see D5: the feed captured only 4 of 232 in-sample Saturdays since 2022-04.** Treat `VOLUME` and `HIGH`/`LOW` as **downward-biased** in ~55% of post-2022 weeks. |
| **Settlement regime** | — | **T+2 → 2013-03-25 → 2023-07-31; T+1 thereafter.** Ex-dividend mechanics changed mid-sample: T+2 implies last-buy-date $=R{-}2$, T+1 implies $R{-}1$. `Div` is built on **record dates only**, so the construction is invariant to the change — but the *lag interpretation* of `Div`$_{t-1}$ is not. Disclose. |
| **Market holidays** | 47 weeks with 1 holiday; 12 weeks with ≤3 sessions | `N_TRADING_DAYS` carries the count. Never re-scale volume to a 5-day week. |
| **Week 2024-06-16** | 72 of 75 firms absent | Excluded from cross-sectional event/crisis detection. |
| **Week 2024-08-25** | 73 of 75 firms, volume empty | Excluded from volume-based metrics. |

## 1.5 Literature actually available (7 PDFs — not "Da, Vlastakis, Swamy")

`thesis books.zip` contains exactly: `A SIMPLE, POSITIVE SEMI-DEFINITE,.pdf` · `DeLongShleiferSummersWaldmann90.pdf` · `GSVI_India.pdf` · `Individual investors' trading behavior in Moscow Exchange and the COVID-19 crisis.pdf` · `Paul Gao Paper.pdf` · `canterbury-nz-034.pdf` (Giles–Lieberman tabulation, source of the M5 critical values) · `information-demand-and-stock-market-volatility-3f9rke46sb.pdf`.

**No Vlastakis and no Swamy are present.** Any methodology section citing them is unsupported by this repository.

## 1.6 Defence: "Why no market-adjusted returns in M2?"

The honest answer has three parts.

1. **The panel has no market index.** It contains 75 firm-level OHLCV series and no IMOEX/RTS series. A market-adjusted return requires either the index or an estimated beta.
2. **Therefore the defensible construction is a residual, not a raw return.** Specify M2 with **sector×week fixed effects** in a pooled estimation:
$$R_{i,t}=\alpha_i+\gamma_{s(i),t}+\beta_1\mathrm{ASVI}_{i,t-1}+\beta_2R_{i,t-1}+\beta_3\mathrm{RV}_{i,t-1}+\dots+\varepsilon_{i,t}$$
$\gamma_{s(i),t}$ absorbs the sector-level market factor without needing an external index, and — critically — **does not absorb ASVI**, which varies within sector-week. This is the recommended correction and should replace raw-return M2 as the headline H2 test, with raw-return M1/M2 retained as the per-firm specification.
3. **Firm FE + time FE in M4 already partial out the aggregate factor** for the pooled tests; the exposure is confined to the per-firm M2 block.

If a committee demands a beta-adjusted return, construct an **equal-weighted 75-firm portfolio return** from the panel itself as the market proxy, and report $\hat\beta_i$ from a rolling 104-week window. This is estimable today from the delivered data — it is the single highest-value addition to the analysis.

---

# PART 2: CRISIS MATRIX (Verified)

Recovered by run-length decoding of `crisis_i_t.csv`. Identical for all 75 firms in every window; **1,725 firm-weeks (5.489%)**.

| Window | Start (Mon) | End (Sun) | Weeks | Trigger | Verification |
|---|---|---|---:|---|---|
| **W1** | 2020-02-24 | 2020-04-19 | 8 | COVID-19 onset crash + oil-price war | Panel-internal (pre-specified ≥40% spike protocol). *Not* "Jan 2020" as the brief states. |
| **W2** | 2022-02-21 | 2022-04-03 | 6 | Invasion + MOEX suspension (2022-02-24 → 03-24) | **VERIFIED EXTERNALLY** — MOEX halted 24.02.2022 (MOEX n41370); CBR closure to 05.03.2022; 100% of firms show 0 trading days in the panel for 3 consecutive weeks |
| **W3** | 2023-09-04 | 2023-09-24 | 3 | Sept-2023 ruble / key-rate crisis | Panel-internal. **This is Sep 2023, not the "Sep 2022 Mobilization" the brief names.** |
| **W4** | 2026-06-22 | 2026-08-02 | 6 | Bear-market capitulation | **VERIFIED EXTERNALLY** — MOEX −4.65% on 22.06.2026, worst day since Sep-2022, index 2,318.2 (Ukrinform/mezha 23.06.2026); 17-week losing streak, longest since 1997 (CEPA 16.07.2026); MOEX ~2,040 by 16.07.2026 (Moscow Times) |

**Detection protocol (pre-specified, not post-hoc).** A firm "spikes" in week $t$ iff $\mathrm{RV}$-ratio $\ge2$ **and** Volume-ratio $\ge2$ vs its own trailing 52-week median (≥30 obs). A week is a stress week if ≥25% of eligible firms spike. A window is confirmed if contiguous stress weeks (1-week bridging) reach a **peak spike fraction ≥40%**. 7 candidate windows were evaluated and rejected with numeric evidence.

### ⚠️ The mobilisation gap — disclosed, not hidden

The September 2022 partial-mobilisation announcement (**2022-09-19**) falls in week `2022-09-19`, which lies **outside all four windows** (W2 ends 2022-04-03; W3 begins 2023-09-04). The raw data shows this is a genuine attention-and-volatility event: GAZP's Cyrillic channel jumps from 160,577 (week 09-12) to **214,597** (week 09-19) and 270,996 (week 09-26); the Latin channel 9,364 → 14,992 → 15,918.

It was excluded because the pre-specified protocol requires a ≥40% *cross-sectional* spike fraction, which the mobilisation week did not reach. **This is a protocol consequence, not a data error** — but it means the thesis's crisis control is blind to the single most-cited 2022 Russian market shock after the invasion. Recommended robustness **r5**: re-estimate M1/M4 with an added `MOBIL_2022_09` dummy and report whether $\beta_1$ moves.

---

# PART 3: SANCTIONS MATRIX (Verified)

`sanction_i_t.csv` is a **permanent step**: 0 before the anchor, 1 from the anchor week to 2026-08-31. **24 firms, 3,687 firm-weeks (11.733%).** A separate `sanction_reaction_i_t.csv` marks 5-week reaction windows only (**35 event windows, 181 firm-weeks, 0.576%**).

**Convention:** the matrix anchor is the **Monday of the week containing** the earliest economically binding EU/US/UK entity-level designation. This is correct and is why anchors sit a few days before the published date.

| Ticker | Company | Matrix anchor (week Mon) | Designation date | Body | Verification source | Status |
|---|---|---|---|---|---|---|
| VTBR | Bank VTB | 2022-02-21 | 2022-02-24 | US OFAC SDN + UK OFSI | US Treasury 24.02.2022; UK Notice 24.02.2022 (RUS0250) | **VERIFIED** |
| NMTP | Novorossiysk Commercial Sea Port | 2022-02-21 | 2022-02-24 | EU Reg. 833/2014 capital-markets + US/UK | EU 23.02.2022 listing; OFAC 24.02.2022 | **VERIFIED** |
| SBER | Sberbank | 2022-04-04 | 2022-04-06 | US OFAC SDN + UK OFSI asset freeze | Global Trade Alert / Latham & Watkins 06.04.2022 | **VERIFIED** |
| ALRS | ALROSA | 2022-04-04 | 2022-04-07 | US OFAC SDN | Treasury 07.04.2022 | **VERIFIED** |
| CBOM | Credit Bank of Moscow | 2022-04-04 | 2022-04-06 | UK OFSI asset freeze | UK Notice 06.04.2022 | **VERIFIED** |
| CHMF | Severstal | 2022-05-30 | 2022-06-02 | US OFAC SDN | Covington 03.06.2022; GL 36 wind-down | **VERIFIED** |
| IRKT | Irkut / Yakovlev (UAC) | 2022-06-27 | 2022-06-28 | US OFAC SDN | Treasury jy0838, 28.06.2022 | **VERIFIED** |
| KMAZ | KAMAZ | 2022-06-27 | 2022-06-28 | US OFAC SDN (also EU/UK) | Treasury jy0838, 28.06.2022 | **VERIFIED** |
| MAGN | MMK | 2022-08-01 | 2022-08-02 | US OFAC SDN | Treasury, 02.08.2022 | **VERIFIED** |
| BSPB | Bank Saint Petersburg | 2023-02-20 | 2023-02-23 | UK OFSI asset freeze | OFSI Notice 15.12.2023 (Listed 24.02.2023, Designated 23.02.2023) | **VERIFIED** |
| USBN | Bank Uralsib | 2023-02-20 | 2023-02-23 | US OFAC SDN + UK OFSI | OFSI Notice 15.12.2023 (RUS0233) | **VERIFIED** |
| MTSS | MTS (via MTS Bank) | 2023-02-20 | 2023-02-24 | US OFAC SDN (subsidiary) | Treasury 24.02.2023 | **PARTIAL** — parent not designated |
| FESH | FESCO / DVMP | 2023-05-15 | 2023-05-18 | UK OFSI asset freeze | OFSI Notice 19.05.2023, RUS1849, Designated 18.05.2023 | **VERIFIED** |
| PLZL | Polyus | 2023-05-15 | 2023-05-18 | UK OFSI asset freeze | OFSI Notice 19.05.2023, Designated 18.05.2023 | **VERIFIED** |
| TRMK | TMK | 2023-05-15 | 2023-05-18 | UK OFSI asset freeze | Interfax 19.05.2023; OFSI Notice 19.05.2023 | **VERIFIED** |
| AFLT | Aeroflot | 2022-05-16 | 2022-05-19 | UK OFSI asset freeze | Interfax/Yermak-McFaul 19.05.2022 | **SINGLE-SOURCE — PROVISIONAL** |
| SIBN | Gazprom Neft | 2025-01-06 | 2025-01-10 | US OFAC SDN + UK OFSI | UK Notice 10.01.2025 (RUS2383, Group 16736); OFAC 10.01.2025 | **VERIFIED** |
| SNGS | Surgutneftegas | 2025-01-06 | 2025-01-10 | US OFAC SDN + UK OFSI | UK Notice 10.01.2025 (RUS2382, Group 16735); OFAC 10.01.2025 | **VERIFIED** |
| LKOH | LUKOIL | 2025-10-20 | 2025-10-22 | US OFAC SDN (UK 15.10.2025) | OFAC recent-actions/20251022; Treasury sb0290 | **VERIFIED** |
| ROSN | Rosneft | 2025-10-20 | 2025-10-22 | US OFAC SDN (UK 15.10; EU trans. ban 23.10) | OFAC recent-actions/20251022; Treasury sb0290 | **VERIFIED** |
| BANE | Bashneft | 2026-04-20 | 2026-04-23 | EU asset freeze, 20th package | Reg. (EU) 2026/509; Reuters 22.04.2026 | **VERIFIED** |
| MFGS | Slavneft-Megionneftegaz | 2026-04-20 | 2026-04-23 | EU asset freeze via parent Slavneft | Reg. (EU) 2026/509 (Slavneft + subsidiaries) | **VERIFIED (parent-level)** |
| JNOS | Slavneft-YANOS | 2026-04-20 | 2026-04-23 | EU asset freeze via parent Slavneft | Reg. (EU) 2026/509 (Slavneft + subsidiaries) | **VERIFIED (parent-level)** |
| AVAN | AKB Avangard | 2026-04-20 | 2026-04-22 | EU transaction ban (eff. 2026-05-14) | **NOT found** in the 20th-package coverage reviewed | ⛔ **UNVERIFIED — EXCLUDED** |

**Score: 20 verified · 3 parent-level verified · 1 provisional single-source · 1 excluded.**

**Excluded per the zero-hallucination rule:** `AVAN`. The repository asserts an EU transaction ban effective 2026-05-14 following an 2026-04-22 designation. The EU 20th package (Reg. (EU) 2026/506 / 2026/509 / 2026/511, adopted 23.04.2026) is confirmed, and its named energy designations are confirmed (Bashneft, Slavneft, seven refineries, Gazprom subsidiaries) — but **Avangard does not appear in any 20th-package source reviewed**. Its 20 firm-weeks are removed from `Sanction` until an EUR-Lex citation is produced.

**Firms with reaction-only windows, no permanent step** (correctly excluded from `Sanction`, present in `SanctionReaction`): GAZP (2022-10-03, EU oil annex 2022-10-06), TATN (2022-10-03; 2026-07-23 affiliate event), plus secondary windows for BSPB, CBOM, FESH, LKOH, NMTP, PLZL, ROSN, SBER, SIBN, TRMK.

**Verified negatives** (designated *not* to include, documented): GMKN, NLMK, IRAO, GAZP, TATN, RTKM (sectoral instruments only, no entity-level freeze), RUAL (**SDN removal** 2019-01-27 — a sanctions-*relief* event), VSMO, SELG, BLNG, UKUZ, ROLO, CHMK, YNDX, MGTS, TTLK, VJGZ, RNFT.

---

# PART 4: DIVIDEND MATRIX (Verified Sample)

**Construction (verified).** `Div_{i,t}=1` for the **4 calendar weeks immediately preceding** each confirmed record week; the record week itself is **excluded**; overlapping same-year windows are unioned, never double-counted. **587 confirmed-paid record dates · 63 paying firms · 2,110 firm-weeks (6.714%).** Source support: 367 rows × 4 sources, 186 × 3, 34 × 2.

**12 zero-dividend firms** (whole sample): BLNG, CHMK, FESH, JNOS, MFGS, MRKK, RNFT, ROLO, UKUZ, UNAC, UTAR, VJGZ.
**58 record dates fall before the panel start** (2018-01-08 … 2018-06-01) and therefore generate no in-panel `Div` cells — correctly handled, `in_panel_window_count=0`.

**⚠️ Payment dates are NOT in the dataset.** `dividend_record_dates.csv` carries record dates only. The brief's requested "Payment Date" column **cannot be filled without fabrication** and is therefore omitted. Likewise **announcement dates** are not held per-row; `period_evidence` records the reporting period each source attributes.

**⚠️ 38 record dates fall on a Saturday or Sunday.** This is **legal and normal in Russia** — the shareholder register closes on a calendar date, not a trading date. Agent 6's proposed check ("no dividend record date falls on a weekend") is **invalid for MOEX** and was not applied as an error test. Distribution of the 587 record dates by weekday: Mon 179 · Tue 164 · Fri 91 · Thu 62 · Wed 53 · **Sun 31 · Sat 7**.

### Live-verified against the primary source (2026-10-05)

I fetched `smart-lab.ru` directly and matched it against the repository table. **8 of 8 rows match exactly** on both record date and amount:

| Ticker | Record date | RUB | Period | Independent confirmation |
|---|---|---:|---|---|
| SBER | 2026-07-20 | 37.64 | FY2025 | smart-lab; T-Bank 17.07.2026; vbr.ru (AGM 30.06.2026, RUB 850.2bn) |
| SBER | 2025-07-18 | 34.84 | FY2024 | smart-lab; finviewer.ru; divvydiary (pay date 01.08.2025) |
| SBER | 2024-07-11 | 33.30 | FY2023 | smart-lab |
| SBER | 2023-05-11 | 25.00 | FY2022 | smart-lab |
| ROSN | 2026-07-09 | 2.27 | FY2025 | smart-lab (yield 0.7% — the collapse is **real**, not a data error) |
| ROSN | 2026-01-12 | 11.56 | Q3-2025 | smart-lab |
| ROSN | 2025-07-20 | 14.68 | FY2024 | smart-lab |
| ROSN | 2025-01-10 | 36.47 | Q3-2024 | smart-lab |

### Top-20 payers — 2024 → 2026-08 confirmed record dates (82 rows)

Source-key: **SL** = `smart-lab.ru/q/<T>/dividend/` · **DH** = `dohod.ru/ik/analytics/dividend/<t>` · **TB** = `tbank.ru/invest/stocks/<T>/dividends/` · **ZR** = `закрытияреестров.рф/<company>/`

| Ticker | Record date | RUB | Period | Sources (n) | Live-verified |
|---|---|---:|---|---|---|
| AKRN | 2024-05-19 | 427 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| AKRN | 2025-06-09 | 534 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| AKRN | 2025-12-09 | 189 | 3кв 2025 / 2025 | SL+DH+TB (3) | repo table |
| AKRN | 2026-08-10 | 235 | 1кв 2026 / 2026 | SL+DH+TB+ZR (4) | repo table |
| AVAN | 2024-05-29 | 33.46 | 1кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| AVAN | 2024-10-02 | 34.7 | 2кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| AVAN | 2024-12-02 | 49.57 | 3кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| AVAN | 2025-04-28 | 28.5 | 4кв 2024 / 2025 | SL+DH+ZR (3) | repo table |
| AVAN | 2025-10-07 | 24.79 | 2кв 2025 / 2025 | SL+DH+ZR (3) | repo table |
| AVAN | 2025-12-23 | 16.1 | 3кв 2025 / 2025 | SL+DH+ZR (3) | repo table |
| AVAN | 2026-04-28 | 22.31 | 4кв 2025 / 2026 | SL+DH+ZR (3) | repo table |
| BSPB | 2024-05-06 | 23.37 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| BSPB | 2024-09-30 | 27.26 | 2кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| BSPB | 2025-05-05 | 29.72 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| BSPB | 2025-10-06 | 16.61 | 2кв 2025 / 2025 | SL+DH+TB+ZR (4) | repo table |
| BSPB | 2026-05-12 | 26.23 | 4кв 2025 / 2026 | SL+DH+TB+ZR (4) | repo table |
| CHMF | 2024-06-18 | 229.81 | 1кв 2024 / 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| CHMF | 2024-09-10 | 31.06 | 2кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| CHMF | 2024-12-17 | 49.06 | 3кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| GCHE | 2024-04-07 | 205.38 | 2023 год / 2024 | SL+DH+ZR (3) | repo table |
| GCHE | 2024-09-29 | 142.11 | 2кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| GCHE | 2025-04-07 | 98.92 | 4кв 2024 / 2025 | SL+DH+ZR (3) | repo table |
| GCHE | 2026-04-07 | 229.37 | 4кв 2025 / 2026 | SL+DH (2) | repo table |
| KAZT | 2024-05-27 | 15 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| KAZT | 2024-12-03 | 7 | 3кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| KAZT | 2025-05-26 | 2.5 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| KAZT | 2025-09-23 | 4 | 2кв 2025 / 2025 | SL+DH+TB+ZR (4) | repo table |
| LKOH | 2024-05-07 | 498 | 2023 год / 2024 | SL+DH+ZR (3) | repo table |
| LKOH | 2024-12-17 | 514 | 3кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| LKOH | 2025-06-03 | 541 | 4кв 2024 / 2025 | SL+DH+ZR (3) | repo table |
| LKOH | 2026-01-12 | 397 | 3кв 2025 / 2025 | SL+DH+ZR (3) | repo table |
| LKOH | 2026-05-04 | 278 | 4кв 2025 / 2026 | SL+DH+ZR (3) | repo table |
| LSNG | 2024-07-02 | 0.4249 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| LSNG | 2025-07-03 | 0.4281 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| LSNG | 2026-06-17 | 0.5379 | 2025 год / 2026 | SL+DH+TB (3) | repo table |
| MAGN | 2024-06-10 | 2.752 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| MAGN | 2024-10-17 | 2.494 | 2кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| MSRS | 2024-07-05 | 0.14282 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| MSRS | 2025-07-08 | 0.15054 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| MSRS | 2026-07-03 | 0.1865 | 2025 год / 2026 | SL+DH+TB (3) | repo table |
| MTSS | 2024-07-16 | 35 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| MTSS | 2025-07-07 | 35 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| MTSS | 2026-07-09 | 35 | 2025 год / 2026 | SL+DH+TB+ZR (4) | repo table |
| NLMK | 2024-05-27 | 25.43 | 2023 год / 2024 | SL+DH+ZR (3) | repo table |
| NMTP | 2024-07-10 | 0.772 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| NMTP | 2025-07-14 | 0.9573 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| NMTP | 2026-07-13 | 1.1448 | 2025 год / 2026 | SL+DH+TB+ZR (4) | repo table |
| NVTK | 2024-03-26 | 44.09 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| NVTK | 2024-10-11 | 35.5 | 2кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| NVTK | 2025-04-28 | 46.65 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| NVTK | 2025-10-06 | 35.5 | 2кв 2025 / 2025 | SL+DH+TB+ZR (4) | repo table |
| NVTK | 2026-04-13 | 47.23 | 4кв 2025 / 2026 | SL+DH+TB+ZR (4) | repo table |
| PHOR | 2024-07-11 | 309 | 1кв 2024 / 4кв 2023 / 2024 | SL+DH+TB+ZR (4) | repo table |
| PHOR | 2024-09-22 | 117 | 2кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| PHOR | 2024-12-22 | 126 | 3кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| PHOR | 2025-06-09 | 87 | 4кв 2024 / 2025 | SL+DH+TB+ZR (4) | repo table |
| PHOR | 2025-10-01 | 273 | 2кв 2025 / 2025 | SL+DH+TB+ZR (4) | repo table |
| PLZL | 2024-12-13 | 130.18 | 3кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| PLZL | 2025-04-25 | 73 | 2024 год / 2025 | SL+DH+TB+ZR (4) | repo table |
| PLZL | 2025-10-13 | 70.85 | 2кв 2025 / 2025 | SL+DH+TB+ZR (4) | repo table |
| PLZL | 2025-12-22 | 36 | 3кв 2025 / 2025 | SL+DH+TB+ZR (4) | repo table |
| PLZL | 2026-05-18 | 56.8 | 4кв 2025 / 2026 | SL+DH+TB+ZR (4) | repo table |
| PLZL | 2026-07-13 | 29.05 | 1кв 2026 / 2026 | SL+DH+TB+ZR (4) | repo table |
| ROSN | 2024-01-11 | 30.77 | 3кв 2023 / 2023 | SL+DH+TB+ZR (4) | repo table |
| ROSN | 2024-07-09 | 29.01 | 2023 год / 2024 | SL+DH+TB+ZR (4) | repo table |
| ROSN | 2025-01-10 | 36.47 | 3кв 2024 / 2024 | SL+DH+TB+ZR (4) | **YES** |
| ROSN | 2025-07-20 | 14.68 | 2024 год / 2025 | SL+DH+TB+ZR (4) | **YES** |
| ROSN | 2026-01-12 | 11.56 | 3кв 2025 / 2025 | SL+DH+TB+ZR (4) | **YES** |
| ROSN | 2026-07-09 | 2.27 | 2025 год / 2026 | SL+DH+TB+ZR (4) | **YES** |
| SIBN | 2024-07-08 | 19.49 | 4кв 2023 / 2024 | SL+DH+TB+ZR (4) | repo table |
| SIBN | 2024-10-14 | 51.96 | 2кв 2024 / 2024 | SL+DH+TB+ZR (4) | repo table |
| SIBN | 2025-07-08 | 27.21 | 4кв 2024 / 2025 | SL+DH+TB+ZR (4) | repo table |
| SIBN | 2025-10-13 | 17.3 | 2кв 2025 / 2025 | SL+DH+TB+ZR (4) | repo table |
| SIBN | 2026-07-06 | 28.11 | 2025 год / 2026 | SL+DH+TB+ZR (4) | repo table |
| TATN | 2024-01-09 | 35.17 | 3кв 2023 / 2023 | SL+DH+ZR (3) | repo table |
| TATN | 2024-07-09 | 25.17 | 2023 год / 2024 | SL+DH+ZR (3) | repo table |
| TATN | 2024-10-08 | 38.2 | 2кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| TATN | 2025-01-08 | 17.39 | 3кв 2024 / 2024 | SL+DH+ZR (3) | repo table |
| TATN | 2025-06-02 | 43.11 | 4кв 2024 / 2025 | SL+DH+ZR (3) | repo table |
| TATN | 2025-10-14 | 14.35 | 2кв 2025 / 2025 | SL+DH+ZR (3) | repo table |
| TATN | 2026-01-11 | 8.13 | 3кв 2025 / 2025 | SL+DH+ZR (3) | repo table |
| TATN | 2026-07-15 | 11.61 | 4кв 2025 / 2026 | SL+DH+ZR (3) | repo table |

**Excluded announced-but-never-paid events (18, each independently evidenced)** — GAZP 2022-07-20 (52.53₽, AGM voted against), CHMF Q4-2021 (109.81₽ withdrawn), MAGN 2022-04-01 (cancelled 24.05.2022), MGNT 2025-01-09 (560₽, EGM no quorum), NLMK 2023-01-11 (98% against) and Q4-2021 (withdrawn), PHOR 2023-10-11 and 2025-07-05, PLZL 2023-06-16, MSNG 2025-07-08 and 2026-03-03, MSTT 2019-12-23, TGKA 2022-07-18 and 2025-07-08, AVAN 2018-06-12 (smart-lab data error), MRKU 2022-10-14 (T-Bank duplicate), OGKB FY2024 first attempt, YNDX 2026-09-21 (outside window).

---

# PART 5: NEWS SHOCK MATRIX — ⛔ **PROVISIONAL / BLOCKED**

**`News_i_t.csv` does not exist in either archive.** The table below is the only surviving per-event listing (`NEWS_LAYER_RECONCILIATION_REPORT.md`, 2026-09-21). Its cited evidence file `work/n4_full.json` is **absent**, so no event URL can be inspected. **It conflicts with `news.md`, which claims 31 events / 23 companies / 89 cells.** Treat every row as PROVISIONAL until the evidence file is recovered.

| ID | Ticker | Onset | Onset week | Category | Duration | Event |
|---|---|---|---|---|---:|---|
| GMKN-1 | GMKN | 2021-02-20 | 2021-02-15 | OPS | 2 | Norilsk concentrator ore-transfer building / crushing-gallery collapse, 3 dead |
| GMKN-3R | GMKN | 2021-03-24 | 2021-03-22 | CAPRET | 3 | Dividend policy switched from EBITDA formula to FCF |
| TRMK-1 | TRMK | 2021-03-09 | 2021-03-08 | MA | 4 | TMK to acquire 86.54% of ChelPipe for RUB 84.2bn |
| YNDX-1 | YNDX | 2019-10-10 | 2019-10-07 | LEGAL | 4 | Duma hearing on the Gorelkin 20% foreign-ownership bill; −16…−20% |
| SFIN-1 | SFIN | 2024-02-26 | 2024-02-26 | CAPRET | 4 | SFI proposes cancelling 55% of charter capital; +27% intraday |
| UPRO-1 | UPRO | 2022-06-13 | 2022-06-13 | MA | 4 | Fortum/Uniper collect binding bids for Russian assets; +30% |
| UPRO-2 | UPRO | 2023-04-25 | 2023-04-24 | LEGAL | 4 | Presidential decree places Uniper's 83.73% under Rosimushchestvo |
| KZOS-1 | KZOS | 2021-04-23 | 2021-04-19 | MA | 3 | TAIF–SIBUR merger announced; KOS inside the perimeter |
| NKNC-1 | NKNC | 2021-04-23 | 2021-04-19 | MA | 2 | TAIF–SIBUR merger announced; NKNK inside the perimeter |
| MGTS-2R | MGTS | 2026-05-14 | 2026-05-11 | CAPRET | 4 | Buyback of up to 10% of prefs at RUB 1501 (≈2× market) |
| AFLT-1 | AFLT | 2020-08-06 | 2020-08-03 | CAPRET | 1 | Board approves issue of up to 1.7bn shares (2.5× capital) |
| AFLT-2 | AFLT | 2020-09-21 | 2020-09-21 | CAPRET | 4 | SPO formally launched, up to 1.7bn shares |
| ABRD-1 | ABRD | 2021-12-28 | 2021-12-27 | MA | 4 | Acquisition of the Sheki Sharab winery, Azerbaijan |
| MVID-1 | MVID | 2025-03-17 | 2025-03-17 | DEBT | 2 | RUB 30bn recapitalisation announced amid distress (ND/EBITDA 6.4×) |
| MVID-2 | MVID | 2024-05-13 | 2024-05-13 | CAPRET | 1 | Board decides 17% charter-capital increase; SFI may take all |
| UTAR-1 | UTAR | 2021-12-20 | 2021-12-20 | DEBT | 4 | Debt restructuring completed; shareholder debt converted to equity |
| UTAR-2 | UTAR | 2020-06-01 | 2020-06-01 | DEBT | 4 | Fifth loan default / restructuring-distress episode |

**Totals: 17 events · 12 companies · 54 firm-weeks (0.172%).** Categories: MA 5, CAPRET 6, DEBT 3, LEGAL 2, OPS 1. `News ∩ Crisis = 0` and `News ∩ Sanction = 0` by a ±2-week pre-test dedup (7 candidates discarded).

**The brief asked for "the top 20–30 undeniable firm-specific transient shocks."** That list cannot be delivered to that standard from this repository: the surviving evidence is 17 events with no retrievable URLs, against a conflicting 31-event claim. `news.md` itself reports that News explains only **17 of the 592 largest RV spikes (2.9%)** and that **66.4% of ≥2× RV weeks remain unexplained**, dominated by illiquid mid-caps rather than by 1,800 hidden events.

---

# PART 6: LIMITATIONS & DISCLOSURES

### A. Could not be verified and was therefore excluded

| Item | Reason | Consequence |
|---|---|---|
| **AVAN sanction anchor (2026-04-22)** | Not found in any EU 20th-package source reviewed | 20 firm-weeks removed from `Sanction`; must be restored only with an EUR-Lex citation |
| **AFLT sanction anchor (2022-05-19)** | Single source (Interfax/Yermak-McFaul); not corroborated by an OFSI notice in this pass | Retained, flagged PROVISIONAL |
| **All 17 News events** | Evidence file `work/n4_full.json` absent; no URL retrievable | `News` is PROVISIONAL; robustness **r2 cannot be run as specified** |
| **The 31-event News variant** | `work/g4_events_final.csv` absent | Unresolvable conflict with the 17-event table |
| **SOE reconciliation (C6)** | `soe_classifications_compiled.csv` and `agent2b_soe.csv` both absent | **H6, §6, sensitivities 5–6 are BLOCKED** |
| **Payment dates (all 587 dividends)** | Not present in `dividend_record_dates.csv` | Part 4 omits the column rather than inventing it |
| **Per-row dividend announcement dates** | Not present | Only `period_evidence` (reporting period) is reported |
| **Vlastakis; Swamy** | Not among the 7 PDFs in `thesis books.zip` | Any citation to them is unsupported |
| **228 post-2022 Saturday sessions** | Absent from the raw investing.com feed | Systematic downward bias in `VOLUME`, `HIGH`, `LOW` (D5) |

### B. Verified limitations that must be disclosed in the thesis

1. **ASVI is not the Da et al. formula** (D1). Corrected variant supplied; **zero significance flips** across 75 firms.
2. **`week_sunday` is off by 8 days in all four control matrices** (D2). Corrected files supplied. Any result produced by joining on `week_sunday` is invalid.
3. **M2 uses raw, not market-adjusted, returns.** Remedy specified in §1.6: sector×week fixed effects, or an equal-weighted 75-firm portfolio beta. Neither has been run.
4. **The mobilisation week (2022-09-19) is outside every crisis window** despite a clear attention spike (GAZP Cyrillic 160,577 → 214,597 → 270,996). Robustness **r5** recommended.
5. **`Sanction` is a permanent step**, so its coefficient identifies a *level regime shift*, not an event-study reaction. Only 7 of 23 tested anchors show a confirmed 2×/2× RV-and-volume reaction — a substantive finding, not a defect.
6. **Driscoll–Kraay with $T/N=5.6$** is adequate but not comfortable; report firm-clustered SEs alongside.
7. **Newey–West bandwidth 4 is pre-registered but the data-driven rule gives 9.** Report both.
8. **38 weekend record dates are legal in Russia** — do not "fix" them.
9. **The settlement regime changed mid-sample** (T+2 → T+1 on 2023-07-31). `Div` is record-date-based and therefore invariant, but the economic meaning of `Div`$_{t-1}$ is not.
10. **YNDX excludes ~2 years of real YDEX search volume** post-2024 (documented prior decision).
11. **ROLO is excluded from all RV-bearing specifications** (1-kopeck tick quantisation).
12. **58 dividend record dates precede the panel start** and generate no `Div` cells — correct, but it means early-2018 `Div` coverage is thin.

### C. Reproduction

`/home/user/audit/` holds `audit1.py`–`audit7.py`, `fix1.py`, `tables.py` — every number in this document is produced by one of these against the downloaded archives. `/home/user/deliverables/` holds `FIX1_asvi_both_variants.csv`, `FIX1_asvi_beta1_impact.csv`, four `*_weeklabel_FIXED.csv` matrices, and the two source tables.

---

# APPENDIX — QA & DEFENCE COMMITTEE RECORD

### Stage 1 QA: **6 / 10**

| Criterion | Score | Finding |
|---|---:|---|
| Equations mathematically sound and literature-aligned | **1 / 2** | M1–M6 sound and HAC/DK rules now explicit — but the delivered ASVI is **not** Da et al. (D1), and the cited Giles–Lieberman row carries an unresolved $q{=}3$ vs $k{=}4$ mismatch |
| Sanction and Dividend matrices fully sourced and verified | **2 / 2** | Sanctions 20/24 verified against OFAC/OFSI/EU primary sources, 1 excluded; dividends 8/8 live-verified against smart-lab, 587 rows internally consistent (0 window violations) |
| MOEX quirks (T+2, Saturdays, suspensions) explicitly handled | **1 / 2** | Suspension, holidays, corporate actions all handled and verified. **T+2 is wrong** (T+1 since 2023-07-31) and **228 of 232 post-2022 Saturdays are missing** (D5) |
| Zero hallucinations or invented data | **1 / 2** | No date was invented; AVAN excluded rather than kept. But the repository ships **two contradictory News tables** and the `week_sunday` label bug, both of which would have propagated into published results |
| Formatting ready for thesis submission | **1 / 2** | Parts 1–4 and 6 are submission-ready. **Parts 5 and the SOE section are not publishable** — the underlying matrices do not exist |

**Loop-back commands issued:** Agent 3 → produce EUR-Lex citation for AVAN or keep it excluded. Agent 1 → recover `News_i_t.csv`, `soe_classifications_compiled.csv`, `agent2b_soe.csv`, `work/n4_full.json`, `work/g4_events_final.csv` from repository history. Agent 4 → adopt canonical ASVI; re-label all matrices on `week_monday + 6`. Agent 2 → quantify the D5 Saturday-volume bias.

### Stage 2 Defence Committee: **PARTIAL VETO**

| Attack | Survives? |
|---|---|
| *"Why didn't you use market-adjusted returns?"* | ⚠️ **PARTIAL** — no answer existed; §1.6 now supplies one (sector×week FE, or a 75-firm equal-weighted portfolio beta). Not yet estimated. |
| *"Why is your ASVI formula different from Da et al.?"* | ✅ **SURVIVES** — the mismatch is confirmed, quantified (corr 0.9998), and shown to change no inference (75/75 concordance, 0 significance flips). The canonical variant is supplied. |
| *"Are you sure about this sanction date?"* | ✅ **SURVIVES** — 20/24 verified against OFAC/OFSI/EU primary sources; the one unverifiable row (AVAN) was **excluded**, not defended. |
| *"Your `week_sunday` is a week off."* | ✅ **CAUGHT AND FIXED** — 8-day error in all four matrices; corrected files supplied. |
| *"Where is your News variable?"* | ❌ **FATAL** — the matrix does not exist. VETO stands on this point. |
| *"Where is your SOE classification?"* | ❌ **FATAL** — the CSV does not exist; H6 is untestable. VETO stands. |
| *"Your T+2 assumption is wrong."* | ✅ **CORRECTED** — T+1 since 2023-07-31; disclosed with its `Div` lag implication. |
| *"Your weekend sessions are missing."* | ❌ **UNRESOLVED** — 228 of 232 Saturdays absent; the raw feed cannot be repaired without re-sourcing prices. |

**Verdict: the document is approved for Parts 1–4 and 6, and vetoed for Part 5 and the SOE analysis.** The blocking items are missing *data*, not missing *work* — no amount of further analysis can manufacture `News_i_t.csv` or `soe_classifications_compiled.csv` from these two archives.

### Stage 3 — self-audit of this document (disclosed because the directive is zero hallucinations)

The first draft of the Part 4 dividend table **failed audit**. It contained **36 of 93 rows that do not exist in `dividend_record_dates.csv`** — dates and amounts extrapolated from the shape of the 2025–26 extract rather than read from source (e.g. `MAGN 2024-06-05 / 3.09`, `NLMK 2024-01-15 / 4.90`, `SIBN 2024-10-14 / 25.05` against a true 51.96) — while **30 genuine source rows were omitted**. This is precisely the failure mode the brief prohibits, and it originated in this document, not in the repository.

The table was rebuilt programmatically directly from the CSV and re-verified row-by-row: **90 parsed rows, 0 discrepancies, 0 source rows missing.** Every date and amount in Part 4 is now traceable to `dividend_record_dates.csv` (82 rows) or to a live `smart-lab.ru` fetch performed on 2026-10-05 (8 rows). The verifier is retained at `audit/verify_doc.py`.

Lesson carried into the QA rubric: *no table in this document was accepted on the basis of having been read — each was re-parsed and diffed against its source file.*
