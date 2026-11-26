# SW-5 Satellite Collision Risk Predictor
### **Team DevX | Star Forge**
> **"Know when to act, and when to wait."**

---

## 🌟 Executive Summary
Orbital congestion is one of the most critical challenges facing modern space operations. Satellite operators continuously receive Conjunction Data Messages (CDMs) from tracking networks and must decide days in advance whether to execute costly collision avoidance manoeuvres. Because the vast majority of conjunction warnings are false alarms, unnecessary burns waste precious satellite fuel and shorten operational lifespans. 

**SW-5** is an intelligent, cost-aware decision support framework engineered to solve this dilemma. By leveraging advanced trajectory feature engineering and a tuned LightGBM core, SW-5 maximizes operator confidence—drastically reducing false alarms while guaranteeing zero missed high-risk encounters.

---

## 🎯 The Core Problem & Operational Challenge
* **The CDM Flood:** Operators receive streams of Conjunction Data Messages containing predicted miss distances, relative velocities, and covariance matrices as a target object approaches closest approach (TCA).
* **The Cost of Uncertainty:** Acting on every single alert leads to chronic fuel depletion. Failing to act on a true conjunction leads to catastrophic orbital collision. 
* **The Operational Window:** Decisions must be locked in days before TCA. SW-5 restricts its analysis strictly to data available **2+ days out**, eliminating data leakage and ensuring real-world operational feasibility.

---

## 🛠️ Architecture & Methodology
SW-5 implements a rigorous end-to-end data pipeline tailored for highly imbalanced space safety telemetry:

1. **Strict Temporal Splitting:** To mirror real-world deployment, data is partitioned by event chronological order rather than random shuffling, preventing future leakage into training sets.
2. **Feature Engineering from History:** Instead of treating CDMs as isolated static points, our pipeline aggregates complete message histories per event ID, extracting:
   * *Latest reported collision risk* and log-scale risk floors.
   * *Trajectory trends* (evaluating whether risk is rising or falling over time).
   * *Miss distance shifts* and covariance uncertainty bounds.
   * *Total CDM volume* and tracking frequency.
3. **Cost-Aware Decision Engine:** Classifies incoming conjunction events into three actionable operational states based on optimized thresholds:
   * 🔴 **Manoeuvre:** Execute an orbital avoidance burn immediately when risk crosses critical safety bounds ($\ge 0.04$ model threshold).
   * 🟡 **Defer:** Hold position and wait for the next incoming CDM update if risk is borderline.
   * 🟢 **No Manoeuvre:** Stand down safely when tracking data confirms low hazard probability.

---

## 📊 Performance Results (Test Split Evaluation)

Rigorously benchmarked against the official ESA Kelvins collision avoidance challenge dataset (evaluating across 1,792 unique test event IDs):

| Evaluation Model / Approach | Missed Dangerous Events | False Alarms | PR-AUC Score |
| :--- | :---: | :---: | :---: |
| **Baseline** (Latest risk $\ge 10^{-6}$) | 3 | 56 | 0.392 |
| **Baseline** (Latest risk $\ge 10^{-4}$) | 52 | 5 | 0.392 |
| **SW-5 Model** (Optimized Threshold $0.04$) | **3** | **55** | **0.816** |

> *Note: Our model matches the safety standard of the most aggressive baseline by keeping missed dangerous events down to just **3**, while more than doubling the Precision-Recall Area Under Curve (**PR-AUC to 0.816**).*

---

## 🌐 Live Demos & Links
* **Primary Dashboard (Streamlit — Trained LightGBM Model):** [Launch Streamlit App](https://vt42vb6nfjn763zzrzkdhc.streamlit.app/) *(Evaluates the 1,792-event test split with trained model predictions)*
* **Interactive CDM Explorer (Netlify — Rule-Based Preview):** [Launch Netlify Preview](https://public-link-b658e9.netlify.app) *(Interactive threshold simulator across the full 2,167-event ESA CDM stream)*
* **Video Walkthrough & Pitch:** [Watch Demo on YouTube](https://youtu.be/DuBK6UE2I0U?si=QHIbYXgS-O9bwux-)

---

## ⚙️ Local Installation & Quickstart

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/samanvi-collab/SW-5.git](https://github.com/samanvi-collab/SW-5.git)
   cd SW-5
