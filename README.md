# 🚢 Exploratory Data Analysis of the Titanic Dataset

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![pandas](https://img.shields.io/badge/pandas-EDA-150458)
![License](https://img.shields.io/badge/License-MIT-green)

> **What really decided who survived the Titanic?** An end-to-end exploratory data analysis using statistical summaries, visualizations and correlation analysis to uncover patterns and key influencing factors.

---

## 📑 Table of Contents
1. [Project Overview](#-project-overview)
2. [Objectives](#-objectives)
3. [Dataset](#-dataset)
4. [Tools & Technologies](#-tools--technologies)
5. [Workflow](#-workflow)
6. [Data Cleaning](#-data-cleaning)
7. [Key Findings & Insights](#-key-findings--insights)
8. [Conclusion & Recommendations](#-conclusion--recommendations)
9. [Limitations & Future Work](#-limitations--future-work)
10. [How to Run](#-how-to-run)
11. [Repository Structure](#-repository-structure)
12. [Author](#-author)
13. [License & Acknowledgements](#-license--acknowledgements)

---

## 📌 Project Overview
This project explores passenger data from the RMS Titanic to understand which factors were linked to survival. It follows a complete EDA workflow: inspecting and cleaning the data, summarizing it statistically, visualizing distributions and relationships, testing correlations, and presenting insights in a structured report.

The goal is not to build a predictive model, but to **develop analytical thinking**: ask questions of the data, check assumptions, and support every claim with evidence.

## 🎯 Objectives
- Use **statistical summaries** (mean, median, spread, skewness) to understand each variable
- Use **visualizations** to reveal distributions, patterns and trends
- Identify **correlations** and **key influencing factors** on survival
- Present insights in a **structured report** ([`reports/EDA_Report.md`](reports/EDA_Report.md))

## 📊 Dataset
- **Source:** Titanic passenger list ([Data Science Dojo copy](https://github.com/datasciencedojo/datasets/blob/master/titanic.csv); originally compiled from the Kaggle "Titanic: Machine Learning from Disaster" competition)
- **Size:** 891 passengers × 12 columns (17 columns after cleaning and feature engineering)
- **Target variable:** `Survived` (0 = died, 1 = survived)

| Column | Type | Description |
|---|---|---|
| PassengerId | int | Unique passenger ID |
| Survived | int | 0 = died, 1 = survived |
| Pclass | int | Ticket class (1 = upper, 2 = middle, 3 = lower) |
| Name | text | Passenger name |
| Sex | category | male / female |
| Age | float | Age in years |
| SibSp | int | Number of siblings / spouses aboard |
| Parch | int | Number of parents / children aboard |
| Ticket | text | Ticket number |
| Fare | float | Ticket price |
| Cabin | text | Cabin number (dropped, 77% missing) |
| Embarked | category | Port: C = Cherbourg, Q = Queenstown, S = Southampton |

**Engineered features:** `Title`, `HasCabin`, `LogFare`, `FamilySize`, `IsAlone`, `AgeGroup`.

## 🛠 Tools & Technologies
Python · pandas · NumPy · Matplotlib · Seaborn · SciPy · scikit-learn (feature ranking only) · Jupyter Notebook · Git & GitHub

## 🔄 Workflow
```
Raw data → Cleaning → Statistical summary → Visualization → Correlation & significance tests → Insights report
```
| Step | Notebook |
|---|---|
| 1. Data cleaning & feature engineering | [`01_data_cleaning.ipynb`](notebooks/01_data_cleaning.ipynb) |
| 2. Statistical summary & visualization | [`02_eda_visualization.ipynb`](notebooks/02_eda_visualization.ipynb) |
| 3. Correlations & key factors | [`03_correlation_insights.ipynb`](notebooks/03_correlation_insights.ipynb) |

## 🧹 Data Cleaning
| Issue | Found | Action |
|---|---|---|
| Missing `Age` | 177 rows (19.9%) | Filled with the **median age of the passenger's title** (Mr, Mrs, Miss, Master, Rare), since age varies strongly by title |
| Missing `Embarked` | 2 rows | Filled with the mode (`S`) |
| Missing `Cabin` | 687 rows (77.1%) | Too sparse to impute → created `HasCabin` flag, dropped the column |
| Duplicates | 0 | None to remove |
| `Fare` outliers | 116 rows above the IQR upper bound (£65.63) | **Kept**: they are genuine first-class fares. Added `LogFare` to handle skew |

## 🔍 Key Findings & Insights

### 1. Data overview
- Overall survival rate: **38.4%** (342 of 891).
- `Age` is roughly symmetric (mean 29.4, median 30). `Fare` is heavily right-skewed (skewness 4.79; median £14.45 vs mean £32.20).

![Distributions](images/01_distributions.png)

### 2. Sex was the strongest factor
- **74.2%** of women survived versus **18.9%** of men.
- Sex has the strongest correlation with survival (**r = 0.54**).

![Survival by factor](images/03_survival_by_factor.png)

### 3. Ticket class mattered: money bought safety
- Survival by class: **1st 63.0%**, **2nd 47.3%**, **3rd 24.2%**.
- Class and fare are linked (r = −0.55), and survivors paid on average **£48.40** versus **£22.12** for those who died.

### 4. Class and sex combined
- First-class women: **96.8%** survived. Third-class men: **13.5%**.
- Third-class women (50%) did much worse than first and second-class women (92–97%), so class mattered even among women.

![Class and sex](images/05_class_sex_survival.png)

### 5. Age and family
- Children (≤12) had the highest survival rate (**57.5%**); seniors the lowest (**22.7%**, but only 22 people).
- Passengers travelling alone survived at **30.4%** versus **50.6%** for those with family. Very large families (5+) did worst, so a small family helped but a large one did not.

![Age](images/04_survival_by_age.png)

### 6. Correlation analysis
![Heatmap](images/07_correlation_heatmap.png)

Correlation with `Survived`:

| Factor | Pearson r |
|---|---|
| Sex (female = 1) | **+0.54** |
| Pclass | **−0.34** |
| HasCabin | +0.32 |
| Fare | +0.26 |
| IsAlone | −0.20 |
| Age | −0.08 |

Statistical tests confirm these are not due to chance (all p < 0.05):

- Chi-square: Sex (p ≈ 1e-58), Pclass (p ≈ 5e-23), Embarked (p ≈ 2e-6), IsAlone (p ≈ 2e-9)
- Welch t-test: Fare (p ≈ 3e-11), Age (p = 0.022)

![Feature importance](images/09_feature_importance.png)

A Random Forest feature-importance ranking was used only to **order** factors. Age, Fare and Sex ranked highest. Continuous variables such as Age and Fare tend to look more important in this method, so the correlations and group comparisons above are the more reliable evidence.

## ✅ Conclusion & Recommendations
- **Who survived depended mostly on who you were**: sex, ticket class and (to a lesser degree) age and family situation.
- The pattern fits the "women and children first" rule, and the large class gap shows that access to lifeboats was unequal.
- **Lesson for risk analysis:** combine factors (class × sex) instead of looking at one at a time; single-factor views hide big differences.

## ⚠️ Limitations & Future Work
- Only 891 of the roughly 2,200 people aboard are in this dataset.
- 20% of ages were imputed, which may slightly smooth age effects.
- Correlation is not causation: class is tied to cabin location, which affected escape routes.
- **Next steps:** logistic-regression and tree models with cross-validation, an interactive Streamlit or Power BI dashboard, and analysis of surnames and ticket groups.

## ▶️ How to Run
```bash
git clone https://github.com/agiagilan2007-ship-it/titanic-eda-project.git
cd titanic-eda-project
pip install -r requirements.txt
jupyter notebook
```
Open the notebooks in `notebooks/` in numerical order (01 → 02 → 03).

## 📁 Repository Structure
```
titanic-eda-project/
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
├── data/
│   ├── raw/titanic.csv              # original dataset (unchanged)
│   └── processed/titanic_clean.csv  # cleaned dataset
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_eda_visualization.ipynb
│   └── 03_correlation_insights.ipynb
├── images/                          # charts used in this README
└── reports/
    └── EDA_Report.md                # structured insights report
```

## 👤 Author
**Agilan**
- GitHub: [agiagilan2007-ship-it](https://github.com/agiagilan2007-ship-it)
- LinkedIn: *add your link here*

⭐ If you found this project useful, please star the repo!

## 📄 License & Acknowledgements
Released under the [MIT License](LICENSE). Dataset: Titanic passenger data from the Kaggle "Titanic: Machine Learning from Disaster" competition, via the Data Science Dojo datasets repository.
