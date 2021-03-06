# Salary-Prediction
Predict salary based on job descriptions
In this repository: 
- 'Salary_Prediction (1)' is the notebook
- 'data.zip' contains the raw data used 
- 'test_salaries (1).zip' contains the predicted salaries of the test dataset.

## Defining the problem
Job salaries and differences between them are determined by a number of various factors, including skillset, experience and the job title itself. 
Given the available datsets, we would like to estimate job salaries to understand key features driving salaries and deploy a model solution to predict salaries to gauge reasonable salaries based on these features. 

## Approach

### 1. Data loading 

- 'train_features': training dataset for each feature of each job ID: the job title, company, degree, major, industry, years of experience and distance from a metropolis (miles).
- 'train_salaries': training dataset of salaries (target variable) for each job ID  
- 'test_features': testing dataset equivalent of the feature train set.

### 2. Data cleaning 

As well as finding the data types and size of each dataset, data cleaning involved discovering and treating missing data, duplicates, invalid data (for example, salaries <= 0) and suspected outliers. Lower outliers are outliers below the 25 percentile - 1.5 * Inter Quartile Range; and upper outliers are above the 75 percentile -1.5 * Inter Quartile Range. 

Below is the statistics summary for the categorical and numeric data. 

#### Categorical data summary
|        | jobId            | companyId   | jobType   | degree      | major   | industry   |
|:-------|:-----------------|:------------|:----------|:------------|:--------|:-----------|
| count  | 1000000          | 1000000     | 1000000   | 1000000     | 1000000 | 1000000    |
| unique | 1000000          | 63          | 8         | 5           | 9       | 7          |
| top    | JOB1362685349471 | COMP39      | Senior    | High School | None    | Web        |
| freq   | 1                | 16193       | 125886    | 236976      | 532355  | 143206     |

#### Numeric data summary: 
|                     |   yearsExperience |   milesFromMetropolis |   salary |
|:--------------------|------------------:|----------------------:|---------:|
| count               |           1e+06   |                1e+06  |   1e+06  |
| mean                |          11.9924  |               49.5293 | 116.062  |
| std                 |           7.21239 |               28.8777 |  38.7179 |
| min                 |           0       |                0      |   0      |
| 25%                 |           6       |               25      |  88      |
| 50%                 |          12       |               50      | 114      |
| 75%                 |          18       |               75      | 141      |
| max                 |          24       |               99      | 301      |
| lower_outlier_check |           0       |                0      |   1      |
| lower_outliers      |         -12       |              -50      |   8.5    |
| upper_outlier_check |           0       |                0      |   1      |
| upper_outliers      |          36       |              150      | 220.5    |

While no missing data, duplicates or invalid data were found, suspected outliers in terms of salary were found and explored further: 
- 5 observations had salaries of zero (below). These are potentially missing salary inputs and were dropped given that we are predicting salaries. 

|        | jobId            | companyId   | jobType        | degree      | major       | industry   |   yearsExperience |   milesFromMetropolis |   salary |
|-------:|:-----------------|:------------|:---------------|:------------|:------------|:-----------|------------------:|----------------------:|---------:|
|  30559 | JOB1362684438246 | COMP44      | Junior         | Doctoral    | Math        | Auto       |                11 |                     7 |        0 |
| 495984 | JOB1362684903671 | COMP34      | Junior         | None        | None        | Oil        |                 1 |                    25 |        0 |
| 652076 | JOB1362685059763 | COMP25      | CTO            | High School | None        | Auto       |                 6 |                    60 |        0 |
| 816129 | JOB1362685223816 | COMP42      | Manager        | Doctoral    | Engineering | Finance    |                18 |                     6 |        0 |
| 828156 | JOB1362685235843 | COMP40      | Vice President | Masters     | Engineering | Web        |                 3 |                    29 |        0 |

- 7,117 observations were suspected upper-end outliers (examples of these outliers are in table below). No actions were taken to treat these observations, as they were reasonable to their tendencies of being higher-up positions and higher educated. Note that these outliers were not tied to specific companies but were mostly found in the oil and finance industries, and most upper outliers majored Engineering and Business. 

|     | jobId            | companyId   | jobType        | degree   | major   | industry   |   yearsExperience |   milesFromMetropolis |   salary |
|----:|:-----------------|:------------|:---------------|:---------|:--------|:-----------|------------------:|----------------------:|---------:|
| 266 | JOB1362684407953 | COMP30      | CEO            | Masters  | Biology | Oil        |                23 |                    60 |      223 |
| 362 | JOB1362684408049 | COMP38      | CTO            | Masters  | None    | Health     |                24 |                     3 |      223 |
| 560 | JOB1362684408247 | COMP53      | CEO            | Masters  | Biology | Web        |                22 |                     7 |      248 |
| 670 | JOB1362684408357 | COMP26      | CEO            | Masters  | Math    | Auto       |                23 |                     9 |      240 |
| 719 | JOB1362684408406 | COMP54      | Vice President | Doctoral | Biology | Oil        |                21 |                    14 |      225 |


*Upper outliers: Salary by Company ID*

![upper_outliers_companyId_salary](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/upper_outliers_companyId_salary.png)

*Upper outliers: Salary by Job Type*

![upper_outliers_jobType_salary](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/upper_outliers_jobType_salary.png)

*Upper outliers: Salary by Degree*

![upper_outliers_degree_salary](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/upper_outliers_degree_salary.png)

*Upper outliers: Salary by Major*

![upper_outliers_major_salary](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/upper_outliers_major_salary.png)

*Upper outliers: Salary by Industry*

![upper_outliers_industry_salary](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/upper_outliers_industry_salary.png)


### 3. Exploratory Data Analysis (EDA) 

For convenience, 'train_data' was define as the merging of training datasets after data cleaning. EDA was performed to better understand and visualise the data. While performing EDA, the numeric and categorical variables were defined as the following: 
- Numeric variables: i) Target variable ('salary') and ii) numeric features - 'yearsExperience' and 'milesFromMetropolis'. 
- Categorical features: 'companyId','jobType','degree','major' and 'industry'. 

#### Numeric features
*Salary*

![EDA_salary](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/numeric_target_plots.png)

*Miles From Metropolis*

![EDA_milesFromMetropolis](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/numeric_feature_plotsmilesFromMetropolis.png)

*Years of Experience*

![EDA_yearsExperience](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/numeric_feature_plotsyearsExperience.png)

#### Categorical features
*Company ID*

![EDA_companyId](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/categorical_feature_plotscompanyId.png)

*Degree*

![EDA_degree](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/categorical_feature_plotsdegree.png)

*Industry*

![EDA_industry](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/categorical_feature_plotsindustry.png)

*Job Type*

![EDA_jobType](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/categorical_feature_plotsjobType.png)

*Major*

![EDA_major](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/categorical_feature_plotsmajor.png)

#### Correlations
The heatmap of correlations below provide the following findings of correlations between the features variables and salary (target variable), and other correlations amongst the features themselves.

In terms of correlations with salary (target variable):
- Job position (jobType) is the variable that positively correlates most to salary, followed by degree (degree), years of experience (yearsExperience) and major (major).
- There is also slight positive correlations between the job industry (industry) and salary.
- There is no significant correlation between companies (companyId) and salary.
- Only key negative correlation of salary is the miles from metropolis (milesFromMetropolis).

Between the feature variables:
- There is a significant positive correlation between major and degree (0.85).
- Job positions are slightly positively correlated to both major and degree.
- There are no other significant collinearities, as they are all close to zero.

*Correlations*

![corr_heatmap](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/corr_heatmap.png)

### 4. Modelling 

A baseline model was created first as a benchmark to gauge the mean squared error (MSE). MSE is the selected model evaluation metric and all the modelling follows a 5-fold cross-validation. 

| Model    | MSE      |
|----------|----------|
| Baseline |399.123   |

The following hypothesised solutions were designed to improve the results, i.e. lowering the MSE, with the best solutions tuned further in hopes of improving the models below a targeted MSE. Furthermore, one hot encoding was applied to the categorical feature variables ('companyId', 'jobType', 'degree', major' and 'industry') and normalisation was applied to the numeric feature variables ('yearsExperience', 'milesFromMetropolis') to further improve results. 

Vanilla models were applied to shortlist which models were to be tuned to provide a lower MSE (goal was set for a MSE below 360).  
![Vanilla_MSEs](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/vanilla_models_mse.png)

From the vanilla models, the two models with the lowest MSEs were selected for tuning - Ridge regression and Gradient Boosting Regressor. 

#### Ridge Regressions (alpha)

|                                   |   mse_mean    |
|:----------------------------------|--------------:|
| Ridge(alpha=0.01, random_state=0) |    384.431525 |
| Ridge(alpha=0.1, random_state=0)  |    384.431524 |
| Ridge(alpha=1, random_state=0)    |    384.431520 |
| Ridge(alpha=10, random_state=0)   |    384.431488 |

Different alphas did not significantly affect the Ridge regression results. The Ridge regression (alpha = 10) has the lowest MSE. 


#### Gradient Boosting Regressors (n_estimators, max_depth)

The Gradient Boosting Regressor (max_depth=7, n_estimators=150, random_state=0) has the lowest MSE of 356.899, which achieves the goal of a MSE less than 360.  

|                                                                          |   mse_mean |   mse_std |
|:-------------------------------------------------------------------------|-----------:|----------:|
| GradientBoostingRegressor(n_estimators=50, random_state=0)               |    440.705 |  0.959394 |
| GradientBoostingRegressor(n_estimators=100, random_state=0)              |    375.398 |  0.929363 |
| GradientBoostingRegressor(n_estimators=150, random_state=0)              |    363.24  |  0.916539 |
| GradientBoostingRegressor(n_estimators=200, random_state=0)              |    359.219 |  0.835095 |
| GradientBoostingRegressor(max_depth=7, n_estimators=50, random_state=0)  |    365.602 |  0.8275   |
| GradientBoostingRegressor(max_depth=7, n_estimators=100, random_state=0) |    357.258 |  0.716826 |
| GradientBoostingRegressor(max_depth=7, n_estimators=150, random_state=0) |    356.899 |  0.689418 |
| GradientBoostingRegressor(max_depth=7, n_estimators=200, random_state=0) |    357.054 |  0.662483 |
| GradientBoostingRegressor(max_depth=9, n_estimators=50, random_state=0)  |    360.88  |  0.901125 |
| GradientBoostingRegressor(max_depth=9,  n_estimators=100, random_state=0 |    357.991 |  0.704527 |
| GradientBoostingRegressor(max_depth=9, n_estimators=150, random_state=0) |    358.628 |  0.638715 |
| GradientBoostingRegressor(max_depth=9, n_estimators=200, random_state=0) |    359.329 |  0.59499  |

*Feature importance* 
![Feature_importances](https://github.com/Bennett-Heung/Salary-Prediction/blob/main/images/feature_importances.png)

The top three most important features are: the Janitor position, years of experience and miles from metropolis. Janitors having the highest feature importance is reasonable as the distribution of salaries for janitors are far less than the other job types, as indicated in the boxplot in the EDA section. Years of experience and miles from metropolis are the other top important features, which is reasonable to their respective visible linear relationships with salary.   


### 5. Deploying 

This Gradient Boosting Regressor (max_depth=7, n_estimators=150, random_state=0) is used to predict the test set. 
