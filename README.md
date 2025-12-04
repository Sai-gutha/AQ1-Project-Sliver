# Real-Time Data Quality & Anomaly Detection – Delivery Logistics

A comprehensive PySpark-based pipeline for validating Technical Data Quality (TDQ), Business Data Quality (BDQ), and performing predictive anomaly detection on delivery logistics data.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Usage](#usage)
- [Performance](#performance)
- [Troubleshooting](#troubleshooting)
- [Documentation](#documentation)

## 🎯 Overview

This notebook implements a **real-time data gatekeeper** pattern that can be extended to monitor 100+ tables. It provides:

- **Technical Data Quality (TDQ)** checks: Schema compliance, completeness (null rates), uniqueness, and timeliness
- **Business Data Quality (BDQ)** checks: Range validations, logical consistency rules, and cross-entity consistency
- **Predictive Anomaly Detection**: Unsupervised machine learning to identify anomalous delivery patterns
- **Time Series Monitoring**: Data drift detection and anomaly rate tracking over time
- **Streaming Simulation**: Structured Streaming example for real-time processing

## ✨ Features

### Technical Data Quality (TDQ)
- ✅ Schema compliance validation
- ✅ Null rate analysis per column
- ✅ Uniqueness checks (primary key validation)
- ✅ Timeliness/freshness monitoring

### Business Data Quality (BDQ)
- ✅ Numeric range validations (distance, duration, cost)
- ✅ Logical consistency rules (status vs. timestamps)
- ✅ Cross-entity consistency (speed-based anomaly detection)

### Anomaly Detection
- ✅ Isolation Forest (when available) or Z-score fallback
- ✅ Feature engineering with categorical encoding
- ✅ Time-based train/test split
- ✅ Anomaly scoring and thresholding

### Monitoring & Visualization
- ✅ Executive dashboard with key metrics
- ✅ Time series drift monitoring
- ✅ Anomaly rate tracking by entity (city, route, driver)
- ✅ Comprehensive visualizations

## 📦 Requirements

### Software
- **Python**: 3.9+ (3.11 recommended for production, 3.13+ supported)
- **Java**: JDK 8 or higher (required for PySpark)
- **Apache Spark**: Included via PySpark package

### Python Packages
See `requirements.txt` for complete list:
- `pyspark>=3.5.0`
- `pandas>=2.2.3` (2.2.3+ required for Python 3.13)
- `matplotlib>=3.9.0`
- `plotly>=5.23.0`
- `pyyaml>=6.0.0`
- `python-dotenv>=1.0.0`

### Hardware Recommendations
- **RAM**: Minimum 4GB, 8GB+ recommended
- **Storage**: SSD preferred for faster Spark startup
- **CPU**: Multi-core recommended for parallel processing

## 🚀 Installation

### 1. Clone or Download the Project

```bash
# If using git
git clone <repository-url>
cd AD

# Or download and extract the project folder
```

### 2. Create Virtual Environment (Recommended)

```bash
# Windows PowerShell
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# Windows CMD
python -m venv .venv
.venv\Scripts\activate.bat

# Linux/Mac
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Verify Installation

```bash
python -c "import pyspark; import pandas; print('✅ All packages installed successfully')"
```

## 🏃 Quick Start

### 1. Prepare Your Data

Place your CSV file in the `archive/` directory:
```
archive/
  └── Delivery_Logistics.csv
```

### 2. Open the Notebook

```bash
# Using Jupyter
jupyter notebook delivery_logistics_anomaly_detection.ipynb

# Using VS Code
code delivery_logistics_anomaly_detection.ipynb
```

### 3. Run the Notebook

1. **Run Cell 1** (Setup): Installs missing packages automatically
2. **Run Cell 2** (Imports & Spark): Creates Spark session (~15-30 seconds first time)
3. **Run remaining cells** sequentially or use "Run All"

### 4. View Results

The notebook will output:
- Data quality metrics
- Anomaly detection results
- Visualizations
- Executive summary dashboard

## 📁 Project Structure

```
AD/
├── archive/
│   └── Delivery_Logistics.csv          # Input data file
├── delivery_logistics_anomaly_detection.ipynb  # Main notebook
├── requirements.txt                    # Python dependencies
├── dq_config.yaml                      # Table configuration
├── README.md                           # This file
├── SETUP_GUIDE.md                      # Detailed setup instructions
└── OPTIMIZATION_SUMMARY.md             # Performance optimizations
```

## ⚙️ Configuration

### Table Configuration (`dq_config.yaml`)

Define table-specific settings:
```yaml
tables:
  delivery_logistics:
    table_name: delivery_logistics
    path: archive/Delivery_Logistics.csv
    primary_key: ["delivery_id"]
    timestamp_col_candidates: ["event_time", "pickup_time"]
    numeric_ranges:
      distance_km: {min: 0.0, max: 1000.0}
```

### Spark Configuration

Modify Spark settings in the notebook cell:
```python
.config("spark.sql.shuffle.partitions", "4")
.config("spark.driver.memory", "3g")
.config("spark.executor.memory", "3g")
```

### Thresholds

Adjust quality thresholds in the notebook:
- Null rate threshold: 20%
- Freshness lag: 1 hour
- Anomaly rate: Top 5%
- Speed threshold: 150 km/h

## 📊 Usage

### Running the Complete Pipeline

1. **Data Loading**: Loads and caches the dataset
2. **TDQ Checks**: Validates technical data quality
3. **BDQ Checks**: Validates business logic
4. **Feature Engineering**: Prepares features for ML
5. **Anomaly Detection**: Trains model and scores data
6. **Monitoring**: Tracks drift over time
7. **Dashboard**: Generates executive summary

### Key Outputs

- **TDQ Summary**: Null rates, schema compliance, uniqueness metrics
- **BDQ Summary**: Rule violations, range checks, consistency metrics
- **Anomaly Scores**: Per-record anomaly scores and flags
- **Time Series**: Anomaly rate trends over time
- **Executive Dashboard**: High-level metrics and visualizations

### Extending to Multiple Tables

The notebook includes a configuration structure (`table_configs`) that allows parameterizing the pipeline for multiple tables. See the "Configuration and Extension to 100+ Tables" section.

## ⚡ Performance

### Optimizations Implemented

- **Single-pass null counting**: ~10-100x faster than per-column filters
- **Reduced shuffle partitions**: Optimized for smaller datasets
- **Strategic caching**: Caches DataFrames after first use
- **Adaptive Query Execution**: Spark automatically optimizes queries
- **Arrow optimization**: Faster Pandas/Spark interop

### Expected Execution Times

| Dataset Size | Estimated Time |
|-------------|----------------|
| < 100K rows | 1-2 minutes |
| 100K - 1M rows | 2-5 minutes |
| > 1M rows | 5-15 minutes |

### First Run vs. Subsequent Runs

- **First run**: 15-30 seconds for Spark session creation
- **Subsequent runs**: 1-5 seconds (reuses existing session)

## 🔧 Troubleshooting

### Common Issues

#### 1. ModuleNotFoundError: No module named 'pyspark'
**Solution**: Run Cell 1 (setup cell) which automatically installs missing packages.

#### 2. Spark session creation is slow
**Solution**: This is normal on first run. Subsequent runs are faster. Ensure Java is installed.

#### 3. pandas installation error (metadata-generation-failed)
**Solution**: 
- Use Python 3.11 instead of 3.13, OR
- Install pandas 2.2.3+: `pip install pandas>=2.2.3`

#### 4. Out of memory errors
**Solution**: Reduce Spark memory settings in the configuration:
```python
.config("spark.driver.memory", "2g")  # Reduce from 3g
.config("spark.executor.memory", "2g")
```

#### 5. CSV file not found
**Solution**: Ensure `archive/Delivery_Logistics.csv` exists, or update the path in the notebook.

### Getting Help

1. Check `SETUP_GUIDE.md` for detailed setup instructions
2. Review `OPTIMIZATION_SUMMARY.md` for performance tips
3. Check Spark logs for detailed error messages

## 📚 Documentation

- **SETUP_GUIDE.md**: Detailed installation and setup instructions
- **OPTIMIZATION_SUMMARY.md**: Performance optimization details
- **dq_config.yaml**: Table configuration examples

## 🎓 Key Concepts

### Technical Data Quality (TDQ)
Measures the technical correctness of data:
- **Schema**: Do columns match expected types?
- **Completeness**: How many missing values?
- **Uniqueness**: Are keys unique?
- **Timeliness**: How fresh is the data?

### Business Data Quality (BDQ)
Measures business logic compliance:
- **Ranges**: Are values within expected bounds?
- **Rules**: Do relationships between fields make sense?
- **Consistency**: Are derived metrics realistic?

### Anomaly Detection
Identifies unusual patterns:
- **Unsupervised**: No labeled data required
- **Isolation Forest**: Preferred method (if available)
- **Z-score**: Fallback method
- **Thresholding**: Top percentile flagged as anomalies

## 🔄 Extending the Pipeline

### Adding New Tables

1. Add entry to `dq_config.yaml`
2. Define primary keys, timestamps, and rules
3. Use the `run_tdq_bdq_anomaly_for_table()` function pattern

### Adding New BDQ Rules

1. Define rule logic in the BDQ section
2. Add to `bdq_rules` in configuration
3. Include in violation summary

### Customizing Anomaly Detection

1. Modify feature engineering section
2. Adjust contamination/threshold parameters
3. Change model type (Isolation Forest vs. Z-score)

---

**Last Updated**: December 2025

**Version**: 1.0

For questions or issues, please refer to the troubleshooting section or open an issue in the repository.

