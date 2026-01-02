# 🧪 Toxicity Prediction Using Molecular Descriptors and Fingerprints (Tox21)

![Toxicity Outline](data/Toxicity_outline.png)

## 📌 Project Overview

This project implements an end-to-end machine learning pipeline for predicting chemical toxicity using the Tox21 dataset. Each toxicity assay is treated as an independent binary classification problem, with the goal of accurately identifying toxic compounds (class = 1) under strong class imbalance.

The workflow combines chemoinformatics feature engineering (RDKit) with machine learning models, emphasizing data leakage prevention, appropriate preprocessing, and scientifically sound evaluation metrics.

---

## 📂 Dataset

- Source: Tox21 Challenge Dataset  
- Input: SMILES strings  
- Targets: Multiple nuclear receptor (NR) and stress response (SR) toxicity assays  
- Key Challenges:
  - Severe class imbalance
  - Missing labels per assay
  - Nonlinear structure–toxicity relationships

---

## 🧬 Feature Engineering

### 1️⃣ Molecular Descriptors (Scalar Features)

Computed using RDKit:

- TPSA  
- Molecular Weight (MW)  
- Hydrogen Bond Donors (HBD)  
- Hydrogen Bond Acceptors (HBA)  
- Rotatable Bonds  
- LogP  
- Ring Count  
- Molar Refractivity  
- Heavy Atom Count  

### 2️⃣ Molecular Fingerprints

- Morgan (ECFP) fingerprints  
- Radius = 3  
- Length = 2048 bits  
- Binary representation (0/1)  

Fingerprints are kept unscaled to preserve chemical substructure information.

---

## 🔄 Data Preprocessing

- Invalid SMILES are safely filtered
- Missing labels are removed per target
- Scalar descriptors undergo **Power Transformation (Yeo–Johnson)**
- PowerTransformer is **fit only on training data** to prevent data leakage
- Fingerprints remain untouched
- StandardScaler is applied **only** to models that require it (Logistic Regression, KNN)

---

## 🤖 Models Evaluated

The following models are evaluated independently for each toxicity target:

- Logistic Regression (with scaling)
- K-Nearest Neighbors (with scaling)
- Random Forest
- XGBoost
- LightGBM (Balanced)
- LightGBM (Unbalanced)

Tree-based models are not scaled, as they are scale-invariant.

---

## 📊 Model Evaluation

- Train/Test split with stratification (80/20)
- Evaluation Metrics:
  - Classification Report
  - Confusion Matrix
  - F1 Score for toxic class (class = 1)

The F1 score of the toxic class is used for model comparison due to strong class imbalance.

---

## 🏆 Model Selection & Saving

- Only the **best-performing model per target** is saved
- Selection is based on **F1 score of class 1**
- The following artifacts are stored:
  - Best trained model (`<target>_best_model.joblib`)
  - PowerTransformer fitted on training data (`<target>_power_transformer.joblib`)

This ensures reproducibility and safe inference.

---

## 🧪 Inference Workflow

1. Compute RDKit descriptors and fingerprints for new molecules
2. Apply saved PowerTransformer to scalar descriptors
3. Concatenate transformed descriptors with fingerprints
4. Use the saved model to predict toxicity

---

## 🔒 Data Leakage Prevention

All preprocessing steps (power transformation and scaling) are fit exclusively on training data and applied to test data, ensuring strict separation between training and evaluation.

---

## 📈 Key Observations

- Tree-based models consistently outperform linear and distance-based models
- Balanced models improve recall for toxic compounds
- Some toxicity assays remain challenging due to extreme imbalance and weak signal

---

## 🚀 Future Improvements

- Threshold tuning for F1 optimization
- Precision–Recall AUC evaluation
- Model interpretability using SHAP
- Multi-task learning across assays

---

## 🏁 Conclusion

This project demonstrates a robust and scientifically sound machine learning pipeline for chemical toxicity prediction. It highlights the importance of careful preprocessing, appropriate evaluation metrics, and model selection in highly imbalanced cheminformatics datasets.

---

## 🛠️ Tech Stack

- Python
- RDKit
- scikit-learn
- XGBoost
- LightGBM
- NumPy, Pandas
- Joblib

---

## 👤 Author

Yash Dhanawate
Data Scientist | Chemoinformatics & Machine Learning  

- 🔗 LinkedIn: https://www.linkedin.com/in/your-linkedin-username  
- 💻 GitHub: https://github.com/Yash-Dhanawate
