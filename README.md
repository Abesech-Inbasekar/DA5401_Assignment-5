# DA5401 — Assignment 5: Manifold Visualization (t-SNE & Isomap)

**Student:** Abesech Inbasekar  
**Roll Number:** ME21B003  

---
## Repository Structure
├── Assignment_5.ipynb # Main Jupyter notebook with preprocessing, t-SNE, Isomap, and analysis
├── README.md # This file – explains project structure and submission details

**Assignment_5.ipynb:**  
Contains the full workflow — data loading, preprocessing, scaling, t-SNE and Isomap embedding,  
visualization, veracity inspection, and comparison of local vs global structure.

---

## 🧩 How to Run

1. **Clone the repository** and open the notebook in **Jupyter** or **Google Colab**.  
2. Place `yeast.arff` and `yeast.xml` inside the `data/` folder (or update the paths in the notebook).  
3. Run all cells sequentially.  

The notebook automatically produces:
- Scaled feature matrix and simplified label mapping (`viz_target`).  
- Multiple t-SNE and Isomap plots with different hyperparameters.  
- Annotated veracity-inspection figure (noisy labels, outliers, mixed regions).  
- Markdown-based analytical explanations and final comparative table.

## Objective

This assignment aims to deepen the understanding of **data veracity** and **multi-label learning challenges**  
in real-world datasets using **non-linear dimensionality reduction** methods.

Specifically, the task involves analyzing the **Yeast gene expression dataset** to visualize and interpret:
- **Noisy / ambiguous labels**  
- **Outliers**  
- **Hard-to-learn samples**

using **t-SNE** and **Isomap**, while studying how these algorithms preserve **local** and **global** manifold structures.

---

## Dataset Overview

- **Dataset:** Yeast Multi-Label Dataset (from MULAN Repository)  
- **Samples:** 2,417  
- **Features:** 103 continuous attributes representing gene-expression and physicochemical descriptors  
- **Labels:** 14 binary functional categories (multi-label target)

| Component | Count | Description |
|------------|--------|-------------|
| Total Columns | 117 | Features (103) + Labels (14) |
| Feature Matrix (X) | 103 | Continuous-valued descriptors |
| Target Matrix (Y) | 14 | Binary (0/1) multi-label indicators |

---

## Methodology

### **Part A — Preprocessing and Label Simplification**

1. **Data Loading:**  
   - Parsed `yeast.arff` and `yeast.xml` to extract 103 feature columns and 14 label columns.
2. **Label Simplification:**  
   - Constructed a compact categorical variable `viz_target` with 4 color categories:
     - `Single[Class1]` — most frequent single-label class  
     - `Single[Class2]` — second single-label class (0 samples; retained for consistency)  
     - `MultiTop[Class3, Class5, Class12, Class13]` — most frequent multi-label combination  
     - `Other` — all remaining samples
3. **Feature Scaling:**  
   - Applied `StandardScaler` to ensure zero mean and unit variance, enabling fair distance computation.

---

### **Part B — t-SNE Implementation and Veracity Inspection**

1. **t-SNE Embedding:**
   - Performed dimensionality reduction on `X_scaled` using different perplexity values (5, 30, 50).
   - Selected **perplexity = 30** as optimal for balanced local–global structure.
2. **Visualization:**
   - Generated 2-D scatter plots color-coded by `viz_target`.
3. **Veracity Inspection:**
   - Identified and analyzed key regions:
     | Category | Approx. Region (x, y) | Observation |
     |-----------|----------------------|--------------|
     | **Noisy / Ambiguous Labels** | (−25→0, −35→−20) | Green (`Single[Class1]`) embedded in blue (`Other`) clusters → possible mislabeling. |
     | **Outliers** | (+45→+50, +30→+40) & (−40, −45) | Isolated blue points → rare expression patterns or experimental noise. |
     | **Hard-to-Learn Samples** | (−10→+10, −5→+10) | Blue–orange overlap → intertwined manifolds, non-linear decision boundaries. |

   - **Inference:**  
     The Yeast dataset shows high label overlap and manifold complexity, explaining classification difficulty.

---

### **Part C — Isomap and Comparison**

1. **Isomap Implementation:**
   - Applied Isomap on `X_scaled` with `n_neighbors = 5, 10, 20`.  
   - Selected **n_neighbors = 10** for best connectivity and smooth manifold layout.

2. **Comparison:**
   | Aspect | **t-SNE** | **Isomap** |
   |---------|------------|-------------|
   | Preserves | Local structure (neighborhoods) | Global manifold geometry |
   | Optimizes | KL divergence (local probabilities) | Geodesic distance (via MDS) |
   | Best for | Revealing small-scale clusters, noisy overlaps | Understanding global continuity |
   | Weakness | Global distances meaningless | Fine local details blurred |

3. **Manifold Curvature Discussion:**
   - The Isomap embedding reveals a **smooth but curved manifold**,  
     implying that gene-expression data lie on a **non-linear surface** in high-dimensional space.  
   - Such curvature increases **classification difficulty**, as classes overlap continuously rather than forming separable clusters.  
   - Simple linear classifiers fail here; non-linear models (kernel SVM, neural networks) are required.

---

## Key Insights

- **t-SNE** acts like a **microscope**: reveals local ambiguities and noisy labels.  
- **Isomap** acts like a **map**: reveals the global terrain and manifold curvature.  
- The Yeast dataset’s **multi-label nature** inherently produces overlapping manifolds →  
  leading to **noisy decision boundaries**, **outliers**, and **hard-to-learn regions**.

---

## Takeaway

Manifold visualization exposes the underlying structure of real biological data:  
it is **continuous, curved, and overlapping** rather than neatly separable.  
Understanding this geometry explains why high classification accuracy is challenging and  
why **data preprocessing, noise handling, and non-linear modeling** are crucial in multi-label biological tasks.

---

## Dependencies

```bash
python>=3.10
numpy
pandas
matplotlib
scikit-learn
scipy


