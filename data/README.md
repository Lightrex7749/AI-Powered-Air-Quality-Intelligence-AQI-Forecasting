# Dataset Information

This project uses the **Air Quality Data in India (2015–2020)** dataset
from Kaggle, specifically the `city_day.csv` file.

## Source

- **Kaggle Dataset:** [Air Quality Data in India](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india)
- **Author:** Rohan Rao
- **File used:** `city_day.csv`

## Dataset Description

- **Rows:** 29,531
- **Columns:** 16 (`City`, `Date`, `PM2.5`, `PM10`, `NO`, `NO2`, `NOx`,
  `NH3`, `CO`, `SO2`, `O3`, `Benzene`, `Toluene`, `Xylene`, `AQI`,
  `AQI_Bucket`)
- **Cities covered:** 26 Indian cities
- **Time period:** 2015-01-01 to 2020-07-01
- Data is aggregated at a **daily** level per city, sourced from CPCB
  (Central Pollution Control Board) monitoring stations.

## Main Columns

| Column | Description |
|---|---|
| `City`, `Date` | City name and observation date |
| `PM2.5`, `PM10` | Particulate matter concentrations |
| `NO`, `NO2`, `NOx`, `NH3` | Nitrogen oxide and ammonia measurements |
| `CO`, `SO2`, `O3` | Carbon monoxide, sulfur dioxide, and ozone |
| `Benzene`, `Toluene`, `Xylene` | Volatile organic compound measurements |
| `AQI`, `AQI_Bucket` | Computed index and air-quality category |

## How to Download

### Option 1: Using `kagglehub` (used in the project notebook)

The notebook automatically downloads the dataset using `kagglehub` —
no manual download is required if you're running the notebook in Google
Colab:

```python
import kagglehub

path = kagglehub.dataset_download('rohanrao/air-quality-data-in-india')
```

### Option 2: Manual Download

1. Visit: https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india
2. Log in / create a free Kaggle account.
3. Click **Download** to get the dataset ZIP file.
4. Extract it and locate `city_day.csv`.
5. Place `city_day.csv` in this `data/` folder, or update the `DATA_PATH`
   variable in the notebook to point to its location.

Before running locally, confirm that the expected file is available:

```text
data/city_day.csv
```

### Option 3: Kaggle API

```bash
pip install kaggle
# Place your kaggle.json API credentials in ~/.kaggle/
kaggle datasets download -d rohanrao/air-quality-data-in-india
unzip air-quality-data-in-india.zip
```

## Note on Redistribution

The raw dataset file is **not included in this repository** due to its
size (~73MB) and to respect the original dataset's licensing/hosting terms
on Kaggle. Please download it directly from the source above.

The repository ignores `data/*.csv` and `data/*.zip`, so downloaded source
archives and extracted raw data stay local and are not committed.