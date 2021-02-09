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

While no missing data, duplicates or invalid data were found, suspected outliers were found and explored further for the salary values: 
- 5 observations had salaries of zero. These are potentially missing salary inputs and were dropped given that we are predicting salaries. 

Plot table of 5

- 7,117 observations were suspected upper-end outliers. No actions were taken to these observations, as they were reasonable with their tendencies to be higher-up positions and higher educated.

Plot table of head for 7,117, and line charts


**3. Exploratory Data Analysis (EDA)**
For convenience, 'train_data' was define as the merging of training datasets after data cleaning. EDA was performed to better understand and visualise the data. While performing EDA, the numeric and categorical variables were defined as the following: 
- Numeric variables: i) Target variable ('salary') and ii) numeric features - 'yearsExperience' and 'milesFromMetropolis'
- Categorical features: 'companyId','jobType','degree','major' and 'industry'. 

Plot results: 

| jobId            | companyId   | jobType   | degree      | major   | industry   |\n+========+==================+=============+===========+=============+=========+============+\n| count  | 1000000          | 1000000     | 1000000   | 1000000     | 1000000 | 1000000    |\n+--------+------------------+-------------+-----------+-------------+---------+------------+\n| unique | 1000000          | 63          | 8         | 5           | 9       | 7          |\n+--------+------------------+-------------+-----------+-------------+---------+------------+\n| top    | JOB1362685349471 | COMP39      | Senior    | High School | None    | Web        |\n+--------+------------------+-------------+-----------+-------------+---------+------------+\n| freq   | 1                | 16193       | 125886    | 236976      | 532355  | 143206     |


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
