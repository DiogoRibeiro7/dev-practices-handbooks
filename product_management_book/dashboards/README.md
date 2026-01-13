# Dashboard Prototypes

This folder contains runnable examples referenced in Chapter 16.

## Contents
- `requirement_quality_dashboard.py` (Streamlit) + `data/sample_quality_scores.csv`
- `model_acceptance_dash_app.py` (Dash)
- `roc_calibration_tool.py` (Plotly + scikit-learn) + `data/model_predictions.csv`
- `ab_test_monitor.py` (Streamlit + SciPy)

## Usage
1. Create a virtual environment and install deps:
   ```bash
   pip install -r requirements.txt
   ```
2. Run the desired dashboard, e.g.:
   ```bash
   streamlit run requirement_quality_dashboard.py
   ```
   or
   ```bash
   python roc_calibration_tool.py
   ```
3. Generated images/HTML reports are written to the project root (prefixed with `sample_`).

