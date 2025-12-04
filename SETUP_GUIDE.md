# Setup Guide - Delivery Logistics Anomaly Detection

## ✅ Solution for pandas Installation Error

**Problem**: `metadata-generation-failed` error when installing pandas on Python 3.13.2

**Root Cause**: Python 3.13 is very new and pandas 2.2.2 doesn't have prebuilt wheels for it.

**Solution**: Install pandas 2.2.3+ which has Python 3.13 wheels.

## Installation Steps

### 1. Upgrade pip and install pandas (already done ✅)

```powershell
python -m pip install --upgrade pip setuptools wheel
python -m pip install --only-binary :all: pandas>=2.2.3
```

### 2. Install remaining packages

```powershell
# Install packages one by one to avoid network issues
python -m pip install pyspark
python -m pip install matplotlib
python -m pip install plotly
python -m pip install pyarrow
python -m pip install pyyaml
python -m pip install python-dotenv
```

Or install all at once (if network is stable):

```powershell
python -m pip install -r requirements.txt
```

### 3. Verify installation

```powershell
python -c "import pandas; import pyspark; print('✅ All packages installed successfully')"
```

## Alternative: Use Python 3.11 (Recommended for Production)

If you continue having issues with Python 3.13, **Python 3.11 is recommended** for Spark/data science workloads:

1. **Download Python 3.11** from [python.org](https://www.python.org/downloads/)
2. **Create new virtual environment**:
   ```powershell
   py -3.11 -m venv .venv311
   .\.venv311\Scripts\Activate.ps1
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   ```

## Current Status

- ✅ **pandas 2.2.3** installed successfully (has Python 3.13 wheel)
- ⚠️ **PySpark** - May need network retry or use Python 3.11
- ✅ **matplotlib, plotly** - Already installed
- ⚠️ **pyarrow** - Network issue encountered, retry needed

## Next Steps

1. Retry installing remaining packages when network is stable
2. Or switch to Python 3.11 for better compatibility
3. Run the notebook: `jupyter notebook delivery_logistics_anomaly_detection.ipynb`

