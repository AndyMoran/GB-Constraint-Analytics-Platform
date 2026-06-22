# GB Constraint Analytics Platform

This is a three‑part analytical and modelling framework for understanding GB transmission constraints, storage feasibility, and flexibility economics. The platform melds data engineering, operational behaviour analysis, and economic evaluation into a unified research environment.

---

## Headline Findings

The analysis established the following physical and economic realities:

**Physical Reality:**
- Constraint burden is highly concentrated within a small number of North-West transmission boundaries.
- Power capability is typically a more important limitation than energy capacity ("Power Before Energy").
- A representative 50MW 4h battery captures only **~1.7%** of total system constraint opportunity due to physical power and energy limits.
- A single storage asset cannot substitute for system-scale transmission constraints; batteries are "buckets", wires are "pipes".

**Economic Reality:**
- Constraint-management revenues cover **<5%** of basic operating costs for all tested configurations.
- A 10,000-iteration Monte Carlo simulation confirms a **0.00% probability** of standalone economic viability under historical conditions.
- The market is trapped in a **"pincer movement"**: the grid physically requires longer-duration assets (4h–8h) to relieve persistent congestion, but current market economics only fund short-duration assets (1h–2h) due to capital expenditure hurdles.
- The principal conclusions remain highly robust under extensive uncertainty, pricing attribution, and sensitivity testing.

**In summary:** 
Transmission constraints create measurable physical and economic value, but this value is structurally insufficient to support standalone battery investment. Constraint revenue acts strictly as a marginal top-up, not a primary business case.

---

## Purpose

The platform provides a structured approach to understanding:
- how transmission constraints form and persist  
- how operational episodes challenge flexibility assets  
- which physical limitations matter most  
- where economic value emerges for storage and other flexible technologies  
- whether that value is sufficient to support investment

The platform is designed as a cumulative research programme in which each layer builds strictly on the validated outputs of the previous layer.

---

## Research Philosophy

The philosophy is simple:

> **Physics Before Economics.** 
> Understand the physical system before evaluating economic value. Analyse operational behaviour before economic opportunity, and evaluate economic opportunity before investment viability.

---

## Engineering & Reproducibility

To ensure institutional-grade rigour, the platform enforces strict software engineering standards:
- **Modular Architecture:** Reusable logic (DCF engines, data validators, cost models) is extracted into a dedicated `src/` module, keeping notebooks focused on orchestration and narrative.
- **Data Contracts:** Every pipeline stage validates schema, row-counts, and physical plausibility before execution.
- **Automated Testing:** Core economic engines are covered by `pytest` unit tests using mocked fixtures to ensure mathematical invariants hold across code updates.
- **Adversarial Review:** All findings are subjected to a "Devil's Advocate" protocol, explicitly probing for unit inconsistencies, threshold justification, and look-ahead bias.

---

## Scope

This project is an empirical, physics-informed analysis of observed transmission constraints.

It is **not** a power-system optimisation model, dispatch model, or investment recommendation.

---

## Project Structure

### **Part I — Data Platform & Signal Engineering (NB01–NB07)**  
Data ingestion, validation, harmonisation, feature engineering, and construction of constraint‑ready analytical datasets.

### **Part II — Constraint Behaviour & Storage Feasibility (NB08–NB12)**  
GB‑wide screening of constraint persistence, burden concentration, clustering, and operational archetypes, followed by a **detailed North‑West case study** evaluating asset survivability, power and energy limitations, and state‑of‑charge dynamics for selected high‑burden transmission boundaries.

### **Part III — Constraint Economics & System‑Level Value (NB13–NB18)**
Economic opportunity screening, physical revenue capture, 15-year DCF investment viability, and Monte Carlo risk assessment. This phase quantifies the divergence between physical system requirements and market incentives, proving the robustness of the non-viability conclusion across 10,000 simulated market scenarios.

---

## Status & Roadmap

- **Completed:** Part I (Data Platform), Part II (Constraint Behaviour), Part III (Constraint Economics & Robustness).
- **Next Phase (Project B):** *Grid Connection Arbitrage & Hybrid Flexibility Economics.* Investigating whether co-locating BESS with curtailed renewable generation and bypassing the grid connection queue can bridge the economic viability gap identified in Part III.

---

### Navigation

- [Part I — Data Platform](1_Data_Platform/README.md)
- [Part II — Constraint Behaviour & Storage Feasibility](2_Constraint_Behaviour/README.md)
- [Part III — Constraint Economics & System‑Level Value](3_Constraint_Economics/README.md)

---

## Disclaimer

This project is an independent analytical research programme.  
Findings describe historical system behaviour and should not be interpreted as forecasts, investment advice, or operational recommendations.

All analysis is implemented in a private research environment. Full notebooks and code are available on request.
