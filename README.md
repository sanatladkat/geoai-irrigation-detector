# 🛰️ GeoAI Crop Stress Detector: Multi-Sensor Thermal Fusion

![Python](https://img.shields.io/badge/Python-3.11-blue.svg)
![Google Earth Engine](https://img.shields.io/badge/Google%20Earth%20Engine-Python%20API-green)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-Machine%20Learning-orange)
![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)

**By the time a crop turns brown on a satellite image, the yield is already destroyed.**

Standard agricultural monitoring relies on optical "greenness" (NDVI) as a lagging indicator of health. This project establishes a **leading indicator** by engineering an automated, zero-compute GeoAI pipeline that detects sub-hectare irrigation failures weeks before physical wilting occurs.

By calculating the relative thermal "fever" of plant canopies using Unsupervised Machine Learning, this system isolates broken irrigation pumps and water stress in extreme heat environments.

### 🎥 Tactical Output
![My Image](assets\anomaly.PNG)

---

## 🧠 The Architecture & Methodology

Standard absolute temperature mapping fails in extreme environments (like the 43°C Indian summer) due to sensor washout. This pipeline bypasses that limitation using dynamic statistical normalization.

1. **Heterogeneous Data Fusion:**
   - **Optical (Sentinel-2):** Maps 10m high-resolution crop boundaries and calculates baseline photosynthetic activity (NDVI).
   - **Thermal (Landsat 9):** Extracts 100m Land Surface Temperature (LST) arrays.
   - **Spatial Masking (ESA WorldCover):** Mathematically deletes non-agricultural noise (rivers, concrete, bare soil) to prevent false positives.

2. **The "Micro-Climate" Z-Score:**
   - Instead of using absolute temperature, the pipeline calculates a relative spatial Z-Score. It compares the exact canopy temperature of a 10m pixel *only* to the statistical mean of its immediate agricultural neighbors. 

3. **Unsupervised Machine Learning:**
   - Data is extracted from Earth Engine into local memory via Pandas.
   - An **Isolation Forest** (`scikit-learn`) autonomously scans the multidimensional feature space (NDVI vs. Thermal Z-Score).
   - The AI flags statistical anomalies: fields that are lush green but baking at functionally impossible surface temperatures (e.g., 50°C+ LST), indicating an immediate cessation of plant transpiration.

---

## ⚙️ Quickstart & Installation

To avoid spatial dependency conflicts (`GDAL`, `rasterio`, `geopandas`), it is highly recommended to use `mamba` and the `conda-forge` channel.

**1. Clone the repository**
```bash
git clone https://github.com/sanatladkat/geoai-irrigation-detector.git
cd geoai-irrigation-detector
```

**2. Create the environment**
```bash
conda create -n geoai python=3.11 -y
conda activate geoai
conda install -n base mamba -c conda-forge -y
mamba install geemap earthengine-api pandas scikit-learn jupyterlab -c conda-forge -y
```

**3. Authenticate with Google Earth Engine**
You must have a registered Google Cloud Project with the Earth Engine API enabled.
```bash
earthengine authenticate
```

**4. Run the Pipeline**
Launch Jupyter Lab and open `notebooks/thermal_anomaly_detection.ipynb`.
```bash
jupyter lab
```

---

## 🛠️ Tech Stack
* **Cloud API:** Google Earth Engine (GEE)
* **Data Processing:** `pandas`, `numpy`
* **Geospatial & Viz:** `geemap`, `ipyleaflet`
* **Machine Learning:** `scikit-learn` (Isolation Forest)
