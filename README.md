# Gene-Type-Prediction  
# DNA Sequence Gene-Type Prediction using Classical ML and Deep Learning

## 1. Introduction

This project focuses on **predicting gene types from DNA nucleotide sequences** using a combination of **classical machine learning** and **deep learning** approaches.

Each DNA sequence consists of four nucleotides:

- **A** – Adenine  
- **T** – Thymine  
- **C** – Cytosine  
- **G** – Guanine  

The objective is to learn patterns from these sequences and classify each gene into a **gene type**. Examples include:

- `PSEUDO`
- `BIOLOGICAL_REGION`
- `OTHER`
- Many additional rare gene types

The dataset is **highly imbalanced**, with a few dominant classes and many sparse ones. To handle this:

- Rare classes were merged into a single **`OTHERS`** class  
- A **one-vs-rest ensemble** was built for important classes  
- Multiple ML/DL models were explored:
  - Logistic Regression (main model)
  - LSTM / BiLSTM
  - CNN–BiLSTM hybrid
  - BiLSTM + Transformer
  - DistilBERT model

The **main "production-ready" model** is:

> **TF-IDF + Logistic Regression Ensemble**  
> Implemented in **`dna-seq-gene-type-prediction (1).ipynb`**

---

## 2. Dataset Description

### 2.1 Source  
Derived from the **NCBI Gene Database**, featuring:

- Gene metadata  
- Gene descriptions  
- Gene types  
- Raw nucleotide sequences  

### 2.2 Columns  
A sample row contains:

- `NCBIGeneID`
- `Symbol`
- `Description`
- `GeneType`
- `GeneGroupMethod`
- `NucleotideSequence`

---

## 3. Data Preprocessing

### 3.1 Cleaning
- Removed leading `<`
- Converted sequences to uppercase
- Dropped unused index columns
- Preserved biological sequence authenticity

### 3.2 Class Merging (Handling Imbalance)
Major classes retained:
- `PSEUDO`
- `BIOLOGICAL_REGION`

All rare classes merged into:
- **`OTHERS`**

### 3.3 Feature Preparation
- For ML models: **TF-IDF (char-level n-grams)**  
- For DL models:
  - Tokenization (char-level)
  - Integer encoding
  - Sequence padding  
- Optional augmentation for rare classes (sequence shuffling)

---

## 4. Main Model: TF-IDF + Logistic Regression

This model was chosen as the **primary production model** because:

- Handles imbalanced data better than deep networks
- TF-IDF captures **n-gram biological motifs**
- Fast to train and deploy
- Highly interpretable
- More stable than DL models under dataset imbalance

### 4.1 Model Workflow
1. Clean raw sequences  
2. Convert to TF-IDF vectors  
3. Train:
   - Multi-class Logistic Regression  
   - One-vs-Rest binary models for major classes  
4. Final prediction:
   - Based on highest probability across ensemble models  

---

## 5. Model Performance

### 5.1 Validation Results
- **Overall accuracy:** **81.43%**  
- Strong precision for:
  - `PSEUDO`
  - `BIOLOGICAL_REGION`
- Ensemble logic improved class-wise recall by **8–12%**

### 5.2 Why Logistic Regression Performs Best
- Better class-weight handling  
- Less prone to overfitting  
- Fast and stable  
- TF-IDF provides extremely effective pattern encoding  

Compared to deep models:
- BiLSTM models struggled with imbalance  
- CNN–BiLSTM required heavy compute  
- DistilBERT is powerful but **not DNA-specific**

---

## 6. Kaggle Notebook (Published)

The end-to-end implementation of the main model is available on Kaggle:

👉 **Kaggle Notebook:**  
https://www.kaggle.com/code/magicps/dna-seq-gene-type-prediction

This notebook includes:
- Full preprocessing pipeline
- TF-IDF feature extraction
- Logistic Regression training
- Confusion matrix & visualizations
- Inference examples
- Sequence length distribution plots  

---

## 7. Additional Models Explored

### 7.1 BiLSTM  
Captures long-term dependencies in DNA sequences.

### 7.2 CNN + BiLSTM Hybrid  
Extracts local motifs via CNN and global order via LSTM.

### 7.3 BiLSTM + Transformer  
Adds multi-head attention for long-range gene pattern detection.

### 7.4 DistilBERT  
Transformer-based model applied to DNA text.  
Not DNA-specific → performance moderate.

**Overall**, Logistic Regression remains the most **reliable, stable, and accurate** for this dataset.

---

## 8. Inference Example (Main Model)

The model returns:
- Predicted gene type  
- Probability distribution  
- Cleaned input sequence  


---

## 9. Comparison Summary

| Model                         | Description | Performance |
|------------------------------|-------------|-------------|
| **Logistic Regression (Main)** | TF-IDF + LR, stable under imbalance | ⭐ **Best overall** |
| BiLSTM                       | Captures order | Medium |
| CNN + BiLSTM                | Motif + context | Good but heavy |
| BiLSTM + Transformer        | Long-range attention | High variance |
| DistilBERT Transformer      | Powerful encoder | Moderate |

---





