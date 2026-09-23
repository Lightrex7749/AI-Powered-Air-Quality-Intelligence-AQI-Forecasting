# 🌫️ AI-Powered Air Quality Intelligence & AQI Forecasting

### AICTE | IBM SkillsBuild Data Analytics with AI Internship 2026 | BharatCares

**Author:** Gaurav Rajbhar

---

## 📌 Overview

Air pollution is a major public health and environmental concern in Indian cities. This project builds an **end-to-end Air Quality Intelligence system** using the CPCB (Central Pollution Control Board) city-level air quality dataset, combining exploratory analytics, leakage-safe AQI forecasting, anomaly detection, and explainable AI into a single coherent pipeline.

This is not a simple "AQI Prediction Using Machine Learning" script — it is a complete analytical case study that investigates real data quality issues, documents genuine dataset artifacts, engineers time-series-safe features, compares multiple models honestly, and explains model behavior using SHAP.

---

## 🎯 Problem Statement

Air quality in Indian cities varies significantly across time and location, driven by seasonal and regional factors. Stakeholders need the ability to **forecast near-future air quality**, **detect abnormal pollution events**, and **understand which factors drive AQI predictions**, in a transparent, interpretable way — without relying on naive random-split evaluation that would leak future information into model training.

## 🎯 Objectives

1. Perform rigorous data quality analysis and cleaning on the CPCB city-level air quality dataset.
2. Conduct exploratory data analysis to understand pollutant distributions, correlations, and temporal/seasonal patterns.
3. Engineer leakage-safe temporal features (lags, rolling statistics) for next-day AQI forecasting.
4. Build and compare three forecasting models — Linear Regression baseline, Random Forest, and XGBoost — using a **chronological** (not random) train/test split.
5. Detect anomalous pollution readings using Isolation Forest.
6. Explain the forecasting model's behavior using SHAP.
7. Summarize findings into a factual, data-driven Air Quality Intelligence report.

---

## 🗂️ Dataset

**Source:** [Air Quality Data in India (2015–2020)](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india) by Rohan Rao, via Kaggle — commonly sourced from CPCB monitoring stations.

**File used:** `city_day.csv`

- **29,531 rows × 16 columns**
- **26 Indian cities**
- **Period:** 2015-01-01 to 2020-07-01
- Fields: `City`, `Date`, pollutant concentrations (`PM2.5`, `PM10`, `NO`, `NO2`, `NOx`, `NH3`, `CO`, `SO2`, `O3`, `Benzene`, `Toluene`, `Xylene`), `AQI`, `AQI_Bucket`.

See [`data/README.md`](data/README.md) for download instructions.

---

## 🧰 Technologies

- Python 3
- Pandas, NumPy — data handling
- Matplotlib, Seaborn — visualization
- Scikit-learn — Linear Regression, Random Forest, Isolation Forest, metrics
- XGBoost — gradient boosting model
- SHAP — model explainability
- Google Colab — development environment

---

## 🧪 Methodology

```
Historical Indian Air Quality (city_day.csv)
          ↓
Data Quality Analysis (missingness, duplicates, invalid values)
          ↓
Data Cleaning (per-city time-based interpolation, median fallback)
          ↓
Exploratory Data Analysis (distributions, correlations, seasonality)
          ↓
Temporal Feature Engineering (lags, rolling averages — per city, leakage-safe)
          ↓
Chronological Train/Test Split (no random shuffling)
          ↓
Model Training & Comparison (Linear Regression, Random Forest, XGBoost)
          ↓
Pollution Anomaly Detection (Isolation Forest)
          ↓
SHAP Explainability
          ↓
Error Analysis & Air Quality Intelligence Summary
```

### Key Design Decisions

- **No random shuffling for forecasting evaluation** — an 80/20 chronological split (train: 2015-01-08 to 2019-12-14, test: 2019-12-15 to 2020-06-30) was used to simulate real-world forecasting and avoid temporal leakage.
- **Lag and rolling features computed per-city** — ensuring no city's temporal sequence leaks into another's.
- **Target variable never imputed** — AQI values were left missing where genuinely missing; only predictor/feature columns were imputed.
- **`Xylene` dropped** (61.3% missing) — too sparse to be reliable.

---

## 🤖 Models & Results

Next-day AQI forecasting, evaluated on a chronological hold-out test set:

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| Linear Regression (Baseline) | 27.24 | 50.68 | 0.671 |
| **Random Forest (Selected)** | **27.20** | **49.96** | **0.681** |
| XGBoost | 27.87 | 50.32 | 0.676 |

**Random Forest** was selected as the best-performing model based on RMSE. Notably, all three models perform comparably, indicating that engineered lag/rolling AQI features capture most of the learnable temporal signal in this dataset.

### SHAP Top Predictive Features

| Rank | Feature | Mean \|SHAP\| |
|---|---|---:|
| 1 | `AQI_roll_mean_7` (7-day rolling AQI average) | 48.34 |
| 2 | `AQI_lag_1` | 13.20 |
| 3 | `PM2.5_lag_1` | 12.97 |

### Anomaly Detection

- **1,243 anomalies detected (5.0%)** using Isolation Forest on AQI + key pollutants.
- Average AQI of anomalies: **545.08** vs. **146.53** for normal observations — validating that flagged points correspond to genuine severe pollution episodes.
- Anomalies were concentrated in **Ahmedabad (686)**, **Delhi (233)**, and **Jorapokhar (94)** — consistent with these cities' high average AQI.

---

## 📊 Key Findings

- **Strong seasonal pattern**: Winter (avg AQI 220.61) and Post-Monsoon (215.47) are significantly worse than Monsoon (115.56).
- **Clear geographic divide**: North Indian cities (Ahmedabad 452.12, Delhi 259.49, Patna 240.78) have far higher average AQI than South/Northeastern cities (Aizawl 34.77, Shillong 53.80, Coimbatore 73.02).
- **Ahmedabad is a persistent outlier** — highest average AQI, most anomalies, and the highest model prediction error (MAE 103.37, ~2.5x the next-worst city) — a consistent, cross-validated pattern.
- **Recent AQI history dominates predictive power**; calendar/seasonal features contribute comparatively little once lag/rolling features are included.
- **Data quality artifacts were identified and documented honestly** — e.g., a median-imputation spike in Gurugram's PM10 distribution, and large real AQI data gaps in Ahmedabad (340-day and 232-day stretches).

---

## 📁 Project Structure

```
AI-Air-Quality-Intelligence/
│
├── notebooks/
│   └── Gaurav_Rajbhar_AQI_Intelligence.ipynb
│
├── data/
│   └── README.md
│
├── Graphs/
│   └── Generated analysis figures (`.png`)
│
├── README.md
├── requirements.txt
├── Gaurav_Rajbhar_AQI_ProjectReport.docx
└── .gitignore
```

---

## ⚙️ Installation & Usage

### Option 1: Google Colab (Recommended)

1. Open `notebooks/Gaurav_Rajbhar_AQI_Intelligence.ipynb` in [Google Colab](https://colab.research.google.com/).
2. Run cells sequentially — the notebook uses `kagglehub` to automatically download the dataset (no manual upload needed).
3. All models, plots, and metrics will be generated in-notebook.

### Option 2: Local Environment

Use Python 3.10 or newer and create an isolated virtual environment before
installing the pinned dependencies:

```bash
git clone https://github.com/Lightrex7749/AI-Powered-Air-Quality-Intelligence-AQI-Forecasting.git
cd AI-Powered-Air-Quality-Intelligence-AQI-Forecasting
python -m venv .venv
# macOS/Linux: source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter notebook notebooks/Gaurav_Rajbhar_AQI_Intelligence.ipynb
```

See [`data/README.md`](data/README.md) for dataset download instructions if running locally.

The notebook also regenerates the analysis figures under `Graphs/`, including
AQI distributions, temporal trends, model comparisons, prediction errors,
anomaly timelines, and SHAP explanations.

### Reproducibility Checklist

1. Install the pinned packages from `requirements.txt`.
2. Download `city_day.csv` using the instructions in `data/README.md`.
3. Run the notebook from the repository root so relative paths resolve.
4. Compare regenerated figures and metrics with the committed analysis outputs.

---

## ⚠️ Limitations

- Reported metrics and figures correspond to the pinned dependency versions in
    `requirements.txt`; rerunning with different versions may produce small
    numerical or rendering differences.
- Uneven city coverage (96 to 2,009 rows per city in the final modeling set).
- High missingness in some pollutants required imputation (`Xylene` dropped entirely; `PM10`, `NH3` imputed with documented artifacts, e.g., a median-fill spike in Gurugram's PM10 data).
- Some cities have zero readings for entire pollutants (e.g., Lucknow has no PM10 data) — a real CPCB monitoring station limitation.
- No weather/meteorological data included, likely contributing to unexplained forecasting variance (R² ≈ 0.68).
- Forecast horizon limited to next-day only.
- Test period includes the COVID-19 lockdown (an atypical event with no precedent in training data).
- Anomaly detection is statistical, not a verified real-world pollution event confirmation.

Full details in the notebook's Limitations section and project report.

---

## 🚀 Future Scope

- Integrate weather/meteorological data.
- City-aware or city-specific modeling to reduce error concentration (e.g., Ahmedabad).
- Longer forecasting horizons (3-day, 7-day ahead).
- Deep learning sequence models (LSTM/GRU).
- Real-time CPCB API integration.
- Interactive dashboard (Streamlit) and API deployment.

---

## 📚 References

This project was developed independently, informed by publicly available Indian AQI analytics repositories for understanding dataset structure and common methodological approaches (e.g., avoiding temporal leakage):

1. [air_cast](https://github.com/Manglam11/air_cast) — Indian AQI forecasting
2. [AQI-Prediction-India-ComparativeStudy](https://github.com/dhriti-kourla/AQI-Prediction-India-ComparativeStudy)
3. [AQI-prediction-system](https://github.com/sayitisha/AQI-prediction-system)
4. [air-aware](https://github.com/gayathri-cg/air-aware)
5. [india-cpcb-aqi](https://github.com/Vonter/india-cpcb-aqi)

**Dataset:** [Air Quality Data in India](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india) by Rohan Rao (Kaggle).

All code, analysis, feature engineering, and conclusions in this repository are independently developed and based on outputs generated from this dataset.

---

## 📄 License

This project is submitted as part of the AICTE | IBM SkillsBuild Data Analytics with AI Internship 2026 (BharatCares). Dataset usage complies with its original Kaggle licensing terms.