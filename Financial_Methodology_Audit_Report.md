# Financial Methodology Audit Report: Break-Even Analysis

**Reviewer:** AI-Assisted Financial Audit  
**Date:** March 12, 2026  
**Subject:** Critical review of financial mathematics, assumptions, and methodology in `Break_even_analysis.ipynb`

---

## Executive Summary

This report evaluates the financial methodology used in the break-even analysis for a 50-aircraft fleet acquisition at \$120M each (\$6B total). The current model uses a simplified Discounted Cash Flow (DCF) approach that accumulates discounted fuel and maintenance savings until they equal the initial investment.

**Overall assessment:** The model contains **10 material methodological deficiencies** that would be flagged in any professional capital budgeting review. The break-even result of ~24 years is unreliable because the model omits tax effects, depreciation shields, residual values, proper cost of capital estimation, and risk quantification. The actual break-even point could be significantly shorter (if tax shields and residual values are included) or longer (if realistic financing costs and risk premia are applied).

---

## Current Model Summary

The existing notebook computes:

$$\text{Break-even year } T: \quad \sum_{t=1}^{T} \frac{N \cdot \left[C_1 + C_2 \cdot (1+g)^{t-1}\right]}{(1+r)^t} \geq C_0$$

Where:
- \(C_1 = 0.20 \times \$15\text{M} = \$3\text{M}\) (fuel savings per aircraft/year)
- \(C_2 = \$2\text{M}\) (maintenance savings per aircraft/year)
- \(g = 0.05\) (maintenance savings growth rate)
- \(r = 0.08\) (discount rate)
- \(N = 50\) aircraft, \(C_0 = \$6{,}000\text{M}\)

---

## Issue 1: Unjustified Discount Rate — Should Use WACC

### Problem

The model uses a flat 8% discount rate with no derivation or justification. In professional capital budgeting, the discount rate must reflect the project's actual cost of capital and risk profile.

### Recommended Replacement: Weighted Average Cost of Capital (WACC)

$$\text{WACC} = \frac{E}{V} \cdot R_e + \frac{D}{V} \cdot R_d \cdot (1 - T_c)$$

Where:
- \(E\) = market value of equity
- \(D\) = market value of debt
- \(V = E + D\) = total firm value
- \(R_e\) = cost of equity (derived from CAPM, see Issue 2)
- \(R_d\) = cost of debt (pre-tax)
- \(T_c\) = corporate tax rate

For airlines, typical WACC ranges from 7–12% depending on leverage and credit quality. The 8% value happens to be within range, but it must be **derived** rather than assumed.

### Citations

| Source | URL |
|---|---|
| Damodaran, A. — "The Cost of Capital: The Swiss Army Knife of Finance" | [https://pages.stern.nyu.edu/~adamodar/pdfiles/papers/costofcapital.pdf](https://pages.stern.nyu.edu/~adamodar/pdfiles/papers/costofcapital.pdf) |
| Damodaran Online — Data & Spreadsheets | [https://pages.stern.nyu.edu/~adamodar/](https://pages.stern.nyu.edu/~adamodar/) |
| Corporate Finance Institute — WACC Calculator | [https://corporatefinanceinstitute.com/resources/financial-modeling/wacc-calculator](https://corporatefinanceinstitute.com/resources/financial-modeling/wacc-calculator) |

---

## Issue 2: No Cost of Equity Derivation — Should Use CAPM

### Problem

The cost of equity is embedded in the arbitrary 8% rate. Professional practice requires deriving it from a risk model.

### Recommended Equation: Capital Asset Pricing Model (CAPM)

$$R_e = R_f + \beta \cdot (R_m - R_f)$$

Where:
- \(R_f\) = risk-free rate (e.g., 10-year U.S. Treasury yield, currently ~4.0–4.5%)
- \(\beta\) = equity beta of the airline (measures systematic risk; airline betas typically 1.0–1.5)
- \(R_m - R_f\) = equity risk premium (historically ~5–7%)

**Example for an airline with \(\beta = 1.3\):**
$$R_e = 4.25\% + 1.3 \times 5.5\% = 11.4\%$$

This is materially different from 8% and would lengthen the break-even period.

### Citations

| Source | URL |
|---|---|
| Investopedia — "Capital Asset Pricing Model (CAPM)" | [https://www.investopedia.com/terms/c/capm.asp](https://www.investopedia.com/terms/c/capm.asp) |
| Investopedia — "Using CAPM to Calculate Cost of Equity" | [https://www.investopedia.com/ask/answers/022515/how-do-i-use-capm-capital-asset-pricing-model-determine-cost-equity.asp](https://www.investopedia.com/ask/answers/022515/how-do-i-use-capm-capital-asset-pricing-model-determine-cost-equity.asp) |
| Wikipedia — "Capital Asset Pricing Model" | [https://en.wikipedia.org/wiki/Capital_Asset_Pricing_Model](https://en.wikipedia.org/wiki/Capital_Asset_Pricing_Model) |

---

## Issue 3: No Depreciation or Tax Shield

### Problem

The model completely ignores that aircraft are depreciable assets. Depreciation creates a **tax shield** — a non-cash expense that reduces taxable income and therefore actual tax paid. This is one of the largest omissions in the current analysis.

### Recommended Addition: Depreciation Tax Shield

Under the U.S. Modified Accelerated Cost Recovery System (MACRS), commercial aircraft (Part 135) follow a **7-year recovery period**:

| Year | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| **MACRS %** | 14.29% | 24.49% | 17.49% | 12.49% | 8.93% | 8.92% | 8.93% | 4.46% |

The annual tax shield for each aircraft:

$$\text{Tax Shield}_t = D_t \times T_c$$

Where \(D_t\) is the depreciation expense in year \(t\) and \(T_c\) is the corporate tax rate (U.S. federal: 21%).

**Example — Year 1 for 50 aircraft at \$120M each:**
$$D_1 = 50 \times \$120\text{M} \times 14.29\% = \$857.4\text{M}$$
$$\text{Tax Shield}_1 = \$857.4\text{M} \times 21\% = \$180.1\text{M}$$

This \$180M tax benefit in Year 1 alone is nearly equal to the model's entire Year 1 discounted savings of \$245M, yet it is completely ignored.

### Citations

| Source | URL |
|---|---|
| NBAA — "Depreciation Schedule for Business Aircraft" | [https://nbaa.org/flight-department-administration/tax-issues/depreciation/depreciation-schedule-for-business-aircraft/](https://nbaa.org/flight-department-administration/tax-issues/depreciation/depreciation-schedule-for-business-aircraft/) |
| Investopedia — "Modified Accelerated Cost Recovery System (MACRS)" | [https://www.investopedia.com/terms/m/macrs.asp](https://www.investopedia.com/terms/m/macrs.asp) |
| IRS Publication 946 — "How to Depreciate Property" | [https://www.irs.gov/pub/irs-pdf/p946.pdf](https://www.irs.gov/pub/irs-pdf/p946.pdf) |
| AvBuyer — "Business Aviation Tax: Benefits of MACRS & ADS" | [https://www.avbuyer.com/articles/aircraft-ownership/business-aviation-tax-the-benefits-of-macrs-amp-ads-58458](https://www.avbuyer.com/articles/aircraft-ownership/business-aviation-tax-the-benefits-of-macrs-amp-ads-58458) |

---

## Issue 4: No Residual / Salvage Value

### Problem

Aircraft are not worthless at the end of their economic life. The model assumes the \$6B investment is fully consumed, when in reality aircraft retain 10–30% of their original value after 20–25 years (more if well-maintained). This overstates the effective cost of the investment.

### Recommended Addition: Terminal Salvage Value

$$\text{NPV}_{\text{adjusted}} = -C_0 + \sum_{t=1}^{T} \frac{\text{FCF}_t}{(1+\text{WACC})^t} + \frac{S_T}{(1+\text{WACC})^T}$$

Where \(S_T\) is the estimated salvage value of the fleet at year \(T\).

**Example:** If 50 aircraft retain 15% of their \$120M purchase price after 25 years:
$$S_{25} = 50 \times \$120\text{M} \times 15\% = \$900\text{M}$$
$$\text{PV of salvage} = \frac{\$900\text{M}}{(1.08)^{25}} = \$131.5\text{M}$$

### Citations

| Source | URL |
|---|---|
| IATA — "Airline Disclosure Guide: Aircraft Acquisition Cost and Depreciation" | [https://www.iata.org/contentassets/4a4b100c43794398baf73dcea6b5ad42/airline-disclosure-guide-aircraft-acquisition.pdf](https://www.iata.org/contentassets/4a4b100c43794398baf73dcea6b5ad42/airline-disclosure-guide-aircraft-acquisition.pdf) |
| IBA Group — Aviation Valuations | [https://www.iba.aero/advisory-services/valuations/](https://www.iba.aero/advisory-services/valuations/) |
| IATA — "Guidance Material and Best Practices for Aircraft Leases" | [https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/ac-leases-4th-edition.pdf](https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/ac-leases-4th-edition.pdf) |

---

## Issue 5: Cash Flows Are Not Free Cash Flows (FCFF)

### Problem

The model only considers fuel and maintenance savings. A proper capital budgeting analysis uses **Free Cash Flow to the Firm (FCFF)**, which accounts for taxes, depreciation, capital expenditures, and working capital changes.

### Recommended Replacement: FCFF-Based Cash Flows

$$\text{FCFF}_t = \text{EBIT}_t \times (1 - T_c) + \text{Depreciation}_t - \text{CapEx}_t - \Delta \text{NWC}_t$$

For this project, the annual FCFF should be:

$$\text{FCFF}_t = (\text{Fuel Savings}_t + \text{Maintenance Savings}_t - \text{Incremental OpEx}_t) \times (1 - T_c) + \text{Depreciation}_t \times T_c - \text{Recurring CapEx}_t$$

Key additions beyond the current model:
- **Tax on savings:** Savings increase taxable income, so only \((1 - T_c)\) of gross savings is realized
- **Depreciation tax shield:** Added back as a non-cash benefit
- **Recurring CapEx:** Heavy maintenance checks (C-checks every 6–8 years, D-checks every 10–12 years) are capital expenditures, not operating costs
- **Working capital:** Spare parts inventory, prepaid maintenance reserves

### Citations

| Source | URL |
|---|---|
| Investopedia — "Free Cash Flow to the Firm (FCFF)" | [https://www.investopedia.com/terms/f/freecashflowfirm.asp](https://www.investopedia.com/terms/f/freecashflowfirm.asp) |
| Corporate Finance Institute — "FCFF Formula" | [https://corporatefinanceinstitute.com/resources/financial-modeling/free-cash-flow-to-firm-fcff](https://corporatefinanceinstitute.com/resources/financial-modeling/free-cash-flow-to-firm-fcff) |
| CFA Institute — "Free Cash Flow Valuation" | [https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/free-cash-flow-valuation](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/free-cash-flow-valuation) |

---

## Issue 6: No IRR, MIRR, or Profitability Index

### Problem

The model only computes a break-even year. Professional capital budgeting requires **multiple complementary metrics** to evaluate a project. A single metric can be misleading.

### Recommended Additions

#### (a) Net Present Value at Project Horizon

$$\text{NPV} = -C_0 + \sum_{t=1}^{n} \frac{\text{FCFF}_t}{(1+\text{WACC})^t} + \frac{S_n}{(1+\text{WACC})^n}$$

**Decision rule:** Accept if NPV > 0.

#### (b) Internal Rate of Return (IRR)

Solve for \(r^*\) such that:

$$0 = -C_0 + \sum_{t=1}^{n} \frac{\text{FCFF}_t}{(1+r^*)^t} + \frac{S_n}{(1+r^*)^n}$$

**Decision rule:** Accept if \(r^* > \text{WACC}\).

#### (c) Modified Internal Rate of Return (MIRR)

$$\text{MIRR} = \left(\frac{FV(\text{positive CFs at reinvestment rate})}{|PV(\text{negative CFs at finance rate})|}\right)^{1/n} - 1$$

MIRR is preferred over IRR because it assumes reinvestment at the firm's cost of capital rather than at the IRR itself (which is often unrealistic).

#### (d) Profitability Index (PI)

$$\text{PI} = \frac{\sum_{t=1}^{n} \frac{\text{FCFF}_t}{(1+\text{WACC})^t}}{C_0}$$

**Decision rule:** Accept if PI > 1.0. A PI of 1.15 means the project returns \$1.15 in present value for every \$1.00 invested.

### Citations

| Source | URL |
|---|---|
| CFA Institute — "Capital Investments and Capital Allocation" | [https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2025/capital-investments-and-capital-allocation](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2025/capital-investments-and-capital-allocation) |
| Investopedia — "Modified Internal Rate of Return (MIRR)" | [https://www.investopedia.com/terms/m/mirr.asp](https://www.investopedia.com/terms/m/mirr.asp) |
| Corporate Finance Institute — "Capital Planning Metrics: NPV, IRR, PI" | [https://corporatefinanceinstitute.com/resources/valuation/capital-planning-metrics-guide/](https://corporatefinanceinstitute.com/resources/valuation/capital-planning-metrics-guide/) |
| AnalystNotes — CFA Level I: NPV, IRR, PI | [https://analystnotes.com/cfa-study-notes-calculate-and-interpret-net-present-value-npv-internal-rate-of-return-irr-payback-period-discounted-payback-period-and-profitability-index-pi-of-a-single-capital-project.html](https://analystnotes.com/cfa-study-notes-calculate-and-interpret-net-present-value-npv-internal-rate-of-return-irr-payback-period-discounted-payback-period-and-profitability-index-pi-of-a-single-capital-project.html) |

---

## Issue 7: Maintenance Cost Model Is Oversimplified

### Problem

The model assumes maintenance savings grow at a constant 5% annually forever. In reality, aircraft maintenance costs follow a **"bathtub curve"**:
1. **Early life (Years 1–3):** Higher costs due to teething issues and warranty-related work
2. **Mid-life (Years 4–12):** Lower, stable routine maintenance
3. **Late life (Years 13+):** Sharply escalating costs due to major structural inspections (C-checks every 6–8 years at \$1–3M each, D-checks every 10–12 years at \$5–12M each)

### Recommended Replacement: Step-Function or Empirical Maintenance Model

Rather than a single geometric growth rate, use a piecewise function:

$$C_{\text{maint}}(t) = \begin{cases} C_{\text{base}} & \text{if } 1 \leq t \leq 5 \\ C_{\text{base}} \cdot (1 + g_1)^{t-5} & \text{if } 6 \leq t \leq 12 \\ C_{\text{base}} \cdot (1 + g_1)^7 \cdot (1 + g_2)^{t-12} & \text{if } t > 12 \end{cases}$$

Where \(g_1 \approx 3\%\) (moderate escalation) and \(g_2 \approx 8\text{–}10\%\) (aggressive late-life escalation). Additionally, **overlay discrete major check costs** in the years they occur.

### Citations

| Source | URL |
|---|---|
| IATA — "Managing and Costing for Aging Aircraft" | [https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/mcaa-1sted-2018.pdf](https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/mcaa-1sted-2018.pdf) |
| Investopedia — "Operating Cost Structure in Airlines" | [https://www.investopedia.com/terms/o/operating-cost.asp](https://www.investopedia.com/terms/o/operating-cost.asp) |

---

## Issue 8: Revenue Model Is Static — No Growth, No Variability

### Problem

The model assumes \$15M annual revenue per aircraft forever. This is unrealistic because:
- Revenue depends on **load factor** (typically 75–87% for airlines), which fluctuates
- Airfares are subject to **yield management** and competitive pressures
- **Inflation** causes nominal revenue to grow over time
- Economic cycles cause demand shocks (e.g., COVID-19 reduced airline revenue by 60%+ in 2020)

### Recommended Improvement

Apply at minimum a nominal revenue growth rate:

$$R_t = R_0 \cdot (1 + g_R)^t$$

Where \(g_R\) is the expected nominal revenue growth rate (airline industry long-run average: ~3–5% nominal). Better yet, model revenue as a function of load factor and yield:

$$R_t = N_{\text{seats}} \times \text{LF}_t \times \text{Yield}_t \times \text{Flights/Year}$$

### Citations

| Source | URL |
|---|---|
| IATA — "Airline Industry Economic Performance" | [https://www.iata.org/en/iata-repository/publications/economic-reports/](https://www.iata.org/en/iata-repository/publications/economic-reports/) |

---

## Issue 9: No Sensitivity Analysis

### Problem

The model produces a single deterministic break-even year with no indication of uncertainty. If the discount rate were 10% instead of 8%, or fuel savings were 15% instead of 20%, the break-even could shift by many years. A financial auditor would flag this as a critical omission.

### Recommended Addition: Sensitivity and Scenario Analysis

#### (a) One-Way Sensitivity (Tornado Diagram)

Vary each input parameter by ±10–20% while holding others constant, and report the change in break-even year / NPV:

| Parameter | Base | Low (−20%) | High (+20%) | NPV Impact |
|---|---|---|---|---|
| Discount rate | 8% | 6.4% | 9.6% | High |
| Fuel savings % | 20% | 16% | 24% | High |
| Aircraft cost | \$120M | \$96M | \$144M | High |
| Maintenance growth | 5% | 4% | 6% | Medium |
| Fleet size | 50 | 40 | 60 | Medium |

#### (b) Monte Carlo Simulation

Assign probability distributions to key inputs and run 10,000+ iterations:

$$\text{NPV}_i = -C_0 + \sum_{t=1}^{n} \frac{\text{FCFF}_t(\omega_i)}{(1+\text{WACC}(\omega_i))^t}$$

Report the **probability distribution of NPV** and the **probability that NPV < 0** (i.e., the probability of a loss).

### Citations

| Source | URL |
|---|---|
| Investopedia — "Sensitivity Analysis" | [https://www.investopedia.com/terms/s/sensitivityanalysis.asp](https://www.investopedia.com/terms/s/sensitivityanalysis.asp) |
| CFA Institute — "Capital Investments and Capital Allocation" (includes scenario analysis) | [https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2025/capital-investments-and-capital-allocation](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2025/capital-investments-and-capital-allocation) |

---

## Issue 10: No Financing Structure — Ignores Debt/Equity Mix and Leasing

### Problem

The model treats the \$6B as a single lump-sum cash outflow. In reality, airlines finance aircraft through complex structures:

- **Operating leases** (off-balance-sheet under old standards, now on-balance-sheet under IFRS 16 / ASC 842)
- **Finance leases** (capital leases)
- **Enhanced Equipment Trust Certificates (EETCs)** — the most common aircraft debt instrument
- **Export credit financing** (e.g., Ex-Im Bank guarantees)
- **Equity** (retained earnings or share issuance)

The financing mix materially changes:
1. The **timing of cash flows** (lease payments vs. upfront purchase)
2. The **tax treatment** (lease payments are fully deductible vs. depreciation + interest deductions for owned aircraft)
3. The **effective cost of capital** (secured aircraft debt can be as low as 3–5%)

### Recommended Improvement

Model at least two scenarios:

**Scenario A — Full Purchase (Debt + Equity):**
$$\text{Annual Debt Service} = C_0 \times \frac{D}{V} \times \frac{R_d}{1 - (1+R_d)^{-n}}$$

**Scenario B — Operating Lease:**
$$\text{Annual Lease Cost} = \text{Lease Rate Factor} \times \text{Aircraft Value}$$

Typical lease rate factors for narrowbodies: 0.7–0.9% per month of aircraft value.

### Citations

| Source | URL |
|---|---|
| IATA — "International Aircraft Financing" | [https://www.iata.org/en/publications/manuals/international-aircraft-financing](https://www.iata.org/en/publications/manuals/international-aircraft-financing) |
| IATA — "Guidance Material and Best Practices for Aircraft Leases" | [https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/ac-leases-4th-edition.pdf](https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/ac-leases-4th-edition.pdf) |

---

## Consolidated Recommendation: Corrected Break-Even Framework

Replacing the current naive model, the auditor-recommended break-even computation should solve for \(T\) in:

$$\sum_{t=1}^{T} \frac{\text{FCFF}_t}{(1+\text{WACC})^t} + \frac{S_T}{(1+\text{WACC})^T} \geq C_0$$

Where:

$$\text{FCFF}_t = \left[\Delta\text{Revenue}_t + \Delta\text{FuelSavings}_t + \Delta\text{MaintSavings}_t - \Delta\text{OpEx}_t\right] \times (1-T_c) + D_t \times T_c - \text{CapEx}_t - \Delta\text{NWC}_t$$

And WACC is derived from:

$$\text{WACC} = \frac{E}{V} \cdot \left[R_f + \beta(R_m - R_f)\right] + \frac{D}{V} \cdot R_d \cdot (1 - T_c)$$

Additionally, the analysis should report: **NPV**, **IRR**, **MIRR**, **PI**, a **sensitivity tornado diagram**, and ideally a **Monte Carlo probability distribution of outcomes**.

---

## Summary of All Issues

| # | Issue | Severity | Impact on Break-Even |
|---|---|---|---|
| 1 | Discount rate is assumed, not derived (should use WACC) | **High** | Could shift ±3–5 years |
| 2 | No CAPM for cost of equity | **High** | Affects WACC input |
| 3 | No depreciation tax shield | **Critical** | Shortens break-even by 3–5 years |
| 4 | No residual / salvage value | **High** | Shortens break-even by 1–3 years |
| 5 | Cash flows are not FCFF (no tax, no working capital) | **Critical** | Fundamentally changes all results |
| 6 | Only break-even reported (no NPV, IRR, MIRR, PI) | **High** | Missing decision-critical metrics |
| 7 | Maintenance cost model is oversimplified | **Medium** | Late-life costs underestimated |
| 8 | Revenue is static (no growth, no variability) | **Medium** | Understates long-run savings |
| 9 | No sensitivity or Monte Carlo analysis | **High** | No risk quantification |
| 10 | No financing structure modeled | **High** | Timing and tax effects ignored |

---

## Master Citation List

| # | Source | URL |
|---|---|---|
| 1 | Damodaran, A. — "The Cost of Capital: The Swiss Army Knife of Finance" | [https://pages.stern.nyu.edu/~adamodar/pdfiles/papers/costofcapital.pdf](https://pages.stern.nyu.edu/~adamodar/pdfiles/papers/costofcapital.pdf) |
| 2 | Damodaran Online — Data & Spreadsheets | [https://pages.stern.nyu.edu/~adamodar/](https://pages.stern.nyu.edu/~adamodar/) |
| 3 | Corporate Finance Institute — WACC Calculator | [https://corporatefinanceinstitute.com/resources/financial-modeling/wacc-calculator](https://corporatefinanceinstitute.com/resources/financial-modeling/wacc-calculator) |
| 4 | Investopedia — "Capital Asset Pricing Model (CAPM)" | [https://www.investopedia.com/terms/c/capm.asp](https://www.investopedia.com/terms/c/capm.asp) |
| 5 | Investopedia — "Using CAPM to Calculate Cost of Equity" | [https://www.investopedia.com/ask/answers/022515/how-do-i-use-capm-capital-asset-pricing-model-determine-cost-equity.asp](https://www.investopedia.com/ask/answers/022515/how-do-i-use-capm-capital-asset-pricing-model-determine-cost-equity.asp) |
| 6 | Wikipedia — "Capital Asset Pricing Model" | [https://en.wikipedia.org/wiki/Capital_Asset_Pricing_Model](https://en.wikipedia.org/wiki/Capital_Asset_Pricing_Model) |
| 7 | NBAA — "Depreciation Schedule for Business Aircraft" | [https://nbaa.org/flight-department-administration/tax-issues/depreciation/depreciation-schedule-for-business-aircraft/](https://nbaa.org/flight-department-administration/tax-issues/depreciation/depreciation-schedule-for-business-aircraft/) |
| 8 | Investopedia — "Modified Accelerated Cost Recovery System (MACRS)" | [https://www.investopedia.com/terms/m/macrs.asp](https://www.investopedia.com/terms/m/macrs.asp) |
| 9 | IRS Publication 946 — "How to Depreciate Property" | [https://www.irs.gov/pub/irs-pdf/p946.pdf](https://www.irs.gov/pub/irs-pdf/p946.pdf) |
| 10 | AvBuyer — "Business Aviation Tax: Benefits of MACRS & ADS" | [https://www.avbuyer.com/articles/aircraft-ownership/business-aviation-tax-the-benefits-of-macrs-amp-ads-58458](https://www.avbuyer.com/articles/aircraft-ownership/business-aviation-tax-the-benefits-of-macrs-amp-ads-58458) |
| 11 | IATA — "Airline Disclosure Guide: Aircraft Acquisition Cost and Depreciation" | [https://www.iata.org/contentassets/4a4b100c43794398baf73dcea6b5ad42/airline-disclosure-guide-aircraft-acquisition.pdf](https://www.iata.org/contentassets/4a4b100c43794398baf73dcea6b5ad42/airline-disclosure-guide-aircraft-acquisition.pdf) |
| 12 | IBA Group — Aviation Valuations | [https://www.iba.aero/advisory-services/valuations/](https://www.iba.aero/advisory-services/valuations/) |
| 13 | IATA — "Guidance Material and Best Practices for Aircraft Leases" | [https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/ac-leases-4th-edition.pdf](https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/ac-leases-4th-edition.pdf) |
| 14 | Investopedia — "Free Cash Flow to the Firm (FCFF)" | [https://www.investopedia.com/terms/f/freecashflowfirm.asp](https://www.investopedia.com/terms/f/freecashflowfirm.asp) |
| 15 | Corporate Finance Institute — "FCFF Formula" | [https://corporatefinanceinstitute.com/resources/financial-modeling/free-cash-flow-to-firm-fcff](https://corporatefinanceinstitute.com/resources/financial-modeling/free-cash-flow-to-firm-fcff) |
| 16 | CFA Institute — "Free Cash Flow Valuation" | [https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/free-cash-flow-valuation](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/free-cash-flow-valuation) |
| 17 | CFA Institute — "Capital Investments and Capital Allocation" | [https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2025/capital-investments-and-capital-allocation](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2025/capital-investments-and-capital-allocation) |
| 18 | Investopedia — "Modified Internal Rate of Return (MIRR)" | [https://www.investopedia.com/terms/m/mirr.asp](https://www.investopedia.com/terms/m/mirr.asp) |
| 19 | Corporate Finance Institute — "Capital Planning Metrics: NPV, IRR, PI" | [https://corporatefinanceinstitute.com/resources/valuation/capital-planning-metrics-guide/](https://corporatefinanceinstitute.com/resources/valuation/capital-planning-metrics-guide/) |
| 20 | AnalystNotes — CFA Level I: NPV, IRR, PI | [https://analystnotes.com/cfa-study-notes-calculate-and-interpret-net-present-value-npv-internal-rate-of-return-irr-payback-period-discounted-payback-period-and-profitability-index-pi-of-a-single-capital-project.html](https://analystnotes.com/cfa-study-notes-calculate-and-interpret-net-present-value-npv-internal-rate-of-return-irr-payback-period-discounted-payback-period-and-profitability-index-pi-of-a-single-capital-project.html) |
| 21 | IATA — "Managing and Costing for Aging Aircraft" | [https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/mcaa-1sted-2018.pdf](https://www.iata.org/contentassets/bf8ca67c8bcd4358b3d004b0d6d0916f/mcaa-1sted-2018.pdf) |
| 22 | IATA — "Airline Industry Economic Performance Reports" | [https://www.iata.org/en/iata-repository/publications/economic-reports/](https://www.iata.org/en/iata-repository/publications/economic-reports/) |
| 23 | Investopedia — "Sensitivity Analysis" | [https://www.investopedia.com/terms/s/sensitivityanalysis.asp](https://www.investopedia.com/terms/s/sensitivityanalysis.asp) |
| 24 | IATA — "International Aircraft Financing" | [https://www.iata.org/en/publications/manuals/international-aircraft-financing](https://www.iata.org/en/publications/manuals/international-aircraft-financing) |

---

*Report generated as part of the ENG3004 Economic Dimension project review.*
