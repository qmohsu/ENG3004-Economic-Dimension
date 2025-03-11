# Economic Decision-Making in Aircraft Fleet Modernization

## Background
SkyAir, an airline company, operates a fleet of aging Boeing 737-800 aircraft. Due to rising fuel costs, increasing maintenance expenses, and new environmental regulations, the airline's management is evaluating whether to **replace the old fleet** with newer, fuel-efficient aircraft such as the **Boeing 737 MAX** or the **Airbus A320neo**. 

This decision involves **significant capital investment**, and **Engineering Economics** plays a crucial role in determining financial feasibility.

---

## Engineering Economic Decision Factors

### Cost of New Aircraft vs. Maintenance of Old Fleet
- **New Aircraft Cost**: Boeing 737 MAX costs **$120 million per aircraft**.
- **Old Aircraft Maintenance**: 
  - **$2 million per aircraft per year**, increasing at **5% annually** due to wear and tear.
- **Fuel Efficiency**: 
  - New aircraft offer **20% fuel savings**, reducing overall operational costs.

### Time Value of Money
- SkyAir applies a **discount rate of 8%** to evaluate present values of future costs and revenues.
- **Fuel savings** and **maintenance reductions** are analyzed over a **10-year period**.

### Break-Even Analysis
- Each **new aircraft generates $15 million per year in revenue**.
- Break-even analysis helps determine **how long it takes to recover the initial investment** through savings and additional revenue.

---

## Economic Decision Models
SkyAir uses **three financial models** to evaluate fleet modernization:

### 1. Net Present Value (NPV)
NPV calculates the total expected cash flows for **keeping the old fleet vs. purchasing new aircraft**:

\[
NPV = \sum_{t=1}^{T} \frac{C_t}{(1 + r)^t} - C_0
\]

Where:
- \( C_t \) = Net cash flow in year \( t \)
- \( r \) = Discount rate (8% or 0.08)
- \( t \) = Year (1 to 10)
- \( C_0 \) = Initial investment cost

#### **(A) NPV for Keeping Old Aircraft**
Each **old aircraft incurs maintenance costs** that grow at **5% annually**:

\[
C_t = C_1 (1 + g)^{t-1}
\]

Where:
- \( C_1 = 2,000,000 \) USD (initial maintenance cost per aircraft)
- \( g = 5\% = 0.05 \) (maintenance cost growth rate)

For all **50 aircraft**:

\[
NPV_{old} = \sum_{t=1}^{10} \frac{50 \times 2,000,000(1.05)^{t-1}}{(1.08)^t}
\]

**Computed Result:**  
\( NPV_{old} = 16,367,107.74 \) USD

#### **(B) NPV for Purchasing New Aircraft**
New aircraft generate **cost savings** from:
1. **Fuel savings**: \( 20\% \) of annual revenue → \( 0.2 \times 15,000,000 = 3,000,000 \) USD per aircraft.
2. **Maintenance savings**: Avoids old aircraft maintenance costs.

Total savings per aircraft per year:

\[
C_t = 3,000,000 + 2,000,000(1.05)^{t-1}
\]

For all **50 aircraft**:

\[
NPV_{new} = - (50 \times 120,000,000) + \sum_{t=1}^{10} \frac{50 \times (3,000,000 + 2,000,000(1.05)^{t-1})}{(1.08)^t}
\]

**Computed Result:**  
\( NPV_{new} = -4,175,132,403.16 \) USD (negative → financial loss under these assumptions).

### 2. Internal Rate of Return (IRR)
The **IRR** is the discount rate \( r \) that makes \( NPV = 0 \):

\[
0 = -C_0 + \sum_{t=1}^{T} \frac{C_t}{(1 + r)^t}
\]

Where:
- \( C_0 = 50 \times 120,000,000 \) USD (initial investment)
- \( C_t = 50 \times (3,000,000 + 2,000,000(1.05)^{t-1}) \) (annual savings)
- Solve for \( r \)

**Computed Result:**  
\( IRR = -11.8\% \)

Since **IRR is negative**, this suggests **the investment does not yield a positive return** under the current assumptions.

### 3. Break-Even Analysis
Break-even occurs when **cumulative savings exceed the initial investment**:

\[
\sum_{t=1}^{T} \frac{C_t}{(1.08)^t} \geq C_0
\]

Where:
- \( C_t = 50 \times (3,000,000 + 2,000,000(1.05)^{t-1}) \) (cost savings per year)
- Solve for the first year \( T \) where savings exceed \( C_0 \).

---

## **Analysis & Decision**
SkyAir compared two options using **NPV and IRR analysis**:

1. **Retain the old fleet**, incurring high maintenance and fuel costs.
2. **Purchase new aircraft**, reducing operational costs over time.

The financial models indicate **purchasing new aircraft is not financially viable under the given assumptions** due to negative NPV and IRR. **Alternative solutions, such as leasing or government subsidies, should be considered** to improve financial feasibility.

---

## **Conclusion**
Based on Engineering Economics:
- **Keeping the old fleet results in increasing operational costs but remains financially feasible**.
- **Purchasing new aircraft leads to a negative NPV and IRR, suggesting a financial loss**.
- **SkyAir should explore other financing options or reconsider fleet renewal timelines**.

**Future Work:**
- Conduct sensitivity analysis on fuel prices and financing methods.
- Evaluate leasing vs. buying models.
- Consider external funding sources such as government incentives for newer aircraft.

---

## **References**
- [Boeing 737 MAX Specifications](https://www.boeing.com/commercial/737max/)
- [Airbus A320neo Fuel Efficiency](https://www.airbus.com/en/products-services/commercial-aircraft/a320-family/a320neo)
- [FAA Maintenance Cost Reports](https://www.faa.gov/)

---

**End of README**
