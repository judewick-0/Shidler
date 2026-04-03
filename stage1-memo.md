# Executive Memorandum

**TO:** Chief Financial Officer, Apex Aerospace Systems Inc.
**FROM:** Treasury Risk Management Analyst
**DATE:** April 3, 2026
**RE:** EUR/USD Foreign Exchange Exposure — Hedging Strategy Recommendation
**COURSE:** FIN 321 · Scenario 4 · Stage 1

---

## 1. Exposure

Apex Aerospace Systems Inc. has contracted to deliver aircraft components to a European buyer. Under the agreement, we are owed **€20,000,000**, payable in euros, with settlement due in approximately **one year** (April 2027).

Because Apex reports in U.S. dollars, this creates a **transaction exposure**: the USD value of our receivable will fluctuate with the EUR/USD exchange rate between now and the payment date. At today's spot rate of **1.1530 USD/EUR** (Yahoo Finance, April 3, 2026), the receivable is currently worth approximately **$23,060,000**.

---

## 2. The Risk

The core risk is straightforward: if the euro weakens against the dollar over the next year, we receive fewer dollars when the payment arrives. The one-year forward rate is already quoted at **1.0935** — meaning the market implies the euro will trade at a discount to today's spot. If we do nothing and the forward rate proves accurate, we would receive approximately **$21,870,000** — roughly **$1,190,000 less** than today's spot value.

A further 5% drop in the euro from current levels (to ~1.0954) would cost us an additional ~$1.15M on top of that. On a $23M receivable, exchange rate moves are not a rounding error — they are a material business risk.

---

## 3. Three Hedging Strategies

### Forward Contract
Lock in the sale of €20,000,000 today at the one-year forward rate of 1.0935, guaranteeing USD proceeds of **$21,870,000** regardless of where the spot rate lands at maturity.

- **Pro:** Complete certainty. Zero cost. Simple to execute.
- **Con:** We give up all upside if the euro strengthens above 1.0935.

### Put Option on EUR
Purchase the right — but not the obligation — to sell euros at a strike near today's spot (1.1530). The premium is **$0.019 per EUR**, or approximately **$380,000** total on €20M.

- **Pro:** Provides a floor on USD proceeds while preserving upside if the euro rises.
- **Con:** Upfront premium cost reduces net proceeds regardless of outcome.

### Collar (Put + Call)
Buy the put ($0.019/EUR) and simultaneously sell a call at the same strike ($0.024/EUR). The call premium collected (~$480,000) more than offsets the put cost (~$380,000), producing a **net credit of ~$100,000**.

- **Pro:** Downside protection at near-zero net cost; small premium income.
- **Con:** Caps upside — if the euro rallies strongly, gains above the call strike are forfeited.

---

## 4. Next Steps

This memo frames the decision. The following three stages will build out the full analysis:

- **Stage 2 (Excel Model):** Build a working spreadsheet computing forward, money market, and option hedge outcomes across a ±5% sensitivity range of EUR/USD at maturity.
- **Stage 3 (Technical Spec):** Document the model's inputs, logic, and assumptions in a formal specification precise enough for another analyst — or an AI — to reconstruct.
- **Stage 4 (Final Analysis + AI Prompt):** Interpret the model results, write a structured AI prompt to regenerate the spreadsheet, and deliver a final executive hedge recommendation with full justification.

The preliminary case favors the **collar strategy** given its near-zero net cost and meaningful downside protection, but the final recommendation will be data-driven following the Stage 2 model build.

---

*Prepared for FIN 321 — International Business Finance | Shidler College of Business, University of Hawaiʻi at Mānoa | Stage 1 Executive Memo | April 3, 2026*
