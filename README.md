# **Visualizing Data Veracity Challenges in Multi-Label Classification (DA5401 Assignment #5)**

**Student Name:** Shreehari Anbazhagan\
**Roll Number:** DA25C020

---
## Project Overview

This project delves into the challenges of real-world machine learning by analyzing the **Yeast dataset**, a benchmark for multi-label classification. The primary objective is to use advanced non-linear dimensionality reduction techniques—**t-SNE** and **Isomap**—to visually inspect the data for veracity issues.  

Instead of simply building a classifier, this analysis focuses on understanding the data's inherent structure and identifying potential problems such as **noisy labels**, **outliers**, and **hard-to-learn samples** where functional categories are highly mixed.

The goal is to tell a **data-driven story** about the challenges a classifier would face, providing a visual foundation for understanding the data's complexity before any modeling is attempted.

---

## Folder Structure & Files

```
project-root/
│
├─ main.ipynb              # Core Jupyter Notebook with all code, visualizations, and explanations
├─ python-version          # Python version used for reproducibility
├─ pyproject.toml          # Project dependencies
├─ uv.lock                 # Locked dependency versions (for uv sync)
├─ .gitignore              # Excludes dataset files
├─ Instructions/           # Assignment PDF (problem statement & instructions)
    └─ DA5401 A5 Manifold Visualization.pdf
└─ datasets/               # Dataset folder
   └─ yeast.csv            #  MULAN Repository - Yeast Data (from Openml ID: 40597)
```

### Notes:

* Run `uv sync` to install dependencies exactly as tested.
* The dataset is **not pushed to GitHub**. Please download using data loading code block in Part A.
* Plots using **plotly are not visible in github** as they use JavaScript to render, Please run them locally or use nbviewer.
* The notebook is **self-contained**: all steps are reproducible without manual intervention.

---

## Dependencies

```python
import os
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.manifold import TSNE, Isomap
from sklearn.datasets import fetch_openml
from sklearn.preprocessing import StandardScaler
```

---

## Analysis Workflow

### 1. Data Preprocessing
- The Yeast dataset, with **103 gene expression features** and **14 binary functional labels**, was loaded.  
- Initial data cleaning handled boolean and categorical data types, converting them into a numerical format.  
- **Feature scaling** was applied using `StandardScaler` to ensure all features contributed equally to distance-based calculations in t-SNE and Isomap.

### 2. Label Simplification for Visualization
To make the plots interpretable, the 14 binary labels were condensed into a single categorical variable with three primary groups:
- The most frequent single-label class (**Single: Class1**)
- The most frequent multi-label combination (**Frequent Multi-Label**)
- An **Other** category for all remaining instances

### 3. t-SNE for Local Structure Analysis
- **t-SNE** was implemented to map the data to 2D, focusing on preserving **local neighborhood structures**.  
- The **perplexity** hyperparameter was tuned systematically (5, 30, 50), with **30** ultimately chosen for the most stable and balanced visualization.

### 4. Isomap for Global Manifold Learning
- **Isomap** was applied to understand the **global geometric structure** of the data.  
- The **n_neighbors** hyperparameter was varied (5, 10, 20, 50) to find the most meaningful representation of the underlying data manifold.

---

## Key Findings

### 1. Severe Class Overlap is the Main Challenge
Both t-SNE and Isomap visualizations conclusively showed that the different functional categories are **heavily intermingled**.  
This indicates that the dataset lies in a **hard-to-learn** region, with no simple boundaries separating classes.

### 2. Data Lies on a Complex Manifold
Isomap revealed an **elongated, non-uniform shape**, suggesting that the data lies on a **curved and complex manifold**.  
This provides a geometric reason for the high classification difficulty—simple linear models would struggle to follow these curves.

### 3. Genes are Inherently Multi-Functional
The label simplification step revealed that the most frequent **multi-label combination** was over **seven times more common** than the top single-label class.  
This highlights that **multi-functionality** is a core characteristic of this dataset.

### 4. Outliers and Ambiguity
While most data was inseparable, a few distinct outliers were identified.  
These may represent **experimental errors** or, more interestingly, **rare biological states** worth deeper investigation.

---

## Conclusion and Recommendations

This project successfully used **advanced visualization** to diagnose key **data veracity challenges** in the Yeast dataset.  
The core insight is that the difficulty in classification arises not from noise alone but from the **inherent ambiguity and overlap** between gene function categories.

### Recommendations for Future Work
- **Use Non-Linear Models:** Employ non-linear classifiers such as **Kernel SVMs** or **Neural Networks** that can learn the complex boundaries revealed by Isomap.  
- **Feature Engineering:** Incorporate **domain-specific biological insights** to engineer features that better capture separable aspects of gene functionality.  
- **Embrace the Ambiguity:** Instead of forcing strict labels, consider **probabilistic** or **fuzzy classification** approaches to naturally model overlapping class boundaries.