# Predicting Student Academic Math Performance

Predictive modeling project exploring whether demographic, family, and lifestyle data — available *before* a school term even begins — can identify students at risk of failing their math course, using the [UCI Student Performance dataset](https://archive.ics.uci.edu/dataset/320/student%2Bperformance) (Cortez & Silva, 2008).

## Business Problem

By the time a school sees a final grade, the term is over and the window for intervention has closed. Schools already collect demographic, family, and lifestyle information on students from day one — but there's no validated way to turn that information into an early warning signal.

**Objective:** Determine whether pre-term data (excluding in-term grades G1/G2) can accurately predict pass/fail outcomes, and identify which factors contribute most to failure risk — so a school could direct limited intervention resources (tutoring, counseling, outreach) toward the students who need them most.

**Research Questions:**
1. Using only pre-term information, how accurately can we predict whether a student will pass or fail their final math grade, and which modeling technique performs best?
2. What specific factors contribute most to failure risk, and how could a school use them to design targeted interventions?

## Dataset

- **395 students**, 33 variables (demographic, family, school, and lifestyle attributes)
- No missing values or duplicate records
- Target: binary pass/fail, where pass = final grade (G3) ≥ 10
- G1 and G2 (first/second period grades) intentionally excluded from the predictive models to preserve a true "early prediction" framing

## Methodology

Three supervised classifiers were built and compared, plus an unsupervised clustering analysis:

| Model | CV Accuracy | Test Accuracy | AUC | Fail Recall |
|---|---|---|---|---|
| Logistic Regression | 69.0% | 65.8% | 0.593 | 35% |
| Classification Tree (depth=1) | 72.2% | 67.1% | 0.588 | 35% |
| K-Nearest Neighbors (k=15) | 68.7% | 68.4% | 0.458 | 8% |
| *Naive "predict everyone passes" baseline* | — | *67.1%* | *0.500* | *0%* |

K-Means clustering (k=2) additionally segmented students by lifestyle/behavioral features (study time, failures, absences, social/alcohol activity), producing a lower-risk cluster (70.7% pass rate) and a higher-risk cluster (58.0% pass rate), with a silhouette score of 0.299.

## Key Findings

- **No model meaningfully beat the naive baseline** of predicting every student passes. AUC values (0.46–0.59) hover only slightly above random guessing, and each classifier caught at most about a third of actual failures (KNN caught almost none).
- **Number of past class failures is the single strongest predictor** across every method tested — it's the classification tree's only split, and it's what separates the K-means clusters most clearly.
- Study time, absences, and social/alcohol activity showed a consistent but weaker secondary relationship with risk, in the expected direction.

## Business Recommendation

**None of these models should be deployed on their own to drive intervention decisions.** Demographic and lifestyle data alone aren't sufficient for reliable early prediction — they should supplement, not substitute for, historical academic performance. In the meantime, number of past failures (paired with low study time, high absences, and high social/alcohol activity) is worth using as a lightweight, human-reviewed flag for optional outreach rather than an automated rule.

## Limitations & Future Work

1. **Excluding G1/G2 by design** likely cost the most predictive signal available — next step is testing models that incorporate G1 once it's available a few weeks into term.
2. **Class imbalance** (265 pass vs. 130 fail) biased models toward predicting "pass" — next step is class weighting, SMOTE, or threshold tuning.
3. **Linear/shallow models** may miss real interactions between risk factors — next step is testing random forest / gradient boosting.
4. **Binary pass/fail framing** discards information about borderline vs. clearly-at-risk students — next step is a tiered risk framing or direct regression on G3.
5. **Clustering used a narrow, 6-variable feature set** with modest separation — next step is expanding features and testing alternative cluster counts/algorithms.

## Repo Contents

| File | Description |
|---|---|
| `student-mat.xlsx` | Source dataset (UCI Student Performance – Math course) |
| `Student_Performance_Predictive_Modeling.ipynb` | Full analysis: EDA, preprocessing, logistic regression, classification tree, K-means clustering, KNN, model comparison |
| `Student_Performance_Predictive_Modeling.pptx` | Executive summary slide deck |

## Tools

Python · pandas · scikit-learn · matplotlib · seaborn

## Author

Colin Haley — August 2026
