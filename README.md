<div align="center">

<img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"/>
<img src="https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/GeoPandas-139C5A?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white"/>

# 📊 Ontario Fuel Price — Data Visualization

**A two-part data visualization project analyzing Ontario's retail fuel prices from 1990–2023,**  
**spanning statistical charts, time series analysis, and geospatial mapping.**

[Assignment 1 — Statistical Charts](#-assignment-1--statistical-charts) · [Assignment 2 — Time Series & Maps](#-assignment-2--time-series--geospatial-mapping) · [Tech Stack](#-tech-stack) · [Setup](#-setup)

---

</div>

## 🗂 Dataset

| Dataset | Source | Description |
|--------|--------|-------------|
| `energy-price.csv` | [Ontario Open Data Portal](https://data.ontario.ca/dataset/fuels-price-survey-information) | Weekly retail fuel prices (cents/litre) across 14 Ontario cities, 1990–present |
| `volcano_db.csv` | [Plotly Datasets](https://github.com/plotly/datasets/blob/master/volcano_db.csv) | Global volcano database with coordinates and country |
| `earthquake.txt` | [Natural Resources Canada](https://earthquakescanada.nrcan.gc.ca/stndon/NEDB-BNDS/bulletin-en.php) | Canadian earthquake records with magnitude and coordinates |

> The fuel price dataset covers **Regular Unleaded Gasoline, diesel, auto propane**, and **compressed natural gas** across cities including Ottawa, Toronto, Windsor, Sudbury, Thunder Bay, and more.

---

## 📁 Assignment 1 — Statistical Charts

> Exploring the distribution and comparison of Regular Unleaded Gasoline prices across Ontario cities in 2023.

### Q1 · Bar Chart — Mean Prices by City (2023)

Compares mean gasoline prices across **14 Ontario cities**, sorted ascending by price.

```python
# Key technique: filter → aggregate → sort → plot
mean_price = df[(df['Fuel Type'] == 'Regular Unleaded Gasoline') &
                (df['Date'].str.contains('2023'))][city].mean()
sorted_cities = sorted(mean_prices_dict, key=lambda x: mean_prices_dict[x])
plt.bar(sorted_cities, [mean_prices_dict[city] for city in sorted_cities])
```

---

### Q2 · Dot Plot — Mean Prices by City (2023)

Same data as Q1, reframed as a **horizontal dot plot** — a cleaner alternative for reading exact values across many categories.

```python
plt.scatter([mean_prices_dict[city] for city in sorted_cities], sorted_cities, marker='o')
```

---

### Q3 · Histogram — Toronto West Price Distribution (2023)

Distribution of weekly prices in Toronto West/Ouest using **15 bins**, revealing price clustering patterns throughout the year.

```python
plt.hist(toronto_west_gas_2023, bins=15, edgecolor='black')
```

---

### Q4 · ECDF — Normalized Cumulative Distribution (2023)

Empirical Cumulative Distribution Function showing the **proportion of weeks** at or below each price point in Toronto West/Ouest.

```python
sorted_prices = np.sort(toronto_west_gas_2023)
ecdf = np.arange(1, len(sorted_prices) + 1) / len(sorted_prices)
plt.plot(sorted_prices, ecdf, marker='.', linestyle='none')
```

---

### Q5 · Boxplot — Price Spread Across Cities (2021–Present)

Horizontal boxplots displaying the **spread, median, and IQR** of gasoline prices across all 14 cities from 2021 onwards.

```python
plt.boxplot([filtered_data[city] for city in cities], labels=cities, vert=False, showfliers=False)
```

---

## 📁 Assignment 2 — Time Series & Geospatial Mapping

> Moving beyond static distributions into temporal trends and geographic data.

### Q1 · Time Series + Moving Average — Ottawa (2000–2023)

Plots weekly Ottawa gasoline prices from 2000–2023, smoothed with a **Simple Moving Average (window = 40 weeks)** to reveal the long-term price trend beneath short-term volatility.

```python
df_filtered['SMA_40'] = df_filtered['Ottawa'].rolling(window=40).mean()

plt.plot(df_filtered['Date'], df_filtered['Ottawa'], label='Original')
plt.plot(df_filtered['Date'], df_filtered['SMA_40'], label='SMA 40', color='red')
```

**What the trend shows:** A steady climb from ~55¢/L in 2000, a spike around 2008, a sharp drop in 2020, and a steep rise into 2022.

---

### Q2 · Geospatial Map — Canadian Volcanoes & Earthquakes

A layered map of Canada visualizing:

- 🔺 **Volcanoes** — filtered from global dataset, plotted as red triangles
- 🟠 **Earthquakes** — color-coded by magnitude (`OrRd` colormap), sourced from NRCan bulletin

```python
canada.plot(ax=ax, color='lightgrey')
gdf_volcanoes.plot(ax=ax, color='red', marker='^', label='Volcanoes')
gdf_earthquakes.plot(ax=ax, column='Magnitude', cmap='OrRd', legend=True, markersize=10)
```

---

## 🛠 Tech Stack

| Tool | Purpose |
|------|---------|
| `pandas` | Data loading, filtering, aggregation, datetime handling |
| `matplotlib` | All chart types — bar, dot, histogram, ECDF, boxplot, time series |
| `numpy` | ECDF computation, array sorting |
| `geopandas` | Geospatial data handling and Canada map rendering |
| `Jupyter Notebook` | Interactive development environment |

---

## ⚙️ Setup

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/ontario-fuel-visualization.git
cd ontario-fuel-visualization

# Install dependencies
pip install pandas matplotlib numpy geopandas

# Launch Jupyter
jupyter notebook
```

Place the datasets in a `data/` folder before running:

```
project/
├── data/
│   ├── energy-price.csv
│   ├── volcano_db.csv
│   └── earthquake.txt
├── assignment1.ipynb
├── assignment2.ipynb
└── README.md
```

---

## ⚠️ Known Warnings

| Warning | Cause | Resolution |
|---------|-------|------------|
| `SettingWithCopyWarning` | Modifying a filtered DataFrame slice | Use `.copy()` after filtering or `.loc[]` for assignment |
| `FutureWarning` (GeoPandas) | `geopandas.datasets` deprecated in v1.0 | Download shapefile directly from [Natural Earth](https://www.naturalearthdata.com/) |

---

<div align="center">

**Mobina Tooranisama** · Data Visualization · Wilfrid Laurier University

</div>
