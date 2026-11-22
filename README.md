# SW-5 Satellite Collision Risk Predictor | Team DevX | STAR FORGE

Know when to act, and when to wait.

## Problem
Satellite operators receive conjunction data messages (CDMs) and must decide days
ahead whether to manoeuvre. Most warnings are harmless, so dangerous events are rare.

## Approach
- Use only CDMs received 2+ days before closest approach (no leakage).
- Features from each event's message history: latest risk, trend, miss distance, uncertainty, CDM count.
- Baseline (latest reported risk) vs LightGBM, event-ordered train/val/test split.
- Cost-aware decision: Manoeuvre, Defer or No manoeuvre.
- Dangerous = final risk of 1e-6 or more (ESA challenge definition).

## Results (test events)
PASTE YOUR RESULTS TABLE HERE

## Live demo
YOUR LIVE LINK HERE (real ESA test-set CDM streams; rule-based preview, not the trained model)

## Video
YOUR VIDEO LINK HERE

## How to run
pip install pandas numpy scikit-learn lightgbm streamlit
python sw5_baseline_and_model.py train_data.csv
streamlit run app.py

## Data
ESA Kelvins collision avoidance challenge dataset (kelvins.esa.int). Not included in this repo.

## Limits
- The public data has no timestamps, so the split is by event order.
- Results come from a small number of dangerous events in the test split.

## Team
Yogan Subramanya Naik (leader), Lakshmi Sagar G, Nishkala K, Pranitha U P
