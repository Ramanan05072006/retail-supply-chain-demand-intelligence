# retail-supply-chain-demand-intelligence

**Retail Analytics | Demand Forecasting | Inventory Optimization | Business Intelligence**

> An end-to-end retail analytics project that transforms historical sales data into demand forecasts, inventory-risk alerts, and replenishment recommendations, with simulated South Indian festival-demand scenarios.

**Project status:** In Progress  
**Primary tools:** Python, SQL, Power BI  
**Forecasting:** Seasonal-Naive Baseline, Holt-Winters Triple Exponential Smoothing  
**Business focus:** Stockout prevention, excess-inventory reduction, demand planning, and inventory turnover

---

## 1. Business Problem

Retail businesses must maintain enough inventory to meet customer demand without tying up excessive capital in unsold products.

Demand can vary by product, store, weekday, promotion, season, and festival period. Inaccurate forecasts can lead to stockouts, lost sales, emergency replenishment, and excess inventory.

This project develops a retail demand-intelligence pipeline to help answer five business questions:

1. How much demand should we expect over the next 7, 14, and 30 days?
2. Which products may run out of stock before the next replenishment?
3. Which products have excessive inventory relative to forecast demand?
4. How much inventory should be reordered, and when?
5. How do seasonal patterns and simulated Pongal and Diwali demand scenarios affect inventory decisions?

The objective is to turn historical sales data into clear, explainable business recommendations.

## 2. Project Objectives

- Analyze historical retail sales at daily, store, and product-family levels.
- Build a reproducible data-cleaning and feature-engineering pipeline.
- Use SQL to calculate retail performance and demand metrics.
- Compare a seasonal-naive baseline against Holt-Winters forecasting.
- Evaluate models using time-based validation without look-ahead bias.
- Simulate inventory levels, supplier lead times, and replenishment decisions.
- Develop traffic-light alerts for critical stock and excess inventory.
- Build an interactive Power BI dashboard for operational and management decisions.
- Investigate hypothetical festival-demand changes using clearly labelled scenarios.

## 3. Dataset

### Primary source

**Kaggle — Store Sales: Time Series Forecasting**

Dataset: https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data

The dataset contains historical sales for retail stores in Ecuador, together with product-family, promotion, store, and supplementary information.

### Key files

| File | Purpose |
|---|---|
| `train.csv` | Historical sales and training features |
| `test.csv` | Dates and features for the competition test period |
| `stores.csv` | Store metadata |
| `oil.csv` | Historical oil-price information |
| `holidays_events.csv` | Holiday and event information |
| `transactions.csv` | Daily transaction counts by store |
| `sample_submission.csv` | Example competition submission format |

The available files and columns will be verified after downloading the dataset.

### Data integrity

Historical sales and source-provided features will remain distinguishable from simulated data.

This dataset does not supply all the inventory-on-hand, purchase-order, unit-cost, and supplier-lead-time information required for a complete inventory-control system. Missing operational data will therefore be generated as synthetic simulation inputs and documented accordingly.

**Important:** The source retailer is not based in South India. Pongal and Diwali will be introduced as hypothetical calendar scenarios to test how the inventory system responds to demand changes. They will not be presented as observed festival effects in the original dataset.

The dataset will be downloaded from its official source. Raw data files will not be committed to GitHub.

## 4. Proposed Technology Stack

| Technology | Purpose |
|---|---|
| Python | Data processing, analysis, forecasting, simulation |
| Pandas and NumPy | Data cleaning and feature engineering |
| Statsmodels | Holt-Winters Triple Exponential Smoothing |
| Scikit-learn | Forecast evaluation utilities where appropriate |
| SQL | Aggregation, joins, window functions, business metrics |
| SQLite or PostgreSQL | Analytical data storage |
| Matplotlib | Exploratory analysis and forecast visualizations |
| Power BI | Interactive business intelligence dashboard |
| Git and GitHub | Version control and project documentation |

## 5. System Architecture

```text
Public Retail Dataset
        |
        v
Data Ingestion and Validation
        |
        v
Cleaning and Feature Engineering
        |
        v
SQL Analytical Data Model
        |
        +--------------------------+
        |                          |
        v                          v
Exploratory Analysis       Demand Forecasting
                                   |
                          +--------+--------+
                          |                 |
                          v                 v
                    Seasonal-Naive     Holt-Winters
                          |                 |
                          +--------+--------+
                                   |
                                   v
                         Forecast Evaluation
                                   |
                                   v
                     Inventory Simulation Engine
                                   |
                   +---------------+---------------+
                   |               |               |
                   v               v               v
                Stockout       Excess Stock     Reorder
                  Risk            Risk         Decisions
                   |               |               |
                   +---------------+---------------+
                                   |
                                   v
                         Power BI Dashboard
                                   |
                                   v
                        Business Recommendations
```

## 6. Analytical Methodology

### Phase 1 — Data Preparation

- Inspect schemas, data types, date coverage, and missing values.
- Validate store and product identifiers.
- Check duplicate records and daily sales totals.
- Identify zero-sales days and distinguish them from missing observations.
- Examine promotions, holidays, and unusual sales spikes.
- Document every transformation and data-quality assumption.

### Phase 2 — Exploratory Data Analysis

Investigate:

- Daily, weekly, and monthly demand patterns.
- Store-level and product-family performance.
- Sales variability and intermittent demand.
- Promotion-associated demand changes.
- Holiday and seasonal patterns.
- Products with consistently high or unstable demand.

### Phase 3 — Demand Forecasting

**Model A: Seasonal-naive baseline**

Use demand from the corresponding previous seasonal period as the benchmark where sufficient history exists.

**Model B: Holt-Winters Triple Exponential Smoothing**

Model demand using:

- Level — the underlying demand magnitude.
- Trend — the direction and rate of change.
- Seasonality — recurring demand patterns.

Test additive or multiplicative seasonality when the data supports the chosen specification.

Forecasting frequency and seasonal period will be selected based on the data and business use case.

### Phase 4 — Model Evaluation

Compare both models on identical chronological holdout periods.

Metrics:

- MAE — Mean Absolute Error.
- RMSE — Root Mean Squared Error.
- WAPE — Weighted Absolute Percentage Error.

Use rolling-origin validation where practical. All training, feature construction, and model selection must respect the forecast date.

The advanced model will not automatically be considered superior; performance must be demonstrated through evaluation.

### Phase 5 — Inventory Intelligence

Build a synthetic inventory simulation with configurable:

- Opening inventory.
- Daily demand.
- Supplier lead time.
- Outstanding orders.
- Safety stock.
- Replenishment quantities.
- Unit costs and inventory valuation.

Calculate inventory coverage, stockout risk, excess inventory, reorder points, and suggested replenishment quantities.

### Phase 6 — Festival-Demand Scenarios

Introduce configurable scenarios for Pongal and Diwali.

For example, test hypothetical demand multipliers of 1.10, 1.25, and 1.40 against a non-festival baseline.

These are experimental assumptions, not established demand effects.

Compare the scenarios to assess:

- Forecast sensitivity.
- Additional inventory requirements.
- Potential stockout exposure.
- Excess inventory after the simulated event.
- Replenishment timing.

### Phase 7 — Power BI Reporting

Connect validated analytical outputs to Power BI and create three report pages:

1. Executive Overview.
2. Demand Forecasting & Seasonality.
3. Inventory Replenishment Control Tower.

## 7. Inventory Alerting Matrix

The initial business rules are:

| Status | Initial rule | Recommended action |
|---|---|---|
| 🔴 Red — Critical | Days of cover < 3 | Prioritize stock review and replenishment |
| 🟡 Yellow — Excess | Days of cover > 60 | Review purchasing plans and inventory reduction |
| 🟢 Green — Normal | Days of cover between 3 and 60 | Monitor demand and turnover |

These thresholds are configurable assumptions, not universal retail standards.

A separate lead-time risk rule will identify products that may run out before a replenishment can arrive, even when days of cover exceed three.

Days of cover will be undefined when forecast demand is zero or unavailable. Such cases will be flagged for review instead of being assigned a misleading status.

### Core metrics

**Days of Cover**

`Available Inventory / Forecast Average Daily Demand`

**Reorder Point**

`Expected Demand During Lead Time + Safety Stock`

**Suggested Order Quantity**

`MAX(0, Target Inventory - Available Inventory - On-Order Inventory)`

The reorder calculation will account for demand during supplier lead time, safety stock, outstanding orders, and the selected inventory policy. Supplier minimum order quantities and order multiples can be added as constraints.

These calculations will initially operate on simulated inventory data, not on actual inventory records from the source retailer.

## 8. Planned Power BI Dashboard

### Page 1 — Executive Overview

- Total sales.
- Inventory value.
- Products at critical stock risk.
- Products with excess inventory.
- Forecast demand.
- Inventory turnover.

### Page 2 — Demand Intelligence

- Actual versus forecast demand.
- Forecast error by model.
- Demand trends by store and product family.
- Promotion and holiday comparisons.
- Festival-scenario analysis.

### Page 3 — Replenishment Control Tower

- Traffic-light inventory alert matrix.
- Days of cover by product and store.
- Supplier lead-time risk.
- Suggested replenishment quantities.
- Estimated inventory value at risk.
- Prioritized actions for the retail manager.

## 9. Repository Structure

```text
retail-supply-chain-demand-intelligence/
|
|-- README.md
|-- requirements.txt
|-- .gitignore
|
|-- data/
|   |-- README.md
|   |-- raw/
|   |-- processed/
|   `-- synthetic/
|
|-- notebooks/
|   |-- 01_data_exploration.ipynb
|   |-- 02_data_cleaning.ipynb
|   |-- 03_demand_analysis.ipynb
|   |-- 04_forecast_baseline.ipynb
|   |-- 05_holt_winters_forecasting.ipynb
|   `-- 06_inventory_simulation.ipynb
|
|-- sql/
|   |-- 01_schema.sql
|   |-- 02_sales_analysis.sql
|   |-- 03_demand_metrics.sql
|   `-- 04_inventory_alerts.sql
|
|-- src/
|   |-- data_ingestion.py
|   |-- data_cleaning.py
|   |-- feature_engineering.py
|   |-- forecasting.py
|   |-- forecast_evaluation.py
|   |-- inventory_simulation.py
|   `-- replenishment.py
|
|-- dashboard/
|   |-- README.md
|   `-- screenshots/
|
|-- reports/
|   |-- figures/
|   |-- forecast_metrics/
|   `-- business_insights.md
|
`-- docs/
    |-- data_dictionary.md
    |-- methodology.md
    `-- assumptions_and_limitations.md
```

## 10. Reproducibility

The project will use a documented Python environment and explicit data-preparation steps.

Once the pipeline is implemented, users should be able to:

1. Download the source dataset.
2. Place the files in the documented raw-data directory.
3. Install the required Python packages.
4. Run the preparation and analysis scripts.
5. Reproduce the forecast evaluation and inventory simulation.
6. Open the Power BI report using the documented data connection.

Raw datasets and generated files that are too large for GitHub will be excluded from version control. Data download instructions and expected file locations will be provided.

## 11. Limitations

- Source sales data represents an Ecuadorian retailer rather than a South Indian retailer.
- Inventory, outstanding purchase orders, and supplier lead times are not fully available in the primary source and must be simulated.
- Festival-demand effects are scenario assumptions until validated against suitable local data.
- Forecast accuracy can vary by product and demand pattern.
- Simulated inventory outcomes are not evidence of actual business savings.
- Historical forecasting performance does not guarantee future results.

## 12. Project Roadmap

- [ ] Select and download the primary dataset.
- [ ] Validate the source schema and date range.
- [ ] Complete exploratory data analysis.
- [ ] Build the cleaning and feature-engineering pipeline.
- [ ] Create the SQL analytical model.
- [ ] Implement the seasonal-naive baseline.
- [ ] Implement Holt-Winters forecasting.
- [ ] Evaluate models using chronological validation.
- [ ] Build the synthetic inventory simulation.
- [ ] Implement stockout, excess-stock, and lead-time alerts.
- [ ] Test Pongal and Diwali demand scenarios.
- [ ] Build the Power BI dashboard.
- [ ] Document findings and limitations.
- [ ] Publish reproducible results and dashboard screenshots.

## 13. Portfolio Outcomes

The completed project is intended to demonstrate practical skills in:

- Python data analysis and data engineering.
- SQL-based business analytics.
- Time-series forecasting and statistical evaluation.
- Inventory planning and supply chain metrics.
- Business rules and decision-support systems.
- Power BI dashboards and data storytelling.
- Reproducible analytics and technical documentation.

**Final objective:** Deliver a transparent, reproducible retail analytics system that connects demand forecasting to inventory decisions and communicates actionable insights to business stakeholders.

---

## Data Source and Attribution

Store Sales — Time Series Forecasting. Kaggle competition dataset.

https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data

Please consult the dataset's current terms and competition rules before redistributing any source data.
