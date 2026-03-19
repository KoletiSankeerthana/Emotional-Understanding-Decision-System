

````
# 📘 Emotional Understanding & Decision System

## Project Overview

This project builds an end-to-end system that understands a user’s emotional state from journal input and provides actionable recommendations. The system is designed to handle real-world challenges such as noisy text, missing data, and conflicting signals.

The pipeline combines:
- Machine learning for emotional understanding  
- Rule-based logic for decision-making  
- Uncertainty modeling for reliability  

The goal is not just prediction, but to **understand → decide → guide**.

---

## Approach

The system consists of three main layers:

### 1. Emotional Understanding Layer
- Predicts **emotional state** using a classification model  
- Predicts **intensity (1–5)** using a regression model  

### 2. Decision Layer
- Determines **what action the user should take** (e.g., breathing, journaling, deep work)  
- Determines **when to take the action** (e.g., now, within_15_min, later_today)  

This layer uses predicted outputs along with stress, energy, and time of day.

### 3. Uncertainty Layer
- Computes a **confidence score (0–1)**  
- Flags uncertain predictions based on:
  - low confidence  
  - short text  
  - conflicting signals  

---

## Feature Engineering

### Text Features
- TF-IDF vectorization (max 3000 features)
- Preprocessing steps:
  - lowercasing  
  - punctuation removal  
  - stopword removal  

Text features capture emotional expression and act as the **primary signal**.

---

### Metadata Features

Numerical features:
- stress_level  
- energy_level  
- sleep_hours  
- duration_min  
- reflection_quality  

Categorical features:
- ambience_type  
- time_of_day  
- previous_day_mood  
- face_emotion_hint  

Processing:
- Numerical features scaled using StandardScaler  
- Categorical features encoded using one-hot encoding  

---

### Feature Combination

Text and metadata features are combined as:

X = (text_features × 0.7) + (metadata × 0.3)

Text is weighted higher as it contains emotional meaning, while metadata provides contextual support.

---

## Model Choice

### Emotional State Prediction
- Model: XGBoost Classifier  
- Reason:
  - Handles mixed feature types effectively  
  - Robust to noisy and real-world data  
  - Performs well on tabular + text combinations  

---

### Intensity Prediction
- Model: XGBoost Regressor  
- Treated as a regression problem  

Reason:
- Intensity values (1–5) are ordinal  
- Regression captures the relative differences between levels better than classification  

Post-processing:
- Predictions are rounded and clipped to the valid range [1, 5]

---

## Uncertainty Handling

Confidence is derived from model probabilities and adjusted using:

- Short text detection (low information input)  
- Conflicting signals (e.g., high stress + high energy)  

The system sets an `uncertain_flag` when:
- confidence is low  
- input is ambiguous  
- signals are inconsistent  

This prevents overconfident or unreliable predictions.

---

## Decision Logic (What + When)

The decision engine uses:
- predicted state  
- intensity  
- stress level  
- energy level  
- time of day  

Examples:
- High stress + high intensity → box_breathing (now)  
- Low energy → rest or light_planning  
- High energy + low stress → deep_work  

Timing decisions:
- urgent → now  
- moderate → within_15_min  
- low → later_today / tonight  

---

## How to Run

### 1. Install dependencies

```bash
pip install pandas numpy scikit-learn xgboost nltk
````

---

### 2. Prepare dataset

Place the following files in the project directory:

```
train.csv
test.csv
```

---

### 3. Run the code

```bash
python main.py
```

(or run the notebook step by step)

---

### 4. Output

The final output will be saved as:

```
predictions.csv
```

---

## Output Format

The output file contains:

```
id
predicted_state
predicted_intensity
confidence
uncertain_flag
what_to_do
when_to_do
```
