# ☕ Coffee Shop Sales & Weather Impact Analysis

## 📌 Project Overview
End-to-end data analysis project exploring whether weather conditions 
influence coffee shop sales in New York City. 
The project combines historical sales data with real weather data 
retrieved via API, covering the full data pipeline: 
Python → SQL Server → Power BI.

---

## 🎯 Business Question
**Does weather (temperature, rainfall) affect daily coffee shop sales?**

---

## 📊 Data Sources
| Source | Description |
|--------|-------------|
| [Maven Roasters Dataset (Kaggle)](https://www.kaggle.com/datasets/agungpambudi/trends-product-coffee-shop-sales-revenue-dataset) | ~149K transactions, 3 NYC stores, Jan–Jun 2023 |
| [Open-Meteo Historical Weather API](https://open-meteo.com/) | Daily weather data for NYC (same period) |

---

## 🔧 Tech Stack
- **Python** (Pandas, Scikit-learn, Requests, SQLAlchemy)
- **SQL Server / SSMS**
- **Power BI** (DAX)
- **Google Colab**

---

## 🔄 Project Pipeline

### Phase 1 — Data Cleaning & Preparation (Python)
- Loaded raw CSV (149,116 rows, 11 columns)
- Verified: 0 missing values, 0 duplicates
- Converted `transaction_date` to datetime format
- Created `total_sales` column (`transaction_qty × unit_price`)
- Aggregated daily sales → 181 rows (one per day)

### Phase 2 — Weather API Integration (Python)
- Called Open-Meteo Historical Weather API for NYC
  (lat: 40.71, long: -74.00, Jan 01 → Jun 30 2023)
- Retrieved daily parameters:
  `avg_temperature`, `max_temperature`, 
  `min_temperature`, `total_precipitation`, `max_wind_speed`
- Merged sales + weather data by date → final dataset: 181 rows × 7 columns

### Phase 3 — SQL Server Analysis
- Imported merged dataset into SQL Server (`coffee_weather_db`)
- Key queries:
  - Rain vs No Rain average sales (`CASE WHEN` + `GROUP BY`)
  - Result: **$3,855 (Rain) vs $3,866 (No Rain)** → minimal difference

### Phase 4 — Machine Learning (Python)
- Built a **Multiple Linear Regression** model (scikit-learn)
- Features: `days_elapsed` + `avg_temperature`
- Target: `total_sales`
- Results:
  - **R² = 0.78** → model explains 78% of daily sales variance
  - **RMSE ≈ $496** → average prediction error
  - Temperature coefficient: **+$34 per additional degree**

### Phase 5 — Power BI Dashboard
- KPIs: Total Sales ($698,812), Avg Temperature (10.33°C), Rain Impact
- Visuals:
  - Monthly sales trend (increasing trend Jan → Jun)
  - Sales vs Temperature combo chart
  - Rain vs No Rain comparison
- Interactive month slicer
- DAX measures: `Ventes_Moyennes_Pluie`, 
  `Ventes_Moyennes_SansPluie`, `Impact_Pluie`

---

## 📈 Key Findings
- ✅ **Temperature has a real impact on sales**: 
  +$34 per additional degree (controlled for time trend)
- ✅ **Rainfall has minimal impact**: 
  only 0.28% difference between rainy and dry days
- ✅ **Weather + time explain 78%** of daily sales variations (R²=0.78)
- ✅ **Sales grew consistently** from ~$2,500/day (January) 
  to ~$5,500/day (June)

---

## ⚠️ Important Note
The strong correlation between temperature and sales (r=0.83) 
is partially explained by the time trend 
(both sales and temperature increase from winter to summer). 
The multiple regression model isolates the **real temperature effect** 
by controlling for time, confirming that temperature 
has a genuine independent impact on sales.

---

## 📁 Repository Structure
