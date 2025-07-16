# 🧬 Warfarin Dose Prediction Based on Genetic Risk Score

This project analyzes the influence of genetic variants **CYP2C9** and **VKORC1** on warfarin dosing by converting genotype data into a quantitative **genetic risk score**. The analysis is based on real-world genotype-dosage data from the PharmGKB database.

---

## 📌 Backgrounds

## 📌 Objectives

- Assess the relationship between **CYP2C9** and **VKORC1** genotypes with therapeutic warfarin dose.
- Translate genotypes into **Genetic Risk Scores (GRS)** for simplified stratification.
- Visualize dose variation across different risk groups.

---

## 📌 Methodology

### 📦 Dataset

- **Source**: [PharmGKB Warfarin Dosing Dataset](https://www.pharmgkb.org/)
- **Key Columns**:
  - `PharmGKB Subject ID`
  - `CYP2C9 Genotype`
  - `VKORC1 Genotype`
  - `Therapeutic Dose of Warfarin (mg/day)`

### 🧼 Data Preprocessing

- Selected only important columns for analysis:
  - `"PharmGKB Subject ID"`, `"CYP2C9 Genotype"`, `"VKORC1 genotype: -1639 G>A (3673); chr16:31015190; rs9923231; C/T"`, and `"Therapeutic Dose of Warfarin"`.
- Renamed columns to simpler and readable names:
  - `"PharmGKB Subject ID"` → `"ID"`
  - `"CYP2C9*Genotype"` → `"CYP2C9"`
  - `"VKORC1 genotype: -1639 G>A (3673); chr16:31015190; rs9923231; C/T"` → `"VKORC1 (rs9923231; C/T)"`
  - `"Therapeutic Dose of Warfarin"` → `"Dose"`
- Removed rows with missing values in `CYP2C9`, `VKORC1 (rs9923231; C/T)`, or `Dose`
- Standardized genotype formatting:
  - `"A/G"` → `"AG"`
  - `"*1 / *3"` → `"*1/*3"` (no spaces)
- Created a new column: `Risk_Score` based on predefined genotype scoring

### 🧮 Genetic Risk Score System

| Gene     | Genotype(s)              | Score |
|----------|--------------------------|-------|
| CYP2C9   | *1/*1                    | 0     |
|          | *1/*2, *1/*3             | 1     |
|          | *2/*3, *3/*3             | 2     |
| VKORC1   | GG                       | 0     |
|          | AG                       | 1     |
|          | AA                       | 2     |

**Total Risk Score = CYP2C9 Score + VKORC1 Score** (Range: 0–4)
  
## 📌**Results and Visualization**

### Number of Patients per Risk Score

<img width="580" height="455" alt="Number of patients by genetic risk score1" src="https://github.com/user-attachments/assets/e6fe62e3-5e32-46f8-ac80-fcd1ef14c245" />

**Figure 1**: Number of patients by genetic risk score

> Most patients fall within Risk Score 0 to 2. Very few samples exist for scores 3 and 4, indicating data imbalance.
> The dataset shows a clear imbalance in the number of patients across genetic risk scores.
> This imbalance may affect the interpretation of dose trends at higher scores, as small sample sizes (especially for score 4) may not represent the broader population. Interpretations involving high scores should be made with caution

To improve clarity, the countplot is annotated with the exact count values above each bar using `ax.text()`. This helps highlight class imbalance and ensures the data distribution is clearly visible at a glance.


### Distribution of warfarin dose by genetic score

<img width="989" height="590" alt="Distribution of warfarin dose by genetic risk score" src="https://github.com/user-attachments/assets/136f9baf-5a22-405a-bf21-73b8c55a144c" />

**Figure 2**: Distribution of warfarin dose by genetic score

> A clear downward trend is observed in median dose from score 0 to 4. The dots represent outliers — patients whose dose needs differ significantly from the rest of the group.

Outliers are detected based on statistical rules (typically values above 1.5 times the interquartile range), and they help identify variation in dose that might be due to other factors such as:

- Age, body weight, or gender
- Drug interactions or comorbidities
- Possible data entry errors

While outliers are valid and expected in real-world data, a filtered version of the plot (with doses ≤100 mg/week) is provided to better visualize typical dosing trends across genetic risk scores.


### Average warfarin dose by genetic risk score

<img width="841" height="547" alt="Average warfarin dose by genetic risk score" src="https://github.com/user-attachments/assets/9782051a-5ad8-486c-9640-179a318e2de5" />

**Figure 3**: Average warfarin dose by genetic risk score


> Based on the genetic risk score (0–4), we analyzed how warfarin dose varies across different levels of genetic sensitivity.

>  **Mean Warfarin Dose by Risk Score:**
> 
    - Risk score 0 → ~52–54 mg/week
    - Risk score 1 → ~47–50 mg/week
    - Risk score 2 → ~43–46 mg/week
    - Risk score 3 → ~38–41 mg/week
    - Risk score 4 → ~35–37 mg/week

> There is a **clear downward trend** in the average dose as the genetic risk score increases. This supports the idea that certain genotypes (e.g. *2/*3 in CYP2C9 or A/A in VKORC1) are associated with **increased sensitivity to warfarin**, requiring lower dosages.

> The line plot with error bars further confirms this pattern with overlapping standard deviations between adjacent scores, indicating some variability but still an overall consistent trend.

### What does the Error Bar mean?

The vertical lines (error bars) represent the **standard deviation**, which tells us how much variation exists within each group.

- **Short error bar** = consistent dose in that group
- **Long error bar** = wider variation in dose needed

When error bars between different scores **don’t overlap**, it suggests a real difference in average dose.

---

## 📌**Conclusion**

This analysis confirms that increasing genetic risk scores (based on **CYP2C9** and **VKORC1**) are generally associated with lower warfarin doses.

This suggests that pre-treatment genetic testing could support safer and more effective warfarin dosing.

> ⚠️ Note: This is a simulation based on rule-based scoring and retrospective data. Clinical application requires validation and population-specific adjustments.

## 👩‍🔬 Author

**Farah Yusri**  
Biomedical & Pharmacogenomics Enthusiast  
[LinkedIn](#) | [GitHub](#) | [Email](farahyusri12@gmail.com)
