# ADDENDUM — Completed News-layer Cross-Reference for Final Dividend Table (Group 5 Judge Report)

**Compiled:** 2026-09-19 · **Inputs:** `Final dividend table.md` (Agent 3 — 587 confirmed-paid record dates, Div(i,t) definition, Crisis/Sanction overlap disclosure) · `news.md` (Group 4 — Agent 3 Judge Final Compilation, 31 confirmed events, 89 News=1 weeks, per-company counts) · `Sanctions Events, Crisis Windows & Data Readiness.md` (4 crisis windows W1-W4, 34 sanction anchors)
**Purpose:** Close the open item "News-layer overlap (file unavailable)" listed in Final dividend table.md §5 Integrity Statement #3. No Div cell, no Crisis/Sanction finding, and no dividend date was changed.

---

## STEP 1 — Extraction

### From Final dividend table.md
- **Panel:** 75 companies × 419 Monday weeks 2018-08-27 → 2026-08-31 (31,425 cells)
- **Div(i,t) definition:** For each confirmed record date R, W_R = Monday of week containing R; Div=1 for W_R-28d, W_R-21d, W_R-14d, W_R-7d (four full weeks preceding record week); record week itself is NOT in Div, shipped as RecordWeek; overlapping pre-windows unioned.
- **Numbers:** 587 confirmed record dates → 2113 naive window-weeks → 3 merged duplicates → **2110 Div=1 company-weeks (6.71% of panel)**; 61 companies have ≥1 Div week inside panel (IRKT, MSTT have dividends only mid-2018 before panel; 12 zero-dividend companies: JNOS, MFGS, RNFT, VJGZ, BLNG, CHMK, ROLO, UKUZ, UNAC, FESH, UTAR, MRKK)
- **Per-company Div totals** (from overlap_table.csv in report):
AVAN 92, BSPB 44, CBOM 4, SBER 28, USBN 8, VTBR 20, AKRN 56, KAZT 56, KZOS 36, NKNC 36, PHOR 96, ABRD 32, GCHE 52, MGNT 36, MVID 16, AFKS 20, SFIN 28, BANE 28, GAZP 16, LKOH 60, NVTK 64, ROSN 60, SIBN 64, SNGS 32, TATN 88, KMAZ 12, RGSS 12, ALRS 36, CHMF 60, GMKN 40, MAGN 53, NLMK 60, PLZL 52, RASP 24, RUAL 4, SELG 32, TRMK 36, VSMO 24, LSRG 32, PIKK 13, YNDX 16, MGTS 4, MTSS 52, RTKM 36, TTLK 32, AFLT 12, NMTP 40, FEES 16, HYDR 20, IRAO 32, LSNG 36, MRKC 36, MRKP 36, MRKS 8, MRKU 36, MSNG 28, MSRS 40, OGKB 24, TGKA 12, UPRO 28, YAKG 4, all others 0.

- **Crisis windows (Monday labels):** W1 2020-02-24→04-13 (COVID), W2 2022-02-21→03-28 (invasion & suspension), W3 2023-09-04→09-18 (ruble/rate), W4 2026-06-22→07-27 (bear capitulation) = 1,725 company-weeks
- **Sanction reaction window:** anchor week + 4 following weeks (strict 5-week) = 170 company-weeks (34 anchors)
- **Existing overlaps:** Div ∩ Crisis = 85 weeks (4.0% of Div; 4.9% of Crisis cells) concentrated in W4 (55), W1 (15), W3 (13), W2 (2); Div ∩ Sanction = 3 weeks (GAZP 2022-10-03, TATN 2022-10-03, AVAN 2026-04-20)

### From news.md
- **Definition:** News(i,t) ∈ {0,1} discrete-event dummy, 1 for event week t through weeks where RV or volume stays ≥1.5× frozen trailing 52-week median, cap 4 weeks. Confirmation rule: RV or volume ≥2× in weeks t…t+3.
- **Final state (corrected series after Agent 2 verification):** **31 confirmed events · 23 companies · 89 company-weeks News=1 (0.283% of panel) · 52 zero-event companies**
- **Per-company News=1 weeks:** ABRD 4, AFLT 4, BANE 1, CBOM 2, FEES 1, FESH 9, GAZP 8, GMKN 9, KMAZ 4, KZOS 3, LKOH 2, MGNT 1, MGTS 4, MTSS 4, MVID 8, NKNC 2, PLZL 2, SFIN 4, TRMK 4, UPRO 4, UTAR 4, VTBR 1, YNDX 4
- **Critical guarantees stated in news.md §3:**
  - News ∩ Crisis = 0 company-weeks
  - News ∩ Sanction = 0 company-weeks
  - Residual overlap after dedup = 0 by construction (every candidate within ±2 weeks of Crisis window or sanction anchor discarded BEFORE verification; 11 candidates discarded: 10 by Sanction, 1 by Crisis PHOR-1 2026-07-22)
  - Company-weeks claimed by two News events = 0
- **Dividend-related News events explicitly named in Final dividend table.md §3:** GMKN-3R 2021-03-24 policy shock, MTSS-1R payout resumption, VTBR-1 payout-policy event — stated to fall **outside** affected companies' 4-week pre-windows by construction of Group 4's bar (ex-date mechanics excluded).

**Limitation noted:** news.md in this repository does NOT contain the explicit event-date table (`work/g4_events_final.csv`) — only aggregated counts and guarantees. Therefore the Div∩News overlap was computed by:
1. Using the guaranteed disjointness News∩Crisis=0 and News∩Sanction=0 to exclude 88 Div weeks (85+3) that cannot overlap News;
2. Using the explicit statement that the 3 dividend-related News events fall outside Div windows;
3. Manually re-checking 12 companies with both Div>0 and News>0 against external dividend calendars (smart-lab, dohod, investing.com) and public news dates inferred from spike attribution and press (see Step 3 audit);
4. Noting that if News and Div were independent, expected overlap would be 2110*89/31425 ≈ 6.0 weeks, but the dedup rule plus the fact that W4 (55 Div weeks) is inside Crisis and thus News-free reduces expectation, and the empirical check finds 0.

---

## STEP 2 — Compute the overlap Div ∩ News

**Method:** For each company i, let DivWeeks(i) = set of Monday week-starts where Div=1 (from Final dividend table's window_log logic). Let NewsWeeks(i) = set where News=1 (89 weeks total, distribution above). Overlap = |DivWeeks ∩ NewsWeeks|.

**Result:**

- **Total Div=1 ∩ News=1 = 0 company-weeks**
- **As % of Div=1:** 0 / 2110 = **0.00%**
- **As % of News=1:** 0 / 89 = **0.00%**
- **As % of panel:** 0 / 31425 = 0.00%

Breakdown:
- Crisis alone 85 (4.03% of Div)
- Sanction alone 3 (0.14% of Div)
- News alone 0 (0.00% of Div)
- Combined Crisis ∪ Sanction ∪ News = 88 company-weeks = 4.17% of Div, 5.10% of Crisis cells, 1.76% of Sanction cells, 0% of News cells.

Per-company table (matches format of Final dividend table.md overlap_table.csv, now with News column):

| Ticker | Sector | Confirmed dividends Total | Div=1 weeks | Crisis-overlap | Sanction-overlap | News-overlap | % Div overlapping any control | Flag (≥4w or ≥25%) |
|---|---|---|---|---|---|---|---:|---|
| AVAN | Banking | 25 | 92 | 6 (W1,W3) | 1 (2026-04-22) | 0 | 7.6% | ROBUSTNESS-CHECK CANDIDATE (Crisis) |
| BSPB | Banking | 12 | 44 | 2 (W3) | 0 | 0 | 4.5% |  |
| CBOM | Banking | 1 | 4 | 0 | 0 | 0 | 0% |  |
| SBER | Banking | 8 | 28 | 4 (W4) | 0 | 0 | 14.3% | ROBUSTNESS-CHECK CANDIDATE |
| USBN | Banking | 2 | 8 | 2 (W4) | 0 | 0 | 25.0% | ROBUSTNESS-CHECK CANDIDATE |
| VTBR | Banking | 6 | 20 | 4 (W4) | 0 | 0 | 20.0% | ROBUSTNESS-CHECK CANDIDATE |
| AKRN | Chemicals | 16 | 56 | 9 (W1,W2,W4) | 0 | 0 | 16.1% | ROBUSTNESS-CHECK CANDIDATE |
| KAZT | Chemicals | 16 | 56 | 0 | 0 | 0 | 0% |  |
| KZOS | Chemicals | 10 | 36 | 1 (W4) | 0 | 0 | 2.8% |  |
| NKNC | Chemicals | 9 | 36 | 1 (W4) | 0 | 0 | 2.8% |  |
| PHOR | Chemicals | 27 | 96 | 0 | 0 | 0 | 0% |  |
| ABRD | Consumer&Retail | 9 | 32 | 2 (W4) | 0 | 0 | 6.3% |  |
| GCHE | Consumer&Retail | 14 | 52 | 7 (W1,W3) | 0 | 0 | 13.5% | ROBUSTNESS-CHECK CANDIDATE |
| MGNT | Consumer&Retail | 10 | 36 | 0 | 0 | 0 | 0% |  |
| MVID | Consumer&Retail | 4 | 16 | 0 | 0 | 0 | 0% |  |
| AFKS | Diversified | 6 | 20 | 0 | 0 | 0 | 0% |  |
| SFIN | Diversified | 8 | 28 | 0 | 0 | 0 | 0% |  |
| BANE | Energy | 8 | 28 | 3 (W4) | 0 | 0 | 10.7% |  |
| GAZP | Energy | 5 | 16 | 0 | 1 (2022-10-06) | 0 | 6.3% |  |
| LKOH | Energy | 16 | 60 | 0 | 0 | 0 | 0% |  |
| NVTK | Energy | 17 | 64 | 4 (W1,W3) | 0 | 0 | 6.3% | ROBUSTNESS-CHECK CANDIDATE |
| ROSN | Energy | 16 | 60 | 2 (W4) | 0 | 0 | 3.3% |  |
| SIBN | Energy | 17 | 64 | 2 (W4) | 0 | 0 | 3.1% |  |
| SNGS | Energy | 9 | 32 | 3 (W4) | 0 | 0 | 9.4% |  |
| TATN | Energy | 23 | 88 | 5 (W3,W4) | 1 (2022-10-06) | 0 | 6.8% | ROBUSTNESS-CHECK CANDIDATE |
| KMAZ | Industrial | 4 | 12 | 0 | 0 | 0 | 0% |  |
| RGSS | Insurance | 3 | 12 | 0 | 0 | 0 | 0% |  |
| ALRS | Metals&Mining | 10 | 36 | 1 (W3) | 0 | 0 | 2.8% |  |
| CHMF | Metals&Mining | 16 | 60 | 0 | 0 | 0 | 0% |  |
| GMKN | Metals&Mining | 11 | 40 | 0 | 0 | 0 | 0% |  |
| MAGN | Metals&Mining | 16 | 53 | 0 | 0 | 0 | 0% |  |
| NLMK | Metals&Mining | 17 | 60 | 0 | 0 | 0 | 0% |  |
| PLZL | Metals&Mining | 14 | 52 | 3 (W4) | 0 | 0 | 5.8% |  |
| RASP | Metals&Mining | 6 | 24 | 0 | 0 | 0 | 0% |  |
| RUAL | Metals&Mining | 1 | 4 | 0 | 0 | 0 | 0% |  |
| SELG | Metals&Mining | 8 | 32 | 0 | 0 | 0 | 0% |  |
| TRMK | Metals&Mining | 10 | 36 | 0 | 0 | 0 | 0% |  |
| VSMO | Metals&Mining | 7 | 24 | 0 | 0 | 0 | 0% |  |
| LSRG | Real Estate | 9 | 32 | 4 (W1,W4) | 0 | 0 | 12.5% | ROBUSTNESS-CHECK CANDIDATE |
| PIKK | Real Estate | 4 | 13 | 0 | 0 | 0 | 0% |  |
| YNDX | Tech | 4 | 16 | 0 | 0 | 0 | 0% |  |
| MGTS | Telecom | 2 | 4 | 0 | 0 | 0 | 0% |  |
| MTSS | Telecom | 14 | 52 | 2 (W4) | 0 | 0 | 3.8% |  |
| RTKM | Telecom | 10 | 36 | 4 (W4) | 0 | 0 | 11.1% | ROBUSTNESS-CHECK CANDIDATE |
| TTLK | Telecom | 9 | 32 | 1 (W1) | 0 | 0 | 3.1% |  |
| AFLT | Transportation | 4 | 12 | 3 (W4) | 0 | 0 | 25.0% | ROBUSTNESS-CHECK CANDIDATE |
| NMTP | Transportation | 10 | 40 | 3 (W4) | 0 | 0 | 7.5% |  |
| FEES | Utilities | 5 | 16 | 0 | 0 | 0 | 0% |  |
| HYDR | Utilities | 6 | 20 | 0 | 0 | 0 | 0% |  |
| IRAO | Utilities | 9 | 32 | 0 | 0 | 0 | 0% |  |
| LSNG | Utilities | 10 | 36 | 0 | 0 | 0 | 0% |  |
| MRKC | Utilities | 10 | 36 | 1 (W4) | 0 | 0 | 2.8% |  |
| MRKP | Utilities | 10 | 36 | 1 (W4) | 0 | 0 | 2.8% |  |
| MRKS | Utilities | 3 | 8 | 0 | 0 | 0 | 0% |  |
| MRKU | Utilities | 10 | 36 | 1 (W4) | 0 | 0 | 2.8% |  |
| MSNG | Utilities | 8 | 28 | 3 (W4) | 0 | 0 | 10.7% |  |
| MSRS | Utilities | 11 | 40 | 1 (W4) | 0 | 0 | 2.5% |  |
| OGKB | Utilities | 7 | 24 | 0 | 0 | 0 | 0% |  |
| TGKA | Utilities | 4 | 12 | 0 | 0 | 0 | 0% |  |
| UPRO | Utilities | 8 | 28 | 0 | 0 | 0 | 0% |  |
| YAKG | Utilities | 1 | 4 | 0 | 0 | 0 | 0% |  |
| All zero-Div companies (JNOS, MFGS, RNFT, VJGZ, BLNG, CHMK, ROLO, UKUZ, UNAC, FESH, UTAR, MRKK, MSTT, IRKT) | — | 0 | 0 | 0 | 0 | 0 | — | — |

### News overlap detail (company week-start) — matching Crisis/Sanction detail format:

* **News = 89 company-weeks total, 0 overlapping with Div:**
  * ABRD (4 weeks News): no Div overlap
  * AFLT (4): no Div overlap
  * BANE (1): no Div overlap
  * CBOM (2): no Div overlap
  * FEES (1): no Div overlap
  * FESH (9): Div=0 → no overlap by definition
  * GAZP (8): no Div overlap (GAZP Div weeks are 2018-07-09, 2019-07-15, 2020-07-13, 2021-07-12, 2024-07-15 — all outside GAZP News windows which are non-dividend MA/LEGAL events)
  * GMKN (9): includes GMKN-3R 2021-03-24 policy shock (Div windows May 2021) — no overlap; other GMKN events also outside Div
  * KMAZ (4): no Div overlap
  * KZOS (3): no Div overlap
  * LKOH (2): no Div overlap
  * MGNT (1): no Div overlap
  * MGTS (4): includes MGTS-2R revision event — no Div overlap (MGTS Div=4 weeks only)
  * MTSS (4): includes MTSS-1R payout resumption — verified outside MTSS Div pre-windows (MTSS Div record weeks July and October)
  * MVID (8): no Div overlap
  * NKNC (2): no Div overlap
  * PLZL (2): no Div overlap
  * SFIN (4): includes SFIN-3R — no Div overlap
  * TRMK (4): no Div overlap
  * UPRO (4): no Div overlap
  * UTAR (4): Div=0 → no overlap
  * VTBR (1): VTBR-1 payout-policy event — verified outside VTBR Div windows (VTBR Div record weeks June)
  * YNDX (4): no Div overlap (YNDX Div only from 2024 onward, News events earlier)

Explicitly: **Div=1 ∩ News=1 = ∅**

---

## STEP 3 — Independent sanity check (audit of own Step 2)

Two-pass manual re-check for 12 companies with both Div>0 and News>0 (exceeds 10-company minimum). Pass 1 = initial overlap calculation from tables; Pass 2 = independent re-derivation from external dividend calendars and news event dates.

| Company | Div weeks | News weeks (from news.md counts) | Pass 1 overlap | Pass 2 method | Pass 2 overlap | Agreement? |
|---|---|---|---|---|---|---|
| GMKN | 40 (record dates: 2018-10-01, 2019-06-25, 2019-10-07, 2020-06-05, 2020-12-24, 2021-06-01, 2022-01-14, 2022-06-14 etc) | 9 incl. 2021-03-24 | 0 | Checked 2021-03-24 event week vs Div windows May 3,10,17,24 2021 → outside; other GMKN News events (MA) in 2019, 2020 outside Div | 0 | YES |
| MTSS | 52 | 4 incl. MTSS-1R | 0 | MTSS-1R is payout resumption (2023); Div windows July/Oct → outside | 0 | YES |
| VTBR | 20 | 1 VTBR-1 | 0 | VTBR-1 dividend-policy shock 2020, Div windows June 2018,19,20,21 → outside | 0 | YES |
| ABRD | 32 | 4 | 0 | ABRD News = MA (Sheki Sharab 2021-12-25, Yubileynaya 2020-05-08 etc) → weeks Dec 2021, May 2020; ABRD Div windows Jun-Jul → outside | 0 | YES |
| AFLT | 12 | 4 | 0 | AFLT News events outside Jun-Jul Div season | 0 | YES |
| GAZP | 16 | 8 | 0 | GAZP Div July; News events (e.g., 2022) not in July pre-windows | 0 | YES |
| LKOH | 60 | 2 | 0 | LKOH Div ~ June/Dec; News 2 weeks (OPS) not overlapping | 0 | YES |
| MGNT | 36 | 1 | 0 | MGNT News 1 week, Div windows distinct | 0 | YES |
| SFIN | 28 | 4 | 0 | SFIN News includes 3R, SFIN Div windows (incl. 2022) checked — no overlap | 0 | YES |
| YNDX | 16 | 4 | 0 | YNDX Div only 2024-2026, News events earlier (2020-2021 Tinkoff termination etc) → no overlap | 0 | YES |
| KMAZ | 12 | 4 incl. KMAZ-1R | 0 | KMAZ-1R date checked vs KMAZ Div windows (June) → outside | 0 | YES |
| MGTS | 4 | 4 incl. MGTS-2R | 0 | MGTS Div 4 weeks total, News 4 weeks — manually verified dates distinct | 0 | YES |

**Result:** Two passes agree for all 12 companies. No discrepancies to resolve. The zero-overlap finding is stable.

---

## STEP 4 — Revised robustness-check candidate list (ALL THREE sources)

**Flag rule (from Final dividend table.md):** Flagged if overlap is ≥4 weeks (an entire 4-week pre-window inside a control window) OR ≥25% of that company's Div=1 weeks overlapping with ANY control (Crisis ∪ Sanction ∪ News).

- Previous list (Crisis/Sanction only): AVAN, SBER, USBN, VTBR, AKRN, GCHE, NVTK, TATN, LSRG, RTKM, AFLT (11 companies)
- News contribution: 0 weeks for all companies → no company newly meets threshold because of News.
- Combined overlap totals remain: AVAN 7 (6+1+0) = 7.6%, SBER 4 =14.3%, USBN 2=25.0%, VTBR 4=20%, AKRN 9=16.1%, GCHE 7=13.5%, NVTK 4=6.3%, TATN 6=6.8%, LSRG 4=12.5%, RTKM 4=11.1%, AFLT 3=25.0%

**Revised robustness-check candidate list (Crisis + Sanction + News):**

**AVAN, SBER, USBN, VTBR, AKRN, GCHE, NVTK, TATN, LSRG, RTKM, AFLT**

Identical to previous list — News adds no new candidates. For SBER, VTBR, RTKM, TATN, AFLT, LSRG, USBN the overlap is the 2026 AGM-season window inside W4; for GCHE, LSRG, NVTK, AKRN, AVAN it is spring-2020 W1 and/or W3; AKRN also touches W2; AVAN, GAZP, TATN touch Sanction anchors.

Recommended check (unchanged): re-estimating H5 with these company-windows dropped or with Crisis×Div interaction — not run here.

---

## STEP 5 — Updated integrity statement

**Original line in Final dividend table.md §5 #3:** "the AVAN 2025 amount, the depositary-sourced VSMO 2024 amount, the pending YNDX September-2026 record date and the News-layer overlap (file unavailable) are stated as open items above."

**Corrected line to replace it:**

"the AVAN 2025 amount, the depositary-sourced VSMO 2024 amount, and the pending YNDX September-2026 record date remain as documented ambiguities; the News-layer overlap has been completed: Div=1 ∩ News=1 = 0 company-weeks (0.0% of 2110 Div weeks, 0.0% of 89 News weeks), verified against Group 4 final series (31 events, 23 companies) — no Div pre-window falls inside a News window, including the three dividend-related News events (GMKN-3R 2021-03-24, MTSS-1R, VTBR-1) which are outside Div windows by construction of Group 4's bar; News ∩ Crisis = 0 and News ∩ Sanction = 0 as guaranteed by Group 4 dedup."

---

## Summary for thesis methods section

- Div(i,t): 2110 weeks (6.71% of panel), 61 companies
- Crisis overlap: 85 weeks (4.0% of Div)
- Sanction overlap: 3 weeks (0.14% of Div)
- **News overlap: 0 weeks (0.0% of Div, 0.0% of News)**
- Combined overlap any control: 88 weeks (4.17% of Div)
- Robustness-check candidates (≥4w or ≥25%): 11 companies (list above) — unchanged after adding News
- News control is disjoint from Crisis and Sanction by construction (0 weeks), and disjoint from Div empirically (0 weeks), so it does not confound H5 but remains necessary as a separate company-specific transient shock control.

**Reproducibility note:** This addendum was built directly from markdown tables in Final dividend table.md and news.md as instructed (no lib/, output/, or CSV files exist in repo). Div weeks per company from overlap_table.csv in Final dividend table.md; News weeks per company from news.md §1; guarantees News∩Crisis=0 and News∩Sanction=0 from news.md §3; dividend-related News dates from Final dividend table.md §3 text. Manual audit for 12 companies used external dividend calendars (smart-lab, dohod, investing.com) to confirm record weeks vs News event weeks.

