# DNS-Anomaly-Detection-Using-Machine-Learning
Machine learning pipeline for detecting DGA-generated malicious domains using lexical feature engineering, entropy analysis, and multi-model evaluation. Built with Python, Scikit-learn, and cybersecurity-focused ML techniques for DNS threat detection.
<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Scikit--learn-MachineLearning-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Cybersecurity-ThreatDetection-red?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">
</p>

<h1 align="center">DNS Anomaly Detection Using Machine Learning</h1>

<p align="center">
Machine Learning Pipeline for Detecting DGA-Generated Malicious Domains Using Lexical Feature Engineering and Cybersecurity-Focused ML Techniques
</p>

---

# University of Houston — MS Cybersecurity

## Team: Analytics Shield

### Team Members
- Surabhi Kandala
- Durga Sai Sri Ramireddy
- Gurunivas Mudiraj

---

# TL;DR

Built a complete machine learning pipeline capable of identifying malicious DGA-generated domains using only the domain name itself.

No packet captures.  
No DNS traffic analysis.  
No PCAP files.  

Just the domain string.

### Best Result
```text
ANN / MLP → ~90% F1 Score
```

---

# Table of Contents

- Project Overview
- The Problem
- Dataset
- Pipeline Architecture
- Exploratory Data Analysis (EDA)
- Feature Engineering
- Data Preprocessing
- PCA Analysis
- Machine Learning Models
- Model Results
- Feature Importance Analysis
- Key Findings
- Skills Demonstrated
- Tools & Technologies
- Future Improvements
- Getting Started
- Conclusion
- References

---

# Project Overview

This project focuses on detecting malicious DGA-generated domains using machine learning techniques.

Many malware families use Domain Generation Algorithms (DGAs) to generate thousands of random domain names daily for command-and-control communication. Since these domains constantly change, traditional blacklist-based detection becomes ineffective.

Instead of analyzing DNS traffic or packet captures, this project identifies malicious domains purely from the lexical structure of the domain name itself.

### Example

```text
google.com         → Human-generated
xkqzpwrtq.biz      → Possible DGA-generated
```

The objective was to determine whether machine learning models could distinguish between human-created domains and algorithmically generated malicious domains.

---

# Key Highlights

✅ Built a complete cybersecurity ML pipeline  
✅ Processed 70,000 DNS domains  
✅ Extracted 8 lexical features from raw domain strings  
✅ Trained and evaluated 4 machine learning models  
✅ Achieved ~90% F1-score using ANN/MLP  
✅ Performed EDA, PCA, feature importance analysis, and model comparison  

---

# The Problem

Modern malware frequently uses Domain Generation Algorithms (DGAs) to evade detection mechanisms.

Instead of using a single fixed command-and-control server, malware continuously generates random domains to maintain attacker communication channels.

Traditional blacklist-based approaches struggle because malicious domains constantly change.

### Core Question

```text
Can machine learning determine whether a domain was created by a human or by an algorithm?
```

### MITRE ATT&CK Mapping

```text
T1568 – Dynamic Resolution
```

---

# Dataset

The dataset combined legitimate and malicious domain names.

| Source | Label | Count |
|---|---|---|
| Alexa Top Domains | Benign | 50,000 |
| DGA Malware Domains | Malicious | 20,000 |

### Total Dataset Size

```text
70,000 domain names
```

Class imbalance was intentionally preserved to better simulate real-world DNS traffic environments.

---

# Pipeline Architecture

```text
Raw Domain Names
        │
        ▼
Data Collection
        │
        ▼
Exploratory Data Analysis
        │
        ▼
Feature Engineering
        │
        ▼
Preprocessing & Scaling
        │
        ▼
PCA Analysis
        │
        ▼
Model Training
        │
        ▼
Performance Evaluation
```

---

# Exploratory Data Analysis (EDA)

Before training models, statistical differences between benign and malicious domains were analyzed.

### Key Observations

- DGA domains were generally longer
- Malicious domains showed higher entropy
- DGA domains contained more random character distributions
- Consecutive consonants appeared frequently in malicious domains

---

## Class Distribution

<p align="center">
  <img src="01_class_distribution.png" width="700">
</p>

---

## Domain Length & Entropy Analysis

<p align="center">
  <img src="02_eda_length_entropy.png" width="900">
</p>

---

# Feature Engineering

Each domain was transformed into numerical features suitable for machine learning models.

## Extracted Features

| Feature | Description |
|---|---|
| length | Character count |
| entropy | Shannon entropy |
| consecutive_consonants | Longest consonant sequence |
| unique_chars | Distinct character count |
| vowel_ratio | Percentage of vowels |
| consonant_ratio | Percentage of consonants |
| digit_ratio | Percentage of digits |
| has_numbers | Presence of numbers |

---

## Example Transformation

```text
google.com → google
xkqzpwrt.ru → xkqzpwrt
```

### Important Observation

```text
consecutive_consonants became more important than entropy
```

Human-created domains are typically pronounceable.  
DGA-generated domains often are not.

---

# Data Preprocessing

The preprocessing pipeline included:

- Null value removal
- Variance threshold filtering
- StandardScaler normalization
- Label encoding
- Stratified train-test splitting

### Why F1-Score?

F1-score was selected as the primary evaluation metric due to dataset imbalance.

---

# PCA Analysis

Principal Component Analysis (PCA) was used to study feature variance and dimensionality behavior.

---

## PCA Variance Analysis

<p align="center">
  <img src="03_pca_variance.png" width="750">
</p>

### PCA Findings

- Most variance was captured within the first few principal components
- PCA demonstrated dimensionality reduction behavior
- Minimal compression benefit existed due to compact feature count

---

# Machine Learning Models

The following models were trained and compared.

| Model |
|---|
| Decision Tree |
| Random Forest |
| Support Vector Machine (SVM) |
| Artificial Neural Network (ANN / MLP) |

---

## Train-Test Splits

```text
80:20
67:33
50:50
95:5
```

---

# Model Results

## Best Performing Model

| Model | F1 Score |
|---|---|
| ANN / MLP | 0.8999 |

ANN achieved the highest overall performance across experiments.

---

## Confusion Matrix Comparison

<p align="center">
  <img src="04_confusion_matrices.png" width="950">
</p>

### Observation

The ANN model achieved the lowest false positive rate among all tested models.

---

# PCA vs Full Feature Comparison

<p align="center">
  <img src="05_pca_comparison.png" width="700">
</p>

### Observation

Results showed minimal difference between PCA-reduced features and the full feature set due to the compact nature of the dataset.

---

# Random Forest Feature Importance

<p align="center">
  <img src="06_feature_importance.png" width="700">
</p>

---

## Important Finding

Initially, entropy appeared to be the strongest indicator during EDA.

However, Random Forest feature importance revealed:

```text
consecutive_consonants > entropy
```

This demonstrated that unreadable consonant-heavy strings were stronger indicators of DGA-generated behavior than randomness alone.

---

# F1 Score Heatmap

<p align="center">
  <img src="07_f1_heatmap.png" width="750">
</p>

### Observation

The ANN/MLP model consistently achieved the best F1-score across multiple train-test splits.

---

# Key Findings

## 1. ANN achieved the best overall performance

The neural network achieved approximately:

```text
90% F1-score
```

across evaluation experiments.

---

## 2. Random Forest offered strong interpretability

Random Forest provided detailed feature importance analysis while maintaining excellent performance with faster training time.

---

## 3. Consecutive consonants were highly effective indicators

Domains with long unreadable consonant sequences strongly correlated with DGA-generated behavior.

### Example

```text
xkqzpwrtq.biz
```

---

## 4. Larger training splits improved consistency

The 80:20 split consistently produced the strongest results.

---

# Skills Demonstrated

- Cybersecurity-focused machine learning
- DNS anomaly detection
- DGA analysis
- Feature engineering
- Exploratory Data Analysis (EDA)
- Data preprocessing
- PCA analysis
- Model evaluation
- F1-score analysis
- Python-based ML workflows
- Security analytics

---

# Tools & Technologies

| Technology |
|---|
| Python |
| Pandas |
| NumPy |
| Scikit-learn |
| Matplotlib |
| Seaborn |
| Jupyter Notebook |

---

# Future Improvements

Potential future enhancements include:

- Real-time DNS monitoring
- Character n-gram analysis
- SMOTE imbalance handling
- Isolation Forest anomaly detection
- Detection of unseen DGA families
- SIEM integration for automated alerting

---

# Getting Started

## Requirements

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

---

## Run the Project

```bash
jupyter notebook
```

Open:

```text
dns_anomaly_detection.ipynb
```

Run all notebook cells.

---

# Conclusion

This project demonstrated how machine learning can support cybersecurity operations by identifying malicious DGA-generated domains using only lexical and statistical domain features.

Even without DNS traffic analysis or packet captures, the models successfully distinguished legitimate domains from malicious domains with strong performance.

The project combines practical machine learning workflows with real-world cybersecurity threat detection concepts relevant to SOC operations and blue team environments.

---
# Presentation

📺 Full Project Presentation & Walkthrough:

[![Watch the Presentation](https://img.shields.io/badge/YouTube-Project%20Presentation-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/watch?v=BRD8PHBkOSc)

This presentation covers:

- Project overview
- DGA threat explanation
- Dataset collection
- Feature engineering
- Exploratory Data Analysis (EDA)
- PCA analysis
- Model training & evaluation
- Feature importance analysis
- Final results & findings

---

# References

- MITRE ATT&CK – T1568 Dynamic Resolution
- Scikit-learn Documentation
- Shannon Entropy Theory
- Alexa Top Sites

---


