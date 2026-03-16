# 🌍 Ecohydrological Nexus Modeling: Machine Learning & SHAP Analysis

## 📌 Context

Understanding the drivers behind ecosystem services (ES) is critical for sustainable landscape management. This project employs a **Machine Learning (ML)** approach coupled with **SHAP (SHapley Additive exPlanations)** to decipher the complex interactions within the Soil-Water-Vegetation nexus.

The study focuses on the peri-urban area of **Parakou (Benin)**, using **sub-catchments** as the primary functional units. Unlike arbitrary grids, sub-catchments reveal true ecohydrological processes by integrating upstream-downstream connectivity.

## 🛠 Materials and Methods

* **Ecosystem Services Quantification**:
* *Carbon Storage* & *Annual Water Yield* (InVEST 3.14 models).
* Data normalized by area (tC/ha and m³/ha) to ensure comparability across different sub-catchment sizes.

* **Feature Engineering (Drivers)**:
* **Landscape Metrics**: `ed` (Edge Density) and `shdi` (Shannon Diversity Index) via the `landscapemetrics` R package.
* **Land Cover**: `forest_cover` (natural vegetation) and `anthropic_press` (croplands + built-up areas).
* **Remote Sensing**: `ndwi` (Water Index) and `lst` (Land Surface Temp) via Google Earth Engine.

* **ML Pipeline**:
* Benchmarking of 6 models: Linear Regression, Ridge, Lasso, Random Forest, AdaBoost, and **XGBoost**.
* **Validation**: Spatial considerations were integrated to mitigate autocorrelation.


* **Explainable AI (XAI)**: Interpretation of non-linear dynamics using **SHAP values**.

## 📊 Results & Key Insights

### 1. Carbon Storage Modeling

* **Descriptive Statistics**: Mean storage of **2305.94 ± 429.69 tC/ha**.
* **Performance**: **XGBoost** outperformed all models ($R^2 = 0.47$), showing a 50% improvement over linear models ($R^2 \approx 0.31$).

| Model | RMSE (tC/ha) | $R^2$ |
| --- | --- | --- |
| **XGBoost** | **285.62** | **0.47** |
| Random Forest | 307.01 | 0.39 |
| Linear Regression | 325.54 | 0.31 |

* **Top Driver**: `forest_cover` (SHAP: +192.65). Carbon storage is primarily a biological function of biomass density.

### 2. Annual Runoff Modeling

* **Descriptive Statistics**: Mean runoff of **168.45 ± 73.43 m³/ha**.
* **Performance**: A radical shift toward non-linearity. Linear models failed completely ($R^2 < 0$), while **XGBoost** maintained a robust **$R^2 = 0.57$**.

| Model | RMSE (m³/ha) | $R^2$ |
| --- | --- | --- |
| **XGBoost** | **19.17** | **0.57** |
| AdaBoost | 20.29 | 0.52 |
| Linear Regression | 36.10 | **-0.50** |

* **Top Driver**: `anthropic_press` (SHAP: +90.46). Urbanization and agriculture are the dominant engines of water runoff, overriding natural precipitation patterns in this peri-urban context.

## 💡 Discussion & Conclusions

1. **The Death of Linearity**: The failure of linear regressions (especially for Runoff) proves that ecohydrological services in fragmented landscapes cannot be modeled with simple equations. Tree-based models (XGBoost) are essential to capture "threshold effects".
2. **Anthropogenic Dominance**: While Carbon is still driven by "Nature" (`forest_cover`), Runoff is now driven by "Humanity" (`anthropic_press`). This decoupling is a major indicator of ecosystem transition in Parakou.
3. **Methodological Rigor**: Choosing the **sub-catchment** as a unit ensures that the results are hydrologically grounded, even if it increases the complexity of the modeling compared to a standard grid.
