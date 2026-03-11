# Accuracy Review Report: Aircraft Fleet Modernization Economic Analysis

**Reviewer:** AI-Assisted Audit  
**Date:** March 11, 2026  
**Subject:** Verification of parameters, computations, and methodology in the SkyAir fleet modernization case study

---

## Executive Summary

This report audits the economic parameters and computations used in the ENG3004 SkyAir fleet modernization analysis. The review identified **10 issues**, including parameter inaccuracies, a fundamental conceptual error in how fuel savings are calculated, and computational inconsistencies between the README and the Jupyter notebook. The overall conclusion of the original analysis (that fleet replacement is not financially viable) happens to be directionally correct, but for the wrong quantitative reasons.

---

## 1. Parameter-by-Parameter Verdict

### 1.1 Boeing 737 MAX Purchase Price: $120M per aircraft

| Aspect | Detail |
|---|---|
| **Stated Value** | $120,000,000 |
| **Verdict** | Overstated by approximately 2x |

**Evidence:** The 2022 Boeing 737 MAX 8 catalog list price was $121.6M (Boeing Commercial, via industry tracking since Boeing stopped publishing official list prices). However, airlines **never pay list price**. According to industry data compiled by Simple Flying and Bolt Flight:

- **Actual market/transaction value:** ~$55M for a 737 MAX 8
- **Typical discounts:** 40--60% off list price for standard orders; bulk orders (e.g., Ryanair's 75-aircraft order) can exceed two-thirds off list price

**Sources:**
- Simple Flying, "How Much Do Boeing 737 MAX Aircraft Cost In 2026?" (2026)
- Bolt Flight, "Boeing 737 MAX vs 737 NG: Unpacking the True Cost and Value" (2024)
- Reuters, "Ryanair buys 75 Boeing MAX jets" (2020) -- confirming deep bulk discounts

**Impact:** Using $120M instead of a realistic ~$55--65M inflates the initial investment from ~$2.75--3.25B to $6B, making the project appear roughly twice as expensive as it would be.

---

### 1.2 Fuel Savings: 20% Improvement

| Aspect | Detail |
|---|---|
| **Stated Value** | 20% fuel burn reduction |
| **Verdict** | Overstated -- should be ~14% |

**Evidence:** Boeing's own specifications and independent analyses consistently cite a **~14% fuel burn reduction** for the 737 MAX 8 compared to the 737-800 (737 NG), driven by:
- CFM LEAP-1B engines (vs. CFM56-7B)
- Advanced split-tip winglets
- Aerodynamic refinements

**Sources:**
- Boeing 737 MAX specifications (boeing.com)
- Simple Flying, "Boeing 737 Max 8 vs 737 800" (2024)
- Flightradar24, "737 MAX vs 737 NG: A Pilot's Perspective" (2024)
- AirInsight, "How well are re-engined narrow-body aircraft performing?" (2024)

**Note:** Boeing and Airbus marketing materials sometimes claim "up to 20%" savings, but this typically refers to the A320neo vs. the *original* A320ceo (not the 737 comparison), or includes operational improvements beyond the airframe alone. The 737 MAX vs. 737-800 comparison is consistently cited at ~14%.

---

### 1.3 Fuel Savings Calculation: 20% of Revenue

| Aspect | Detail |
|---|---|
| **Stated Method** | Fuel savings = 20% x $15M revenue = $3M/aircraft/year |
| **Verdict** | Fundamental conceptual error |

**Explanation:** The analysis computes fuel savings as a percentage of **revenue**. Fuel savings should be a percentage of **fuel cost**. Revenue and fuel cost are entirely different quantities.

**Correct approach using industry data:**
- A Boeing 737-800 burns ~5,000 lbs (~750 gallons) of jet fuel per hour (Planenerd; EUROCONTROL)
- Average utilization: ~3,000 flight hours/year (industry average for narrowbodies)
- Jet fuel price: ~$2.58/gallon (BTS, July 2024)
- **Annual fuel cost per aircraft:** ~750 gal/hr x 3,000 hr x $2.58/gal = **~$5.8M**
- **14% fuel savings:** 0.14 x $5.8M = **~$0.81M per aircraft per year**

The analysis uses $3.0M per aircraft per year -- approximately **3.7x the realistic figure**.

**Sources:**
- Bureau of Transportation Statistics (BTS), "U.S. Airlines' July 2024 Fuel Cost per Gallon" (2024)
- EUROCONTROL Standard Inputs for Economic Analyses, "Aircraft Operating Costs" (2022)
- Planenerd, "The Fuel Consumption of a Boeing 737" (2024)

---

### 1.4 Old Aircraft Maintenance Cost: $2M per aircraft per year

| Aspect | Detail |
|---|---|
| **Stated Value** | $2,000,000/aircraft/year |
| **Verdict** | Significantly understated |

**Evidence:** IATA's FY2023 Maintenance Cost Experience (MCX) report, based on data from **27 airlines operating 2,241 aircraft**, finds:

- **Average annual maintenance cost:** ~$4.59M per aircraft (737 NG class)
- **Maintenance cost per flight hour:** $1,499/FH
- **Average fleet age in sample:** 10.6 years
- **Average daily utilization:** 8.38 flight hours

The analysis uses $2M, which is **less than half** the IATA-benchmarked figure.

**Sources:**
- IATA, "FY2023 Maintenance Cost Experience (MCX) Report" (2024) -- based on 27 airlines, 2,241 aircraft
- EUROCONTROL, "Aircraft Operating Costs -- Standard Inputs v10.0.0" (2022)

---

### 1.5 Maintenance Savings from New Aircraft

| Aspect | Detail |
|---|---|
| **Stated Assumption** | New aircraft eliminate all old-fleet maintenance costs |
| **Verdict** | Incorrect -- 737 MAX has similar maintenance costs to 737 NG |

**Evidence:** Industry analysis from AirInsight confirms that the 737 MAX and 737 NG have **comparable maintenance costs**, including similar by-the-hour engine maintenance contracts. The primary cost advantage of the MAX is fuel efficiency, **not reduced maintenance**.

New aircraft also have maintenance costs (they are not zero). The correct approach would model the **difference** in maintenance costs between old and new fleets, which is modest.

**Sources:**
- AirInsight, "How well are re-engined narrow-body aircraft performing?" (2024)
- IATA MCX Report FY2023

---

### 1.6 Maintenance Cost Growth Rate: 5% per year (uniform)

| Aspect | Detail |
|---|---|
| **Stated Value** | 5% annual growth, applied uniformly |
| **Verdict** | Oversimplified -- real escalation depends heavily on aircraft age |

**Evidence:** Research from RAND Corporation and the Defense Acquisition University shows maintenance cost escalation varies dramatically by age bracket:

| Aircraft Age | Annual Maintenance Cost Escalation |
|---|---|
| 0--6 years (off warranty) | ~17.6% |
| 6--12 years (mature) | ~3.5% |
| 12--25 years (older) | ~0.7% (statistically insignificant) |

A separate DHS study found **8.0% escalation per year of fleet age** in government fleets. The uniform 5% assumption ignores these age-dependent dynamics.

**Sources:**
- RAND Corporation, "Older Commercial Aircraft Have Relatively Stable Total Maintenance Costs," Research Brief RB-206 (2002)
- Defense Acquisition University, "Maintenance Cost Growth in Aging Aircraft," Defense ARJ 101 (2024)

---

### 1.7 Annual Revenue per Aircraft: $15M

| Aspect | Detail |
|---|---|
| **Stated Value** | $15,000,000/aircraft/year |
| **Verdict** | Substantially understated |

**Evidence:** Southwest Airlines reported record operating revenue of **$27.5 billion** in 2024 with **847 aircraft**, yielding approximately **$32.5M revenue per aircraft**. Even budget carriers with lower yields operate in the $20--30M range per narrowbody.

**Sources:**
- Southwest Airlines, "Fourth Quarter and Full Year 2024 Results," PR Newswire (January 2025)
- Southwest Airlines 2024 Annual Report (southwestairlinesinvestorrelations.com)

**Note:** While $15M might be plausible for a very small regional carrier or a carrier in a developing market, it is significantly below industry norms for a 50-aircraft narrowbody operator.

---

### 1.8 Discount Rate: 8%

| Aspect | Detail |
|---|---|
| **Stated Value** | 8% |
| **Verdict** | Reasonable |

**Evidence:** The 8% discount rate aligns well with industry benchmarks:

| Source | WACC |
|---|---|
| Montana DoR Airline Capitalization Study (2024) | 7.76% |
| American Airlines (AAL) | 9.2% |
| Air Transport Services Group (ATSG) | 7.2% |
| Air Canada (AC.TO) | 6.5% |

An 8% rate falls within the 6.5--9.2% range observed across the industry.

**Sources:**
- Montana Department of Revenue, "2024 Capitalization Rate Study -- Airlines & Freight" (2024)
- Alpha Spread / ValueInvesting.io WACC calculations for AAL, ATSG

---

## 2. Computational Errors

### 2.1 NPV_old Is Computed for 1 Aircraft, Labeled as 50

The README states:

> For all **50 aircraft**: NPV_old = $16,367,107.74

**Verification:** Computing for **one** aircraft at $2M/year, 5% growth, 8% discount, 10 years:

$$\text{NPV}_{\text{old}} = \sum_{t=1}^{10} \frac{2{,}000{,}000 \times (1.05)^{t-1}}{(1.08)^t} \approx 16{,}367{,}108$$

This matches the stated figure. However, the formula shown includes the factor of 50, which would give ~$818M. The $16.37M figure is for **one aircraft only**.

**Correct value for 50 aircraft:** $818,355,387

Meanwhile, NPV_new = -$4,175,132,403 appears to be computed for all 50 aircraft (investment of $6B). This means the README compares a **per-aircraft** NPV_old with a **fleet-wide** NPV_new, which invalidates the comparison.

---

### 2.2 Break-Even Notebook: Discount Rate Discrepancy

The notebook code declares `r = 0.08`, but the output is inconsistent with this value.

**Expected Year 1 output with r = 0.08:**

$$\frac{50 \times (3 + 2)}{1.08} = \frac{250}{1.08} = 231.48$$

**Actual Year 1 output:** 245.10

**This matches r = 0.02:**

$$\frac{250}{1.02} = 245.10$$

**Further confirmation:** Year 3 output = 245.24, which matches r = 0.02 calculation: $50 \times (3 + 2 \times 1.05^2) / 1.02^3 = 260.25 / 1.0612 = 245.24$.

**Implication:** The notebook was likely run with `r = 0.02` and the code was later edited to show `r = 0.08` without re-execution. With the stated r = 0.08, the present value of all future savings converges to approximately $5.2B, which is **less than** the $6B initial investment -- meaning **break-even would never occur**, not at year 24.

---

### 2.3 IRR = -11.8%: Not Independently Verified in Code

The README states IRR = -11.8% but no computation for IRR is provided in the notebook. Given the errors in other computed values, this figure should be treated as unverified.

---

## 3. Missing Factors in the Analysis

| Missing Factor | Impact |
|---|---|
| **Residual/salvage value of old fleet** | The 50 Boeing 737-800s have market value of ~$15--30M each (Simple Flying, 2026), totaling $750M--$1.5B. Selling these would offset the new aircraft purchase cost. |
| **Revenue from selling old aircraft** | Not modeled. This is a standard component of fleet replacement analysis. |
| **Residual value of new aircraft at end of analysis period** | A 10-year-old 737 MAX would retain significant value (~$30--50M), which should be included as a terminal value. |
| **Inflation on fuel prices** | Fuel prices are volatile and have trended upward (BTS: $1.98/gal in 2019 to $2.58/gal in 2024). Higher future fuel prices would increase the value of fuel-efficient aircraft. |
| **Leasing vs. buying** | The README mentions leasing as an alternative but does not model it. Monthly lease rates for a 737 MAX 8 are ~$400K/month (~$4.8M/year), far less than $120M purchase. |
| **Tax depreciation / MACRS** | Aircraft are depreciable assets under IRS/tax codes. Depreciation shields reduce the effective cost of new aircraft. |
| **Financing structure** | The analysis assumes full cash purchase. Most fleet acquisitions use a mix of debt, ECA financing, and operating leases. |
| **Environmental compliance costs** | The README mentions "new environmental regulations" but does not quantify any compliance costs or carbon credits. |
| **Phased replacement** | Replacing all 50 aircraft simultaneously is unrealistic. A phased replacement (e.g., 10 per year over 5 years) is standard practice. |

---

## 4. Summary Verdict Table

| Parameter | Project Value | Industry-Verified Value | Source | Verdict |
|---|---|---|---|---|
| Aircraft price | $120M (list) | ~$55--65M (market) | Simple Flying; Bolt Flight | Overstated ~2x |
| Fuel savings % | 20% | ~14% | Boeing specs; Flightradar24 | Overstated |
| Fuel savings basis | % of revenue | % of fuel cost | Conceptual (BTS fuel data) | Fundamental error |
| Fuel savings $/yr | $3.0M/aircraft | ~$0.81M/aircraft | BTS; EUROCONTROL | Overstated ~3.7x |
| Maintenance cost | $2.0M/aircraft/yr | ~$4.59M/aircraft/yr | IATA MCX FY2023 | Understated ~2.3x |
| Maint. escalation | 5%/yr uniform | 0.7--17.6% by age | RAND Corp; DAU | Oversimplified |
| Revenue/aircraft | $15M/yr | ~$25--35M/yr | Southwest 2024 Annual Report | Understated ~2x |
| Discount rate | 8% | 6.5--9.2% (WACC) | Montana DoR 2024 | Reasonable |
| NPV_old | $16.37M (50 aircraft) | ~$818M (50 aircraft) | Computation check | Error (1 aircraft, not 50) |
| Break-even year | 24 | Never (at r=8%) | Computation check | Output uses r~2%, not 8% |
| IRR | -11.8% | Unverified | No code provided | Unverified |

---

## 5. Recommendations for a More Realistic Computation

### 5.1 Use Realistic Transaction Prices
Replace the $120M list price with a market-based acquisition cost of **$55--65M per aircraft** (or model a leasing scenario at ~$400K/month). Source this from Cirium/Ascend or ISTAT appraisals.

### 5.2 Fix the Fuel Savings Calculation
Calculate fuel savings as a percentage of **fuel cost**, not revenue:
```
Annual fuel cost = fuel_burn_rate (gal/hr) x flight_hours/year x fuel_price ($/gal)
Annual fuel savings = fuel_cost x 0.14
```

### 5.3 Use IATA-Benchmarked Maintenance Costs
Use ~$4.59M/aircraft/year (IATA MCX FY2023) as the baseline, with age-dependent escalation curves from RAND rather than a flat 5%.

### 5.4 Model Maintenance Costs for BOTH Fleets
New aircraft also have maintenance costs. Model the **net difference** in maintenance between old and new fleets, not assume new aircraft maintenance is zero.

### 5.5 Include Residual Values
- Old fleet sale proceeds: ~$15--30M per aircraft (Simple Flying, 2026)
- Terminal value of new fleet at end of analysis horizon

### 5.6 Extend the Analysis Horizon
A 10-year horizon is short for aircraft with 25--30 year economic lives. Use **20--25 years** or include a terminal/residual value to capture the full lifecycle.

### 5.7 Model Phased Replacement
Replace aircraft in batches (e.g., 10/year over 5 years) rather than all 50 simultaneously. This is operationally realistic and spreads capital expenditure.

### 5.8 Add Sensitivity Analysis
Test the NPV/IRR under:
- Fuel price scenarios: $2.00, $2.50, $3.00, $3.50/gallon
- Discount rate scenarios: 6%, 8%, 10%
- Aircraft utilization: 2,500 vs. 3,000 vs. 3,500 FH/year

### 5.9 Consider Financing Structures
Model debt financing (e.g., 70/30 debt-equity), Export Credit Agency (ECA) financing, and sale-leaseback options, which are how airlines actually finance fleet acquisitions.

### 5.10 Fix Computational Errors
- Re-run the notebook with the correct discount rate (verify r=0.08 is actually used)
- Ensure NPV_old uses the 50-aircraft multiplier consistently
- Implement IRR computation in code (e.g., using `numpy.irr` or `scipy.optimize`)
- Add NPV computation to the notebook alongside break-even

---

## 6. References

1. **Boeing 737 MAX Pricing:** Simple Flying, "How Much Do Boeing 737 MAX Aircraft Cost In 2026?" (2026). [Link](https://simpleflying.com/how-much-boeing-737-max-cost-2026/)
2. **Aircraft Market Values:** Bolt Flight, "Boeing 737 MAX vs 737 NG: Unpacking the True Cost and Value" (2024). [Link](https://boltflight.com/boeing-737-max-vs-737-ng-unpacking-the-true-cost-and-value-of-each-generation/)
3. **Fuel Efficiency (14%):** Flightradar24, "737 MAX vs 737 NG: A Pilot's Perspective" (2024). [Link](https://www.flightradar24.com/blog/aviation-explainer-series/737-max-vs-737-ng-a-pilots-perspective/)
4. **Fuel Efficiency:** Simple Flying, "Boeing 737 Max 8 vs 737 800" (2024). [Link](https://simpleflying.com/boeing-737-max-vs-737-800/)
5. **Maintenance Costs:** IATA, "FY2023 Maintenance Cost Experience (MCX) Report" (2024). [Link](https://www.iata.org/contentassets/8437020db31a4717b70677d9b06b1a45/fy2023-mcx-report_public.pdf)
6. **Maintenance Escalation by Age:** RAND Corporation, "Older Commercial Aircraft Have Relatively Stable Total Maintenance Costs," RB-206 (2002). [Link](https://rand.org/pubs/research_briefs/RB206.html)
7. **Maintenance Escalation (DHS):** Defense Acquisition University, "Maintenance Cost Growth in Aging Aircraft," Defense ARJ 101 (2024). [Link](https://www.dau.edu/library/darj/arj-101/arj-101-ross)
8. **Fuel Prices:** Bureau of Transportation Statistics, "U.S. Airlines' July 2024 Fuel Cost per Gallon" (2024). [Link](https://www.bts.gov/newsroom/us-airlines-july-2024-fuel-cost-gallon-16-june-2024-aviation-fuel-consumption-35-pre)
9. **Operating Costs:** EUROCONTROL, "Aircraft Operating Costs -- Standard Inputs for Economic Analyses v10.0.0" (2022). [Link](https://ansperformance.eu/economics/cba/standard-inputs/v10.0.0/chapters/aircraft_operating_costs.html)
10. **Revenue Benchmark:** Southwest Airlines, "Q4 and Full Year 2024 Results," PR Newswire (Jan 2025). [Link](https://www.prnewswire.com/news-releases/southwest-airlines-reports-fourth-quarter-and-full-year-2024-results-302364072.html)
11. **Airline WACC:** Montana Department of Revenue, "2024 Capitalization Rate Study -- Airlines & Freight" (2024). [Link](https://revenuefiles.mt.gov/files/DOR-Publications/Capitalization-Rate-Studies/2024/2024-Capitalization-Rate-Study-Airlines-Freight.pdf)
12. **737-800 Residual Values:** Simple Flying, "How Much Does A Boeing 737-800 Cost In 2026?" (2026). [Link](https://simpleflying.com/how-much-boeing-737-800-cost-2026/)
13. **MAX Maintenance Comparison:** AirInsight, "How well are re-engined narrow-body aircraft performing?" (2024). [Link](https://airinsight.com/how-well-are-re-engined-narrow-body-aircraft-performing/)
14. **Fuel Consumption:** Planenerd, "The Fuel Consumption of a Boeing 737" (2024). [Link](https://planenerd.com/boeing-737-fuel-consumption/)
15. **Ryanair Bulk Discounts:** Reuters, "Ryanair buys 75 Boeing MAX jets in largest order since grounding" (2020). [Link](https://www.reuters.com/business/ryanair-buys-75-boeing-max-jets-largest-order-since-grounding-2020-12-04/)

---

*End of Report*
