# Stage 4 — Final Analysis, Prompt Engineering & Recommendation

**TO:** Chief Financial Officer, Apex Aerospace Systems Inc.
**FROM:** Treasury Risk Management Analyst
**DATE:** April 3, 2026
**RE:** FX Hedge Strategy — Final Recommendation
**COURSE:** FIN 321 · Scenario 4 · Stage 4

---

## A. Exposure Summary

Apex Aerospace Systems Inc. is owed **€20,000,000** from a European aircraft components buyer, with payment due in approximately **365 days (April 2027)**. Because Apex reports in U.S. dollars, this receivable creates **transaction exposure**: every cent the euro depreciates between today and settlement reduces our realized USD proceeds.

At the spot rate of **1.1530 USD/EUR** recorded April 3, 2026, the receivable is currently worth **$23,060,000**. The 1-year forward market is already pricing the euro at a discount — 1.0935 — implying that holding the receivable unhedged and simply converting at maturity is not a neutral decision. It is an active bet that the euro will recover above the forward rate. Given that a 5% depreciation from spot would reduce proceeds to **$21,907,000**, a loss of over $1.15 million versus today's value, inaction carries meaningful downside. This analysis evaluates five strategies and delivers a clear recommendation.

---

## B. Summary of Hedge Outcomes

All results reflect the base case where the euro ends the year at today's spot rate (S_T = 1.1530). The sensitivity section below covers the full ±5% range.

| Strategy | USD Proceeds at S_T = Spot | vs. No Hedge | Key Insight |
|----------|---------------------------:|-------------:|-------------|
| No Hedge | $23,060,000 | — | Full upside and full downside; proceeds move dollar-for-dollar with EUR/USD |
| Forward Hedge | $21,870,000 | −$1,190,000 | Absolute certainty at the cost of missing any euro recovery above 1.0935 |
| Money Market Hedge | $23,464,956 | +$404,956 | Synthetic forward via borrow/convert/invest; outperforms the given forward due to the interest rate differential |
| Put Option | $22,680,000 | −$380,000 | Purchases a floor at today's spot; premium cost is $380,000 but upside is fully preserved above 1.1530 |
| Call Option (sold) | $22,580,000 | −$480,000 | Selling a call against the receivable generates premium income but hard-caps proceeds at the strike — unattractive as a standalone strategy for a EUR receiver |
| **Collar (Recommended)** | **$23,160,000** | **+$100,000** | Long put + short call; net premium is a **credit** of $100,000; floor is locked at $23,160,000 across all depreciation scenarios |

**A note on the forward vs. money market gap.** The scenario-given forward of 1.0935 diverges from the covered interest rate parity-implied rate of 1.1732 (= 1.1530 × 1.043 / 1.025). This means the money market hedge, which replicates parity, produces $23,464,956 — approximately $1.6 million more than the forward hedge. In practice, this gap signals either a wider market interest-rate differential than the estimates used here, or a risk premium baked into the quoted forward. The CFO should verify the live 1-year EUR/USD forward before execution; if the market forward is near 1.0935, the money market hedge is materially superior on this contract.

---

## C. Sensitivity Interpretation

The table below shows USD proceeds for each strategy as EUR/USD moves ±5% from today's spot.

| S_T | % Move | No Hedge | Forward | MM Hedge | Put Option | Collar |
|-----:|-------:|---------:|--------:|---------:|-----------:|-------:|
| 1.09535 | −5% | $21,907,000 | $21,870,000 | $23,464,956 | $22,680,000 | $23,160,000 |
| 1.10688 | −4% | $22,137,600 | $21,870,000 | $23,464,956 | $22,680,000 | $23,160,000 |
| 1.11841 | −3% | $22,368,200 | $21,870,000 | $23,464,956 | $22,680,000 | $23,160,000 |
| 1.12994 | −2% | $22,598,800 | $21,870,000 | $23,464,956 | $22,680,000 | $23,160,000 |
| 1.14147 | −1% | $22,829,400 | $21,870,000 | $23,464,956 | $22,680,000 | $23,160,000 |
| **1.15300** | **0%** | **$23,060,000** | **$21,870,000** | **$23,464,956** | **$22,680,000** | **$23,160,000** |
| 1.16453 | +1% | $23,290,600 | $21,870,000 | $23,464,956 | $22,910,600 | $23,160,000 |
| 1.17606 | +2% | $23,521,200 | $21,870,000 | $23,464,956 | $23,141,200 | $23,160,000 |
| 1.18759 | +3% | $23,751,800 | $21,870,000 | $23,464,956 | $23,371,800 | $23,160,000 |
| 1.19912 | +4% | $23,982,400 | $21,870,000 | $23,464,956 | $23,602,400 | $23,160,000 |
| 1.21065 | +5% | $24,213,000 | $21,870,000 | $23,464,956 | $23,833,000 | $23,160,000 |

**In depreciation scenarios (EUR falls):** The collar is the clear winner at every level from −1% to −5%, locking in $23,160,000 regardless of how far the euro falls. The put also activates and provides a floor, but at $22,680,000 — $480,000 below the collar floor — because of the net premium difference. The forward provides certainty but at the lowest absolute level ($21,870,000), and the money market hedge's flat $23,464,956 outperforms all other strategies. The unhedged position bleeds $230,000 for every 1% euro depreciation.

**In appreciation scenarios (EUR rises):** Above the current spot, the no-hedge position captures full upside — at +5%, proceeds reach $24,213,000. The put option participates in the upside above 1.1530, reaching $23,833,000 at +5% after the premium cost. The collar remains capped at $23,160,000 because the short call activates, forfeiting gains above the strike. The forward and money market hedge are entirely flat regardless of the euro's direction.

**Key takeaway:** The collar provides the best risk-adjusted outcome when the primary concern is **protecting the downside** while eliminating the premium burden. It dominates the forward in every scenario and dominates the put in all depreciation scenarios.

---

## D. Strategic Recommendation

**Recommended strategy: Collar (Long Put + Short Call at K = 1.1530)**

The collar is the superior strategy for Apex on this receivable. The core logic is simple: Apex *receives* the call premium it sells, which more than covers the put premium it buys, producing a **net premium credit of $100,000** ($480,000 received − $380,000 paid). This means Apex earns money on the hedge while simultaneously locking in a floor of **$23,160,000** — $1,290,000 above the forward contract's guaranteed level and $480,000 above the put-only floor.

The only trade-off is that EUR appreciation above 1.1530 is capped. Given that the 1-year forward market is already pricing the euro below today's spot (1.0935 vs. 1.1530), the market consensus does not expect significant EUR appreciation. Paying for full upside optionality under these conditions is not well-compensated.

**If the CFO requires absolute certainty and the priority is budget predictability above all else,** the money market hedge at $23,464,956 is the second choice — provided the live parity calculation is confirmed. It outperforms the quoted forward by $1.6 million and requires no option premium.

**If the CFO believes the euro will appreciate significantly above 1.1530,** the put option at $22,680,000 floor preserves full upside participation, though it costs $380,000 in premium relative to doing nothing.

The forward contract, while simple, is the least attractive among active hedges given its $21,870,000 locked-in level — $1,190,000 below today's spot value and $1,290,000 below the collar floor.

---

## E. Executive Justification

**Cash flow stability.** Apex's contract is for $20M+ in a single receivable. A drop to $21.9M if EUR falls 5% is not a rounding error — it is a $1.15M swing that can affect quarterly earnings, debt covenants, and capital allocation decisions. The collar eliminates this tail risk entirely and does so without cost.

**Budget certainty.** Treasury and FP&A teams need a reliable USD figure to build into annual budgets and cash flow forecasts. The collar's $23,160,000 floor is a number the business can plan around. Every dollar below that floor is a variance that requires explanation to senior management.

**Zero net premium cost.** Unlike a standalone put option, which requires an upfront cash outlay of $380,000, the collar is self-financing. The call premium received ($480,000) exceeds the put premium paid ($380,000) by $100,000 — Apex actually *receives* $100,000 net at inception. This removes the liquidity argument against hedging entirely.

**Optionality preserved within limits.** The collar does not sacrifice all upside. The floor at $23,160,000 already exceeds the unhedged outcome for every depreciation scenario from 0% to −5%. In flat or mildly appreciating markets, the collar still outperforms the forward and the put.

**Accounting implications (optional).** Under ASC 815 (U.S. GAAP) or IFRS 9, a qualifying cash flow hedge designation for this collar would allow changes in fair value to flow through Other Comprehensive Income (OCI) rather than the income statement, reducing earnings volatility. Proper hedge documentation, effectiveness testing, and version-controlled records — as maintained in this GitHub repository — are prerequisites for hedge accounting treatment and would also serve as audit evidence.

---

## F. Structured AI Prompt

The following prompt is designed to instruct an AI system (e.g., Claude, GPT-4) to regenerate the complete FX hedging Excel workbook from scratch, consistent with the Stage 3 technical specification.

---

```
# GOAL

You are a treasury financial modeling assistant. Build a complete, professional
FX hedging Excel workbook (.xlsx) for a EUR receivable exposure analysis. The
model must compare five strategies — forward hedge, money market hedge, put
option, call option, and collar — across a ±5% sensitivity range and produce
an executive-ready summary. Follow every instruction below precisely. Do not
infer missing values; use only what is explicitly provided.

---

# INPUT VARIABLES

Use the following named ranges as the single source of truth for all formulas.
Every formula in the workbook must reference these names — never hardcode values.

| Named Range | Value      | Unit     | Description                        |
|-------------|-----------|----------|------------------------------------|
| FC_AMT      | 20000000  | EUR      | Foreign currency receivable amount |
| S0_in       | 1.1530    | USD/EUR  | Spot rate at inception             |
| F0_in       | 1.0935    | USD/EUR  | 1-year forward rate                |
| R_USD       | 0.043     | Decimal  | USD 1-year interest rate (4.30%)   |
| R_FC        | 0.025     | Decimal  | EUR 1-year interest rate (2.50%)   |
| K_PUT       | 1.1530    | USD/EUR  | Put option strike (at-the-money)   |
| K_CALL      | 1.1530    | USD/EUR  | Call option strike (at-the-money)  |
| PREM_PUT    | 0.019     | USD/EUR  | Put premium per EUR (no multiplier)|
| PREM_CALL   | 0.024     | USD/EUR  | Call premium per EUR (no multiplier)|
| T_DAYS      | 365       | Days     | Time to settlement                 |

Data sources:
- S0_in: Yahoo Finance / RoboForex, April 3, 2026
- F0_in, FC_AMT, PREM_PUT, PREM_CALL: FIN 321 Scenario 4 assignment
- R_USD: U.S. 1-year Treasury bill rate, April 3, 2026
- R_FC: ECB deposit rate estimate, April 3, 2026

---

# WORKBOOK STRUCTURE

Create a workbook with the following tabs in order:
1. FX Hedge Model  (primary model)
2. Blank Template  (identical structure, all values and formulas cleared)
3. Notes & Assumptions  (data sources, conventions, change log)

---

# MODEL LOGIC

## Section 1 — Inputs (Yellow Cells)
- Create a table with columns: Variable | Named Range | Value | Units | Source
- Enter all 10 named ranges from the INPUT VARIABLES table above
- Highlight all value cells with YELLOW fill (#FFFF00)
- All value cells must be the only cells an analyst needs to change for a new scenario
- Define Excel named ranges for every variable using the names above

## Section 2 — Forward Hedge
Formula: USD_FWD = FC_AMT × F0_in
- Label result cell clearly; apply GRAY fill (#D9D9D9) to all output cells
- This value must be constant (does not vary with S_T)

## Section 3 — Money Market Hedge (3-Step Logic)
Show each sub-step explicitly with labels, formulas, and computed values:

  Step 3a — Borrow PV of EUR receivable:
    PV_FC = FC_AMT / (1 + R_FC)

  Step 3b — Convert EUR to USD at spot:
    USD_0 = PV_FC × S0_in

  Step 3c — Invest USD to maturity:
    USD_MM = USD_0 × (1 + R_USD)

- After Step 3c, add a PARITY CHECK row:
    Parity_Diff = USD_FWD − USD_MM
  Label it: "Parity Check (should be ≈ $0; any gap reflects forward rate divergence)"

## Section 4 — Option Hedges (at a Single S_T)
- Add a yellow input cell for Assumed S_T (default = S0_in)
- Compute for Put, Call, and Collar side-by-side:

  Total Premium Cost:
    Put:    FC_AMT × PREM_PUT
    Call:   FC_AMT × PREM_CALL
    Collar: (PREM_PUT − PREM_CALL) × FC_AMT   [negative = net credit]

  Option Payoff:
    Put payoff:    MAX(K_PUT − S_T, 0) × FC_AMT
    Call payoff:   −MAX(S_T − K_CALL, 0) × FC_AMT   [short call]
    Collar payoff: Put payoff + Call payoff

  Net USD Proceeds:
    Put:    S_T × FC_AMT + Put payoff    − FC_AMT × PREM_PUT
    Call:   S_T × FC_AMT + Call payoff   − FC_AMT × PREM_CALL
    Collar: S_T × FC_AMT + Collar payoff − (PREM_PUT − PREM_CALL) × FC_AMT

- Apply GREEN fill (#E2EFDA) to all formula/calculation cells
- Apply GRAY fill (#D9D9D9) to all Net USD Proceeds output cells

## Section 5 — Sensitivity Table (±5% Range)
- Create a table with 11 rows (n = −5, −4, −3, −2, −1, 0, +1, +2, +3, +4, +5)
- Columns: S_T | % vs S₀ | No Hedge | Forward | MM Hedge | Put | Call | Collar
- Formulas for each row (using named ranges only):
    S_T       = S0_in × (1 + n/100)
    No Hedge  = S_T × FC_AMT
    Forward   = FC_AMT × F0_in
    MM Hedge  = (FC_AMT / (1 + R_FC)) × S0_in × (1 + R_USD)
    Put       = S_T × FC_AMT + MAX(K_PUT − S_T, 0) × FC_AMT − PREM_PUT × FC_AMT
    Call      = S_T × FC_AMT − MAX(S_T − K_CALL, 0) × FC_AMT − PREM_CALL × FC_AMT
    Collar    = S_T × FC_AMT + MAX(K_PUT−S_T,0)×FC_AMT − MAX(S_T−K_CALL,0)×FC_AMT
                − (PREM_PUT − PREM_CALL) × FC_AMT
- Highlight the S_T = S0_in baseline row in YELLOW
- Add an embedded LINE CHART: S_T on x-axis, USD proceeds on y-axis, one colored series per strategy

## Section 6 — Summary Output (Gray Cells)
Create a summary KPI table with columns: Strategy | USD Proceeds | vs. No Hedge | Comments
Rows (using formulas referencing cells above, not hardcoded values):
  - No Hedge (at Spot)
  - Forward Hedge
  - Money Market Hedge
  - Put Option
  - Call Option
  - Collar
  - [Blank row labeled: "Hedge Recommendation — To be completed in Stage 4"]

---

# COLOR CODING CONVENTION

Apply strictly throughout the entire workbook:
| Color   | Fill Hex | Usage                                |
|---------|----------|--------------------------------------|
| Yellow  | #FFFF00  | All user-editable inputs             |
| Blue    | #BDD7EE  | Hardcoded assumptions (labeled)      |
| Green   | #E2EFDA  | All formula / calculation cells      |
| Gray    | #D9D9D9  | All output / summary / KPI cells     |
| White   | #FFFFFF  | Non-data rows, labels                |
| Navy    | #1F3864  | Section header backgrounds           |

Section header rows: Navy fill (#1F3864), white bold text, 11pt Arial
All body text: 10pt Arial
Column headers in tables: Navy fill, white bold text

---

# VERIFICATION CHECKS

After building the model, verify the following — each check should be a visible
labeled row in the workbook:

1. PARITY CHECK: Parity_Diff = USD_FWD − USD_MM
   Expected: approximately $0 if using parity-implied forward;
   a non-zero gap here flags that the given forward deviates from interest rate parity.

2. OPTION LOGIC CHECK: At S_T = K_PUT = 1.1530, the put payoff must equal $0.
   Net put proceeds = S0_in × FC_AMT − PREM_PUT × FC_AMT = $22,680,000

3. COLLAR NET PREMIUM: (PREM_CALL − PREM_PUT) × FC_AMT must equal +$100,000 (net credit).

4. SENSITIVITY CONTINUITY: No Hedge column must increase by exactly
   S0_in × 0.01 × FC_AMT = $23,060 per 1% S_T step.

5. ZERO FORMULA ERRORS: All cells must resolve to numeric values.
   No #REF!, #VALUE!, #DIV/0!, #N/A, or #NAME? errors permitted.

---

# FORMATTING REQUIREMENTS

- Font: Arial throughout
- Section headers: 11–12pt bold, navy fill, white text, border
- Body: 10pt
- Currency cells: $#,##0 format
- Percentage cells: 0.0% format
- Exchange rate cells: 0.0000 format
- All rows: minimum height 16pt; header rows 20pt
- Column widths: label columns ≥ 28 characters wide; value columns ≥ 14 wide
- Freeze panes at row 7 (below inputs header)
- All tables must have visible borders using thin silver (#B2B2B2) gridlines

---

# NOTES & ASSUMPTIONS TAB

Include a separate tab titled "Notes & Assumptions" with the following documented:
- All 10 named-range sources with access dates
- Day-count convention (simplified annual: 1 + r)
- Option style (European; exercise at maturity only)
- Parity convention and any noted divergence
- Transaction costs and bid-ask spreads (excluded; reason stated)
- Color coding legend
- Stage reference (companion spec: stage3-spec-LASTNAME.md)

---

# EXPORT

Save the workbook as: lastname-first-stage2-model.xlsx
Two tabs minimum: "FX Hedge Model" (filled) and "Blank Template" (structure only, values cleared).
Return confirmation of zero formula errors from a recalculation check.
```

---

## Extra Credit — Areas for Further Study

### 1. AI Skills & Automation

The structured prompt in Section F demonstrates what modern AI-augmented treasury workflows look like at their most basic level: a human writes a spec, an AI executes it. But the pipeline can go much further. A Claude Skill configured with the Stage 3 specification and live market data access could, on any given day, pull the current EUR/USD spot rate and 1-year forward from Bloomberg or Yahoo Finance, substitute the live values into the named-range input table, regenerate the sensitivity model, and return a fully recalculated workbook — all from a single `/rebuild-hedge-model` command. Monte Carlo simulation is a natural extension: rather than a deterministic ±5% grid, the AI could run 10,000 paths of EUR/USD drawn from a GBM process calibrated to implied volatility from the options market, producing probability-weighted expected proceeds and Value-at-Risk metrics for each strategy. This would transform the Stage 2 model from a scenario snapshot into a live risk management tool, updated daily, with no manual intervention from the treasury team.

### 2. GitHub & Version Control as Audit Infrastructure

Every commit in this project — Stage 1 memo, Stage 2 workbook, Stage 3 specification, Stage 4 final analysis — represents a timestamped, tamper-evident record of the analytical process. Under ASC 815 hedge accounting, firms must document hedge designation, risk management objective, and effectiveness assessment *at inception* and maintain that documentation throughout the hedge's life. A GitHub repository satisfies all three requirements: the commit timestamp proves when the hedge was designated, the diff history shows every revision to the model and its rationale, and the tagged release at each stage produces a clean audit trail that a Big 4 auditor can follow without a single email exchange. The AI prompt in Section F strengthens this further — if the model ever needs to be reconstructed to resolve an audit question, the prompt provides a reproducible, version-controlled set of instructions that will regenerate the identical output. This is the financial modeling equivalent of a reproducible research pipeline in science, and it is increasingly what sophisticated treasury functions and their auditors expect.

---

*Prepared for FIN 321 — International Business Finance | Shidler College of Business, University of Hawaiʻi at Mānoa | Stage 4 Final Analysis | April 3, 2026*
