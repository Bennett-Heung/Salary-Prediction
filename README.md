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
- 7,117 observations were suspected upper-end outliers. No actions were taken to these observations, as they were reasonable with their tendencies to be higher-up positions and higher educated. 

The training datasets were merged after data cleaning for convenience, defined as 'train_data'.

**3. Exploratory Data Analysis (EDA)**
EDA was performed to better understand and visualise the data. While performing EDA, the numeric and categorical variables were defined as the following: 
- Numeric variables: i) Target variable ('salary') and ii) numeric features - 'yearsExperience' and 'milesFromMetropolis'
- Categorical features: 'companyId','jobType','degree','major' and 'industry'. 

Plot results: 

**4. Modelling**
