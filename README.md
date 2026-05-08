# Interconnections Between Banks and Non-Bank Financial Intermediaries

> **To what extent do cross-holdings between banks and NBFIs contribute to the propagation of systemic risk at the international level?**

*Stat'App Project — ENSAE / IP Paris × ACPR – Banque de France | Academic Year 2025–2026*

---

## Overview

This repository contains the full research code and outputs for a statistical project conducted in collaboration with the **Autorité de Contrôle Prudentiel et de Résolution (ACPR – Banque de France)**. The project investigates the financial interconnections between traditional banks and Non-Bank Financial Intermediaries (NBFIs) and their role as potential channels for systemic risk propagation.

Since the 2008 financial crisis, NBFIs have grown to represent more assets than the banking sector globally (USD 239 trillion vs. 183 trillion in 2021). Yet banks and NBFIs remain deeply intertwined through loans, credit lines, debt securities, and derivatives. Understanding the structure and dynamics of these exposures is central to macroprudential surveillance.

---

## Research Question

**In what measure do cross-holdings between banks and NBFIs contribute to the propagation of systemic risk internationally?**

The study addresses this through three complementary lenses:
1. **Descriptive analysis**: structure, geography, and currency composition of bank-to-NBFI exposures
2. **Short-term modelling**: regression analysis of macrofinancial determinants and ARIMA forecasting
3. **Systemic risk assessment**: VAR modelling and Granger causality testing between financial stress and exposures

---

## Data

**Primary source:** [BIS Locational Banking Statistics (LBS)](https://www.bis.org/statistics/bankstats.htm)

| Feature | Detail |
|---|---|
| Coverage | ~95% of international banking activity worldwide |
| Frequency | Quarterly |
| Period studied | Q4 2013 – Q3 2025 (NBFI sector available from end-2013) |
| Dimensions | Reporting country, counterparty country, counterparty sector, instrument type, currency, position type |
| Position types | Claims (assets) and Liabilities |

**Supplementary macrofinancial series:**

| Variable | Role |
|---|---|
| Real GDP growth | Business cycle |
| Short-term interest rate | Monetary policy |
| Yield curve slope | Term structure |
| VIX | Global market stress |
| High-yield spread | Credit risk |
| US Dollar Index | Dollar funding channel |
| CISS | European systemic financial stress |

---

## Methods

### Descriptive Statistics
- Aggregated time-series visualisation of bank-to-NBFI and interbank exposures
- Breakdowns by instrument (loans & deposits, debt securities, derivatives), currency (USD, EUR, JPY, GBP, CHF), and counterparty geography
- **Network mapping** of bilateral exposure corridors via proportional imputation

### Concentration Analysis — RiskIndex
A composite indicator derived from the normalised Herfindahl-Hirschman Index (HHI):

$$\text{RiskIndex}_{i,t} = s_{i,t} \times HHI^{\text{norm}}_{i,t}$$

where $s_{i,t}$ is the country's share in total NBFI exposures and $HHI^{\text{norm}}_{i,t}$ measures currency concentration in foreign-currency exposures. The aggregate version sums across all counterparty countries.

### Regression Analysis
Linear models explaining quarterly changes in bank-to-NBFI exposures (global and euro area) as a function of macrofinancial variables. Four specifications are compared — benchmark, dynamic with lagged dependent variable, stacked lags, GDP×VIX interaction — using adjusted R², AIC/BIC, and out-of-sample RMSE.

### ARIMA Modelling
Box-Jenkins methodology applied to total bank claims on NBFIs (n = 48 quarterly observations). Stationarity tested via ADF, Phillips-Perron, and KPSS. Competing models filtered on coefficient significance and Ljung-Box residual diagnostics. Final model: **ARIMA(0,1,1) with drift**, forecasting ~USD 18 trillion by 2028.

### VAR & Granger Causality
Bivariate VAR(1) on differenced euro-area bank-to-NBFI exposures and ΔCISS, estimated over Q1 2014 – Q2 2025. Analysis includes:
- Static Granger causality tests (full sample)
- Impulse Response Functions (Cholesky identification, bootstrap confidence intervals)
- Forecast Error Variance Decomposition (FEVD)
- **Rolling-window Granger causality** (20-quarter windows) to detect time-varying transmission

---

## Key Findings

- NBFI exposures grow **structurally faster** than interbank exposures over 2014–2025 (~200% vs. ~25%)
- Exposures are dominated by **loans & deposits** denominated in **USD**, concentrated in a few financial hubs: United States, Cayman Islands, United Kingdom, Luxembourg, Ireland, Japan
- The RiskIndex identifies the **Cayman Islands, UK, and US** as the principal poles of currency-concentration risk
- The selected ARIMA(0,1,1) with drift confirms a **difference-stationary** process — exposures accumulate via permanent shocks rather than reverting to a fixed trend
- The **dynamic regression** is the best-performing model (R²_adj = 0.36): VIX is positively and CISS negatively associated with exposure changes
- **Rolling-window Granger tests** reveal a statistically significant CISS → exposure channel over 2014–2019 (7 consecutive windows, p < 5%), which **breaks abruptly** when 2020 COVID observations enter the window
- No evidence of a reverse channel (exposures → stress) at any horizon, consistent with FEVD results (>96% of exposure variance is self-driven)

> **Caveat:** The LBS cover only on-balance-sheet positions. The off-balance-sheet channels most discussed in the literature — credit lines, derivatives, prime brokerage — are not captured. A negative result should be read as "no detectable signal in this measure", not "no channel exists."

---

## Repository Structure

```
.
├── data/                              # Raw and processed data (BIS LBS + macro series)
├── src/
│   ├── stats.Rmd                      # Descriptive statistics and concentration analysis
│   └── ARIMA_GRANGER_extended-2.Rmd  # ARIMA, VAR and Granger causality
├── img/
│   ├── ARIMA plots/                   # ARIMA diagnostics and forecasts
│   ├── RiskIndex/                     # RiskIndex and HHI charts
│   ├── descriptive statistics/        # Aggregated exposure charts
│   └── risk mapping/                  # Network graphs
├── docs/                              # Supporting documentation
├── report/                            # Final report (PDF)
└── README.md
```

---

## References

1. Acharya, V.V., Chauhan, R.S., Rajan, R., & Steffen, S. (2024). *Shadow Always Touches the Feet: Implications of Bank Credit Lines to Non-Bank Financial Intermediaries.* NBER Working Paper.
2. European Systemic Risk Board (2025). *EU Non-bank Financial Intermediation Risk Monitor 2025.* ESRB Risk Monitor Report.
3. Friesen, Q., Van den Heuvel, S., & Murdock, W. (2024). *Shifting Dynamics in Bank Funding of NBFIs: The Rise of Credit Lines.* Federal Reserve Board, FEDS Notes.
4. Cetorelli, N., Goldberg, L.S., & Gambacorta, L. (2023). *Where Do Banks End and NBFIs Begin?* NBER Working Paper.
5. McCauley, R.N., Bénétrix, A.S., McGuire, P.M., & von Peter, G. (2019). Financial deglobalisation in banking? *Journal of International Money and Finance*, 94, 116–131.


---

## Authors

Project made by Jean-Médérick BESSI, Yasmine BOUTCHICH, Massil ZOUAGHI and Ralph NADER.
