# Aegis: Cost-Aware Real-Time Fraud Detection System

End-to-end fraud detection pipeline built using PySpark and Databricks. This project simulates a production-grade system by integrating streaming ingestion, machine learning, decision logic, drift monitoring, and cost-based optimization.

---

## Overview

Aegis is designed as a **systems-level fraud detection pipeline**, not just a classification model. It processes transactions continuously, predicts fraud risk, converts predictions into business actions, and monitors reliability under data drift.

---

## Dataset

- Source: PaySim (synthetic financial transactions)  
- Raw rows: 50,500  
- Cleaned rows: 49,962  
- Fraud cases: 69  
- Fraud rate: **0.138%**

---

## Pipeline Structure

01_data_cleaning → 02_streaming_pipeline → 03_fraud_model →
04_decision_layer → 05_drift_detection → 06_threshold_optimization


---

## Notebooks and Expected Outputs

### 1. Data Cleaning (`01_data_cleaning.ipynb`)

- Initial row count: 50,500  
- Initial fraud count: 70  
- Corrupted `isFraud` rows: 33  
- Unconvertible `amount` rows: 505  

**Final results:**
- Final row count: 49,962  
- Final fraud count: 69  
- Fraud rate: 0.138%  
- Null check: all zeros across all 11 columns  

---

### 2. Streaming Pipeline (`02_streaming_pipeline.ipynb`)

- Streaming enabled: True  
- Total rows written: 49,962  
- Output columns: 13 (11 original + 2 engineered)  
- Pipeline runs without errors  

**Engineered features:**
- `is_high_risk_type`  
- `balance_diff`  

---

### 3. Fraud Model (`03_fraud_model.ipynb`)

- Total rows: 49,962  
- Fraud cases: 69  

**Train/Test split:**
- Train rows: 39,922 (56 fraud)  
- Test rows: 10,040 (13 fraud)  

**Class weights:**
- Fraud: 356.4464  
- Non-fraud: 0.5007  

**Performance:**
- Precision: 0.9998  
- Recall: 0.9907  
- F1 Score: 0.9942  
- AUC: 0.993  

- Model save: successful  

---

### 4. Decision Layer (`04_decision_layer.ipynb`)

**Model configuration:**
- numTrees = 100  
- numClasses = 2  
- numFeatures = 7  

**Decision distribution:**
- APPROVE: 48,401  
- FLAG: 1,389  
- BLOCK: 172  

**Crosstab results:**
- Legit × APPROVE: 48,399  
- Legit × FLAG: 1,378  
- Legit × BLOCK: 116  
- Fraud × APPROVE: 2  
- Fraud × FLAG: 11  
- Fraud × BLOCK: 56  

- Heatmap renders successfully  

---

### 5. Drift Detection (`05_drift_detection.ipynb`)

- Reference window: 40,685 rows  
- Current window: 9,277 rows  
- Total: 49,962  

**Drift metrics:**
- PSI (amount): 0.0034 → Stable  
- All 7 features: no drift detected  

**Validation (injected drift):**
- PSI: 0.0034 → 0.6403  
- KS: 0.0136 → 0.3039  

- Visualization renders successfully  

---

### 6. Threshold Optimization (`06_threshold_optimization.ipynb`)

- Cost-optimal threshold: t = 0.20 → $40,140  
- Phase 4 threshold: t = 0.70 → $19,501,740  
- Cost gap: $19,461,600  

- Cost curve renders successfully  

---

## Key Results

- F1 Score: **0.9942**  
- AUC: **0.993**  
- Fraud detection rate: **97.1%**  
- False-positive block rate: **0.23%**  
- Drift detection validated via controlled injection  

---

## How to Run

```bash
git clone https://github.com/YOUR_USERNAME/aegis-fraud-detection.git
cd aegis-fraud-detection