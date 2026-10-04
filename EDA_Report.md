# EDA Report: What Influenced Survival on the Titanic?

**Author:** Agilan  **Dataset:** Titanic passengers (891 rows, 12 columns)  **Tools:** Python, pandas, Seaborn, SciPy

## 1. Executive summary
Of 891 passengers, 38.4% survived. Survival was driven mainly by **sex** (women 74.2% vs men 18.9%) and **ticket class** (1st 63.0%, 2nd 47.3%, 3rd 24.2%). Together they create large gaps: first-class women survived at 96.8%, third-class men at 13.5%. Age and family situation played a smaller role.

## 2. Business-style question
Which passenger characteristics are most associated with survival, and how strong are those associations?

## 3. Data quality
- Missing: Age 177 (19.9%), Cabin 687 (77.1%), Embarked 2.
- No duplicate rows.
- Actions: Age imputed by title median; Embarked by mode; Cabin replaced with a `HasCabin` flag.
- Fare has 116 high outliers (above £65.63), kept because they are real first-class fares.

## 4. Statistical summary
| Variable | Mean | Median | Std | Skewness |
|---|---|---|---|---|
| Age | 29.4 | 30.0 | 13.3 | 0.44 |
| Fare | 32.2 | 14.5 | 49.7 | 4.79 |
| FamilySize | 1.9 | 1 | 1.6 | 2.73 |

Fare is highly right-skewed, so medians and log scales are used for comparisons.

## 5. Findings
| # | Finding | Evidence |
|---|---|---|
| 1 | Women survived far more than men | 74.2% vs 18.9%; r = 0.54; chi-square p ≈ 1e-58 |
| 2 | Higher class meant higher survival | 63.0% / 47.3% / 24.2%; r = −0.34; p ≈ 5e-23 |
| 3 | Class gap persists within each sex | Women: 96.8% / 92.1% / 50.0%. Men: 36.9% / 15.7% / 13.5% |
| 4 | Survivors paid higher fares | £48.40 vs £22.12; Welch t-test p ≈ 3e-11 |
| 5 | Children had the best odds | Age ≤ 12: 57.5%; over 60: 22.7% (n = 22) |
| 6 | Travelling alone lowered survival | 30.4% alone vs 50.6% with family; very large families (5+) did worst |
| 7 | Port of embarkation was associated with survival | Cherbourg 55.4%, Queenstown 39.0%, Southampton 33.9% (likely reflects class mix) |

## 6. Correlation with survival
Sex +0.54, Pclass −0.34, HasCabin +0.32, Fare +0.26, IsAlone −0.20, Age −0.08, Parch +0.08, SibSp −0.04, FamilySize +0.02.

Multicollinearity worth noting: Pclass and HasCabin (−0.73), Pclass and Fare (−0.55), SibSp and FamilySize (0.89).

## 7. Recommendations
1. Analyse factors in combination (class × sex), not in isolation.
2. Treat fare, class and cabin as one "socio-economic" group of factors, since they are strongly correlated.
3. For any predictive follow-up, use cross-validation and keep `Sex`, `Pclass`, `Age`, `FamilySize` and `Fare` as core features.

## 8. Caveats
Only part of the passenger list is included; ages were partly imputed; findings are associations, not proof of cause.
