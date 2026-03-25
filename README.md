# 📡 Real-Time-Wireless-MCS-Prediction

## 📌 Problem Statement

Modulation and Coding Scheme (MCS) determines the optimal modulation and coding rate for wireless communication based on channel conditions.

The objective of this project is to build a machine learning model that predicts the **best MCS level** given input features such as signal strength, SNR, attenuation, distance, and FFT-based features.

---

## 📊 Dataset Overview

* **Total samples:** 5000
* **Features (14):**

  * Signal metrics: `signal_strength`, `snr`, `attenuation`
  * Spatial metric: `distance_to_tower`
  * Frequency-domain features: `fft_0` → `fft_9`
* **Target:** `mcs_level` (6 classes: 0–5)

### Key Observations

* No missing values in dataset
* Features are continuous and well-distributed
* Class distribution is approximately balanced
* FFT features show very similar statistical distributions (potential redundancy)

---

## ⚙️ Data Preprocessing

The following preprocessing steps were applied:

* Train-test split (80-20) with stratification
* Feature scaling using `StandardScaler` (for models like SVM)
* Feature engineering (domain-inspired transformations)

---

## 🧠 Feature Engineering

Additional features were created to better capture wireless channel behavior:

* `snr_bin` → discretized SNR levels
* `snr_log` → non-linear transformation of SNR
* `path_loss_proxy` → approximated path loss
* `snr_over_dist` → signal degradation with distance
* `signal_snr_ratio` → relationship between signal strength and noise

👉 Total features increased from **14 → 20**

### Observation

Despite adding domain-inspired features, **no significant improvement in model performance was observed**, indicating weak correlation between features and target.

---

## 🤖 Model Selection

The problem was treated as a **multi-class classification task**.

The following models were used:

* Random Forest
* XGBoost
* LightGBM

### Justification

* Tree-based models handle:

  * Non-linear relationships
  * Feature interactions
  * Mixed feature scales
* Boosting models (XGBoost, LightGBM) are generally strong for structured/tabular data

---

## 📈 Results

| Model         | Accuracy | F1 Score | CV Score |
| ------------- | -------- | -------- | -------- |
| Random Forest | 0.1610   | 0.1608   | 0.1945   |
| XGBoost       | 0.1630   | 0.1631   | 0.1955   |
| LightGBM      | 0.1600   | 0.1591   | 0.2008   |

### Key Insight

* All models perform close to **random baseline (~16%)**
* No model significantly outperforms others

---

## 📊 Visualization & Analysis

The following plots were used:

* Class distribution
* Feature correlation heatmap
* SNR vs MCS distribution
* Model comparison (Accuracy, F1, CV)
* Confusion matrix

### Observations

* Significant overlap in feature distributions across MCS classes
* Weak separability between classes
* Confusion matrix shows high misclassification across all classes

---

## 🔍 Integrity Checks & Statistical Tests

To validate whether the dataset contains meaningful signal, several tests were performed:

### 1. Correlation Test

* Spearman correlation between features and target ≈ 0
* Indicates **no monotonic relationship**

### 2. Shuffle Test

* Model trained on real vs shuffled labels:

  * Real accuracy ≈ 0.174
  * Shuffled accuracy ≈ 0.170

👉 Conclusion:
Model performs similarly even with random labels → **target is independent of features**

---

### 3. Chi-Square Test

* All features show high p-values
* Indicates **statistical independence from target**

---

### 4. Additional Checks

* Feature statistics show similar distributions
* No meaningful signal pattern detected
* Target vs index correlation ≈ 0 (no ordering bias)

---

## ⚠️ Final Conclusion

* The dataset exhibits **very weak or no relationship between input features and MCS level**
* Machine learning models are unable to learn meaningful patterns
* Performance remains at **random baseline (~16%)**

### Key Takeaway

> The limitation lies not in model choice or feature engineering, but in the **lack of predictive signal in the dataset itself**

---

## 🚀 Future Work

* Use real-world or physics-based datasets with stronger feature-target relationships
* Incorporate additional features like CQI, SINR mapping, or channel state indicators
* Explore deep learning models for potential non-linear feature extraction

---

## 📌 Summary

* Implemented full ML pipeline (preprocessing → feature engineering → modeling → evaluation)
* Applied multiple models and optimization techniques
* Conducted rigorous statistical validation
* Identified dataset limitations through systematic analysis

---

## 👨‍💻 Author

Saket Jasuja
