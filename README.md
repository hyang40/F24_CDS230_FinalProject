# F24_CDS230_FinalProjec
# What Drives Smartphone Prices? Feature Analysis Across Price Tiers

Analysis of 980 smartphones and 20+ hardware specs to find which feature combinations best explain price, and whether the answer changes between budget, mid-range and flagship phones.

> CDS 230 · George Mason University · Fall 2024
> Team project (6 members) · Python / pandas / scikit-learn

---

## Research Question

> Which combination of mobile phone features has the greatest impact on its price, and does that combination differ by price tier?

**Goal:** give buyers a data-based view of which specs are actually worth paying for at each price level.

## Dataset

- **Smartphones dataset** (Kaggle): 980 models, 22 columns covering processor, memory, battery, display, cameras, 5G and OS
- Filtered to the **five largest brands by global market share** (Samsung, Apple, Xiaomi, Vivo, Oppo) → **509 phones**
- Missing processor specs filled manually from Geekbench; columns with heavy missingness (`fast_charging`, `avg_rating`) dropped; one extreme price outlier removed

## Analytical Methods

**1. Data cleaning & feature engineering**
- Missing-value audit, manual backfilling, outlier removal
- Engineered `screen_resolution` (pixel count) and `ppi` (pixel density) from raw resolution and screen size

**2. Price-tier segmentation**
- Split phones into **Low / Mid / High** tiers at the 33rd and 67th price percentiles
- Ran categorical, continuous and binary feature analysis separately for each tier

**3. Exploratory data analysis**
- Correlation heatmap across all numeric features
- Histograms, box plots, KDE plots and binned heatmaps by tier and brand

**4. Modeling** (scikit-learn, 70/30 train/test split)
- Simple linear regression for single features
- Multiple linear regression on feature pairs, overall and within each tier
- Evaluated with R², adjusted R² and RMSE on held-out test data

## Key Results

**Overall drivers of price** (correlation with price, all 509 phones)

| Feature | r |
|---|---|
| Processor speed | 0.75 |
| Internal memory | 0.70 |
| Expandable memory slot | −0.64 |
| Screen resolution | 0.62 |

**Best overall model:** processor speed + internal memory → **R² = 0.70** (adj. 0.70) on the test set, up from 0.53 with processor speed alone.

**What matters changes by tier**

| Tier | Strongest correlates | Best 2-feature model (test R²) |
|---|---|---|
| Low | Internal memory (0.63), rear camera (0.60), RAM (0.58) | Memory + rear camera: **0.58** |
| Mid | Front camera (0.43), processor speed (0.41) | Processor + front camera: **0.39** |
| High | Internal memory (0.60), resolution (0.53), processor speed (0.50) | Memory + processor: **0.50** |

- **Budget phones** compete on storage and camera megapixels.
- **Mid-range** prices are the hardest to explain from specs alone; no single feature dominates.
- **Flagships** are priced on storage and processor performance.
- **5G** goes from 12% of budget phones to 89% of flagships, while **expandable storage** flips the other way (96% → 19%), as premium phones drop the SD card slot.

## Limitations

- **Single random split.** Most R² values come from one train/test split without a fixed seed, so exact numbers can shift between runs; cross-validation would make them more reliable.
- **Range restriction within tiers.** Splitting by price narrows the price range in each group, which naturally lowers within-tier R².
- **Brand not modeled.** Brand premium (especially Apple) likely explains much of the remaining variance but was not encoded as a feature.
- **Correlated features.** Processor speed, memory and resolution move together, so individual coefficients should not be read as independent effects.
- **Association, not causation.** The models describe how specs and price move together in the market, not what sets prices.

## Repository Contents

| File | Description |
|---|---|
| `Group1_Final.ipynb` | Full analysis notebook |
| `Group1_Final.pdf` | Exported notebook with all outputs and figures |
| `smartphones.csv` | Original dataset |
| `smartphones_updated.csv`, `filtered_data_updated.csv` | Cleaned and manually backfilled data |

## My Role

- **Data preparation:** loaded and audited the dataset, backfilled missing specs, filtered brands and removed outliers
- **Price-tier framework:** designed the Low/Mid/High percentile segmentation that structured the rest of the analysis
- **Tier-level feature analysis:** built the categorical, continuous and binary analyses for each tier and their visualizations
- **Multiple linear regression:** built and evaluated the overall and tier-specific multi-feature models

## Tech Stack

Python · pandas · NumPy · scikit-learn · matplotlib · seaborn · Plotly · Jupyter
