# COMBINED DATA FILE — Crisis(t), Sanction(i,t), News(i,t), Div(i,t), SOE(i)
## 75-company MOEX weekly panel · 419 weeks (2018-08-27 → 2026-08-31) · 31,425 cells

**Assembled:** 2026-09-20 (Agent 5) from `builderA_section_R2.md` (Event Controls) and
`builderB_section_R2.md` (Dividends + State Ownership). **QA:** QA1 10/10 · QA2 10/10 (Round 2).
**Assembly rule:** variable sections are included VERBATIM from the QA-passed Round-2 texts —
no figure or finding altered; only this framing, the table of contents, the consolidated overlap
table (§6.0), the consolidated gap register (§7), and the merged integrity statement (§8) are
Agent-5 text. Cross-reference map: "A §x" = Part I section x below; "B §x" = Part II section x;
"[via-A §x]" inside Part II points to Part I.

## Table of contents

- §0 Merged provenance and conventions
- §1 Crisis(t) — market-wide stress windows [A §1 verbatim]
- §2 Sanction(i,t) — company-specific permanent step [A §2 verbatim]
- §3 News(i,t) — company-specific transient shocks [A §3 verbatim]
- §4 Div(i,t) — dividend pre-record dummy [B §1 verbatim]
- §5 SOE(i) — time-invariant state-ownership dummy [B §2 verbatim]
- §6 Consolidated overlap disclosure (§6.0 table + §6.1 event-pair computations [A §4] + §6.2 Div-pair computations [B §3])
- §7 Consolidated gaps, blockers & methodology-amendment register
- §8 Merged integrity statement (incl. Agent-5 no-alteration confirmation)
- §9 Builder/QA audit trail

## §0 Merged provenance and conventions

- **Sources used (data):** ONLY the four permitted reports — `Sanctions Events, Crisis Windows &
  Data Readiness.md` [SANC] + `news.md` [NEWS] for Part I; `Final dividend table.md` [DIV] +
  `UNIFIED_PAPER_about_state_ownership.md` [SOE] for Part II. Every value cites its source
  section; derived values are labeled ("(d)" / "Builder arithmetic").
- **Governing reference:** `methodology_final.md` [METH] — rules only, never a data source.
- **Forbidden:** `iqbal thesis data.zip`, `thesis books.zip` (never opened/consulted);
  `agent_prompt_dividend_dates_reexport.md` (ignored); `News_Div_Overlap_Addendum.md` (observed
  present; Div∩News recomputed directly WITHOUT it — consistency note only, zero values sourced).
- **Week labels:** Sunday (price files) vs Monday (panel grid); Monday = Sunday + 1 day [METH §2].
  Both carried for every window (see §1.2, §2.6).
- **Joint sanction glossary (both parts, QA2-aligned):** tested-23 (spike-tested anchors) /
  step-24 (permanent-step companies = 23 + AFLT) / reaction-34-PROVISIONAL (5-week windows,
  170 cells cited) / secondary (later/sectoral/effective/affiliate/relief — never the step).

---
## §1 Crisis (Part I — Builder A §1)(t) — market-wide stress windows

### 1.1 Construction rule (as specified in sources)

A company "spikes" in week *t* iff `RV_ratio(t) ≥ 2 AND Vol_ratio(t) ≥ 2` vs its own trailing
52-week median (minimum 30 valid observations) [SANC §3.2–§3.3; METH §3(a)]. A week is a "stress
week" iff ≥25% of eligible companies spike. A window is confirmed iff contiguous stress weeks
(one-week bridging allowed) reach a peak spike fraction ≥40%. Weeks with <25 eligible companies
are excluded as artifacts [SANC §3.3]. `RV = (High−Low)/Close` in the SANC engine [SANC §3.2];
note METH §2 (C4) reverts the primary RV to `ln(High/Low)` — the windows below are carried as
confirmed in SANC and are not re-estimated here.

### 1.2 The four confirmed windows (exact dates, both label systems)

| # | Window | Sunday labels [SANC §4.1] | Monday labels (= Sun+1) | Length | Peak week (Sun) | Peak fraction | Peak median RV | Identification [SANC §4.1] |
|---|---|---|---|---|---|---|---|---|
| W1 | COVID-19 crash | 2020-02-23 → 2020-04-12 | 2020-02-24 → 2020-04-13 | 8 wks | 2020-02-23 | 0.773 (58/75) | 3.74 | Pandemic selloff; OPEC+ breakdown 2020-03-06 |
| W2 | Invasion & suspension | 2022-02-20 → 2022-03-27 | 2022-02-21 → 2022-03-28 | 6 labels / 5 trading wks | 2022-02-20 | 0.733 (55/75) | 9.38 | Invasion 2022-02-24; MOEX suspension 2022-02-28–03-24; CBR 20% 2022-02-28 |
| W3 | Ruble/rate-hike crisis | 2023-09-03 → 2023-09-17 | 2023-09-04 → 2023-09-18 | 3 wks | 2023-09-10 | 0.453 (34/75) | 1.93 | RUB>100/USD Aug-2023; CBR 8.5→12% (2023-08-15), 12→13% (2023-09-15); volume-led, mid-caps |
| W4 | 2026 bear capitulation | 2026-06-21 → 2026-07-26 | 2026-06-22 → 2026-07-27 | 6 wks | 2026-06-21 | 0.560 (42/75) | 3.22 | 3-yr low ~2026-07-06; CBR-cut disappointment 06-19; oil weakness; EU 20th pkg Apr-2026, bank bans eff. 05-14, 21st pkg 07-23; 2nd peak 2026-07-19 (0.507) |

- **Erratum log (Round 2, QA1 F-A1):** the Round-1 draft of this table printed "2020-02-21"
  in the W2 Monday cell; corrected to 2022-02-21 above. Monday weeks used in computation are
  2022-02-21, 02-28, 03-07, 03-14, 03-21, 03-28.
- **Monday week-lists (for overlap computation):** W1: 02-24, 03-02, 03-09, 03-16, 03-23, 03-30,
  04-06, 04-13 (2020). W2: 02-21, 02-28, 03-07, 03-14, 03-21, 03-28 (2022). W3: 09-04, 09-11,
  09-18 (2023). W4: 06-22, 06-29, 07-06, 07-13, 07-20, 07-27 (2026). Total 8+6+3+6 = **23
  Monday-weeks**; 23 × 75 = **1,725 company-weeks (5.49% of panel)** [NEWS §3: 1,725 / 5.49% ✓
  recomputed: 1725/31425 = 5.489%].
- Cross-source agreement: NEWS §3 gives Sunday spans identical to SANC §4.1
  (2020-02-23→04-12; 2022-02-20→03-27; 2023-09-03→09-17; 2026-06-21→07-26) [NEWS §3 ✓].

### 1.3 Verification method and evidence (carried, not re-estimated)

- Independent from-scratch engine (`engine.py`, `crisis_detect.py`, `verify_sanctions.py`,
  `events.py`), lookahead-safe (baseline strictly pre-event; 0 violations in 182 tests:
  52 sanctions + 130 crisis-sample) [SANC §0, §3.2, §8.2].
- ≥10-company recomputed samples per window at peak week: W1 11/15 crossed both 2× (e.g. AFLT
  6.41/5.67; misses SBER/MTSS/NKNC crossed one week later); W2 13/15 (SBER 28.29/13.64 −47.6%,
  VTBR 19.54/5.31 −48.8%, ROSN 21.51/6.74, YNDX 17.31/10.10); W3 5/15 in sample but 34/75
  panel-wide (stress in second-tier names: NMTP 3.32/6.25, MRKC 3.51/7.32; mega-caps quiet —
  honestly characterized as panel-level confirmation); W4 9/15 (AVAN 4.45, MGNT 3.52/2.79,
  MFGS 3.50/2.39, SBER 3.34/2.91) [SANC §4.1].
- **Seven rejected candidates, both directions verified** [SANC §4.2]: pre-invasion 2022-01-16
  (0.36 — strongest rejection, borderline-flagged); Omicron 2021-11-21 (0.253); mobilization
  2022-09-18 (0.32, RV-only — volume failed); artifact 2024-06-16 (3 eligible, excluded);
  Prigozhin 2023-06-18 (<0.25); Kursk 2024-08-04 (<0.25); Ryabkov 2025-10-11 (<0.25, retraced
  +5–11% within week).

### 1.4 Crisis data gaps

None for the window list itself: all four spans, peaks, fractions and identification are explicit
in both permitted sources. Company-week enumeration (which weeks for which company) is trivially
complete because Crisis(t) is market-wide and identical across companies [METH §3(a)].

---

## §2 Sanction (Part I — Builder A §2)(i,t) — company-specific PERMANENT step

### 2.1 Construction rule

- **Anchor event** = first entity-level designation (SDN blocking / asset freeze / full
  transaction ban) of the company, its direct parent, or (for holdcos) its operating subsidiary —
  whichever is economically binding [SANC §3.4].
- **Confirmation test:** RV_ratio ≥ 2 AND Vol_ratio ≥ 2 at the event week vs trailing 52-week
  median (min 30 obs; one flagged exception: RUAL ~21 weeks, baseline relaxed to ≥15) [SANC §3.2].
  Single-crossed-ratio cases are reported as borderline/partial, never reclassified [SANC §7.5].
- **The dummy is a PERMANENT step:** Sanction(i,t) = 0 before the anchor week, 1 from the anchor
  week onward, forever [SANC §12 cond. 2: "Sanction dummy = 1 from the anchor week onward per
  company"; METH §3(b)]. Confirmation is NOT required for inclusion: only 7 of 23 anchors show a
  confirmed reaction — retained as a documented finding [SANC §6.5; METH §3(b)].
- **Designation dates** (not GL-expiry/effective dates) govern [SANC §5.1].

### 2.2 The 23 tested anchor events (date, instrument, source, verification result)

| # | Company | Anchor date | Instrument / jurisdiction | Primary source [SANC §5.2/§13] | Event bar (Sun) | RV ratio | Vol ratio | Week return | Verdict |
|---|---|---|---|---|---|---|---|---|---|
| 1 | VTBR | 2022-02-24 | US SDN (EO 14024) + UK freeze | OFAC Feb-24-2022 action (FAQ 974); UK FCDO | 2022-02-20 | 19.54 | 5.31 | −48.8% | CONFIRMED |
| 2 | SBER | 2022-04-06 | US SDN full blocking + UK freeze (coordinated) | Treasury jy0705; UK action | 2022-04-03 (d) | 4.35 | 1.94 | −7.0% | BORDERLINE (vol 1.94) |
| 3 | CBOM | 2022-04-06 | UK asset freeze | UK 2022-04-06 action (Bloomberg/FCDO) | 2022-04-03 (d) | 2.12 | 0.73 | −6.2% | NOT confirmed |
| 4 | ALRS | 2022-04-07 | US SDN | OFAC action Apr-7-2022 | 2022-04-03 (d) | 2.76 | 0.67 | −12.2% | NOT confirmed |
| 5 | CHMF | 2022-06-02 | US SDN (+Mordashov/Severgroup) | OFAC/State Jun-2-2022 | 2022-05-29 | 7.60 | 2.55 | −31.3% | CONFIRMED |
| 6 | KMAZ | 2022-06-28 | US SDN defense (+subs, CEO) | Treasury jy0838 | 2022-06-26 (d) | 1.61 | 0.69 | −5.2% | NOT confirmed |
| 7 | IRKT | 2022-06-28 | US SDN on parent UAC (Irkut via 50% rule) | Treasury jy0838 | 2022-06-26 | 5.81 | 22.42 | +56.4% | CONFIRMED — CONFOUNDED (short squeeze; exclude/flag in event studies) |
| 8 | MAGN | 2022-08-02 | US SDN (+Rashnikov) | Treasury jy0905 | 2022-07-31 | 2.82 | 3.14 | −11.4% | CONFIRMED |
| 9 | NMTP | 2022-02-25 | EU sectoral listing (Reg 2022/328); freeze later 2025-02-24 | EU Reg 2022/328; 16th pkg | 2022-02-20 (d) | 14.98 | 1.83 | −33.0% | BORDERLINE (vol 1.83; invasion-week confound) |
| 10 | BSPB | 2023-02-24 | US SDN + UK freeze (financial sector) | Treasury jy1296; UK coordinated | 2023-02-19 (d) | 1.00 | 1.70 | −4.1% | NOT confirmed |
| 11 | USBN | 2023-02-24 | US SDN + UK freeze | Treasury jy1296; UK coordinated | 2023-02-19 (d) | 1.32 | 2.56 | +4.3% | NOT confirmed (partial: vol only) |
| 12 | MTSS | 2023-02-24 | US SDN + UK freeze on SUBSIDIARY MTS Bank (parent not designated) | Treasury jy1296; UK coordinated | 2023-02-19 (d) | 0.70 | 0.52 | +0.7% | NOT confirmed |
| 13 | TRMK | 2023-05-18 | UK freeze first (OFSI Notice 19/05/2023); US SDN later 2024-02-23 | OFSI Notice 19/05/2023 (primary); OFAC 20240223 | 2023-05-14 (d) | 0.95 | 2.03 | +3.1% | NOT confirmed (partial: vol only; US step 1.81/1.25 also unconfirmed) |
| 14 | FESH | 2023-05-18 | UK freeze (OFSI Notice 19/05/2023); EU freeze later 2025-10-23 | OFSI Notice 19/05/2023 (primary); EU 19th pkg | 2023-05-14 (d) | 0.55 | 1.51 | +1.4% | NOT confirmed |
| 15 | PLZL | 2023-05-19 | US SDN (gold; GL 66 to 2023-08-17) | OFAC May-19-2023 | 2023-05-14 (d) | 0.97 | 0.88 | −2.7% | NOT confirmed |
| 16 | SIBN | 2025-01-10 | US + UK SDN (EO 14024+13662; GL 117) | Treasury Jan-10-2025 (coordinated) | 2025-01-05 | 2.60 | 3.16 | −4.6% | CONFIRMED |
| 17 | SNGS | 2025-01-10 | US + UK SDN | Treasury Jan-10-2025 | 2025-01-05 (d) | 1.63 | 1.03 | −4.7% | NOT confirmed |
| 18 | ROSN | 2025-10-22 (US; UK step 2025-10-15 — see §2.4 D1) | US SDN energy (+34 subs; GLs to 11-21-2025); sectoral since 2022 | Treasury sb0290 | 2025-10-19 (d) | 1.64 | 1.19 | −6.8% | NOT confirmed |
| 19 | LKOH | 2025-10-22 (US; UK step 2025-10-15 — see §2.4 D1) | US SDN energy (+34 subs) | Treasury sb0290 | 2025-10-19 | 3.28 | 2.28 | −10.7% | CONFIRMED (UK step alone 1.93/1.19 unconfirmed) |
| 20 | BANE | 2026-04-22 | EU asset freeze (20th pkg) | EU 20th pkg Reg (2026-04-22) | 2026-04-19 (d) | 0.50 | 0.84 | +1.0% | NOT confirmed |
| 21 | MFGS | 2026-04-22 | EU freeze on PARENT Slavneft | EU 20th pkg | 2026-04-19 | 2.72 | 11.80 | +0.7% | CONFIRMED (volume-led; pre-adoption week already elevated) |
| 22 | JNOS | 2026-04-22 | EU freeze on PARENT Slavneft | EU 20th pkg | 2026-04-19 (d) | 1.99 | 33.38 | +3.3% | BORDERLINE (RV 1.99, misses by 0.01; move mostly pre-adoption week) |
| 23 | AVAN | 2026-04-22 | EU transaction ban, 20th pkg (effective 2026-05-14) | EU 20th pkg; 20-bank list | 2026-04-19 (d) | 0.62 | 1.37 | +0.15% | NOT confirmed |

All ratios/returns/verdicts from [SANC §6.1–§6.2, §10]. Count check: 7 confirmed + 3 borderline
+ 13 not-confirmed = 23 ✓ [SANC §6.5]. Borderline band as reported: one ratio crossed 2×, the
other in [1.83, 1.99] — SBER (4.35/1.94), NMTP (14.98/1.83), JNOS (1.99/33.38) ✓ [SANC §6.5].
Footnote (QA1 F-A2): "(d)" = Builder-A-derived Sunday label (Sunday starting the Mon–Fri week
containing the anchor date); unmarked bars are carried from [SANC §6.1]. Anchor DATES in this
table are all carried; only the (d)-marked bar labels are derived.

### 2.3 Other verified sanctions events (not among the 23 anchors)

- **Sanctions RELIEF (positive shock):** RUAL removed from SDN **2019-01-27** (with EN+/ESE):
  RV 3.32 / Vol 18.36, −9.4% ("sell-the-news"); ~21-week baseline caveat; CONFIRMED as an
  information shock. Analytical warning carried: the 2× test detects information shocks, not
  damage direction [SANC §6.1].
- **AFLT — verified but untested designation:** UK asset freeze **2022-05-19** (aviation; UK
  flight ban since 2022-02-24). Verified in Check 2 [SANC §5.2] but deliberately NOT subjected
  to the anchor spike test (week sits in post-invasion desensitized baseline; invasion-week
  reaction captured under W2) [SANC §10 AFLT row].
**Compilation decision D-AFLT:** AFLT receives Sanction(i,t) = 1 from 2022-05-19 onward
(verified binding entity-level designation per the §3.4 anchor definition; SANC §12 cond. 2 keys
the step to the anchor week per company). METH §3(b) states verbatim: "23 primary anchors from
OFAC/OFSI/EU primary lists, verified via the 2× test over the 4 weeks following the anchor."
Builder-A inference (labeled, QA1 F-A3): the "23" counts the spike-tested anchors of [SANC §6]
(AFLT was verified but deliberately not spike-tested [SANC §10]); nothing in METH §3(b) prohibits
a verified 24th designation from entering the step. Flagged as a judgment call; downstream may
restrict to the 23 tested anchors (sensitivity: one company).
- **TATN — affiliate-only:** EU oil annex 2022-10-06 (sectoral) + affiliate Tatneft-Samara EU
  freeze **2026-07-23** (21st pkg): RV 3.59 / Vol 2.98, **+25.0%**, CONFIRMED (mid-W4 squeeze)
  [SANC §6.3]. **Decision D-TATN:** TATN itself was never entity-designated → permanent step
  stays 0; the affiliate event is disclosed as a secondary event (it enters the reaction-window
  overlap accounting in §4.4, not the step).
- **Sectoral-only (no entity freeze → step stays 0):** GAZP (EU oil annex 2022-10-06; US Dir-3),
  NVTK (oil annex; Arctic LNG 2 subsidiary US-SDN 2023-09-14 — subsidiary SPV, report categorizes
  NVTK as sectoral-only, carried as such), RTKM (US Dir-3 2022-02-24), HYDR (US Dir-3),
  GMKN (US/UK nickel/copper import bans Apr-2024; Potanin personal UK listing only) [SANC §5.2,
  §6.4, §10].
- **Parent/owner-level only, no anchor test (step stays 0):** RASP (parent Evraz plc EU/UK),
  UNAC (parent Rostec UK 2022-02-24), RNFT (owner Gutseriev listed; company not) [SANC §6.4, §10].
  (Contrast: IRKT/MFGS/JNOS ARE anchors via parent designations and DO enter the step — the
  report's distinction, carried exactly.)
- **Secondary designation steps** (later designations of already-anchored names; tested, generally
  unconfirmed — incremental restrictions carry little new information) [SANC §6.3]: TRMK US SDN
  2024-02-23 (1.81/1.25); EU SWIFT VTBR (tested at reopening bar 2022-03-20: 5.40/0.68, flagged);
  8th-package oil annex 2022-10-06 and cap effectiveness 2022-12-05 (7 oil companies × 2 dates,
  no confirmations, RV 0.2–1.6 — economically correct, not a data failure); BSPB EU ban
  2025-07-18 (eff. 08-09); FESH/PLZL EU freezes + ROSN/SIBN EU transaction bans 2025-10-23
  (19th pkg); NMTP EU freeze 2025-02-24 (16th pkg).
- **Negative verification:** 47 of 75 companies confirmed NOT entity-designated by US/EU/UK
  (best-documented: GMKN, NLMK, IRAO — CJEU C-147/25 ruled 2026-09-03 — GAZP, TATN, RTKM)
  [SANC §6.4]. Caveat carried: "no designation found" reflects exhaustive package review, not a
  machine diff against consolidated lists (bulk downloads blocked, §5.3) [SANC §5.3–§5.4].

### 2.4 The permanent step: first-designation week per company

Sanction(i,t) = 1 from the anchor week onward [SANC §12 cond. 2; METH §3(b)]. Anchor weeks:

| Company | First designation (entity-level or binding parent/subsidiary) | Anchor week (Mon) | Step weeks in panel |
|---|---|---|---|
| VTBR | 2022-02-24 (US+UK) | 2022-02-21 | all weeks ≥ 2022-02-21 |
| NMTP | 2022-02-25 (EU sectoral-first; freeze 2025-02-24) — note D-NMTP | 2022-02-21 | ≥ 2022-02-21 |
| SBER | 2022-04-06 (US+UK) | 2022-04-04 | ≥ 2022-04-04 |
| CBOM | 2022-04-06 (UK) | 2022-04-04 | ≥ 2022-04-04 |
| ALRS | 2022-04-07 (US) | 2022-04-04 | ≥ 2022-04-04 |
| AFLT | 2022-05-19 (UK) — decision D-AFLT | 2022-05-16 | ≥ 2022-05-16 |
| CHMF | 2022-06-02 (US) | 2022-05-30 | ≥ 2022-05-30 |
| KMAZ | 2022-06-28 (US) | 2022-06-27 | ≥ 2022-06-27 |
| IRKT | 2022-06-28 (US, parent UAC) | 2022-06-27 | ≥ 2022-06-27 |
| MAGN | 2022-08-02 (US) | 2022-08-01 | ≥ 2022-08-01 |
| BSPB | 2023-02-24 (US+UK) | 2023-02-20 | ≥ 2023-02-20 |
| USBN | 2023-02-24 (US+UK) | 2023-02-20 | ≥ 2023-02-20 |
| MTSS | 2023-02-24 (US+UK, subsidiary MTS Bank) | 2023-02-20 | ≥ 2023-02-20 |
| TRMK | 2023-05-18 (UK first; US 2024-02-23 later) | 2023-05-15 | ≥ 2023-05-15 |
| FESH | 2023-05-18 (UK first; EU 2025-10-23 later) | 2023-05-15 | ≥ 2023-05-15 |
| PLZL | 2023-05-19 (US first; EU 2025-10-23 later) | 2023-05-15 | ≥ 2023-05-15 |
| SIBN | 2025-01-10 (US+UK) | 2025-01-06 | ≥ 2025-01-06 |
| SNGS | 2025-01-10 (US+UK) | 2025-01-06 | ≥ 2025-01-06 |
| ROSN | 2025-10-15 (UK first; US 2025-10-22 tested) — note D1 | 2025-10-13 | ≥ 2025-10-13 |
| LKOH | 2025-10-15 (UK first; US 2025-10-22 tested) — note D1 | 2025-10-13 | ≥ 2025-10-13 |
| BANE | 2026-04-22 (EU) | 2026-04-20 | ≥ 2026-04-20 |
| MFGS | 2026-04-22 (EU, parent Slavneft) | 2026-04-20 | ≥ 2026-04-20 |
| JNOS | 2026-04-22 (EU, parent Slavneft) | 2026-04-20 | ≥ 2026-04-20 |
| AVAN | 2026-04-22 (EU; ban effective 2026-05-14) | 2026-04-20 | ≥ 2026-04-20 |

- **D1 (ROSN/LKOH first-date):** per the §3.4 "first entity-level designation" rule, the step runs
  from the UK freeze (2025-10-15) although spike-testing used the US date (2025-10-22)
  [SANC §5.2 UK list: ROSN, LKOH 2025-10-15; §6.1 LKOH row]. Both dates are disclosed; the two
  candidate start weeks differ (Mon 10-13 vs 10-20) but neither falls in a crisis window, so the
  §4.4 overlap counts are unaffected by this choice.
- **D-NMTP:** NMTP's tested anchor is sectoral-first (EU Reg 2022/328); the entity freeze came
  2025-02-24. The step is keyed to the tested anchor (2022-02-25) following the report; the
  sectoral-first character is disclosed.
- Step companies: **24** (23 tested anchors + AFLT per D-AFLT). Strict-23 sensitivity: drop AFLT.
- Company-count reconciliation (stated "28 of 75 with ≥1 verified sanctions event" [SANC §0]): 23
  anchor companies + AFLT + TATN (affiliate/secondary tests) + RUAL (relief test) + NVTK + GAZP
  (8th-package sectoral tests) = 28 ✓ (reconciliation derived from [SANC §6.3, §10]).

### 2.5 The "34 anchors × 5-week" reaction window (carried concept; provisional reconciliation)

NEWS §3 and the dividend layer cite "34 confirmed anchors (Group-1 §10) × 5-week reaction window
= 170 company-weeks (0.541%)" [NEWS §3]. The row-level 34-list is not explicit in the permitted
sources. Candidate reconciliation (PROVISIONAL — labeled per METH §8, not asserted as Group-1's
list): 23 tested anchors (§2.2) + 11 named later-step designations: TRMK-US 2024-02-23, PLZL-EU
2025-10-23, FESH-EU 2025-10-23, NMTP-EU-freeze 2025-02-24, BSPB-EU-ban 2025-07-18, ROSN-EU-ban
2025-10-23, SIBN-EU-ban 2025-10-23, ROSN-UK 2025-10-15, LKOH-UK 2025-10-15, AFLT-UK 2022-05-19,
TATN-affiliate 2026-07-23 (all named in [SANC §5.2/§6.3/§10]) = 34 ✓ arithmetically. The 170-week
aggregate is carried as cited; downstream use of company-level reaction weeks beyond the anchor
weeks in §2.4 requires Group-1's exact 34-row list (absent from permitted sources → §8 flag).

### 2.6 Joint sanction vocabulary + consolidated week table (Round 2 — shared with Builder B)

**JOINT GLOSSARY [QA2 F-Q2-AB1] — both builders use these terms identically:**
- **tested-23:** the 23 spike-tested anchor events (§2.2 rows 1–23) [SANC §6.1–§6.2].
- **step-24:** the 24 permanent-step companies (§2.4) = tested-23 companies + AFLT (D-AFLT).
- **reaction-34 (PROVISIONAL):** the 34-event × 5-week reaction-window concept cited at 170
  company-weeks [NEWS §3]; row-level list = the §2.5 candidate (tested-23 + 11 named later
  steps), provisional pending Group-1’s exact list.
- **secondary:** later steps, sectoral instruments, effective dates, affiliate/relief events
  (§2.3) — enter reaction-window overlap accounting where cited, never the permanent step.

**Consolidated week table** (anchor DATES carried; all Mon/Sun labels Builder-A weekday
derivations: Mon = Monday of the week containing the date, Sun = Mon − 1):

Step companies (tested-23 + AFLT): VTBR 2022-02-24 → Mon 02-21/Sun 02-20 (2022);
NMTP 2022-02-25 → Mon 02-21/Sun 02-20; SBER 2022-04-06 → Mon 04-04/Sun 04-03;
CBOM 2022-04-06 → Mon 04-04/Sun 04-03; ALRS 2022-04-07 → Mon 04-04/Sun 04-03;
AFLT 2022-05-19 → Mon 05-16/Sun 05-15; CHMF 2022-06-02 → Mon 05-30/Sun 05-29;
KMAZ 2022-06-28 → Mon 06-27/Sun 06-26; IRKT 2022-06-28 → Mon 06-27/Sun 06-26;
MAGN 2022-08-02 → Mon 08-01/Sun 07-31; BSPB 2023-02-24 → Mon 02-20/Sun 02-19;
USBN 2023-02-24 → Mon 02-20/Sun 02-19; MTSS 2023-02-24 → Mon 02-20/Sun 02-19;
TRMK 2023-05-18 → Mon 05-15/Sun 05-14; FESH 2023-05-18 → Mon 05-15/Sun 05-14;
PLZL 2023-05-19 → Mon 05-15/Sun 05-14; SIBN 2025-01-10 → Mon 01-06/Sun 01-05;
SNGS 2025-01-10 → Mon 01-06/Sun 01-05; ROSN 2025-10-15 (UK first; US 10-22 tested) → Mon
10-13/Sun 10-12; LKOH 2025-10-15 (UK first; US 10-22 tested) → Mon 10-13/Sun 10-12;
BANE 2026-04-22 → Mon 04-20/Sun 04-19; MFGS 2026-04-22 → Mon 04-20/Sun 04-19;
JNOS 2026-04-22 → Mon 04-20/Sun 04-19; AVAN 2026-04-22 → Mon 04-20/Sun 04-19.

Secondary / effective dates: TATN-affiliate 2026-07-23 → Mon 07-20/Sun 07-19;
VTB SWIFT effective 2022-03-12 → Mon 03-07/Sun 03-06 (inside W2 span — effective date, not an
anchor); SBER/CBOM SWIFT effective 2022-06-14 → Mon 06-13/Sun 06-12; EU 8th-pkg oil annex
2022-10-06 → Mon 10-03/Sun 10-02; price-cap effective 2022-12-05 → Mon 12-05/Sun 12-04;
TRMK-US 2024-02-23 → Mon 02-19/Sun 02-18; NMTP-EU-freeze 2025-02-24 → Mon 02-24/Sun 02-23;
BSPB-EU-ban 2025-07-18 → Mon 07-14/Sun 07-13; 19th-pkg actions 2025-10-23 (PLZL/FESH freezes,
ROSN/SIBN bans) → Mon 10-20/Sun 10-19. (RUAL relief 2019-01-27 excluded: relief, not a
designation; no sanction week attaches.)

### 2.7 SANC-side sector grouping note (for Builder B’s YAKG dispute record)

[QA2 F-Q2-AB2:] the sanctions source’s per-company summary groups YAKG under Utilities — row
"FEES, HYDR*, IRAO, LSNG, MRKC/K/KP/S/U, MSNG, MSRS, OGKB, TGKA, UPRO, YAKG | Utilities"
[SANC §10]. Builder A takes no position on the correct assignment (the E3 frozen mapping
governs; raw folder names are a forbidden source for this compilation) — this line exists solely
to complete the dispute record owned by Builder B.

---

## §3 News (Part I — Builder A §3)(i,t) — company-specific TRANSIENT shocks

### 3.1 Definition and rule (exact)

`News(i,t) ∈ {0,1}`: discrete-event dummy, 1 for the event week of a confirmed company-specific
exceptional event and following weeks while the company's RV or volume stays ≥1.5× its frozen
trailing 52-week median, hard cap 4 weeks [NEWS §1]. Baselines use strictly pre-event data;
CONFIRMED iff RV or volume ≥2× in weeks t…t+3 (a fully missing week counts as confirming —
prescribed but inert: fires for 0 of 31 events) [NEWS §1, §4.4]. Duration rule: extend while
≥1.5×, stop at first week below, cap 4; longer elevation flagged, never attributed [NEWS §1].
**Mandatory dedup BEFORE testing:** discard any candidate within ±2 weeks of a confirmed Crisis
window or that company's confirmed sanction date [NEWS §2–§3; METH §3(c)].
**Role** [METH §3(c)]: News enters no headline equation (M1–M6, §§5–6); functions are
spike-attribution accounting, sensitivity checks 1–2 inputs, and the dedup guarantee; estimation
only via robustness r2.

### 3.2 Final state (corrected series)

**31 confirmed events · 23 companies · 89 company-weeks News=1 (0.283% of panel) · 52 zero-event
companies** [NEWS §1]. Categories: MA 10, CAPRET 8, EARN 5, LEGAL 4, OPS 2, LEAD 1, DEBT 1
(sum 31 ✓) [NEWS §1]. Company-weeks claimed by two events: **0** [NEWS §1]. Revision history
carried: 26 → 31 events (+5: MGTS-2R, KMAZ-1R, SFIN-3R, MTSS-1R re-admitted missed qualifiers +
GMKN-3R bar-inconsistency repair), 69 → 89 cells (+20); pre-revision numbers retained wherever
computed [NEWS §1, §4.3].

### 3.3 Per-company News=1 weeks (complete, 23 companies)

| Company | News=1 weeks | Company | News=1 weeks | Company | News=1 weeks |
|---|---|---|---|---|---|
| ABRD | 4 | GAZP | 8 | MTSS | 4 |
| AFLT | 4 | GMKN | 9 | MVID | 8 |
| BANE | 1 | KMAZ | 4 | NKNC | 2 |
| CBOM | 2 | KZOS | 3 | PLZL | 2 |
| FEES | 1 | LKOH | 2 | SFIN | 4 |
| FESH | 9 | MGNT | 1 | TRMK | 4 |
| — | — | MGTS | 4 | UPRO | 4 |
| UTAR | 4 | VTBR | 1 | YNDX | 4 |

[NEWS §1] Sum recomputed: 89 ✓ (4+4+1+2+1+9+8+9+4+3+2+1+4+4+8+2+2+4+4+4+4+1+4 = 89).
Zero-event companies: the other 52 of 75 (correct statement: "no *verified* discrete event was
found" [NEWS §5]).

### 3.4 Named events and boundary cases (all date-bearing statements in NEWS)

- **GMKN-3R:** dividend-*policy* shock, news onset **2021-03-24** (board date 2021-03-29; either
  passes; choice moves one week) [NEWS §4.3c]. Event week (Mon): 2021-03-22.
- **MTSS-1R:** payout resumption after suspension; boundary case (habitual payer vs resumption),
  included on suspension-and-reaction tie-breaker [NEWS §4.4b]. Date not stated in NEWS.
- **VTBR-1:** payout-policy event establishing the CAPRET class reading [NEWS §4.3]. Date not
  stated. CAPRET bar: payout-*policy* shocks IN, mechanical ex-date gaps OUT (costs SFI's −48.2%
  ex-date week 2025-12-22 and all ex-date drops: VTBR −26.8%, SBER −10.1%, MTSS −12.2%,
  TTLK −20.0%) [NEWS §4.4a].
- **MGTS-2R, KMAZ-1R, SFIN-3R:** missed qualifiers re-admitted [NEWS §4.3]. Dates not stated.
- **Discarded at dedup (11):** 10 by confirmed sanction anchor (in `work/g4_candidates.csv` —
  file absent from permitted sources, candidate identities not stated in NEWS) + **PHOR-1,
  2026-07-22, inside W4** (in `work/g4_dedup_log.csv`) [NEWS §3]. Each retained as "found, already
  covered — not double-counted" [NEWS §3–§4.2].
- **Famous failure:** NLMK 2024-02-24 drone attack — 1.22/0.57/0.65/1.23, recorded and EXCLUDED
  [NEWS §4.1].
- **Persistence flags:** 21 of 31 events carry the flag (≥1.5× in ≥3 weeks after the cap:
  recorded, flagged, never attributed) [NEWS §5].
- **Excluded as not company-specific** (not dedup): 2022-09-19 mobilisation, 2026-03 third-tier
  oil rally [NEWS §4.4d].

### 3.5 Spike-attribution accounting (the News justification numbers)

- Universe: 8 largest single-week RV spikes per company × 74 companies = **592 spikes** (ROLO
  excluded: tick-quantisation artifact) [NEWS §2].
- **Padded** (crisis windows as used for dedup; sanction anchor ±2 wks): explained **368/592 =
  62.2%**, unexplained 224/592 = 37.8%. Breakdown: Crisis alone 336 (56.8%), **Crisis+Sanction 11
  (1.9%)**, Sanction alone 4 (0.7%), **News alone 17 (2.9%)** [NEWS §2]. Marginal reach: Crisis
  347, Sanction 15, News 17; every News spike is missed by both Crisis and Sanction [NEWS §2].
- **Strict** (crisis exactly as dated; sanction = anchor week + 0…4): explained 301/592 = 50.8%,
  unexplained 291/592 = 49.2% [NEWS §2].
- **≥2×-week universe** (4,041 weeks): News 45 (1.1%), Crisis 1,284 (31.8%), Sanction 53 (1.3%);
  together **33.6%**, **66.4% unexplained** [NEWS §2]. **Derived check (Builder A's arithmetic,
  labeled):** 45+1284+53 = 1382 (34.20%); stated joint 33.6% × 4041 ≈ 1358; implied double-covered
  weeks ≈ 24. Since News∩Crisis = News∩Sanction = 0 [NEWS §2–§3], the ~24 must be Crisis∩Sanction
  reaction-window overlap — independent corroboration of the §4.4 finding. Most of the ≥2×
  universe sits in the 2×–3× band (2,415 weeks, 75.4% unexplained): illiquid mid-cap noise, not
  hidden news [NEWS §2].
- Revision sensitivity: pre-revision series would give 61.7% padded (5 revision events move 3
  spikes) [NEWS §2; METH §7.1 target 62.2%→61.7% ✓ consistent].

### 3.6 News data gaps (METH §8 disclosure)

The full per-event table (`work/g4_events_final.csv`: 31 events with dates/durations) and the
dedup/candidate logs are **absent from the permitted sources** — NEWS carries only the aggregates
above, the per-company counts, and the named events in §3.4. **Consequence:** per-event News
week-lists are UNRECOVERABLE here; the §4.2–§4.3 zero-overlap verifications rest on (a) the
dedup mechanism + discard counts as audited, (b) exact checks of the two date-bearing cases
(PHOR-1, GMKN-3R), and (c) per-company consistency — NOT on full week-by-week recomputation.
Any downstream use requiring event-level News weeks needs `News_i_t.csv` (absent) and is flagged
PROVISIONAL until recovered [METH §8].

---

## §4 Div (Part II — Builder B §1)(i,t) — dividend pre-record window dummy

### 1.1 Panel and definition (exact)

- Panel: 75 companies × 419 Monday weeks (2018-08-27 → 2026-08-31), 31,425 cells [DIV header].
- Rule: for each confirmed record date R of company i, W_R = Monday of the week containing R;
  `Div(i,t)=1` for W_R−28d, W_R−21d, W_R−14d, W_R−7d (four full weeks preceding the record week);
  the record week itself is EXCLUDED (ex-dividend gap under T+2/T+1) and shipped separately as
  `RecordWeek(i,t)`; overlapping same-company windows are UNIONED (1/0, never 2) [DIV §2].
- Date type: every date is a RECORD date (реестр); no ex-date substitution; three last-trading-day
  contaminations resolved (GCHE 2023-10-01, PHOR 2024-09-22, VSMO 2023-06-05); 38 weekend record
  dates (legal; e.g. AKRN 2024-05-19, ALRS 2018-07-14/2021-07-04/2024-10-19, AVAN 2018-07-08/
  2021-12-12, GCHE 2019-04-07/2021-10-03) [DIV §1].
- Confirmed-paid standard (identical both eras): meeting approval + record date passed + ≥2
  independent listings as closed + no reversal/cancellation [DIV §1].
- Lag disclosure carried [METH §4 C8]: Div(i,t−1) = 1 in {W_R−21d,…,W_R} — the lag shifts the
  window forward one week and INCLUDES the record week. Estimation-stage consequence; the raw
  construction in this file keeps the record week excluded.

### 1.2 Headline counts (all recomputed from stated components)

- **587 confirmed-paid record dates** = 334 (2018–2021) + 253 (2022-01-01…2026-08-31) [DIV §1] ✓.
  Per year: 2018: 81, 2019: 90, 2020: 81, 2021: 82, 2022: 45, 2023: 57, 2024: 64, 2025: 49,
  2026: 38 → sum 587 ✓ (Builder-B arithmetic).
- **63 paying companies; 12 zero-dividend companies:** JNOS, MFGS, RNFT, VJGZ, BLNG, CHMK, ROLO,
  UKUZ, UNAC, FESH, UTAR, MRKK [DIV §1] (63+12 = 75 ✓).
- Source support: 525 rows × 4 sources (smart-lab, dohod, T-Bank, ЗакрытияРеестров.рф), 54 × 3,
  8 × 2 → 587 ✓. The eight two-source rows (with extra evidence): AVAN 2018-07-08, AVAN
  2019-06-25, RGSS 2021-01-12, RGSS 2024-03-18, RGSS 2026-06-15, USBN 2024-06-04, USBN 2026-07-06,
  VSMO 2022-07-11 [DIV §1].
- Construction: 587 dates → 529 with ≥1 pre-window week in panel (528 all four; 58 pre-panel
  2018 dates truncated, not shifted); naive 2113 window-weeks − 3 merged duplicates =
  **2110 Div=1 company-weeks (6.71%)** [DIV §2] ✓ (2110/31425 = 6.713%).
- **61 companies** with ≥1 Div week in panel (IRKT, MSTT pay only pre-panel mid-2018) [DIV §2]
  (63 − 2 = 61 ✓).
- Amount conventions (documentation only): split-adjusted bases (VTBR ×5000, GMKN/PLZL post-split),
  same-day multi-component summed, RUAL 2022 in USD; open ambiguity AVAN 2025-04-28 (28.50 vs
  21.07 RUB — date not in doubt); VSMO 2024 depositary-sourced [DIV §1].
- Edge effects: pre-2018-09-23 windows truncated [DIV §2]; **YNDX 2026-09-21 pending** (outside
  window, unconfirmable): its window WOULD set YNDX Div=1 for 2026-08-24 + 08-31; shipped as 0
  with a documented mechanical flip-if-confirmed [DIV §1–§2].
- Cross-era check carried: same pipeline/clustering/standard/convention both eras; 16 of 18
  exclusions fall in 2022–2026 (period property); multi-date company-years 85 (early) / 54
  (late) [DIV §4].

### 1.3 All 18 exclusions (explicit, nothing else excluded) [DIV §1]

| Ticker | Announced date | Amount | Category | Reason |
|---|---|---|---|---|
| AVAN | 2018-06-12 | 34.42 | unverifiable-single-source | smart-lab-only row contradicted by AGM calendar (record 08.07.2018, 6.20 RUB); SL data error |
| CHMF | not set (withdrawn) | 109.81 | announced-not-paid | Q4-2021 recommendation withdrawn spring 2022; AGM: no dividend |
| GAZP | 2022-07-20 | 52.53 | announced-not-paid | Board recommended; AGM 30.06.2022 voted AGAINST; never paid |
| MAGN | 2022-04-01 | 3.55 | announced-not-paid | Recommended 25.02.2022; cancelled 24.05.2022; AGM: no FY2021 dividend |
| MGNT | 2025-01-09 | 560 | announced-not-paid | EGM 26.12.2024 no quorum; never paid; FY2024/FY2025 AGMs: no dividend |
| MRKU | 2022-10-14 | 0.0249 | source-error-duplicate | TB-only duplicate of FY2021 dividend (record 28.06.2022); TB error |
| MSNG | 2025-07-08 | 0.226 | announced-not-paid | AGM June-2025 no votes; re-proposed, rejected again EGM 19.02.2026 |
| MSNG | 2026-03-03 | 0.226064 | announced-not-paid | 2nd recommendation; EGM voted >80% against; never paid |
| MSTT | 2019-12-23 | 11.29 | announced-not-paid | EGM 12.12.2019 no quorum; never paid (TB "paid" = TB error) |
| NLMK | 2023-01-11 | 2.6 | announced-not-paid | EGM 31.12.2022 rejected 98% against; never paid |
| NLMK | not set (withdrawn) | 12.18 | announced-not-paid | Q4-2021 recommendation withdrawn 2022; AGM: no 2021 dividend |
| OGKB | 2025-07 (planned) | 0.0598 | announced-not-paid | 1st attempt failed; re-recommended Sep-2025, approved Oct-2025 → PAID record 05.11.2025 (in table) |
| PHOR | 2023-10-11 | 126.0 | announced-not-paid | EGM 30.09.2023 did not adopt; replaced by 291 RUB (record 25.12.2023, paid) |
| PHOR | 2025-07-05 | 201.0 | announced-not-paid | EGM 27.06.2025 did not adopt; never paid |
| PLZL | 2023-06-16 | 42.87 | announced-not-paid | AGM 06.06.2023 no quorum; repeat AGM 07.07.2023: not to pay |
| TGKA | 2022-07-18 | 0.001125 | announced-not-paid | AGM 04–05.07.2022 no votes; never paid |
| TGKA | 2025-07-08 | 0.000828802 | announced-not-paid | AGM 19.06.2025 no votes; never paid |
| YNDX | 2026-09-21 (pending) | n/a | outside-window-pending | After cut-off; unconfirmable; not used |

### 1.4 Per-company table (all 75: confirmed totals, Div weeks, Crisis/Sanction overlaps, flags) [DIV §3]

Ticker | Sector (as printed) | 2018–21 | 2022–26 | Total | Div wks | Crisis-ov | Sanction-ov | Flag
---|---|---|---|---|---|---|---
AVAN | Banking | 13 | 12 | 25 | 92 | 6 (W1,W3) | 1 (2026-04-22) | FLAG
BSPB | Banking | 4 | 8 | 12 | 44 | 2 (W3) | 0 |
CBOM | Banking | 1 | 0 | 1 | 4 | 0 | 0 |
SBER | Banking | 4 | 4 | 8 | 28 | 4 (W4) | 0 | FLAG
USBN | Banking | 0 | 2 | 2 | 8 | 2 (W4) | 0 | FLAG
VTBR | Banking | 4 | 2 | 6 | 20 | 4 (W4) | 0 | FLAG
AKRN | Chemicals | 11 | 5 | 16 | 56 | 9 (W1,W2,W4) | 0 | FLAG
KAZT | Chemicals | 8 | 8 | 16 | 56 | 0 | 0 |
KZOS | Chemicals | 5 | 5 | 10 | 36 | 1 (W4) | 0 |
NKNC | Chemicals | 4 | 5 | 9 | 36 | 1 (W4) | 0 |
PHOR | Chemicals | 17 | 10 | 27 | 96 | 0 | 0 |
ABRD | Consumer&Retail | 4 | 5 | 9 | 32 | 2 (W4) | 0 |
GCHE | Consumer&Retail | 8 | 6 | 14 | 52 | 7 (W1,W3) | 0 | FLAG
MGNT | Consumer&Retail | 8 | 2 | 10 | 36 | 0 | 0 |
MVID | Consumer&Retail | 4 | 0 | 4 | 16 | 0 | 0 |
AFKS | Diversified | 4 | 2 | 6 | 20 | 0 | 0 |
SFIN | Diversified | 2 | 6 | 8 | 28 | 0 | 0 |
BANE | Energy | 3 | 5 | 8 | 28 | 3 (W4) | 0 |
GAZP | Energy | 4 | 1 | 5 | 16 | 0 | 1 (2022-10-06) |
JNOS | Energy | 0 | 0 | 0 | 0 | 0 | 0 |
LKOH | Energy | 8 | 8 | 16 | 60 | 0 | 0 |
MFGS | Energy | 0 | 0 | 0 | 0 | 0 | 0 |
NVTK | Energy | 8 | 9 | 17 | 64 | 4 (W1,W3) | 0 | FLAG
RNFT | Energy | 0 | 0 | 0 | 0 | 0 | 0 |
ROSN | Energy | 7 | 9 | 16 | 60 | 2 (W4) | 0 |
SIBN | Energy | 8 | 9 | 17 | 64 | 2 (W4) | 0 |
SNGS | Energy | 4 | 5 | 9 | 32 | 3 (W4) | 0 |
TATN | Energy | 9 | 14 | 23 | 88 | 5 (W3,W4) | 1 (2022-10-06) | FLAG
VJGZ | Energy | 0 | 0 | 0 | 0 | 0 | 0 |
KMAZ | Industrial | 2 | 2 | 4 | 12 | 0 | 0 |
RGSS | Insurance | 1 | 2 | 3 | 12 | 0 | 0 |
ALRS | Metals&Mining | 7 | 3 | 10 | 36 | 1 (W3) | 0 |
BLNG | Metals&Mining | 0 | 0 | 0 | 0 | 0 | 0 |
CHMF | Metals&Mining | 13 | 3 | 16 | 60 | 0 | 0 |
CHMK | Metals&Mining | 0 | 0 | 0 | 0 | 0 | 0 |
GMKN | Metals&Mining | 8 | 3 | 11 | 40 | 0 | 0 |
MAGN | Metals&Mining | 13 | 3 | 16 | 53 | 0 | 0 |
NLMK | Metals&Mining | 16 | 1 | 17 | 60 | 0 | 0 |
PLZL | Metals&Mining | 8 | 6 | 14 | 52 | 3 (W4) | 0 |
RASP | Metals&Mining | 5 | 1 | 6 | 24 | 0 | 0 |
ROLO | Metals&Mining | 0 | 0 | 0 | 0 | 0 | 0 |
RUAL | Metals&Mining | 0 | 1 | 1 | 4 | 0 | 0 |
SELG | Metals&Mining | 4 | 4 | 8 | 32 | 0 | 0 |
TRMK | Metals&Mining | 5 | 5 | 10 | 36 | 0 | 0 |
UKUZ | Metals&Mining | 0 | 0 | 0 | 0 | 0 | 0 |
VSMO | Metals&Mining | 4 | 3 | 7 | 24 | 0 | 0 |
LSRG | Real Estate | 5 | 4 | 9 | 32 | 4 (W1,W4) | 0 | FLAG
MSTT | Real Estate | 1 | 0 | 1 | 0 | 0 | 0 |
PIKK | Real Estate | 4 | 0 | 4 | 13 | 0 | 0 |
IRKT | Tech | 1 | 0 | 1 | 0 | 0 | 0 |
UNAC | Tech | 0 | 0 | 0 | 0 | 0 | 0 |
YNDX | Tech | 0 | 4 | 4 | 16 | 0 | 0 |
MGTS | Telecom | 2 | 0 | 2 | 4 | 0 | 0 |
MTSS | Telecom | 9 | 5 | 14 | 52 | 2 (W4) | 0 |
RTKM | Telecom | 5 | 5 | 10 | 36 | 4 (W4) | 0 | FLAG
TTLK | Telecom | 4 | 5 | 9 | 32 | 1 (W1) | 0 |
AFLT | Transportation | 2 | 2 | 4 | 12 | 3 (W4) | 0 | FLAG
FESH | Transportation | 0 | 0 | 0 | 0 | 0 | 0 |
NMTP | Transportation | 5 | 5 | 10 | 40 | 3 (W4) | 0 |
UTAR | Transportation | 0 | 0 | 0 | 0 | 0 | 0 |
FEES | Utilities | 5 | 0 | 5 | 16 | 0 | 0 |
HYDR | Utilities | 4 | 2 | 6 | 20 | 0 | 0 |
IRAO | Utilities | 4 | 5 | 9 | 32 | 0 | 0 |
LSNG | Utilities | 4 | 6 | 10 | 36 | 0 | 0 |
MRKC | Utilities | 4 | 6 | 10 | 36 | 1 (W4) | 0 |
MRKK | Utilities | 0 | 0 | 0 | 0 | 0 | 0 |
MRKP | Utilities | 4 | 6 | 10 | 36 | 1 (W4) | 0 |
MRKS | Utilities | 3 | 0 | 3 | 8 | 0 | 0 |
MRKU | Utilities | 4 | 6 | 10 | 36 | 1 (W4) | 0 |
MSNG | Utilities | 4 | 4 | 8 | 28 | 3 (W4) | 0 |
MSRS | Utilities | 5 | 6 | 11 | 40 | 1 (W4) | 0 |
OGKB | Utilities | 4 | 3 | 7 | 24 | 0 | 0 |
TGKA | Utilities | 4 | 0 | 4 | 12 | 0 | 0 |
UPRO | Utilities | 8 | 0 | 8 | 28 | 0 | 0 |
YAKG | Utilities (DISPUTED — see §2.6) | 1 | 0 | 1 | 4 | 0 | 0 |

Column-sum checks (Builder-B arithmetic): Total 587 ✓; Div weeks 2110 ✓ (61 nonzero rows);
Crisis-ov 85 ✓; Sanction-ov 3 ✓; flags 11 ✓ (verified individually in §3.5).
Merge-vs-truncation note (QA1 F-B1): MAGN 53 and PIKK 13 are the only Div totals not divisible
by 4 — no contradiction with the "3 merged duplicates" aggregate [DIV §2]. The 2113 naive sum
already reflects pre-panel truncation (587×4 = 2348; 2348 − 2113 = 235 weeks truncated across
the 58 pre-panel 2018 dates plus partial windows), so MAGN’s 64→53 gap is truncation of
pre-panel 2018 windows (MAGN holds 13 early-era dates), while the residual 3 merges are
consistent with PIKK’s 16→13 (4 tightly-spaced 2018–2021 dates sharing 3 weeks). Values
unchanged; explanation added.

### 1.5 Row-level date list: BLOCKER (METH §8 declaration)

**Checked explicitly as instructed:** `Final dividend table.md` contains ONLY aggregate counts
(per-year, per-era, per-company totals), the 18-row exclusion list, source-support counts, and
the 88 explicitly-listed overlap weeks (§3.1–§3.3 below). **The full 587-row record-date list
(`final_dividend_table.csv` / `Div_window_log.csv`) is NOT in the permitted sources.** Per METH
§3(d) — "Requires the full row-level date list (587 confirmed dates, 63 paying companies target);
a per-company aggregate cannot substitute (an aggregate carries no positional information)" — and
(2,110 week-cells, of which only the 88 overlapping cells are explicitly dated here) is
UNRECOVERABLE from the permitted sources. No dates are approximated from totals. Blocked downstream items: H5 estimation with Div(i,t); the METH §4-C8 Div_l1 record-week inclusion count; exhaustive Div∩News re-derivation (§3.4).
The overlap disclosure (§3) is complete as far as the source’s explicit week-lists go.

---

## §5 SOE (Part II — Builder B §2)(i) — time-invariant state-ownership dummy

### 2.1 Definition and rules (exact) [SOE §2.1]

`SOE = 1` iff the Russian government (federal or sub-federal, direct or via state-controlled
holding) holds **≥25% of VOTING shares** at the most recent reliable disclosure; else 0;
UNDETERMINED where genuinely absent/contradictory (valid output, never guess). Seven rules:
evidence-first; indirect control traced ≥1 layer; golden shares recorded separately (never
trigger); temporary administration ≠ ownership (UPRO, TGKA); sub-federal counts but flagged
REGIONAL (federal-only robustness = one filter); UNDETERMINED available (never needed);
reference year recorded per company (post-2022 disclosure partial). Evidence tiers: A
(official/≥2 concordant) = 72, B (documented opacity: LKOH, SNGS, CBOM) = 3 [SOE §2.1].

### 2.2 Headline result

**35 × SOE=1 · 40 × SOE=0 · 0 × UNDETERMINED** [SOE §2.2]. State level: federal-primary 28 ·
federal+regional dual route 4 (ALRS, BANE, LSNG, MSNG) · regional-primary 3 (TATN, TTLK, UTAR)
[SOE §2.2]. Sector pattern: Utilities 13/14, Transportation 4/4, Energy 8/13 state-heavy;
Chemicals/Consumer/Diversified/Real Estate 0-for-14; Metals&Mining 2/15 (ALRS, VSMO) [SOE §2.2;
sector counts per SOE §1.1].

### 2.3 Full 75-row determination table [SOE App.A] (condensed routes; every classification exact)

\# | Ticker | SOE | Route / controller (state block or private controller) | Ref yr | Tier
---|---|---|---|---|---
1 | AVAN | 0 | K. Minovalov 99.27%+0.72% via Alkor Holding | 2018–19 | A
2 | BSPB | 0 | Saveliev ~24% + Vernye Druzya ~28% + dispersed | 2016–18 | A
3 | CBOM | 0 | Rossium/Sudarikov ("Region") control since Oct-2024; 2025 control change UNDISCLOSED — monitoring | 2024–25 | B (App.A as-printed A; SOE §2.4 correction 5: A→B; CSV authoritative)
4 | SBER | 1 | RF (MinFin) 50%+1 = 52.32% votes | 2020–25 | A
5 | USBN | 0 | L. Kogan 81.81%, Tsvetkov 11.35% | 2019–23 | A
6 | VTBR | 1 | RF/Rosimushchestvo 60.9% common | 2016–24 | A
7 | AKRN | 0 | Kantor structures ~70% | 2022 | A
8 | KAZT | 0 | Gerasimenko family + mgmt vehicles | 2023–25 | A
9 | KZOS | 0 | SIBUR via TAIF ~64%; SINKH 19.87% BELOW threshold | 2021–22 | A
10 | NKNC | 0 | SIBUR via TAIF ~83% | 2021–22 | A
11 | PHOR | 0 | Guryev family 43.7%, Litvinenko 20.6% | 2022–26 | A
12 | ABRD | 0 | Titov family ~90% | 2025 | A
13 | GCHE | 0 | Mikhailov/Babayev family ~82% | 2019–24 | A
14 | MGNT | 0 | Marathon Group 29.75%; VTB 29.1% HELD 2018–21, FULLY EXITED by Jan-2022 (time-varying flag) | 2025 | A*
15 | MVID | 0 | B. Uzhakhov 53.633% (MBO Jul-2024) | 2024–25 | A
16 | AFKS | 0 | V. Evtushenkov 49.2% + F. Evtushenkov 15.2% | 2022–25 | A
17 | SFIN | 0 | S. Gutseriev structures ~85% | 2021 | A
18 | BANE | 1 | Rosneft 57.7%; Bashkortostan 25%+1 (partial sale thru mid-2026); golden share | 2023–26 | A
19 | GAZP | 1 | RF 50.23% (Rosimushch. 38.37 + Rosneftegaz 10.97 + Rosgazifikatsiya 0.89) | 2018–24 | A
20 | JNOS | 1 | via Slavneft (99.7% Rosneft+GPN parity) | 2021 | A
21 | LKOH | 0 | private; Alekperov ~30% (register opaque) | 2021–25 | B
22 | MFGS | 1 | Slavneft 56.424% ← Rosneft+GPN (≈96.9% state-related per verifier) | 2021 | A
23 | NVTK | 0 | Mikhelson 24.76%, Timchenko 23.49%, Total 19.4%; Gazprom only 9.9% below threshold | 2018–26 | A
24 | RNFT | 0 | Gutseriev 37.15% voting; state banks hold PREFS ONLY (Trust 19.23%, VTB 8.48% non-voting) | 2021–24 | A*
25 | ROSN | 1 | Rosneftegaz (100% RF) 40.4% | 2021–23 | A
26 | SIBN | 1 | Gazprom 95.7% (Gazprom = RF 50.23%) | 2024 | A
27 | SNGS | 0 | never disclosed; ~70% closed mgmt structures; no state block ≥25% in any disclosure | 2017–25 | B
28 | TATN | 1 REG | SINKH (100% Tatarstan) 27.23% charter = 29% votes; Republic ≈34% common; golden share | 2023 | A
29 | VJGZ | 1 | via Slavneft-Megionneftegaz ← Slavneft ← Rosneft+GPN | 2021 | A
30 | KMAZ | 1 | Rostec 47.1%; Avtoinvest 23.54% private | 2019–23 | A
31 | RGSS | 1 | VTB Group >99% (via Otkritie Dec-2022); sale announced, NOT completed — FLAG | 2023–26 | A*
32 | ALRS | 1 | RF 33.03% + Yakutia 25%+1 + uluses 8% (dual route) | 2024–26 | A
33 | BLNG | 0 | MMK group >95% ← Rashnikov | 2013 | A
34 | CHMF | 0 | Mordashov 77.03% via Severgroup | 2024–26 | A
35 | CHMK | 0 | via Mechel ← Zyuzin family >50% | 2023 | A
36 | GMKN | 0 | Interros 37%, Rusal 26.4%, Crispian ~4% — all private | 2025–26 | A
37 | MAGN | 0 | Rashnikov 79.76% (OOO Altair since 2022) | 2022–23 | A
38 | NLMK | 0 | Lisin via Fletcher ~79.4% | 2020 | A
39 | PLZL | 0 | Islamic Support Fund 46.35% + Akropol 29.99% | 2022 | A
40 | RASP | 0 | Evraz group 90.9–93.24% ← Abramovich/Abramov/Frolov | 2021–25 | A
41 | ROLO | 0 | Seligdar 85.12% ← Maximus 50.62% (private individuals) | 2024 | A
42 | RUAL | 0 | En+ 56.88% ← Deripaska 44.95% (VTB exited En+ Feb-2020) | 2019–20 | A*
43 | SELG | 0 | OOO Maximus ~50.6% (4 private individuals) | 2024 | A
44 | TRMK | 0 | 90.6% re-registered to TMK top managers Mar-2022; OFAC "state" tag = supplier note — FLAG | 2022–24 | A*
45 | UKUZ | 0 | via Mechel >95% ← Zyuzin family | 2008–23 | A
46 | VSMO | 1 | Rostec (RT-Razvitie biznesa) EXACTLY 25%+1 — KNIFE-EDGE; Shelkov 65.27% operates | 2023–25 | A*
47 | LSRG | 0 | A. Molchanov 55.1% (+E. Molchanov 11%, mgmt 6.9%) | 2024–25 | A
48 | MSTT | 0 | Stroyproektholding (Rotenberg) 96.97%; VEB.RF link only via non-listed Natproektstroy | 2024 | A*
49 | PIKK | 0 | VTB fully exited 2023-08-22; Gordeev 15.15% (2025); register partly opaque — caveat (App.A text pre-squeeze-out; §2.4 correction 4 + §2.5 monitoring list carry the 2026 squeeze-out state) | 2023–25 | A*
50 | IRKT | 1 | via UAC ≥85% ← Rostec 92.31% | 2016–23 | A
51 | UNAC | 1 | Rostec 92.31% (+VEB.RF 5.6%) | 2020–26 | A
52 | YNDX | 0 | ZPIF Konsortsium.Pervyi (mgmt-led private); golden shares (FOI + Fond menedzherov) — NO state equity | 2024 | A*
53 | MGTS | 0 | MTS 94.7% (99.1% common) ← Sistema 42.09% | 2025 | A
54 | MTSS | 0 | AFK Sistema 42.09% (private) | 2022–26 | A
55 | RTKM | 1 | Rosimushchestvo 38.2% common + VTB 8.44% + Telecom Investitsii 20.98% + VEB 3.36% (≈71%) | 2025 | A
56 | TTLK | 1 REG | SINKH (Tatarstan) 87.21% | 2025 | A
57 | AFLT | 1 | RF/Rosimushchestvo 73.77% (post-2022); 2026 sale plan ≠ completed | 2022–26 | A
58 | FESH | 1 | Rosatom 92.5% consolidated; Nov-2025 pledge; Jun-2026 JV 51/49 w/ DP World — state ≥25% either way | 2023–26 | A
59 | NMTP | 1 | Transneft 60.62% + RF 20% direct + RZhD 5.3% | 2018–23 | A
60 | UTAR | 1 REG | KhMAO 38.8% + Tyumen Oblast 8.4% | 2020 | A
61 | FEES | 1 | RF 77.02% placed (88.04% pre-merger; AR2023: 75.28%) | 2023–26 | A
62 | HYDR | 1 | Rosimushchestvo 62.2% (+VTB 12.37%) | 2024 | A
63 | IRAO | 1 | Rosneftegaz 26.37–27.63% + FSK 8.57% (≈35% federal combined) | 2022–24 | A
64 | LSNG | 1 | Rosseti 69.36% + Saint Petersburg 28.80% (dual route) | 2023–26 | A
65 | MRKC | 1 | Rosseti 50.69% | 2023–25 | A
66 | MRKK | 1 | Rosseti 99.76% | 2023 | A
67 | MRKP | 1 | Rosseti 50.41% | 2023–24 | A
68 | MRKS | 1 | Rosseti 57.90% | 2023 | A
69 | MRKU | 1 | Rosseti 55.23% | 2023 | A
70 | MSNG | 1 | GPH 53.5–53.8% (←Gazprom←RF) + Moscow 26.4% | 2019–24 | A
71 | MSRS | 1 | FSK-Rosseti 50.90% (+GPB 9.38%) | 2026 | A
72 | OGKB | 1 | GPH ~73.4% (50.3–77% across sources) | 2019–25 | A
73 | TGKA | 1 | GPH 51.8%; Fortum 29.45% foreign-owned (Decree-302 temp admin covers PAO Fortum, not this block) | 2024–25 | A*
74 | UPRO | 0 | Uniper SE RETAINS TITLE to 83.73%; RF temporary administration (Decree 302, 2023) — NOT ownership | 2023–24 | A*
75 | YAKG | 0 | A. Avdolyan ≈44% direct+indirect (A-TEK 45% / A-Property 20.16% / ZPIF Yunion 25%, post-2023) — private | 2019–26 | A

(A* = special-case note in §2.4.) Count check: SOE=1 rows = 35 ✓ (SBER, VTBR, BANE, GAZP, JNOS,
MFGS, ROSN, SIBN, TATN, VJGZ, KMAZ, RGSS, ALRS, VSMO, IRKT, UNAC, RTKM, TTLK, AFLT, FESH, NMTP,
UTAR, FEES, HYDR, IRAO, LSNG, MRKC, MRKK, MRKP, MRKS, MRKU, MSNG, MSRS, OGKB, TGKA); SOE=0 = 40 ✓;
UNDETERMINED = 0 ✓. Tier B = 3 (LKOH, SNGS, CBOM) ✓.

### 2.4 Special cases (each explained) [SOE §2.3]

Knife-edge VSMO (25%+1 → SOE=1 by letter; private-controlled economically; flip-test candidate);
temp-admin UPRO (SOE=0, no state equity despite state management) / TGKA (SOE=1 via GPH 51.8%
regardless); golden-shares-only YNDX (SOE=0; TATN/BANE golden shares coexist with equity routes);
state-prefs-no-votes RNFT (SOE=0); sub-threshold KZOS 19.87% / NKNC Tatarstan residual / NVTK
Gazprom 9.9% (SOE=0, tagged); time-varying MGNT (state 2018–21 via VTB 29.1%, private after;
panel's only mid-sample change); tier-B opacity LKOH/SNGS/CBOM (CBOM most monitoring-worthy);
announced≠completed RGSS (VTB >99%; sale deadline 2027-04-01) / AFLT (23.76% sale plan; RF keeps
≈50%) / BANE (Bashkortostan partial sale; federal route unaffected) / FESH (Rosatom 92.5%; pledge
≠ sale; DP World JV pending); private chains TRMK (mgmt 90.64%) / MSTT (Rotenberg 96.97%) /
RUAL (En+ 56.88%).

### 2.5 Verification, monitoring, and the C6 reconciliation block

- Independent verification: 22 fresh 2026-09-18 re-checks (≥20 required), all PASS, 0 FAIL;
  +2 chain inheritances +2 official re-confirmations = 27/75 tickers touched; six content
  corrections applied (none changed a determination); 9 malformed CSV rows found and repaired
  (unescaped commas: TGKA/UPRO/MTSS; spurious field pairs: CHMK/RASP/ROLO/RUAL/UKUZ/MGTS/MTSS;
  TGKA column misplacement); post-repair report↔CSV agreement 75/75 [SOE §2.4].
  NOTE: the CSVs themselves (`group2/soe_classifications.csv` etc.) are absent from the permitted
  sources — the verification outcomes above are carried as reported in [SOE §2.4, App.D.1].
- Monitoring list (re-check before locking panel): RGSS (sale → flip to 0), PIKK (squeeze-out,
  record 2026-10-20; flip only if state owner emerges), CBOM (undisclosed 2025 control change),
  BANE (robust either way), AFLT (robust either way) [SOE §2.5].
- **C6 RECONCILIATION — BLOCKED (METH §8 declaration):** METH §3(e) mandates reconciling the
  35/40 appendix table against the 34/41 pre-screen table (`agent2b_soe.csv`) BEFORE H6, Section 6,
  or sensitivities 5–6, naming every differing firm with citations and vintages. `agent2b_soe.csv`
  is NOT among the permitted sources (only the 35/40 appendix side in [SOE App.A] is available).
  Only the screen artifact is checkable here: MRKK is SOE=1 [SOE App.A #66] and screen-excluded,
  consistent with METH §3(e)'s "34-vs-33 is a screen artifact" note. **H6, Section 6, and
  sensitivities 5–6 are therefore BLOCKED until the reconciliation row is published** [METH §3(e)].
- Carried robustness definitions: federal-only = `state_level == "federal"` (drops TATN, TTLK,
  UTAR to 0; keeps 4 dual-route) [SOE §4.1]; VSMO flip-test [SOE §4.1; METH §7.6].

### 2.6 Sector-label cross-check (DIV §3 vs SOE App.A) — ONE dispute found

All 75 tickers compared: **74 agree exactly** (including the historically unstable IRKT/UNAC =
Tech and MSTT = Real Estate in both files). **ONE dispute: YAKG** — DIV §3 prints "Utilities";
SOE App.A #75 prints "Energy". SOE §1.1's own counts (Energy 13 / Utilities 14, summing with all
sectors to 75 with SOE 35/40 ✓) are consistent ONLY with YAKG ∈ Energy (else Energy 12 /
Utilities 15). Per METH §1 (E3/C2) the frozen mapping comes from raw-archive folder names with
folder-governs resolution — folder names are in a FORBIDDEN source for this compilation, so the
dispute CANNOT be resolved here.
**§8 flag:** YAKG sector DISPUTED — Energy side: SOE App.A #75 + SOE §1.1 counts (Energy 13 /
Utilities 14); Utilities side: DIV §3 as-printed row + SANC §10 grouping [via-A §2.7].
Audit-trail counts (QA1 F-B5): as-printed DIV §3 implies Energy 12 / Utilities 15; the SOE side
implies 13/14. Both pairs recorded; neither adopted. No sector-dependent result may treat either
assignment as settled until the E3 frozen table is built from folders; hard-coded sector names
prohibited downstream [METH §1, §5].

---

## §6 Consolidated overlap disclosure

### §6.0 Consolidated table (Agent-5 assembly; every figure copied unchanged from §6.1/§6.2)

| Pair | Type | Value (company-weeks) | Detail | Status |
|---|---|---|---|---|
| Crisis ∩ News | guaranteed-zero (dedup) | **0** | ±2-wk pre-test dedup; PHOR-1 2026-07-22 correctly discarded (in W4); GMKN-3R wk 03-22-2021 correctly retained; 0 dual-claimed weeks | audit-trail-verified (§6.1); exhaustive re-derivation blocked (event table absent) |
| Sanction ∩ News | guaranteed-zero (dedup) | **0** | 10 sanction-anchored discards; GMKN-negative + MTSS-1R/VTBR-1 screen checks pass (bounded) | audit-trail-verified (§6.1); same limit |
| Crisis ∩ Sanction | MEASURED (amendment A1) | **onset: 2 of 23 in windows (+1 secondary); reaction: 10 (+2 sec); permanent-step: 204 (195 strict-23)** | VTBR 2022-02-24 in W2; NMTP 2022-02-25 in W2; TATN-affil 2026-07-23 in W4; corroborated (11 joint spikes; ~24 implied double-covered) | exactly measured; METH §3 guarantee filed as Amendment Candidate A1 (§6.1) |
| Div ∩ Crisis | measured | **85 (4.0% of Div; 4.9% of crisis)** | W1 15 + W2 2 + W3 13 + W4 55; full week-lists in §6.2 | fully enumerated |
| Div ∩ Sanction | measured | **3 (0.14% of Div)** | GAZP 2022-10-03, TATN 2022-10-03 (Mon-10-03-2022 window of 2022-10-06 annex), AVAN 2026-04-20 (Mon-04-20-2026 window of 2026-04-22 pkg); reaction-window concept | fully enumerated |
| Div ∩ News | measured (bounded) | **0 (0.0% of Div; 0.0% of News)** | grades: 2 definitional (13 News wks) + 9 bounded (20 Div wks excluded) + 12 fully-carried; NOT zero-by-dedup (85+3 elsewhere; totals intact) | bounded recomputation (§6.2); exhaustive needs week files |
| Div ∩ (Crisis ∪ Sanction ∪ News) | measured | **88 (4.17% of Div)** | 85+3+0, pairwise disjoint | complete on explicit lists |
| Robustness flags (≥4 wks or ≥25%) | derived | **11 companies** | AVAN, SBER, USBN, VTBR, AKRN, GCHE, NVTK, TATN, LSRG, RTKM, AFLT (= METH target 11) | check not run (correctly — not a compilation deliverable) |

SOE(i) is time-invariant — no temporal overlap concept applies (correctly absent everywhere).

---
## §6.1 Event-pair overlap computations (Part I — Builder A §4) verification: the three zero-pairs (independent computation)

### 4.1 Method

Each pair is checked from this section's own listed dates/weeks: Monday crisis week-lists (§1.2),
anchor dates/weeks (§2.2/§2.4), and News facts (§3). Computation is shown, not just concluded.

### 4.2 Crisis ∩ News = 0 company-weeks ✓ AUDIT-TRAIL-VERIFIED (not exhaustively re-derived — see §3.6)

1. **Mechanism:** every candidate within ±2 weeks of a confirmed Crisis window was discarded
   BEFORE the data test; survivors re-tested to sit outside all pads [NEWS §3–§4.2].
2. **Discard audit:** 11 discarded, of which exactly 1 by a Crisis window: **PHOR-1, 2026-07-22**
   [NEWS §3]. **Independent check:** 2026-07-22 falls in Monday week 2026-07-20 ∈ W4
   (06-22→07-27) ✓ — the discard is correctly placed; no other crisis-window discard is claimed.
3. **Named-event check:** GMKN-3R event week Mon 2021-03-22: outside W1 (ends 04-13-2020), W2
   (starts 02-21-2022), W3, W4 ✓ — correctly retained.
4. **Residual:** "residual overlap after dedup: zero by construction… every surviving event
   re-tested to sit outside all pads" [NEWS §3]; "Company-weeks claimed by two News events = 0"
   [NEWS §1] rules out internal double-counting.
5. **Limit (carried):** full week-by-week recomputation needs `g4_events_final.csv` (absent, §3.6);
   the 0 is therefore VERIFIED-BY-AUDIT-TRAIL (mechanism + discard counts + both date-bearing
   checks pass exactly), not by exhaustive re-derivation. No statement in either permitted source
   places any News week inside any crisis window.

### 4.3 Sanction ∩ News = 0 company-weeks ✓ AUDIT-TRAIL-VERIFIED (not exhaustively re-derived — see §3.6)

1. **Mechanism:** same ±2-week pre-test dedup against each company's confirmed sanction date
   [NEWS §3; METH §3(c)].
2. **Discard audit:** 10 candidates discarded by confirmed sanction anchors [NEWS §3] (identities
   in absent `work/g4_candidates.csv`; count carried).
3. **Named-event cross-checks (all pass):** GMKN (no designation — GMKN is a verified negative
   [SANC §6.4]) → GMKN-3R cannot collide with a GMKN sanction date ✓. MTSS-1R: MTSS's sanction
   date is 2023-02-24 (subsidiary MTS Bank); MTSS-1R is a payout-resumption event that survived
   the ±2-week screen [NEWS §4.3–§4.4] → outside 2023-02-24 ± 2 wks by the carried screen ✓
   (exact MTSS-1R date not stated in NEWS — limit disclosed). VTBR-1: VTBR sanction 2022-02-24;
   VTBR-1 survived the screen → outside 2022-02-24 ± 2 wks ✓ (exact date not stated — limit
   disclosed).
4. **Residual:** zero by construction per the re-test [NEWS §3]. Same §3.6 limit as §4.2.

### 4.4 Crisis ∩ Sanction ≠ 0 — MEASURED NONZERO (methodology-amendment case)

**Finding:** under every definition operative in the sources, Crisis ∩ Sanction is NONZERO. The
computation:

**(a) Anchor-onset level (designation dates vs crisis spans):**
- VTBR 2022-02-24 ∈ W2 (Sun 02-20→03-27; Mon wk 02-21) ✓ INSIDE.
- NMTP 2022-02-25 ∈ W2 ✓ INSIDE (sectoral-first anchor; disclosed).
- TATN-affiliate 2026-07-23 ∈ W4 (Sun 06-21→07-26; Mon wk 07-20) ✓ INSIDE (secondary event).
- All other 21 anchor onsets fall outside all four windows (checked date-by-date against §1.2:
  SBER/CBOM 04-06-2022 and ALRS 04-07-2022 post-date W2's 03-27/03-28 end; June-2022 anchors in
  the inter-crisis gap; 2023 anchors between W2 and W3(09-03); 2025 anchors between W3 and
  W4(06-21); 2026-04-22 anchors pre-date W4's 06-21 start).
- Result: **2 of 23 anchor onsets inside crisis windows** (+ 1 secondary inside).

**(b) Reaction-window level (anchor Mon-week + 4 following weeks vs 23 crisis Mon-weeks):**
- VTBR weeks {02-21, 02-28, 03-07, 03-14, 03-21} (2022) — all 5 ∈ W2 → **5**.
- NMTP weeks {02-21, 02-28, 03-07, 03-14, 03-21} (2022) — all 5 ∈ W2 → **5**.
- All other anchors' 5-week windows touch no crisis week (nearest misses: none within 4 weeks
  of any window edge — verified against §1.2 lists).
- TATN-affiliate (secondary) weeks {07-20, 07-27, 08-03, 08-10, 08-17} (2026): 07-20, 07-27 ∈
  W4 → **2** (secondary; reported separately).
- Result: **10 anchor reaction-weeks inside crisis windows** (+ 2 secondary).

**(c) Permanent-step level (Sanction=1 weeks vs crisis weeks, §2.4 steps × §1.2 lists):**
- VTBR, NMTP: 15 crisis weeks each (W2 6 + W3 3 + W4 6) = 30.
- SBER, CBOM, ALRS, AFLT, CHMF, KMAZ, IRKT, MAGN, BSPB, USBN, MTSS, TRMK, FESH, PLZL: 9 each
  (W3 3 + W4 6) = 126.
- SIBN, SNGS, ROSN, LKOH, BANE, MFGS, JNOS, AVAN: 6 each (W4) = 48.
- Result: **204 company-weeks** (24 step-companies incl. AFLT; 195 on the strict 23-anchor set).
  = 11.8% of the 1,725 crisis cells. Shown per company above; recomputable from §1.2 + §2.4.

**(d) Corroboration inside the permitted sources (not new data):**
- NEWS §2 padded breakdown shows **Crisis+Sanction 11 spikes (1.9%)** — joint coverage exists.
- NEWS §3 states "the 2022 packets fall inside W2's window for some names" (handled by a
  priority convention for attribution, not by dedup of the dummies).
- The §3.5 derived check implies ~24 double-covered weeks in the ≥2× universe, attributable only
  to Crisis∩Sanction.
- SANC §6.2 NMTP row: "the invasion-week crash dominates" — explicit confounding acknowledgment.

**(e) Disposition:** METH §3 (Overlap measurement) states "Crisis ∩ Sanction… must equal exactly 0;
nonzero is a construction bug to fix." This cannot hold jointly with METH §3(b)'s own permanent-step
definition and the confirmed invasion-day VTBR designation (2022-02-24 ∈ W2): no construction
choice available to this builder (windows and anchors are both confirmed in the sources) can
produce a zero. The defect is therefore in the methodology sentence, not in the data.
**Filed as METHODOLOGY AMENDMENT CANDIDATE A1** (per METH §9 freeze protocol — this builder does
not amend the methodology; it flags): proposed correction — "Crisis ∩ News and Sanction ∩ News
equal exactly 0 by the §3(c) dedup construction; Crisis ∩ Sanction is MEASURED AND DISCLOSED
(like the Div pairs), never zero by construction, because Sanction is a permanent step and the
2022-02-24/25 designations fall inside W2." **Affected METH sections (QA2 F-Q2-M1): §3
Overlap-measurement paragraph ONLY. Explicitly unaffected:** M1–M6 and Sensitivity equations
(each includes Crisis and Sanction simultaneously — no equation change needed); the amendment
converts one guarantee sentence into a measured disclosure. Builder B's Div-overlap Sanction
concept (5-week reaction window per the dividend source's own §3 rule) is unaffected — see the
§2.6 joint glossary shared with Builder B.

---

## §6.2 Div-pair overlap computations (Part II — Builder B §3) recomputation: Div vs Crisis / Sanction / News

### 3.0 Concepts (no dedup of Div, ever)

Div was built INDEPENDENTLY with no deduplication against Crisis/Sanction/News; overlaps are
measured and disclosed, never removed [DIV §5.4; METH §3(d)]. Crisis weeks via [via-A §1.2]
(Monday lists W1–W4); Sanction concepts per the JOINT GLOSSARY [via-A §2.6] (tested-23 /
step-24 / reaction-34-PROVISIONAL / secondary): the Div-overlap disclosure below uses the
REACTION window (anchor Mon-week + 4 following weeks) keyed to cited reaction anchors [DIV §3]
(170 company-weeks cited), NOT the permanent step; News = 89 News=1 weeks, 31 events, 23
companies via [via-A §3] with News∩Crisis = News∩Sanction = 0 via [via-A §4.2–§4.3].

### 3.1 Div ∩ Crisis = 85 company-weeks (4.0% of Div; 4.9% of crisis cells) [DIV §3]

- W1 15 + W2 2 + W3 13 + W4 55 = 85 ✓ (Builder-B arithmetic); 85/2110 = 4.03%; 85/1725 = 4.93%.
- W4 (AGM season) holds 55/85; W2 holds only 2 (dividends collapsed spring-2022) [DIV §3].
- Full week-lists (every overlapping cell, Monday week-starts) [DIV §3]:
  - **W1 (15):** AVAN 2020-03-30, 2020-04-06, 2020-04-13; AKRN 2020-03-16, 2020-03-23,
    2020-03-30, 2020-04-06; GCHE 2020-03-09, 2020-03-16, 2020-03-23, 2020-03-30; NVTK 2020-04-06,
    2020-04-13; LSRG 2020-04-13; TTLK 2020-04-13.
  - **W2 (2):** AKRN 2022-02-21, 2022-02-28.
  - **W3 (13):** AVAN 2023-09-04, 2023-09-11, 2023-09-18; BSPB 2023-09-11, 2023-09-18; GCHE
    2023-09-04, 2023-09-11, 2023-09-18; NVTK 2023-09-11, 2023-09-18; TATN 2023-09-11, 2023-09-18;
    ALRS 2023-09-18.
  - **W4 (55):** SBER 06-22, 06-29, 07-06, 07-13; USBN 06-22, 06-29; VTBR 06-22, 06-29, 07-06,
    07-13; AKRN 07-13, 07-20, 07-27; KZOS 06-22; NKNC 06-22; ABRD 06-22, 06-29; BANE 06-22, 06-29,
    07-06; ROSN 06-22, 06-29; SIBN 06-22, 06-29; SNGS 06-22, 06-29, 07-06; TATN 06-22, 06-29,
    07-06; PLZL 06-22, 06-29, 07-06; LSRG 06-22, 06-29, 07-06; MTSS 06-22, 06-29; RTKM 06-22,
    06-29, 07-06, 07-13; AFLT 06-22, 06-29, 07-06; NMTP 06-22, 06-29, 07-06; MRKC 06-22; MRKP
    06-22; MRKU 06-22; MSNG 06-22, 06-29, 07-06; MSRS 06-22 (all 2026) [DIV §3].
- Per-company crisis-ov column sums to 85 ✓ (§1.4 check).

### 3.2 Div ∩ Sanction = 3 company-weeks (0.14% of Div) [DIV §3]

- anchor 2022-10-06 (EU 8th-package oil annex): **GAZP 2022-10-03, TATN 2022-10-03**.
- anchor 2026-04-22 (EU 20th pkg): **AVAN 2026-04-20**.
- **Concept note (Builder B, joint glossary [via-A §2.6]):** the 3 weeks are keyed to
  REACTION-window anchors [DIV §3], NOT to permanent steps: GAZP 2022-10-03 + TATN 2022-10-03 sit
  in the Mon-2022-10-03 week of the 2022-10-06 sectoral oil-annex reaction window (secondary-class
  anchor; GAZP has no entity designation and TATN’s step is affiliate-only per [via-A §2.3]); AVAN
  2026-04-20 sits in the Mon-2026-04-20 week of the 2026-04-22 20th-package reaction window
  (tested-23 anchor for AVAN [via-A §2.2 #23]). Reaction-window ≠ step by design; both concepts
  carried side by side without conflation.

### 3.3 Combined Div overlap (Crisis ∪ Sanction): 88 company-weeks = 4.17% of Div

85 + 3 = 88 (disjoint sets — no week is both; verified by comparing the §3.1 and §3.2 lists:
no common company-week) ✓; 88/2110 = 4.17%.

### 3.4 Div ∩ News = 0 company-weeks — RECOMPUTED DIRECTLY (bounded verification)

**Method (from [DIV] + [via-A] only):** (i) exclude Div weeks that cannot be News weeks via the
carried disjointness News∩Crisis = News∩Sanction = 0 [via-A §4.2–§4.3]; (ii) apply DIV §3's
statement that the three dividend-related News events (GMKN-3R 2021-03-24, MTSS-1R payout
resumption, VTBR-1 payout-policy) fall outside the affected companies' 4-week pre-windows by
Group-4's ex-date-mechanics bar; (iii) per-company bounded check over the 21 dual-positive
companies + 2 News-only companies below.

| Company | Div wks | News wks [via-A §3.3] | Basis / Grade (QA1 F-B4 + QA2 F-Q2-B1) |
|---|---|---|---|
| ABRD | 32 | 4 | [E] bounded: 2/32 Div wks excluded via W4 (§3.1); rest carried screen [via-A §4.2] |
| AFLT | 12 | 4 | [E] bounded: 3/12 excluded via W4; rest carried |
| BANE | 28 | 1 | [E] bounded: 3/28 excluded via W4; rest carried |
| CBOM | 4 | 2 | [C] FULLY carried (0 excludable) |
| FEES | 16 | 1 | [C] FULLY carried (0 excludable) |
| FESH | 0 | 9 | [D] 0 BY DEFINITION (Div=0) |
| GAZP | 16 | 8 | [E] bounded: 1/16 excluded via sanction anchor 2022-10-06 (§3.2); rest carried |
| GMKN | 40 | 9 (incl. GMKN-3R wk 03-22-2021) | [C] FULLY carried + DIV §3 dividend-event statement (GMKN-3R outside pre-windows) |
| KMAZ | 12 | 4 | [C] FULLY carried (0 excludable) |
| KZOS | 36 | 3 | [E] bounded: 1/36 excluded via W4; rest carried |
| LKOH | 60 | 2 | [C] FULLY carried (0 excludable) |
| MGNT | 36 | 1 | [C] FULLY carried (0 excludable) |
| MGTS | 4 | 4 | [C] FULLY carried (0 excludable) |
| MTSS | 52 | 4 (incl. MTSS-1R) | [E] bounded: 2/52 excluded via W4 + DIV §3 dividend-event statement; rest carried |
| MVID | 16 | 8 | [C] FULLY carried (0 excludable) |
| NKNC | 36 | 2 | [E] bounded: 1/36 excluded via W4; rest carried |
| PLZL | 52 | 2 | [E] bounded: 3/52 excluded via W4; rest carried |
| SFIN | 28 | 4 | [C] FULLY carried (0 excludable) |
| TRMK | 36 | 4 | [C] FULLY carried (0 excludable) |
| UPRO | 28 | 4 | [C] FULLY carried (0 excludable) |
| UTAR | 0 | 4 | [D] 0 BY DEFINITION (Div=0) |
| VTBR | 20 | 1 (VTBR-1) | [E] bounded: 4/20 excluded via W4 + DIV §3 dividend-event statement; rest carried |
| YNDX | 16 | 4 | [C] FULLY carried (0 excludable) |

**Grade legend:** [D] definitional — 2 companies, 13 News weeks (FESH 9 + UTAR 4); [E] bounded — 9
companies, 20 Div weeks excluded of their combined totals, remainder carried; [C] fully carried — 12
companies (CBOM, FEES, GMKN, KMAZ, LKOH, MGNT, MGTS, MVID, SFIN, TRMK, UPRO, YNDX), 0 excludable.
9 + 12 + 2 = 23 ✓.

**Result: Div=1 ∩ News=1 = 0 company-weeks** (0.0% of 2,110 Div weeks; 0.0% of 89 News weeks).
**NOT zero-by-deduplication:** Div was never deduplicated (DIV §5.4; totals intact at 2110; Div
shows 85 + 3 overlaps elsewhere) — the 0 is empirical/bounded, and Div is nonzero with
everything except News.
**§8 LIMIT (stated plainly):** full week-by-week recomputation requires `Div_window_log.csv` AND
`News_i_t.csv` — BOTH absent from permitted sources. The 0 above is therefore a BOUNDED
recomputation (exclusion logic + per-company consistency + DIV §3's dividend-event statement),
not an exhaustive re-derivation; the 13 News weeks at Div=0 companies (FESH 9 + UTAR 4) are the
only part verifiable by definition. Downstream H5 News-sensitivity uses are PROVISIONAL on the
week-level files.
**Addendum consistency note (no values sourced):** the separately-maintained
`News_Div_Overlap_Addendum.md` (repo root) also reports 0 for this pair; it was NOT used in the
recomputation above (which uses only [DIV] + [via-A]) and contributes no value to this section.

### 3.5 Robustness-check candidates (flag rule: ≥4 overlapping weeks OR ≥25% of own Div weeks) [DIV §3]

Recomputed per company from §1.4 (Crisis-ov + Sanction-ov; News adds 0 per §3.4):
AVAN 7/92 = 7.6% FLAG (≥4); SBER 4/28 = 14.3% FLAG; USBN 2/8 = 25.0% FLAG; VTBR 4/20 = 20% FLAG;
AKRN 9/56 = 16.1% FLAG; GCHE 7/52 = 13.5% FLAG; NVTK 4/64 = 6.3% FLAG; TATN 6/88 = 6.8% FLAG
(5 crisis + 1 sanction); LSRG 4/32 = 12.5% FLAG; RTKM 4/36 = 11.1% FLAG; AFLT 3/12 = 25.0% FLAG.
All other 64 companies fail both prongs (nearest misses at 3 weeks: BANE 3/28, SNGS 3/32, PLZL
3/52, NMTP 3/40, MSNG 3/28 — each <4 and <25%).
**Final list (11): AVAN, SBER, USBN, VTBR, AKRN, GCHE, NVTK, TATN, LSRG, RTKM, AFLT** [DIV §3] ✓ —
matches METH §3's "target 11 companies" exactly; News adds no candidate (conditional on §3.4's
carried 0). Recommended check (re-estimate H5 dropping these / Crisis×Div interaction) is NOT run
here [DIV §3].

---

## §7 Consolidated gaps, blockers & methodology-amendment register (Agent-5 assembly)

| ID | Gap / blocker | Full declaration | Downstream consequence | Status |
|---|---|---|---|---|
| G1 | 587-row Div record-date list absent (only aggregates + 88 overlap weeks present) | §4 (B §1.5) | Full Div(i,t) unrecoverable; H5, METH §4-C8 Div_l1 count, exhaustive Div∩News blocked | BLOCKER (§8-declared) |
| G2 | 31-event News date/duration table absent (only counts + named events present) | §3 (A §3.6) | Per-event News weeks unrecoverable; zero-pairs audit-trail-verified only | LIMIT (§8-declared) |
| G3 | Row-level 34-anchor list absent (aggregate 170 cited) | §2 (A §2.5) | reaction-34 = provisional 23+11 candidate | PROVISIONAL |
| G4 | `agent2b_soe.csv` absent — C6 reconciliation one-sided (35/40 side only) | §5 (B §2.5) | H6, Section 6, sensitivities 5–6 BLOCKED | BLOCKED |
| G5 | YAKG sector disputed (Energy: SOE App.A + §1.1 counts 13/14; Utilities: DIV §3 + SANC §10) | §5 (B §2.6) | No sector-dependent result settled for YAKG; E3 frozen table governs | DISPUTED |
| A1 | METH §3 "Crisis∩Sanction = 0" unsatisfiable (VTBR 2022-02-24 in W2 under METH's own permanent-step rule) | §6.1 (A §4.4e) | Guarantee → measured disclosure; M1–M6 equations unaffected | FILED (METH §9 protocol) |

---
## §8 Merged integrity statement

1. **Provenance:** Crisis/Sanction/News values trace to [SANC]/[NEWS]; Div/SOE values trace to
   [DIV]/[SOE]; cross-layer facts in Part II trace via [via-A]; rules trace to [METH]. Full
   per-builder declarations in §9. QA1 verified 34/34 spot-checks + citation specificity (10/10).
2. **No forbidden sourcing:** final Agent-5 check — `iqbal thesis data.zip` and `thesis books.zip`
   appear in this file ONLY in two provenance/declaration lines (§0 merged provenance and item 2
   here); neither was opened or consulted for any value. The old prompt
   file was ignored. The addendum contributed zero values (consistency note in §6.2 only).
3. **No alteration (Agent-5 confirmation):** §1–§5, §6.1, §6.2 are verbatim inclusions of the
   QA-passed Round-2 section bodies (only their `##` headings gained Part labels above); §6.0 and
   §7 copy figures unchanged (verified: 0/89/2110/1725/170/85/3/0/88/11 all match source sections).
4. **Gaps honored:** G1–G5 + A1 are declared, not glossed; provisional/blocked/disputed labels and
   quantitative consequences travel with every affected figure. METH §8 standing rule observed
   end to end. QA2 verified all relationships + methodology conformance (10/10).

---
## §9 Builder/QA audit trail (verbatim builder integrity statements; QA verdicts)

### Builder A integrity statement (A §5 verbatim)

## 5. Builder A integrity statement

1. Every value above traces to `[SANC]` or `[NEWS]` sections/tables named beside it, or is labeled
   Builder-A arithmetic (sums, label shifts, §4 intersections) recomputable from those values.
2. The two forbidden archives were never opened or consulted; the old prompt file was ignored; no
   dividend/SOE/addendum value was used.
3. Gaps are stated, not glossed: full News event-date table absent (§3.6); row-level 34-anchor
   list absent (§2.5 provisional); Crisis∩Sanction honestly nonzero with amendment filed (§4.4e).
4. Standing rule observed [METH §8]: nothing was substituted, approximated, or silently
   reconstructed; provisional items are labeled PROVISIONAL with their quantitative consequence.
5. Round-2 change log (no value altered except the F-A1 typo correction): F-A1 W2 cell fixed +
   erratum log; F-A2 (d) bar labels + footnote; F-A3 D-AFLT exact quote + labeled inference;
   F-A4 inclusion–exclusion premise; F-A5 heading retitles; F-Q2-A1/AB1 §2.6 joint glossary +
   consolidated week table; F-Q2-AB2 §2.7 SANC-side YAKG note; F-Q2-M1 A1 affected-sections
   precision. No new source consulted.
### Builder B integrity statement (B §4 verbatim)

## 4. Builder B integrity statement

1. Every value traces to [DIV] or [SOE] sections/tables named beside it, to [via-A] compiled
   dates for cross-layer facts, or to labeled Builder-B arithmetic (sums, percentages, §3 set
   operations) recomputable from those values.
2. Forbidden archives never opened or consulted; old prompt file ignored; no news.md/sanctions
   value used except via Builder A's compiled dates; no addendum value used (consistency note only).
3. Gaps stated, not glossed: 587-row Div date table absent = BLOCKER (§1.5); C6 SOE reconciliation
   blocked (§2.5); YAKG sector disputed (§2.6); Div∩News bounded, not exhaustive (§3.4).
4. Standing rule observed [METH §8]: no substitution, approximation, or silent reconstruction;
   provisional items labeled with quantitative consequence.
5. Round-2 change log (no value altered — all fixes are citations, labels, notes): F-B1
   merge-vs-truncation footnote; F-B2 verbatim §3(d) quote + blocked-items list; F-B3 CBOM tier
   dual citation + PIKK vintage note; F-B4/F-Q2-B1 Basis/Grade column + legend; F-B5 DIV-implied
   counts; F-Q2-AB1 joint-glossary rewrite of §3.0/§3.2; F-Q2-AB2 SANC-side dispute line. No new
   source consulted.
METH §8]: no substitution, approximation, or silent reconstruction;
   provisional items labeled with quantitative consequence.
, approximation, or silent reconstruction;
   provisional items labeled with quantitative consequence.
### QA verdicts

- QA1 Round 1: 7/10 (10 findings, all closed in R2) → **QA1 Round 2: 10/10 — PERFECTION CONFIRMED.**
- QA2 Round 1: 7/10 (5 findings, all closed in R2) → **QA2 Round 2: 10/10 — PERFECTION CONFIRMED.**
- Loop exit condition met (both QA at 10/10): Agent 5 ran; Agent 6 publishes.
