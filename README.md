# Loans Data Analysis: US Loans in 2015

Analysis of 2015 US consumer loan data from LendingClub, this project includes data preparation, descriptive analysis in Power BI, Random Forest classification of repayment outcomes, and TF-IDF + k-means clustering of borrowers' job titles. The analysis was presented as a scientific poster.

![Poster preview](figures/poster_preview.jpg)

## Data preparation

The saved notebook outputs describe **421,095 input records**. Preparation removes unused columns, maps state abbreviations to full names, converts employment length to numeric values, and maps loan grades to an ordinal scale.

- **Memory optimisation:** categorical and compact numeric types reduce recorded memory use from **226.6 MB to 80.4 MB**, then to **61.4 MB** after further mapping, about a **73% reduction** overall.
- **Outlier filtering:** annual income is filtered using bounds derived from the 5th and 95th percentiles, and records with debt-to-income ratio (DTI) of 41 or above are excluded.
- **Missing values:** employment titles and employment length have missing entries. Missing or unmatched job titles remain a limitation of the occupation analysis.

## Descriptive analysis and findings

Power BI visualisations examine loan purpose, funded amount, annual income, FICO score, grade, home ownership, state, and repayment status through state charts, scatter plots, distributions, and word clouds.

- **Loan purpose:** debt consolidation accounts for approximately **59%** of the loans in the exported purpose counts, followed by credit cards at approximately **24%** and home improvement at approximately **6%**. Together, these categories dominate the recorded borrowing purposes. See [`data/purposes.csv`](data/purposes.csv).
- **Geographical patterns:** the poster identifies California as having the largest loan count, followed by Texas and New York. Loan counts and charged-off proportions are shown separately to distinguish borrowing volume from repayment outcomes.
- **Borrower characteristics:** income, funded amount, FICO score, and grade are compared visually.

The [exported Power BI graphs](poster/descriptive_graphs.pdf) provide the supporting visualisations.

## Loan repayment prediction and feature importance

The prediction task distinguishes **Fully Paid** from **Charged Off** loans. Other statuses, including current loans, are excluded from model training. The notebook uses one-hot encoding, standardisation, and a scikit-learn `RandomForestClassifier` with `max_depth=10` and `random_state=1`.

| Feature importance | Correlation matrix |
|---|---|
| ![Feature importance](figures/feature_importance.png) | ![Correlation heatmap](figures/correlation_heatmap.png) |

- **Recorded cross-validation results:** stratified 5-fold cross-validation produces accuracies of **74.47–74.81%**, averaging approximately **74.65%**.
- **Feature importance:** the fitted forest assigns the highest importance to grade (**36.4%**), DTI (**15.2%**), FICO score (**11.5%**), annual income (**8.1%**), and the 60-month term indicator (**7.6%**). These are model impurity-based importance scores, not causal effects or changes in repayment probability.

## Job-title clustering and findings

The clustering notebook processes **395,318 job-title entries**, including repeated titles. It normalises title text, consolidates common abbreviations in the vocabulary, and converts titles to TF-IDF features before applying k-means.

An initial **38-cluster** solution is refined by manually merging related groups, separating business and banking titles, and reclustering the broad residual group. The resulting dictionary contains **40 categories, including “other”**. Examples include managers, nurses, engineers, teachers, technicians, and marketing/sales roles.


## Data

The raw dataset (accepted loans issued in 2015, originally published by LendingClub) is **not included** because of its size. The notebooks expect it as `data_accepted_2015.xlsx` in the working directory. Intermediate Excel files produced by the notebooks are also excluded.
