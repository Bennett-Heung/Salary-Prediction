# Salary-Prediction
Predict salary based on job descriptions

## Defining the problem
Job salaries and differences between them are determined by a number of various factors, including skillset, experience and the job title itself. 
Given the available datsets, we would like to estimate job salaries to understand key features driving salaries and deploy a model solution to predict salaries to gauge reasonable salaries based on these features. 

## Approach
*1. Data loading* 
  - 'train_features': training dataset for each feature of each job ID: the job title, company, degree, major, industry, years of experience and distance from a metropolis (miles).
  - 'train_salaries': contains salaries (target variable) for each job ID  
  - 'test_features': testing dataset equivalent of the feature train set.

2. Data cleaning: 
