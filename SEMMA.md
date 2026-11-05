# semma_student_performance.ipynb

*A careful Sample, Explore, Modify, Model, and Assess workflow using student records*

## Introduction

Student-performance prediction can be useful when it supports tutoring, advising, and timely academic assistance. It becomes problematic when a prediction is treated as a permanent label or a substitute for human judgment.

This project applies the SEMMA methodology to the Kaggle copy of the UCI Student Performance dataset. The project has two related objectives: predict a student’s final mathematics grade and identify students who may need academic support.

The distinction between these objectives matters because the dataset includes G1 and G2, earlier grades that are strongly related to the final grade G3.

## The Dataset

The mathematics dataset contains 395 students and 33 variables. The variables describe school, demographics, family and social characteristics, study time, absences, previous failures, support programs, lifestyle factors, previous grades G1 and G2, and final grade G3.

The final grade ranges from 0 to 20. In this project, a student is defined as at risk when G3 is below 10. This is an analytical definition for the demonstration, not a universal definition of academic difficulty.

## Sample

The first SEMMA stage creates reproducible modeling samples. A fixed random seed is used for the train/test split. The regression task and classification task use their own splits because the classification task requires stratification to preserve the at-risk proportion.

The sample contains no missing values or duplicate rows. Even when a dataset appears clean, the Sample stage should document its size, population, split strategy, and limitations.

## Explore

Exploration examines grade distributions, absences, study time, previous failures, school differences, and correlations among G1, G2, and G3.

The strongest observation is the relationship between previous and final grades. The correlations are approximately 0.801 for G1 and G3, 0.905 for G2 and G3, and 0.852 for G1 and G2.

This is useful for end-of-term prediction, but it creates a timing problem. If the intended use is early intervention before earlier grades are available, including G1 and G2 would make the claim misleading.

## Modify

The project creates two feature views.

The end-of-term grade model includes G1 and G2 because those grades are available for an end-of-term estimate.

The early-warning risk model excludes G1 and G2 and uses the remaining demographic, social, study, attendance, and school variables.

Both feature views use leakage-safe preprocessing. Numeric variables are imputed and standardized. Categorical variables are imputed and one-hot encoded. The transformations are fit only on the training partition.

## Model

The grade-prediction task uses Linear Regression as an interpretable baseline and Random Forest Regression as a nonlinear benchmark. The early-warning task uses balanced Logistic Regression and a Decision Tree Classifier.

The random forest achieved an RMSE of approximately 2.023 and an R² of approximately 0.800 on the held-out test set. The model’s typical error was about two grade points on a 0–20 scale, although errors vary across students.

The early-warning logistic model achieved ROC-AUC of approximately 0.727 when G1 and G2 were excluded. This is a more honest estimate of how well available early information ranks students who may eventually fall below the selected threshold.

## Assess

The assessment metrics match the two tasks. Regression uses MAE, RMSE, and R². Early-warning classification uses ROC-AUC, precision, recall, F1, accuracy, and a confusion matrix.

The final result should not be presented as a definitive judgment about a student. A prediction should trigger supportive review, not punishment, exclusion, or denial of educational services.

## Mapping SEMMA to SAS Enterprise Miner

The Python workflow maps naturally to SAS Enterprise Miner concepts:

| SEMMA stage | Project implementation | SAS-style equivalent |
|---|---|---|
| Sample | Reproducible train/test partitions | Sample and partition nodes |
| Explore | Statistics and visualizations | StatExplore and graphical nodes |
| Modify | Imputation, encoding, scaling, feature views | Transformation and replacement nodes |
| Model | Regression, random forest, logistic regression, tree | Modeling nodes |
| Assess | Error metrics, ROC-AUC, confusion matrix | Model comparison and assessment nodes |

The repository provides a Python implementation and SAS mapping. It should not be described as a SAS Enterprise Miner execution unless the workflow is actually run in licensed SAS software.

## Lessons Learned

SEMMA is useful for keeping the analytical workflow organized. The most important modeling decision was not the algorithm. It was deciding which information would realistically be available at the time an intervention is supposed to happen.

## Reproduce the Project

- GitHub: Replace with your public repository URL
- Google Colab: Replace with your Colab URL
- Walkthrough video: Replace with your YouTube URL

## References

- Dataset: https://www.kaggle.com/dskagglemt/student-performance-data-set/metadata
- Original provenance: https://archive.ics.uci.edu/dataset/320/student%2Bperformance
- Methodology: https://support.sas.com/documentation/cdl/en/emcs/66392/HTML/default/n0pejm83csbja4n1xueveo2uoujy.htm
