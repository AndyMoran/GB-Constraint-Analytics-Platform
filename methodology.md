# Methodology

This document outlines the analytical pipeline, mathematical frameworks, and modelling boundaries used to connect physical transmission constraint behaviour with battery storage investment economics.

## 1. The Analytical Hierarchy

The project strictly enforces a **Physics Before Economics** hierarchy. Economic value is never evaluated before physical feasibility is proven. The analysis progresses through four distinct phases:

1. **Physical Burden & Survivability** (Constraints Study): Establishes the temporal clustering, persistence, and power/energy limits of constraint episodes.
2. **Economic Opportunity Screening** (NB13): Quantifies the theoretical upper bound of revenue (Gross Opportunity) using system stress premiums.
3. **Physical Revenue Capture** (NB14): Applies rigorous asset-level power and energy limits to calculate the technically accessible revenue (Capturable Opportunity).
4. **Asset Economics & Risk** (NB15 & NB18): Introduces Capex, Opex, degradation, and Monte Carlo simulations to test standalone financial viability and quantify uncertainty.

## 2. Core Modelling Framework

### 2.1 Physical Capture Model
To evaluate how much constraint opportunity a battery can capture, the model applies strict thermodynamic and electrical limits at the episode level:
* **Power Limit (Static Failure):** The asset cannot discharge faster than its inverter rating ($P_{asset}$). If the system requires $P_{curtailment} > P_{asset}$, the asset captures only a proportional fraction of the value.
* **Energy Limit (Dynamic Depletion):** The asset cannot discharge more total energy than its capacity allows ($E_{asset} = P_{asset} \times Duration$). 
* **Capture Ratio:** Calculated as the minimum of the theoretical energy deliverable and the asset's physical energy capacity, divided by the total gross constraint energy.

### 2.2 Economic Valuation (The Stress Premium)
Absolute MW curtailment data is not natively available in the public dataset. Therefore, the model uses a parametric conversion framework:
* **Dimensionless to Physical:** Loading ratios are converted to physical MW using a parametric line capacity assumption.
* **Pricing Signal:** Economic value is derived from the **System Stress Premium** (Imbalance Price minus Day-Ahead Price) during operational episodes. Only positive stress periods are monetised to provide conservative estimates.
* **Invariance Proof:** The model tests multiple pricing methodologies (Mean, Median, P95, Max). While absolute £ values swing by up to 18x, the physical capture ratio remains invariant at ~1.75%, proving that physical friction dominates pricing uncertainty.

### 2.3 Financial Viability & Monte Carlo Risk
Standalone viability is tested using a vectorised, 15-year Discounted Cash Flow (DCF) model:
* **Base Case:** Real, pre-tax, unlevered. 8% discount rate, 2% annual degradation.
* **Revenue Sufficiency Test:** A binary screening filter checking if annual constraint revenue covers basic Fixed O&M.
* **Viability Threshold:** A closed-form calculation determining the exact £/MW-year required from stacked markets (Arbitrage, Capacity Market) to achieve NPV = £0.
* **Monte Carlo Simulation:** 10,000 iterations sampling wide distributions of revenue, Capex (£200k–£400k/MWh), Opex, and discount rates to estimate the probability distribution of NPV under uncertainty in market, cost, and financing assumptions..

### 2.4 The model should be interpreted as a screening framework rather than a dispatch model.

It estimates:

- Physical accessibility
- Technical capture potential
- Economic upper bounds
- Financial viability

It does not estimate realised trading performance or operational dispatch outcomes.

Consequently, positive results should be viewed as necessary but not sufficient conditions for investment viability.
Conversely, negative results provide strong evidence against viability because they fail under idealised assumptions.

## 3. Data Pipeline

The analytical pipeline is fully reproducible and modularised:

1. **Ingestion:** Raw market signals and constraint data are ingested, validated against strict data contracts, and stored as versioned Parquet files.
2. **Clustering:** Individual constraint events are clustered into **Operational Episodes** based on recovery-gap thresholds, respecting the temporal persistence of the grid.
3. **Vectorised Processing:** Episode-level physics and economics are calculated using vectorised Pandas/NumPy operations to ensure computational efficiency across 10,000+ episodes and 40 asset configurations.
4. **Modular Architecture:** Reusable logic (cost models, DCF engines, data validators) is extracted into a `src/` module, ensuring notebooks remain focused on orchestration and visualisation.

## 4. Scope Boundaries & Limitations

To maintain analytical rigour, the following elements are explicitly excluded from the current modelling framework:

* **No Dispatch Optimisation:** The model assumes perfect foresight and frictionless intra-episode dispatch. It does not model bidding strategies, latency, or State-of-Charge (SoC) conflicts between stacked revenue streams.
* **No Topological Power-Flow:** The parametric MW conversion is a screening estimate, not a substitute for detailed AC/DC power-flow simulation or thermal rating models.
* **No Co-location/Hybridisation:** The economic baseline assumes a standalone merchant BESS. Co-location with renewables (which shares grid connection costs) is excluded but acknowledged as a potential pathway to viability.
* **No Forecasting:** The analysis is strictly historical. It evaluates what *did* happen, not what *will* happen. Future market evolution (e.g., Locational Energy Pricing) is discussed strategically but not modelled numerically.

## 5. Reproducibility & Testing

The project adheres to strict engineering standards to ensure findings are robust and reproducible:
* **Data Contracts:** Every notebook validates required columns and data types before execution.
* **Domain Plausibility Checks:** Automated assertions prevent physically impossible outputs (e.g., capture ratios > 1.0, negative durations).
* **Unit Testing:** Core economic engines in `src/` are covered by `pytest` unit tests using mocked fixtures, ensuring mathematical invariants hold across code updates.
* **Adversarial Review:** All findings are subjected to a "Devil's Advocate" protocol, explicitly probing for unit inconsistencies, threshold justification, and look-ahead bias before conclusions are published.

The principal findings were independently stress-tested through:

- Alternative revenue attribution methodologies
- MW conversion sensitivity analysis
- Capex sensitivity analysis
- Discount-rate sensitivity analysis
- Monte Carlo simulation

The objective was not to optimise outcomes, but to determine whether the conclusions survive reasonable variations in modelling assumptions.