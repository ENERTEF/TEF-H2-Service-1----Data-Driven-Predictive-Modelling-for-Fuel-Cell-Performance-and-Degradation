# Data-Driven Predictive Modelling for Fuel Cell Performance and Degradation  
### Technical Manual & Service Specification v1.0

Author: University of Technology of Belfort-Montbéliard (UTBM)  
Version: 1.0  
Last updated: 11 February 2026  

---

# 1. Business Context & Definitions

The Data-Driven Predictive Modelling for Fuel Cell Performance and Degradation service delivers short- and long-term predictions of fuel-cell performance evolution and degradation for hydrogen-based energy systems. The service is designed to support operational monitoring, predictive maintenance, and lifecycle optimisation for fuel-cell operators, system integrators, and technology developers.

Accurate and timely insight into fuel-cell performance degradation is critical to maintain efficiency, avoid unplanned downtime, and reduce maintenance and operational costs. By leveraging historical and operational data, the service enables early detection of degradation trends, continuous assessment of system health, and informed decision-making under varying operating and environmental conditions. The service is intended for regular operational use and is developed with clear expectations regarding data availability, prediction robustness, and transparency, ensuring its suitability for industrial and research-oriented hydrogen applications.

---

## Key terms

• Fuel Cell System / Test Bench:  
The service is applied to hydrogen fuel-cell systems available on the UTBM experimental platforms. These systems include laboratory fuel-cell stacks, dedicated PEMFC and SOFC test benches, and hybrid mobility setups. Experiments are conducted offline with sub-second temporal resolution, and the resulting datasets are hosted on local platforms accessible to authorised users  

• Elapsed Time:  
Measurements are indexed using elapsed operating time, representing the cumulative duration of fuel-cell operation and aligned with the data acquisition frequency of the experimental platforms. The datasets typically cover long experimental campaigns, ranging from several hundreds to several thousands of operating hours, which enable detailed analysis of performance evolution and degradation behaviour throughout the fuel-cell lifetime  

• Electrical Performance Variables:  
The core performance information consists of electrical measurements such as fuel-cell voltage, current, and power. These variables serve as the primary indicators for assessing system behaviour, efficiency, and health evolution over time  

• Operating and System Parameters:  
In addition to electrical signals, the service can exploit complementary operating parameters that influence fuel-cell performance and ageing. These include reactant gas flow rates, reactant pressures, stack and component temperatures, and other operation-related measurements available from the experimental setup. Incorporating these parameters allows a more comprehensive representation of degradation mechanisms under varying operating conditions  

• Prediction Horizon:  
Time window ahead for which performance evolution or degradation is estimated, ranging from short-term forecasts (hours to days under steady conditions) to longer-term degradation trend prediction under dynamic operation  

• Resolution / Granularity:  
The temporal resolution of inputs and outputs depends on the dataset and corresponds to the sampling frequency of the available measurements. It ranges from several seconds to hourly data, depending on the experimental setup and application. The forecast step is therefore defined accordingly and matches the native temporal resolution of the input data, ensuring consistency between measurements and prediction outputs  

• Model Validation / Backtesting:  
Evaluation of predictive performance using historical datasets, including testing on unseen operating periods to emulate real operational use and assess accuracy for health monitoring and maintenance support  

---

# 1.1 Fuel-Cell Operator and Developer Context

This service is intended for fuel-cell operators, system integrators, and technology developers working with hydrogen fuel-cell systems. Its primary objective is to support performance monitoring, degradation assessment, and maintenance planning by providing reliable predictions of fuel-cell performance evolution and health state over time. By enabling early identification of degradation trends, the service helps reduce operational uncertainty, avoid unexpected downtime, and optimise maintenance strategies, which are critical for improving system availability and cost efficiency.

In practical use, the service supports both short-term and long-term prognostic tasks, depending on the availability of operational data. When only voltage measurements are available and the fuel cell operates under steady-state conditions, the service relies on historical voltage time series to perform short-term prediction of voltage evolution over near-future horizons (up to two days ahead). When additional operating parameters are available, such as current, reactant flow rates, pressures, and temperatures, the service extends its capability to long-term degradation prediction, even under dynamic load conditions. This enriched representation allows the models to capture the cumulative influence of operating regimes on fuel-cell performance over time. As a result, the service can be applied across different monitoring configurations, providing prognostic insights for systems with limited sensing (e.g. voltage-only) and more advanced setups with comprehensive operational measurements.

The service operates within a well-defined scope in which the service provider delivers predictive outputs related to fuel-cell performance, degradation evolution, and health state assessment through a transparent and documented modelling framework. Model configurations, assumptions, and versions are explicitly managed to ensure traceability and reproducibility of results. Users, such as fuel-cell operators, system integrators, or researchers, are responsible for providing the necessary operational measurements, including electrical signals and relevant operating parameters, along with contextual information describing the monitored fuel-cell system. The service provider oversees the full data-processing and modelling workflow, including data validation, quality assurance, and the evaluation of model to ensure that the predictive results are meaningful and dependable.

---

# 2. Problem Statement

The objective of this service is to provide a reliable and operational AI-based capability for predicting the performance evolution and degradation of hydrogen fuel-cell systems. The service is designed to support condition monitoring and maintenance planning by delivering consistent and traceable predictions.

The service operates on historical and newly acquired operational data and provides performance and state of health predictions over predefined analysis horizons. Inputs and outputs are aligned using elapsed operating time, ensuring consistency across long-term experimental campaigns. Each prediction is generated with clear metadata describing the analysis period, model configuration, and data sources, enabling reproducibility and auditability of results. Predicted values and derived health indicators must remain physically and operationally plausible with respect to the monitored fuel-cell system.

---

# 3. Data Description

The service uses multiple hydrogen fuel-cell datasets covering stack powers from 300 W to 7.7 kW. The datasets contain time-series measurements of electrical, thermal, and flow parameters recorded under various operating and degradation conditions. Key datasets include ALICE, Ballard Nexa, BZ100, Horizon, and PM200, with BZ100 available as an open-access dataset. Additional data may be provided by ongoing experimental activities and digital models within the TEF H₂ satellite.

---

# 3.1 Data Dictionary for ALICE Dataset

The ALICE dataset comprises historical experimental measurements acquired from a low-voltage PEM fuel-cell stack with a nominal power rating of up to 940 W. The dataset is maintained in UTBM’s internal data repositories and provided in standard tabular formats. Access to the data is subject to confidentiality and usage constraints in accordance with the project’s data management and non-disclosure agreements.

| Variable | Variable name | Type | Measurement unit | Description | Allowed values / Examples |
|----------|--------------|------|------------------|-------------|---------------------------|
| Time | heure | Timestamp | HH:MM:SS | Timestamp of each recorded measurement | 14:42:03 |
| Total time | tpstot(ms) | Numeric | ms | Cumulative elapsed time | 173770353 |
| Loop time | tpsboucle(ms) | Numeric | ms | Duration of acquisition loop | 300 |
| Cell voltage | U1 → U20 | Numeric | V | Voltage of each individual cell in the stack. | 0,946 |
| H₂ inlet temperature | TeH2 | Numeric | °C | Temperature of hydrogen gas entering the stack | 23.764 |
| H₂ outlet temperature | TsH2 | Numeric | °C | Temperature of hydrogen gas exiting the stack | 33.964 |
| Air inlet temperature | TeAIR | Numeric | °C | Temperature of air entering the stack | 22.703 |
| Air outlet temperature | TsAIR | Numeric | °C | Temperature of air exiting the stack | 41.995 |
| Water inlet temperature | TeEAU | Numeric | °C | Temperature of water entering the stack | 52.661 |
| Water outlet temperature | TsEAU | Numeric | °C | Temperature of water exiting the stack | 52.091 |
| Air inlet pressure | PeAir | Numeric | mbar | Pressure of air entering the stack | 971.739 |
| Air outlet pressure | PsAir | Numeric | mbar | Pressure of air exiting the stack | 976.304 |
| H₂ inlet pressure | PeH2 | Numeric | mbar | Pressure of hydrogen entering the stack | 976.132 |
| H₂ oulet pressure | PsH2 | Numeric | mbar | Pressure of hydrogen exiting the stack | 972.496 |
| H₂ inlet flow | DeH2 | Numeric | L/min | Flow rate of hydrogen entering the stack | 7.499 |
| H₂ outlet flow | DsH2 | Numeric | L/min | Flow rate of hydrogen exiting the stack | 0.040 |
| Air inlet flow rate | DeAir | Numeric | L/min | Flow rate of air entering the stack | 6,923 |
| Air outlet flow rate | DsAir | Numeric | L/min | Flow rate of air exiting the stack | 0,083 |
| Current | Courant | Numeric | A | Electric current | 0,025 |
| Boiling temperature | Tbou | Numeric | °C | Temperature near the boiling point | 20,434 |
| H₂ supply temperature | TsupcapH2 | Numeric | °C | Temperature of the hydrogen supply line | 26,927 |
| H₂ collector temperature | TcolH2 | Numeric | °C | Temperature of the hydrogen collection line. | 23,289 |
| Stack voltage | Upile | Numeric | V | Total voltage measured across the fuel cell stack. | 11,259 |
| H₂ vaporizer temperature | TvapoH2 | Numeric | °C | Temperature of the hydrogen vaporizer | 22,042 |
|  | Trech | Numeric | °C |  | 49,439 |
| Water flow rate | Deau | Numeric | L/min | Flow rate of water | 1,636 |
| Saturation temperature | Tsat | Numeric | °C |  | 24,143 |
| H₂ water flow | DeauH2 | Numeric | L/min | Water flow rate measured in the hydrogen circuit | 1,115 |
| H₂ trace heater temperature | Ttracehy | Numeric | °C | Temperature of the hydrogen line trace heater. | 24,415 |
|  | niveauBH2 | Numeric |  |  | 0,005 |
|  | THTBH2 | Numeric |  |  | 21,999 |
|  | TBSBH2 | Numeric |  |  | 22,066 |
| H₂ relative humidity | HrH2 | Numeric |  | Relative humidity of the hydrogen stream | 13,097 |
|  | Ttbousep | Numeric | °C | Temperature of | 22,193 |
| Air relative humidity | HrAir | Numeric |  | Relative humidity of the air stream. | 5,369 |
|  | TbouH2cons | Numeric | °C |  | 1,468 |
| Air relative humidity | HrAirFC | Numeric |  | Relative humidity of the air inside the fuel cell | 4,141 |
|  | TLCAircons | Numeric | °C |  | 58 |

---

# 3.2 Data Dictionary for Ballard Nexa Dataset

The Ballard Nexa dataset comprises historical experimental measurements collected from a low-voltage PEM fuel-cell stack with a nominal power rating of 1200 W. The dataset is stored in UTBM’s internal repositories, provided in standard tabular formats, and subject to confidentiality and access restrictions in accordance with the project’s data management and non-disclosure agreements.

| Variable | Variable name | Type | Measurement unit | Description | Allowed values / Examples |
|---|---|---|---|---|---|
| Date | Date | Timestamp | DD/MM/YYYY | Calendar date of measurement | 31/10/2008 |
| Hour | Hour | Timestamp | HH:MM:SS | Time of measurement during the day. | 10:16:53 |
| Stack temperature | Stack T (C) | Numeric | °C | Measured temperature of the fuel cell stack | 26,31 |
| Stack temperature (rounded) | Stack T Rounded (C) | Numeric | °C | Rounded value of stack temperature | 25 |
| Stack current | Stack I (A) | Numeric | A | Instantaneous current produced by the fuel cell stack. | 0,48 |
| Stack current (rounded) | Stack I Rounded (A) | Numeric | A | Rounded current value | 0 |
| Stack life time (s) | Stk Life t (s) | Numeric | s | Cumulative operating time of the fuel cell stack in seconds. | 355575 |
| Stack life time (h) | Stack life (h) | Numeric | h | Cumulative operating time of the fuel cell stack in hours. | 97,77 |
| Stack voltage | Stack V (V) | Numeric | V | Measured voltage of the fuel cell stack | 40,92 |

---

# 3.3 Data Dictionary for BZ100 Dataset

The BZ100 dataset consists of historical experimental measurements collected from a low-voltage PEM fuel-cell stack with a nominal power rating of 1000 W. The dataset is available as an open-access resource.

| Variable | Variable name | Type | Measurement unit | Description | Allowed values / Examples |
|---|---|---|---|---|---|
| Time | Time (h) | Timestamp | h | Elapsed operating time since the start of the test. | 0,000156 |
| Cell voltage | U1 → U5 (V) | Numeric | V | Individual voltages of each cell in the fuel cell stack. | 0,658 |
| Stack voltage | Utot (V) | Numeric | V | Total voltage of the 5-cell stack (sum of individual cell voltages). | 3,317 |
| Current density | J (A/cm²) | Numeric | A·cm⁻² | Current density produced by the fuel cell. | 0,7016 |
| Stack current | I (A) | Numeric | A | Total current of the fuel cell stack. | 70,164 |
| H₂ inlet temperature | TinH2 (°C) | Numeric | °C | Hydrogen temperature at the inlet. | 25,93 |
| H₂ outlet temperature | ToutH2 (°C) | Numeric | °C | Hydrogen temperature at the outlet. | 38,89 |
| Air inlet temperature | TinAIR (°C) | Numeric | °C | Air temperature at the inlet. | 41,92 |
| Air outlet temperature | ToutAIR (°C) | Numeric | °C | Air temperature at the outlet. | 51,5 |
| Water inlet temperature | TinWAT (°C) | Numeric | °C | Water temperature at the inlet. | 53,81 |
| Water outlet temperature | ToutWAT (°C) | Numeric | °C | Water temperature at the outlet. | 55,5 |
| Air inlet pressure | PinAIR (mbara) | Numeric | mbar | Air pressure measured at the inlet. | 1302,2 |
| Air outlet pressure | PoutAIR (mbara) | Numeric | mbar | Air pressure measured at the outlet. | 1269,6 |
| H₂ outlet pressure | PoutH2 (mbara) | Numeric | mbar | Hydrogen pressure measured at the outlet. | 1305,32 |
| H₂ inlet pressure | PinH2 (mbara) | Numeric | mbar | Hydrogen pressure measured at the inlet. | 1292,49 |
| H₂ inlet flow rate | DinH2 (l/mn) | Numeric | L·min⁻¹ | Hydrogen flow entering the stack. | 4,799 |
| H₂ outlet flow rate | DoutH2 (l/mn) | Numeric | L·min⁻¹ | Hydrogen flow exiting the stack. | 2,115 |
| Air inlet flow rate | DinAIR (l/mn) | Numeric | L·min⁻¹ | Air flow entering the stack. | 23,038 |
| Air outlet flow rate | DoutAIR (l/mn) | Numeric | L·min⁻¹ | Air flow exiting the stack. | 21,331 |
| Water flow rate | DWAT (l/mn) | Numeric | L·min⁻¹ | Flow rate of the water. | 2,02 |
| Fuel cell air humidity | HrAIRFC (%) | Numeric | % | Relative humidity of the air supplied to the fuel cell. | 48,9 |

---

# 3.4 Data Dictionary for Horizon Dataset

| Variable | Variable name | Type | Measurement unit | Description | Allowed values / Examples |
|---|---|---|---|---|---|
| DC voltage | VDC | Numeric | V | output DC bus voltage (load) | 47,774 |
| Fuel cell voltage | VFC | Numeric | V | Voltage measured at the fuel cell | 51,281 |
| Fuel cell current | IFC | Numeric | A | Current delivered by the fuel cell | 0,046 |
| DC current | IDC | Numeric | A | output DC bus current (load) | 0,006 |
| Fuel cell control current | IFC_control | Numeric | A | Fuel cell control current | 0,128 |
| Time | T | Timestamp |  | Elapsed test time | 6896,239 |
| Power control | P_control | Numeric | W | Power consumption of control units | 6,56 |
| Fuel cell power | PFC | Numeric | W | Power output of the fuel cell | 2,35 |
| DC Power | PDC | Numeric | W | Electrical DC power | 0,47 |
| Total power | PFCs | Numeric | W | Total power output of fuel cell (if multiple FC) | 8,92 |
| System efficiency | Eff | Numeric |  | Efficiency of the system | 0,0287 |

---

# 3.5 Data Dictionary for PM200 Dataset

The Horizon dataset consists of historical experimental measurements acquired from a low-voltage PEM fuel-cell stack with a nominal power rating of 300 W. The dataset is stored in UTBM’s internal repositories, provided in standard tabular formats, and subject to access restrictions and confidentiality requirements defined by the project’s data management and non-disclosure agreements.

| Variable | Variable name | Type | Measurement unit | Description | Allowed values / Examples |
|---|---|---|---|---|---|
| Operating hours | Operating hours (h) | Timestamp | h | Total cumulative operating time of the fuel cell system | 0,543 |
| Mean cell voltage | Mean cell voltage (mV) | Numeric | mV | Average cell voltage | 627 |

---

# 4. Analytics, Scope & Update Frequency

• Temporal scope: Predictions cover defined short- and medium-term horizons depending on the use case, ranging from near-term performance evolution (hours to days) to longer-term degradation trend assessment. Outputs are aligned with the native temporal resolution of the input data and referenced to elapsed operating time.

• Update frequency: Predictions are generated on demand upon request.

• Output format:  
For each monitored fuel-cell system, the service returns:

- Analysis reference and validity window  
- Time-indexed performance predictions  
- Traceability metadata  

---

# 5. Evaluation Protocols & Metrics

• Mean Absolute Error (MAE)  
• Root Mean Squared Error (RMSE)

---

# 6. Deliverables & Submissions

### Reports
1. Pre-Service Report  
2. Intermediate Report  
3. Final Report  

### Technical
- API documentation  
- Deployment setup  
- Versioning  
- Security documentation  

---
