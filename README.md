# 📡 MCS Prediction using Machine Learning & Deep Learning

## 📌 Problem Statement

Modulation and Coding Scheme (MCS) determines the optimal modulation format and coding rate for wireless communication based on channel conditions.

The goal of this project is to build models that predict the **MCS level (0–5)** using features such as signal strength, SNR, attenuation, distance, and FFT-based features.

---

## 📊 Dataset Overview

* **Samples:** 5000
* **Features:** 14
* **Target:** `mcs_level` (6 classes)

### Features include:

* Signal metrics → `signal_strength`, `snr`, `attenuation`
* Spatial metric → `distance_to_tower`
* Frequency domain → `fft_0` to `fft_9`

### Observations:

* No missing values
* Balanced class distribution
* FFT features show similar distributions → likely redundant
* Feature statistics indicate high overlap across classes

---

## ⚙️ Data Preprocessing

* Train-test split: 80–20 (stratified)
* Feature scaling (StandardScaler for DL/SVM)
* Basic exploratory data analysis (EDA)
* Correlation analysis

---

## 🧠 Feature Engineering

Domain-inspired features were created:

* `snr_bin` → discretized SNR levels
* `snr_log` → non-linear transformation
* `path_loss_proxy` → approximated path loss
* `snr_over_dist` → signal degradation with distance
* `signal_snr_ratio` → signal-to-noise interaction

👉 Total features increased from **14 → 20**

### Outcome:

Despite incorporating domain knowledge, **no significant performance improvement** was observed, indicating weak feature-target relationships.

---

## 🤖 Machine Learning Approach

### Models Used:

* Random Forest
* XGBoost
* LightGBM

### Justification:

* Suitable for tabular data
* Capture non-linear relationships
* Handle feature interactions well

---

## 📈 ML Results

| Model         | Accuracy | F1 Score | CV Score |
| ------------- | -------- | -------- | -------- |
| Random Forest | 0.1610   | 0.1608   | 0.1945   |
| XGBoost       | 0.1630   | 0.1631   | 0.1955   |
| LightGBM      | 0.1600   | 0.1591   | 0.2008   |

### Observations:

* All models perform close to **random baseline (~16%)**
* No model shows significant improvement
* Feature engineering does not improve performance

---

## 🧠 Deep Learning Approach

### Model: Residual MLP

* Input: 14 features
* Hidden size: 256
* 4 Residual blocks
* Activation: GELU
* Dropout: 0.3
* Parameters: ~536K

### Why Residual MLP?

* Enables deeper architectures
* Stabilizes training
* Captures complex non-linear relationships

---

## ⚙️ Training Setup

* Optimizer: Adam
* Learning rate: 0.001
* Batch size: 256
* Loss: CrossEntropy
* Early Stopping applied (patience = 10)

---

## 📊 DL Results

* **Test Accuracy:** 0.177
* **Weighted F1:** 0.176

### Observations:

* Training accuracy → **~95% (memorization)**
* Validation/Test → **~16–18%**
* Severe overfitting observed
* Early stopping slightly improves generalization

---

## 🔍 Statistical & Integrity Analysis

To validate whether meaningful patterns exist in the dataset, multiple tests were performed:

### 1. Correlation Test

* Spearman correlation ≈ 0
* No monotonic relationship between features and target

### 2. Shuffle Test

* Real accuracy ≈ 0.174
* Shuffled accuracy ≈ 0.170

👉 Model performs similarly even with random labels

---

### 3. Chi-Square Test

* High p-values across features
* Indicates statistical independence

---

### 4. Additional Checks

* Feature distributions overlap heavily across classes
* No identifiable structure linking inputs to MCS
* Target independent of row index

---

## 📊 Visualization Insights

Key plots used:

* Class distribution
* Feature correlation heatmap
* SNR vs MCS distribution
* Model comparison plots
* Confusion matrix

### Key Findings:

* Heavy overlap between feature distributions
* No separable clusters for different MCS levels
* Confusion matrix shows uniform misclassification

---

## ⚠️ Final Conclusion

> The dataset does not contain sufficient predictive signal to determine MCS levels.

### Key Evidence:

* ML models perform at random baseline (~16%)
* Deep Learning model overfits but fails to generalize
* Statistical tests confirm feature-target independence

---

## 🎯 Core Insight

> Model performance is fundamentally limited by the information content in the dataset, not by the choice of algorithm.

---

## 🚀 Future Work

* Use real-world datasets with stronger physical relationships
* Include CQI, SINR mapping, or channel state indicators
* Reformulate problem as regression or ordinal classification
* Explore physics-informed ML approaches

---

## 📁 Repository Structure

* `ml_notebook.ipynb` → Traditional ML pipeline
* `dl_notebook.ipynb` → Deep Learning implementation
* `README.md` → Project report

---

## 👨‍💻 Author

Saket Jasuja
