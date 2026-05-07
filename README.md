# DNS-Anomaly-Detection-Using-Machine-Learning
Machine learning pipeline for detecting DGA-generated malicious domains using lexical feature engineering, entropy analysis, and multi-model evaluation. Built with Python, Scikit-learn, and cybersecurity-focused ML techniques for DNS threat detection.
<p align="center"> <img src="https://img.shields.io/badge/Python-3.10-blue?style=for-the-badge"> <img src="https://img.shields.io/badge/Scikit--learn-MachineLearning-orange?style=for-the-badge"> <img src="https://img.shields.io/badge/Cybersecurity-ThreatDetection-red?style=for-the-badge"> <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge"> </p>
DNS Anomaly Detection Using Machine Learning
University of Houston — MS Cybersecurity
Team: Analytics Shield
Team Members
Surabhi Kandala
Durga Sai Sri Ramireddy
Gurunivas Mudiraj
Project Summary

This project focuses on detecting malicious DGA-generated domains using machine learning techniques.

Many malware families use Domain Generation Algorithms (DGAs) to generate thousands of random domain names daily for command-and-control communication. Since these domains constantly change, traditional blacklist-based detection becomes ineffective.

Instead of analyzing DNS traffic or packet captures, this project identifies malicious domains purely from the lexical structure of the domain name itself.

Example:

google.com         → Human-generated
xkqzpwrtq.biz      → Possible DGA-generated

The objective was to determine whether machine learning models could distinguish between human-created domains and algorithmically generated malicious domains.

Key Highlights

✅ Built a complete cybersecurity ML pipeline
✅ Processed 70,000 DNS domains
✅ Extracted 8 lexical features from raw domain strings
✅ Trained and evaluated 4 machine learning models
✅ Achieved ~90% F1-score using ANN/MLP
✅ Performed EDA, PCA, feature importance analysis, and model comparison

Dataset

The dataset combined legitimate and malicious domain names.

Source	Label	Count
Alexa Top Domains	Benign	50,000
DGA Malware Domains	Malicious	20,000

Total Dataset Size:

70,000 domain names

Class distribution was intentionally kept imbalanced to better simulate real-world network traffic patterns.

Pipeline Architecture
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
Exploratory Data Analysis (EDA)

Before training models, we analyzed differences between benign and malicious domains.

Key observations:

DGA domains were generally longer
Malicious domains showed higher entropy
DGA domains contained more random character distributions
Consecutive consonants appeared frequently in malicious domains
Class Distribution
<p align="center"> <img src="01_class_distribution.png" width="700"> </p>
Domain Length & Entropy Analysis
<p align="center"> <img src="02_eda_length_entropy.png" width="900"> </p>
Feature Engineering

Each domain was transformed into numerical features suitable for machine learning models.

Extracted features included:

Feature	Description
length	Character count
entropy	Shannon entropy
consecutive_consonants	Longest consonant sequence
unique_chars	Distinct character count
vowel_ratio	Percentage of vowels
consonant_ratio	Percentage of consonants
digit_ratio	Percentage of digits
has_numbers	Presence of numbers

Example transformation:

google.com → google
xkqzpwrt.ru → xkqzpwrt

One of the most interesting findings was that:

consecutive_consonants became more important than entropy

Human-created domains are typically pronounceable.
DGA-generated domains often are not.

Data Preprocessing

The preprocessing pipeline included:

Null value removal
Variance threshold filtering
Feature scaling using StandardScaler
Label encoding
Stratified train-test splitting

F1-score was selected as the primary evaluation metric due to dataset imbalance.

PCA Analysis

Principal Component Analysis (PCA) was used to study feature variance and dimensionality behavior.

PCA Variance Analysis
<p align="center"> <img src="03_pca_variance.png" width="750"> </p>

PCA showed that most variance was captured within the first few principal components.

Machine Learning Models

The following models were trained and compared:

Model
Decision Tree
Random Forest
Support Vector Machine (SVM)
Artificial Neural Network (ANN / MLP)

Different train-test split combinations were tested:

80:20
67:33
50:50
95:5
Model Results
Best Performing Model
Model	F1 Score
ANN / MLP	0.8999

ANN achieved the highest overall performance across experiments.

Confusion Matrix Comparison
<p align="center"> <img src="04_confusion_matrices.png" width="950"> </p>

The ANN model achieved the lowest false positive rate among all tested models.

PCA vs Full Feature Comparison
<p align="center"> <img src="05_pca_comparison.png" width="700"> </p>

Results showed minimal difference between PCA-reduced features and the full feature set due to the compact nature of the dataset.

Random Forest Feature Importance
<p align="center"> <img src="06_feature_importance.png" width="700"> </p>
Important Observation

Initially, entropy appeared to be the strongest indicator during EDA.

However, Random Forest feature importance revealed:

consecutive_consonants > entropy

This demonstrated that unreadable consonant-heavy strings were stronger indicators of DGA activity than randomness alone.

F1 Score Heatmap
<p align="center"> <img src="07_f1_heatmap.png" width="750"> </p>

The ANN/MLP model consistently achieved the best F1-score across multiple train-test splits.

Key Findings
1. ANN achieved the best overall performance

The neural network achieved approximately:

90% F1-score

across evaluation experiments.

2. Random Forest offered strong interpretability

Random Forest provided detailed feature importance analysis while maintaining excellent performance with faster training time.

3. Consecutive consonants were highly effective indicators

Domains with long unreadable consonant sequences strongly correlated with DGA-generated behavior.

Example:

xkqzpwrtq.biz
4. Larger training splits improved consistency

The 80:20 split consistently produced the strongest results.

Skills Demonstrated
Cybersecurity-focused machine learning
DNS anomaly detection
DGA analysis
Feature engineering
Exploratory Data Analysis (EDA)
Data preprocessing
PCA analysis
Model evaluation
F1-score analysis
Python-based ML workflows
Security analytics
Tools & Technologies
Technology
Python
Pandas
NumPy
Scikit-learn
Matplotlib
Seaborn
Jupyter Notebook
Future Improvements

Potential future enhancements include:

Real-time DNS monitoring
Character n-gram analysis
SMOTE imbalance handling
Isolation Forest anomaly detection
Detection of unseen DGA families
SIEM integration for automated alerting
Getting Started
Requirements
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
Run the Project
jupyter notebook

Open:

dns_anomaly_detection.ipynb

Run all notebook cells.

Conclusion

This project demonstrated how machine learning can support cybersecurity operations by identifying malicious DGA-generated domains using only lexical and statistical domain features.

Even without DNS traffic analysis or packet captures, the models successfully distinguished legitimate domains from malicious domains with strong performance.

The project combines practical machine learning workflows with real-world cybersecurity threat detection concepts relevant to SOC operations and blue team environments.

References
MITRE ATT&CK – T1568 Dynamic Resolution
Scikit-learn Documentation
Shannon Entropy Theory
Alexa Top Sites
