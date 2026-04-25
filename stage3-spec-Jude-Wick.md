<div style="border-top: 6px solid #024731; border-bottom: 1px solid #B2B2B2; padding: 12px 0; margin-bottom: 24px; font-family: 'Open Sans', Helvetica, Arial, sans-serif;">
  <div style="color: #024731; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; font-size: 0.85rem;">University of Hawaiʻi at Mānoa · Shidler College of Business</div>
  <div style="color: #000000; font-weight: 700; font-size: 1.25rem; margin-top: 4px;">FIN-321 International Finance &amp; Securities</div>
  <div style="color: #525252; font-weight: 400; font-size: 0.95rem;">FX Transaction Hedging Project — Technical Specification</div>
</div>

# Apex Aerospace Systems Inc. — FX Transaction Hedge Model · Technical Specification

> <span style="color:#024731; font-weight:700;">Post-build specification</span> documenting the Stage 2 Excel hedge model for Scenario 4, validating it against scenario data, and articulating the refinements required for a production-grade version. Drives the Stage 4 AI prompt and final analysis.

| Field | Value |
|-------|-------|
| **Created by** | Treasury Risk Management Analyst |
| **Updated by** | Treasury Risk Management Analyst |
| **Date Created** | 2026-04-03 |
| **Date Updated** | 2026-04-03 |
| **Version** | 1.0 |
| **LLM Used** (optional) | Claude Sonnet 4.6 — used to scaffold model structure, validate formula logic, and draft specification sections |
| **Role** | Treasury Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | `docs/templates/excel/lastname-first-stage2-model.xlsx` |

---

## 1. Problem Statement

Apex Aerospace Systems Inc., a U.S.-based aerospace manufacturer reporting in USD, has contracted to deliver aircraft components to a European buyer and expects a **€20,000,000 receivable** settling in approximately **365 days** (April 2027). Because the functional currency is USD and the receivable is denominated in EUR, the firm carries full transaction exposure: any depreciation of the euro against the dollar between contract execution and settlement date will reduce realized USD proceeds and compress operating cash flow. At the spot rate of 1.1530 USD/EUR recorded on April 3, 2026, the receivable has a face value of approximately **$23,060,000** — a material sum where even a 2-cent adverse move in EUR/USD translates to a $400,000 loss. This specification documents the analytical framework used to quantify and compare five strategies — **no hedge**, **forward hedge**, **money-market hedge**, **put option**, and **collar** — and produces the sensitivity evidence that will support the Stage 4 hedging recommendation.

---

## 2. Inputs (Known Variables)

All inputs are exposed as workbook named ranges so the Calculation Flow (§4) is portable across Excel, Python, and AI prompts. Market inputs (spot, forward, rates, premia) are the only cells an analyst should adjust for scenario work. Dates, sources, and access timestamps are recorded in the Notes & Assumptions tab.

### 2.1 Core Inputs

| Standardized Name | Description | Unit | Value | Source |
|-------------------|-------------|------|------:|--------|
| `FC_AMT` | Foreign-currency notional receivable | EUR | 20,000,000 | Scenario 4 |
| `S0_in` | Spot exchange rate at inception | USD/EUR | 1.1530 | Yahoo Finance / RoboForex, Apr 3 2026 |
| `F0_in` | 1-year forward rate to settlement | USD/EUR | 1.0935 | Scenario 4 |
| `R_USD` | USD 1-year interest rate | Annual % | 4.30% | U.S. 1-yr T-bill rate, Apr 3 2026 |
| `R_FC` | EUR 1-year interest rate | Annual % | 2.50% | ECB deposit rate estimate, Apr 3 2026 |
| `K_PUT` | Put option strike price | USD/EUR | 1.1530 | Set at-the-money (= S0_in) |
| `K_CALL` | Call option strike price | USD/EUR | 1.1530 | Set at-the-money (= S0_in) |
| `PREM_PUT` | Put premium per unit of FC | USD/EUR | 0.019 | Scenario 4 |
| `PREM_CALL` | Call premium per unit of FC | USD/EUR | 0.024 | Scenario 4 |
| `T_DAYS` | Days to settlement | Days | 365 | 1-year horizon per scenario |

### 2.2 Derived / Intermediate Values

| Name | Description | Formula |
|------|-------------|---------|
| `DF_USD` | USD accumulation factor | `1 + R_USD` *(simplified annual form)* |
| `DF_FC` | EUR accumulation factor | `1 + R_FC` *(simplified annual form)* |
| `FV_PREM_PUT` | Future value of put premium at settlement | `−PREM_PUT × FC_AMT × DF_USD` |
| `FV_PREM_CALL` | Future value of call premium at settlement | `−PREM_CALL × FC_AMT × DF_USD` |
| `S_T_grid` | Sensitivity spot grid at settlement | `S0_in × (1 + n%)` for n = −5, −4, … +5 |
| `USD_NO_HEDGE` | USD proceeds under no hedge | `S_T × FC_AMT` |
| `NET_COLLAR_PREM` | Net collar premium (credit received) | `(PREM_CALL − PREM_PUT) × FC_AMT` = +$100,000 |

> <span style="color:#024731;">**Note on parity:**</span> The implied forward rate under covered interest-rate parity is `S0_in × DF_USD / DF_FC = 1.1530 × 1.043 / 1.025 ≈ 1.1733`. The scenario-given forward of 1.0935 diverges from this parity estimate, suggesting either a wider interest rate differential than estimated or a market premium priced in. This gap is flagged as a model assumption — the scenario-given forward is used as authoritative for the forward hedge calculation; the parity-derived forward is used for the money-market hedge. The difference is documented in the parity-check row of the model.

---

## 3. Assumptions & Constraints

- **Quote convention:** All rates expressed as **USD per unit of EUR** (USD/EUR). A higher quote means EUR appreciation — better for Apex as a EUR receiver.
- **Horizon:** Single-maturity model; `T_DAYS = 365`. The simplified annual form `(1 + r)` is used for both legs, equivalent to setting `BASIS = 365` and using the `(1 + r × T/BASIS)` general form.
- **Day-count basis:** Model uses simplified annual compounding `(1 + r)`. A rigorous build would split into `BASIS_USD = 360` (ACT/360) and `BASIS_FC = 365` (ACT/365) — flagged as a refinement in §6.2.
- **Parity:** The money-market hedge replicates the covered-interest-parity forward. Any gap versus the scenario-given `F0_in` is a parity-check signal, not a model error.
- **Option premium:** Paid upfront in USD, quoted per 1 EUR (no contract multiplier). Carried forward at `R_USD` to place on the same footing as settlement-date USD proceeds.
- **Option style:** European — exercise only at maturity.
- **Strikes:** Both `K_PUT` and `K_CALL` default to `S0_in = 1.1530` (at-the-money). Override cells are exposed as yellow inputs.
- **Collar construction:** Long put + short call on the same EUR notional. Net premium = call received − put paid = $0.024 − $0.019 = **+$0.005/EUR** → **net credit of $100,000** on €20M.
- **Counterparty / credit risk:** Excluded. All derivatives assumed frictionless and creditworthy.
- **Transaction costs & bid-ask spreads:** Excluded from the base case. Flagged as a sensitivity candidate in §6.
- **Tax / accounting treatment:** Excluded. Model reports pre-tax cash outcomes only.
- **Scenario construction:** Future spot `S_T` is varied deterministically across the grid; no probability weights or implied-volatility distribution are applied.

---

## 4. Calculation Flow

Described in named-range pseudocode so the logic is portable across Excel, Python, and AI prompts. All formulas are written for a **receivable** exposure.

### Step 1 — Derived Inputs

```
DF_USD        = 1 + R_USD                          → 1.0430
DF_FC         = 1 + R_FC                           → 1.0250
FV_PREM_PUT   = −PREM_PUT × FC_AMT × DF_USD        → −$396,340
FV_PREM_CALL  = −PREM_CALL × FC_AMT × DF_USD       → −$500,640
```

### Step 2 — Forward Hedge (Certainty Benchmark)

```
USD_FWD = FC_AMT × F0_in
        = 20,000,000 × 1.0935
        = $21,870,000
```

Locked-in at t₀; invariant across the `S_T` grid.

### Step 3 — Money-Market Hedge (Parity Check)

Three-step logic:

```
Step 3a: Borrow PV of EUR receivable today
         PV_FC = FC_AMT / DF_FC = 20,000,000 / 1.025 = 19,512,195 EUR

Step 3b: Convert EUR to USD at spot
         USD_0 = PV_FC × S0_in = 19,512,195 × 1.1530 = $22,497,561

Step 3c: Invest USD to maturity
         USD_MM = USD_0 × DF_USD = 22,497,561 × 1.043 = $23,465,066
```

> **Parity check:** `USD_MM ≈ $23,465,066` vs. `USD_FWD = $21,870,000`. The ~$1.6M gap reflects the divergence between the scenario-given forward (1.0935) and the parity-implied forward (~1.1733). This is documented in the parity-check row of the model summary.

### Step 4 — Option Hedges

**Put option (floor with upside):**

```
USD_PUT(S_T) = S_T × FC_AMT + MAX(K_PUT − S_T, 0) × FC_AMT + FV_PREM_PUT
             = MAX(S_T, K_PUT) × FC_AMT + FV_PREM_PUT
```

At `S_T = S0_in = 1.1530`: `USD_PUT = 1.1530 × 20M + 0 − 395,340 = $22,664,660`

**Call sold (collar cap leg):**

```
CALL_PAYOFF(S_T) = −MAX(S_T − K_CALL, 0) × FC_AMT    (short call)
USD_CALL(S_T) = S_T × FC_AMT + CALL_PAYOFF(S_T) + FV_PREM_CALL
```

**Collar (put + short call):**

```
USD_COLLAR(S_T) = S_T × FC_AMT
                + MAX(K_PUT − S_T, 0) × FC_AMT        (put payoff)
                − MAX(S_T − K_CALL, 0) × FC_AMT       (short call payoff)
                − (PREM_PUT − PREM_CALL) × FC_AMT     (net premium: credit)
```

Since `K_PUT = K_CALL`, the collar produces bounded outcomes:
- Floor: `K_PUT × FC_AMT + NET_COLLAR_PREM` = `$23,060,000 + $100,000 = $23,160,000`
- Cap: same as floor at equal strikes → collar collapses to a synthetic forward with a small credit

### Step 5 — Sensitivity Table

For each `S_T` in `S_T_grid` where `S_T = S0_in × (1 + n/100)` for n = −5 to +5:

| Column | Output | Formula |
|--------|--------|---------|
| S_T | Ending spot rate | `S0_in × (1 + n%)` |
| % vs S₀ | Move from spot | `n%` |
| No Hedge | `USD_NO_HEDGE` | `S_T × FC_AMT` |
| Forward | `USD_FWD` | `FC_AMT × F0_in` (constant) |
| MM Hedge | `USD_MM` | `(FC_AMT / DF_FC) × S0_in × DF_USD` (constant) |
| Put Option | `USD_PUT(S_T)` | `MAX(S_T, K_PUT) × FC_AMT + FV_PREM_PUT` |
| Call Option | `USD_CALL(S_T)` | `MIN(S_T, K_CALL) × FC_AMT + FV_PREM_CALL` |
| Collar | `USD_COLLAR(S_T)` | See Step 4 formula above |

### Step 6 — Summary Metrics

```
USD_FLOOR_PUT     = MIN(USD_PUT across S_T_grid)    → ~$22,264,660
USD_BASE_k        = USD_k at S_T = S0_in            → see §5.1
HEDGE_PROFIT_k    = USD_k − USD_NO_HEDGE            → per-row, in sensitivity grid
```

---

## 5. Outputs

| Output | Description | Format | Purpose |
|--------|-------------|--------|---------|
| Input panel | All named-range inputs with units, sources, access dates | Top of model tab | Single source of truth |
| Strategy summary | `USD_FWD`, `USD_MM`, `USD_BASE_k` per strategy, `USD_FLOOR_PUT` | Gray summary table | Executive at-a-glance |
| Sensitivity table | USD proceeds for each strategy across `S_T_grid` ±5% | 11-row table | Core analytical evidence |
| Sensitivity chart | Line chart of USD outcome vs. `S_T`, one series per strategy | Embedded line chart | Visual comparison |
| Notes tab | Assumptions, data sources, model conventions | Separate tab | Reproducibility / audit |
| Blank template tab | Identical structure with values cleared | Second tab | Student / analyst reuse |
| Stage 4 recommendation | Executive memo with AI prompt | Separate `.md` deliverable | Downstream output |

### 5.1 Computed Base-Case Values (at S_T = S0_in = 1.1530)

| Strategy | USD Proceeds | Hedge Profit vs. No Hedge |
|----------|-----------:|-------------------------:|
| No Hedge | $23,060,000 | — |
| Forward Hedge | $21,870,000 | −$1,190,000 |
| Money Market Hedge | $23,464,956 | +$404,956 |
| Put Option | $22,680,000 | −$380,000 |
| Collar | $23,160,000 | +$100,000 |

> **Interpretation:** At `S_T = S0_in`, the forward hedge locks in a known shortfall vs. spot. The collar generates a small net credit ($100K) while providing a floor. The money-market hedge outperforms the given forward due to the parity gap documented in §4 Step 3. Note: the model deducts the raw premium (not future-valued) — the put hedge profit of −$380,000 equals the raw premium cost; future-valuing at R_USD would shift this to −$396,340, flagged as a Stage 4 improvement in §6.2.

---

## 6. Model Review — What Worked & What to Improve

### 6.1 What Worked

- **Five-strategy comparison on one canvas.** No hedge, forward, money market, put, and collar are all priced against the same `S_T` grid, making trade-off inspection immediate across the full ±5% range.
- **Color-coding convention applied throughout.** Yellow inputs, blue assumptions, green formula cells, and gray output/KPI cells allow a colleague to audit the model in under five minutes.
- **Named ranges on all 10 core inputs.** Every variable from `FC_AMT` through `PREM_CALL` is a named range, making the sensitivity table formulas readable pseudocode rather than opaque cell addresses.
- **Parity check row built in.** The summary section explicitly computes `USD_FWD − USD_MM` so any covered-interest-parity violation is visible immediately.
- **Notes & Assumptions tab.** All data sources, rate estimates, and modeling conventions are documented with access dates, satisfying the reproducibility requirement.
- **Blank template tab.** The second sheet copies all formatting and labels but clears all values, giving a clean starting point for a fresh build or a peer-review exercise.

### 6.2 What to Improve

- **Day-count basis is simplified.** The current model uses `(1 + r)` annual compounding. A production build should introduce `T_DAYS` and split `BASIS` into `BASIS_USD = 360` (ACT/360) and `BASIS_FC = 365` (ACT/365) so the model is reusable at non-annual tenors and uses market-standard day-count conventions for each leg.
- **Parity divergence is unresolved.** The scenario-given forward (1.0935) differs materially from the parity-implied forward (~1.1733). The model documents this gap but does not explain it. A production build should either (a) source a verified 1-year EUR/USD forward quote to reconcile, or (b) use the parity-derived forward consistently and note the scenario forward as an override.
- **Collar with equal strikes is a synthetic forward.** Because `K_PUT = K_CALL = S0_in`, the collar collapses to a bounded position equivalent to a synthetic forward plus a net credit. A more informative collar would set `K_PUT < S0_in` (out-of-the-money put) and `K_CALL > S0_in` (out-of-the-money call) to create a genuine range. This should be parameterized with separate input cells in Stage 4.
- **No hedge-profit columns in the sensitivity table.** The current table shows absolute USD proceeds but does not include `HEDGE_PROFIT_k = USD_k − USD_NO_HEDGE` sub-columns. Adding these isolates the hedge value-add and makes the trade-off quantitative rather than visual.
- **Winner / best-hedge labels absent.** The sensitivity table does not include `ARGMAX` label columns identifying the dominant strategy at each spot level. These are a useful UX addition for a non-quant reader.
- **Option premium is not future-valued.** The current model deducts the raw premium from proceeds rather than carrying it forward at `R_USD` to settlement. Strict financial accuracy requires `FV_PREM = PREM × FC_AMT × DF_USD` as shown in §4.
- **Transaction costs not modeled.** Bid-ask spreads on the forward and a bid-ask on option premia are excluded. Real treasury desks never transact at the mid. A sensitivity knob for spread cost should be added.
- **Sensitivity step size should be formula-driven.** The current grid hard-codes `S0_in × (1 ± n%)`. A cleaner build drives the grid off a single `STEP_FRAC` named range input so the range and granularity are adjustable from the inputs section.

### 6.3 Auditability Checklist

- [x] Every input has a standardized named range from §2.1
- [x] Color coding applied: yellow inputs, blue assumptions, green formulas, gray outputs
- [x] Forward hedge correctly computes `FC_AMT × F0_in`
- [x] Money-market hedge follows three-step logic
- [x] Parity check row present and labeled
- [x] Put payoff uses `MAX(K_PUT − S_T, 0)` correctly across the grid
- [x] Collar premium computed as net `PREM_CALL − PREM_PUT`
- [x] Sensitivity grid spans `S0_in × (1 ± 5%)` in 1% steps (11 rows)
- [x] Notes tab records all data sources with access date
- [ ] Option premium future-valued at `R_USD` *(to be corrected in Stage 4)*
- [ ] Hedge-profit columns added to sensitivity table *(Stage 4 improvement)*
- [ ] Collar strikes separated (`K_PUT ≠ K_CALL`) *(Stage 4 improvement)*
- [ ] `BASIS_USD` / `BASIS_FC` day-count split implemented *(Stage 4 improvement)*

---

## 7. Sensitivity Plan

- **Grid:** `S_T_grid` spans `S0_in × (1 ± 5%)` in 1% increments → 11 rows including the baseline at `S_T = S0_in`.
- **Strategies plotted:** no hedge, forward, money market, put option, call option, collar.
- **Primary chart:** line chart with `S_T` on the x-axis and USD proceeds on the y-axis. The forward and money-market series are horizontal by construction; the no-hedge series is a straight line through the origin; the put is piecewise-linear with a kink at `K_PUT`; the collar is bounded between floor and cap.
- **Key insight the chart communicates:** the trade-off between **certainty** (forward / money-market — flat lines), **optionality** (put — kinked payoff preserving upside), **bounded range** (collar — floored and capped), and **naked exposure** (no hedge — steepest slope, unbounded in both directions).
- **At S_T = −5% (1.09535):** No hedge produces $21,907,000; forward holds at $21,870,000; put floor activates — payoff = (1.1530 − 1.09535) × 20M = $1,153,000 — net proceeds = $21,907,000 + $1,153,000 − $380,000 = **$22,680,000**; collar floor also holds at **$23,160,000**.
- **At S_T = +5% (1.21065):** No hedge produces $24,213,000; put expires worthless so net proceeds = $24,213,000 − $380,000 = $23,833,000; collar upside is capped at $23,160,000 since short call activates; forward remains flat at $21,870,000.

---

## 8. Limitations & Next Steps

**Limitations.** This specification does not incorporate:

- Partial, layered, or dynamic hedging (model assumes static full-notional hedge executed at t₀)
- Credit, counterparty, and settlement risk on derivative instruments
- Implied-volatility-based option pricing (premia are scenario-given inputs, not Black-Scholes outputs)
- Accounting treatment (ASC 815 / IFRS 9 hedge accounting designation and OCI impact)
- Multi-currency or multi-horizon portfolio effects
- Bid-ask spreads, broker commissions, or margin requirements
- Probability-weighted expected outcomes (all scenarios equally weighted in the grid)

**Next steps — Stage 4 will:**

(a) Translate the sensitivity evidence into a structured CFO recommendation memo covering exposure summary, hedge outcomes table, sensitivity interpretation, strategic recommendation, and executive justification.

(b) Formalize the AI prompt using §4 as the instruction block and §6.2 as the improvement brief, with all named-range variables explicitly stated and color-coding / header conventions specified.

(c) Implement at least two §6.2 improvements — priority order: (1) future-value the option premium, (2) separate collar strikes into `K_PUT` and `K_CALL` with distinct input cells, (3) add hedge-profit columns to the sensitivity table.

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 1.0 | 2026-04-03 | Treasury Risk Management Analyst | Initial post-build draft — Scenario 4, Apex Aerospace |

---

## Appendix B — Brand & Formatting Standards

All FIN-321 deliverables conform to the **University of Hawaiʻi at Mānoa Brand Style Guide** as codified in `docs/_branding/design.json` (v1.0.0).

### B.1 Color Palette

| Token | Hex | Usage |
|-------|-----|-------|
| UH Green | `#024731` | Headings, section banners, key accents |
| Black | `#000000` | Body text, formula cells, borders |
| Silver | `#B2B2B2` | Subtle borders, rules, table gridlines |
| White | `#FFFFFF` | Page backgrounds, inverse text |
| Neutral-600 | `#525252` | Secondary text, captions, source notes |
| UH Green 50 | `#E6F2EF` | Callout backgrounds |
| Yellow *(Excel only)* | `#FFFF00` | Input cells — not a brand color |

### B.2 Workbook Color Convention

| Element | Color | Notes |
|---------|-------|-------|
| Input cells | Yellow fill `#FFFF00` | Editable by analyst |
| Assumption cells | Blue text `#0000FF` | Hardcoded scenario values |
| Formula cells | Black text `#000000` | All calculations |
| Cross-tab links | UH Green text `#024731` | Mirrors brand primary |
| Section header rows | UH Green fill, white text | Bold, 12 pt Arial |
| Output / KPI cells | Gray fill `#D9D9D9` | Summary results |

### B.3 File & Deliverable Conventions

- **Spec file name:** `stage3-spec-LASTNAME.md`
- **Workbook file name:** `lastname-first-stage2-model.xlsx`
- **Stage 1 memo:** `docs/decisions/stage1-memo.md`
- **All files** committed to GitHub with descriptive commit messages per the project workflow.

---

<div style="border-top: 1px solid #B2B2B2; padding-top: 8px; margin-top: 24px; font-family: 'Open Sans', Helvetica, Arial, sans-serif; font-size: 0.8rem; color: #525252;">
  Prepared per UH Mānoa brand standards (<code>docs/_branding/design.json</code> v1.0.0). Primary green <span style="display:inline-block; width:10px; height:10px; background:#024731; border:1px solid #000;"></span> <code>#024731</code> · Black <span style="display:inline-block; width:10px; height:10px; background:#000000; border:1px solid #B2B2B2;"></span> <code>#000000</code> · Silver <span style="display:inline-block; width:10px; height:10px; background:#B2B2B2; border:1px solid #000;"></span> <code>#B2B2B2</code> · Body type Open Sans Regular, 11–12 pt for printed copies · ADA-compliant contrast · Flush-left, ragged-right alignment · No red body type · No custom palettes or gradients.
</div>
