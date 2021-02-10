# Salary-Prediction
Predict salary based on job descriptions

## Defining the problem
Job salaries and differences between them are determined by a number of various factors, including skillset, experience and the job title itself. 
Given the available datsets, we would like to estimate job salaries to understand key features driving salaries and deploy a model solution to predict salaries to gauge reasonable salaries based on these features. 

## Approach
**1. Data loading** 
- 'train_features': training dataset for each feature of each job ID: the job title, company, degree, major, industry, years of experience and distance from a metropolis (miles).
- 'train_salaries': training dataset of salaries (target variable) for each job ID  
- 'test_features': testing dataset equivalent of the feature train set.

**2. Data cleaning**
As well as finding the data types and size of each dataset, data cleaning involved discovering and treating for missing data, duplicates, invalid data (for example, salaries <= 0) and suspected outliers (observations outside 25 percentile -1.5 * Inter Quartile Range and 75 percentile -1.5 * Inter Quartile Range). 

Below is the summary of descriptive statistics for both the categorical and numeric data. 

Categorical data summary:
|        | jobId            | companyId   | jobType   | degree      | major   | industry   |
|:-------|:-----------------|:------------|:----------|:------------|:--------|:-----------|
| count  | 1000000          | 1000000     | 1000000   | 1000000     | 1000000 | 1000000    |
| unique | 1000000          | 63          | 8         | 5           | 9       | 7          |
| top    | JOB1362685349471 | COMP39      | Senior    | High School | None    | Web        |
| freq   | 1                | 16193       | 125886    | 236976      | 532355  | 143206     |

Numeric data summary: 
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
- 5 observations had salaries of zero. These are potentially missing salary inputs and were dropped given that we are predicting salaries. 

|        | jobId            | companyId   | jobType        | degree      | major       | industry   |   yearsExperience |   milesFromMetropolis |   salary |
|-------:|:-----------------|:------------|:---------------|:------------|:------------|:-----------|------------------:|----------------------:|---------:|
|  30559 | JOB1362684438246 | COMP44      | Junior         | Doctoral    | Math        | Auto       |                11 |                     7 |        0 |
| 495984 | JOB1362684903671 | COMP34      | Junior         | None        | None        | Oil        |                 1 |                    25 |        0 |
| 652076 | JOB1362685059763 | COMP25      | CTO            | High School | None        | Auto       |                 6 |                    60 |        0 |
| 816129 | JOB1362685223816 | COMP42      | Manager        | Doctoral    | Engineering | Finance    |                18 |                     6 |        0 |
| 828156 | JOB1362685235843 | COMP40      | Vice President | Masters     | Engineering | Web        |                 3 |                    29 |        0 |

- 7,117 observations were suspected upper-end outliers. No actions were taken to these observations, as they were reasonable with their tendencies to be higher-up positions and higher educated.

Example of five upper-end outliers 
|     | jobId            | companyId   | jobType        | degree   | major   | industry   |   yearsExperience |   milesFromMetropolis |   salary |
|----:|:-----------------|:------------|:---------------|:---------|:--------|:-----------|------------------:|----------------------:|---------:|
| 266 | JOB1362684407953 | COMP30      | CEO            | Masters  | Biology | Oil        |                23 |                    60 |      223 |
| 362 | JOB1362684408049 | COMP38      | CTO            | Masters  | None    | Health     |                24 |                     3 |      223 |
| 560 | JOB1362684408247 | COMP53      | CEO            | Masters  | Biology | Web        |                22 |                     7 |      248 |
| 670 | JOB1362684408357 | COMP26      | CEO            | Masters  | Math    | Auto       |                23 |                     9 |      240 |
| 719 | JOB1362684408406 | COMP54      | Vice President | Doctoral | Biology | Oil        |                21 |                    14 |      225 |



**3. Exploratory Data Analysis (EDA)**
For convenience, 'train_data' was define as the merging of training datasets after data cleaning. EDA was performed to better understand and visualise the data. While performing EDA, the numeric and categorical variables were defined as the following: 
- Numeric variables: i) Target variable ('salary') and ii) numeric features - 'yearsExperience' and 'milesFromMetropolis'. 
- Categorical features: 'companyId','jobType','degree','major' and 'industry'. 

Plot results: 

The correlation heatmap 


**4. Modelling**
A baseline model was created first as a benchmark to gauge the mean squared error (MSE). MSE is the selected model evaluation metric and all the modelling follows a 5-fold cross-validation. 

Table: baseline model | MSE

The following hypothesised solutions were designed to improve the results, i.e. lowering the MSE, with the best solutions tuned further in hopes of improving the models below a targeted MSE. Furthermore, one hot encoding was applied to the categorical feature variables ('companyId', 'jobType', 'degree', major' and 'industry') and normalisation was applied to the numeric feature variables ('yearsExperience', 'milesFromMetropolis') to further improve results. 

Table: Model | MSE

From the following vanilla models, the two models with the lowest MSEs were selected for tuning - Ridge regression and Gradient Boosting Regressor. 

Table: Ridge Regression alpha's | MSE

Different alphas did not significantly affect the Ridge regression results, with alpha = 10 showing the lowest MSE. 

Table: Gradient Boosting Regressors (n_estimators, max_depth) | MSE 

The Gradient Boosting Regressor with the lowest MSE is ... (is it less than the goal of MSE 360?) and the feature importance is: 

Plot feature importance
Table feature importance 

**5. Deploying**
This Gradient Boosting Regressor( ) is used to predict the test set. 
