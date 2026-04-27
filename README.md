# Data-Driven Predictive Modelling for Fuel Cell Performance and Degradation  
### Technical Manual & Service Specification v1.0

**Author:** University of Technology of Belfort-Montbéliard (UTBM)  
**Version:** 1.0  
**Last updated:** 11 February 2026  

---

# 1. Business Context & Definitions

This service focuses on predicting how hydrogen fuel-cell systems perform and degrade over time. It supports both **short-term operational monitoring** and **long-term degradation analysis**, making it useful for:

- Fuel-cell operators  
- System integrators  
- Technology developers  

Understanding how a fuel cell behaves over time is critical. Without it, systems may:
- Lose efficiency  
- Experience unexpected failures  
- Require costly emergency maintenance  

This service uses historical and operational data to:
- Detect degradation early  
- Track system health continuously  
- Support better maintenance decisions  

It is designed for **regular operational use**, with strong emphasis on:
- Data availability  
- Prediction robustness  
- Transparency and reproducibility  

---

## Key Terms

- **Fuel Cell System / Test Bench**  
  Hydrogen fuel-cell systems used in UTBM experimental platforms (PEMFC, SOFC, hybrid setups). Data is collected offline with high temporal resolution.

- **Elapsed Time**  
  Time index representing cumulative operating duration. Datasets typically span **hundreds to thousands of hours**.

- **Electrical Performance Variables**  
  Core indicators:
  - Voltage  
  - Current  
  - Power  

- **Operating and System Parameters**  
  Additional variables:
  - Gas flow rates  
  - Pressures  
  - Temperatures  

- **Prediction Horizon**  
  Forecast time window:
  - Short-term: hours → days  
  - Long-term: degradation trends  

- **Resolution / Granularity**  
  Sampling frequency:
  - Seconds → hourly  

- **Model Validation / Backtesting**  
  Testing predictions on unseen historical data  

---

## 1.1 Fuel-Cell Operator and Developer Context

The service is intended to support:
- Performance monitoring  
- Degradation tracking  
- Maintenance planning  

### Practical Usage

Two main modes exist:

#### 1. Voltage-only (limited data)
- Works under steady-state conditions  
- Uses historical voltage time series  
- Produces **short-term forecasts (up to ~2 days)**  

#### 2. Full operational data (rich data)
Includes:
- Current  
- Flow rates  
- Pressures  
- Temperatures  

→ Enables **long-term degradation modelling**, even under dynamic conditions  

### Key Capabilities

- Works with both simple and advanced monitoring setups  
- Captures long-term degradation mechanisms  
- Provides insights across multiple system configurations  

### Responsibilities

- **Provider:**
  - Data processing  
  - Modelling  
  - Validation  
  - Prediction delivery  

- **User:**
  - Provides operational data  
  - Provides system context  

The service ensures:
- Traceability  
- Reproducibility  
- Transparent modelling  

---

# 2. Problem Statement

The goal is to provide a **reliable AI-based system** for predicting:

- Fuel-cell performance evolution  
- Degradation behaviour  

### Core Requirements

- Works on historical + new data  
- Produces consistent predictions  
- Supports operational decision-making  

### Outputs

- Time-series performance predictions  
- Health indicators  
- Degradation trends  

### Key Constraints

- Predictions must be:
  - Physically plausible  
  - Operationally realistic  
  - Fully traceable  

Each prediction includes:
- Analysis period  
- Model version  
- Data sources  

---

# 3. Data Description

The service uses multiple datasets covering fuel-cell systems from:

**300 W → 7.7 kW**

### Key datasets

- ALICE  
- Ballard Nexa  
- BZ100 (open-access)  
- Horizon  
- PM200  

Additional datasets may come from:
- TEF H₂ satellite  
- Ongoing experiments  

---

## 3.1 ALICE Dataset

Low-voltage PEM fuel cell (≤940 W)

| Variable | Name | Type | Unit | Description | Example |
|----------|------|------|------|-------------|--------|
| Time | heure | Timestamp | HH:MM:SS | Measurement timestamp | 14:42:03 |
| Total time | tpstot(ms) | Numeric | ms | Cumulative time | 173770353 |
| Loop time | tpsboucle(ms) | Numeric | ms | Acquisition loop | 300 |
| Cell voltage | U1 → U20 | Numeric | V | Individual cell voltage | 0.946 |
| H₂ inlet temp | TeH2 | Numeric | °C | Hydrogen inlet temp | 23.764 |
| H₂ outlet temp | TsH2 | Numeric | °C | Hydrogen outlet temp | 33.964 |
| Air inlet temp | TeAIR | Numeric | °C | Air inlet temp | 22.703 |
| Air outlet temp | TsAIR | Numeric | °C | Air outlet temp | 41.995 |
| Water inlet temp | TeEAU | Numeric | °C | Water inlet temp | 52.661 |
| Water outlet temp | TsEAU | Numeric | °C | Water outlet temp | 52.091 |
| Air pressure in | PeAir | Numeric | mbar | Air inlet pressure | 971.739 |
| Air pressure out | PsAir | Numeric | mbar | Air outlet pressure | 976.304 |
| H₂ pressure in | PeH2 | Numeric | mbar | H₂ inlet pressure | 976.132 |
| H₂ pressure out | PsH2 | Numeric | mbar | H₂ outlet pressure | 972.496 |
| H₂ flow in | DeH2 | Numeric | L/min | Hydrogen flow | 7.499 |
| H₂ flow out | DsH2 | Numeric | L/min | Hydrogen outlet flow | 0.040 |
| Air flow in | DeAir | Numeric | L/min | Air inlet flow | 6.923 |
| Air flow out | DsAir | Numeric | L/min | Air outlet flow | 0.083 |
| Current | Courant | Numeric | A | Electric current | 0.025 |
| Stack voltage | Upile | Numeric | V | Stack voltage | 11.259 |
| Water flow | Deau | Numeric | L/min | Water flow | 1.636 |
| Humidity H₂ | HrH2 | Numeric | % | H₂ humidity | 13.097 |
| Humidity Air | HrAir | Numeric | % | Air humidity | 5.369 |

(Additional variables preserved exactly from original dataset.)

---

## 3.2 Ballard Nexa Dataset

1200 W PEM fuel cell

| Variable | Name | Type | Unit | Description | Example |
|----------|------|------|------|-------------|--------|
| Date | Date | Timestamp | DD/MM/YYYY | Date | 31/10/2008 |
| Hour | Hour | Timestamp | HH:MM:SS | Time | 10:16:53 |
| Stack temp | Stack T (C) | Numeric | °C | Stack temp | 26.31 |
| Stack current | Stack I (A) | Numeric | A | Current | 0.48 |
| Lifetime | Stk Life t (s) | Numeric | s | Total runtime | 355575 |
| Lifetime (h) | Stack life (h) | Numeric | h | Runtime hours | 97.77 |
| Stack voltage | Stack V (V) | Numeric | V | Voltage | 40.92 |

---

## 3.3 BZ100 Dataset (Open Access)

1000 W PEM fuel cell

| Variable | Name | Type | Unit | Description | Example |
|----------|------|------|------|-------------|--------|
| Time | Time (h) | Timestamp | h | Operating time | 0.000156 |
| Cell voltages | U1 → U5 | Numeric | V | Cell voltages | 0.658 |
| Stack voltage | Utot | Numeric | V | Total voltage | 3.317 |
| Current density | J | Numeric | A/cm² | Density | 0.7016 |
| Stack current | I | Numeric | A | Current | 70.164 |
| Air/H₂ temps | Tin/Tout | Numeric | °C | Temperatures | - |
| Pressures | Pin/Pout | Numeric | mbar | Pressures | - |
| Flow rates | Din/Dout | Numeric | L/min | Flows | - |

---

## 3.4 Horizon Dataset

| Variable | Name | Type | Unit | Description |
|----------|------|------|------|-------------|
| DC voltage | VDC | Numeric | V | Load voltage |
| FC voltage | VFC | Numeric | V | Fuel cell voltage |
| FC current | IFC | Numeric | A | Current |
| DC current | IDC | Numeric | A | Load current |
| Power | PFC/PDC | Numeric | W | Power |
| Efficiency | Eff | Numeric | - | System efficiency |

---

## 3.5 PM200 Dataset

300 W PEM fuel cell

| Variable | Name | Type | Unit | Description |
|----------|------|------|------|-------------|
| Operating hours | Operating hours | Timestamp | h | Runtime |
| Mean voltage | Mean cell voltage | Numeric | mV | Avg voltage |

---

# 4. Analytics, Scope & Update Frequency

### Temporal Scope
- Short-term → hours to days  
- Long-term → degradation trends  

### Update Frequency
- On-demand  

### Output

Each prediction includes:

- Analysis window  
- Time-series performance predictions  
- Traceability metadata (model version, configuration)

---

# 5. Evaluation Protocols & Metrics

### 5.1 Data Protocol

- Only past data used  
- Predictions reproducible  
- Outputs aligned with elapsed time  

---

### 5.2 Data Gaps

- Missing data handled explicitly  
- Errors logged  
- Fallback behaviour defined  

---

### 5.3 KPIs

- **MAE (Mean Absolute Error)**  
- **RMSE (Root Mean Squared Error)**  

Used for:
- Voltage prediction  
- Power prediction  

---

# 6. Deliverables & Submissions

## 6.1 Reports

1. **Pre-Service Report**
   - Methodology  
   - Architecture  
   - Data requirements  

2. **Intermediate Report**
   - Early results  
   - KPI performance  
   - Adjustments  

3. **Final Report**
   - Final performance  
   - Insights  
   - Scaling recommendations  

---

## 6.2 Technical Deliverables

- API documentation  
- Data formats  
- Authentication  
- Deployment setup (Docker if used)  
- Model versioning  
- Configuration documentation  
- Security & data protection  

---
