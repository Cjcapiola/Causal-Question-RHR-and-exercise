
# Relationship Between Cardiovascular Exercise and Resting Heart Rate: A Quantitative Analysis

## Abstract

Cardiovascular exercise is widely recognized for its beneficial effects on longevity and overall health, a view supported by recommendations from the Centers for Disease Control and Prevention (CDC), which advise adults aged 18 and older to engage in at least 150 minutes of moderate-intensity cardiovascular activity per week. However, these guidelines are based on broad epidemiological studies that may not account for individual variations. One potentially important biometric indicator of longevity and cardiovascular health is an individual's resting heart rate. Despite its significance, the direct relationship between the duration of moderate-intensity cardiovascular exercise and its impact on resting heart rate has not been quantified in the absence of confounding variables. This paper looks to define a relationship between time spent undergoing cardiovascular activity and resting heart rate in order to provide a more concrete biometric based recommendation for moderate-intensity cardiovascular exercise duration.  Using data sets from the National Health and Nutrition Examination Survey (NHANES) we will be able to extract resting heart rates and average duration of moderate-intensity cardiovascular exercise and define a relationship between the two that will enable an individual to use their current resting heart rate and generate a weekly cardiovascular duration that will allow them to lower their resting heart rate to a range that is optimal for longevity. 
RESULTS
CONCLUSION


## Introduction

### Background

Cardiovascular diseases remain a leading cause of mortality worldwide. Exercise, particularly of cardiovascular nature, has been endorsed for its protective effects against cardiovascular diseases. However, generalized exercise guidelines do not consider individual variability, such as resting heart rate, that could modulate the benefits of exercise.

### Objective

The primary objective is to establish a quantifiable relationship between the duration of moderate-intensity cardiovascular exercise and its impact on resting heart rate, considering potential confounding variables.

## Data Source and Preprocessing

### Data Collection

The data utilized in this analysis originates from the National Health and Nutrition Examination Survey (NHANES). This data set was sourced from open access data provided via the researchers involved in the project. However, it contained a number of variables that can be considered confounding and also might exert an influence on changes in resting heart rate. Additionally, epidemiological data is hard to work with as variables are often presented in different ways either as discrete variables or as continos variables. These variables can also be considered as possible colliders. For example, someone who is training for an endurance race and therefore has a high duration of average weekly moderate-intensity cardio (Variable A) might have a decrease in resting heart rate (Variable B). However, it can be seen that someone who is attempting to run an endurance rate might also quit smoking (Variable C). A person with a high resting heart rate (Variable B) might also decide to quit smoking (Variable C) to improve their health. In this way, A can be said to be potentially causal on B and C while B can also be said to be potentiall causal on C making C a collider. As the NHANES data set is proceed, it will have to be adjusted for the presence of confounding variables and colliders.

The data set used for this project can be downloaded from the link below:

[Download NHANES Data Set](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/NHANES%20Data%20Set.xlsx)


### Variables

1. **Dependent Variable**: Change in Heart Rate
2. **Independent Variables**: Cardio Duration, Age, Smoking, Systolic and Diastolic Blood Pressure, Caffeine Usage, Anti-Hypertensive medication, Fasting Blood Sugar

### Data Preprocessing

The data set includes variables that are recorded as yes or no, for the purposes of easy data processing, these variables must be altered to reflect binary values that can be easily analyzed.

```python
# Code for data preprocessing
nhanes_data['Smoking'] = nhanes_data['Smoking'].map({'No': 0, 'Yes': 1})
nhanes_data['Anti_Hypertensive'] = nhanes_data['Anti_Hypertensive'].map({'No': 0, 'Yes': 1})
```

## Exploratory Data Analysis (EDA)

### Data Distribution

To provide a better understanding of what this data distibution looks like, the data can be plotted as histograms.
```python
# Histogram code
sns.histplot(nhanes_data['Cardio_Duration'], bins=30, kde=True)
sns.histplot(nhanes_data['Change_in_Heart_Rate'], bins=30, kde=True)
sns.histplot(nhanes_data['Smoking'], bins=30, kde=True)
sns.histplot(nhanes_data['Systolic_BP'], bins=30, kde=True)
sns.histplot(nhanes_data['Diastolic_BP'], bins=30, kde=True)
sns.histplot(nhanes_data['Caffeine_Usage_mg'], bins=30, kde=True)
sns.histplot(nhanes_data['Anti_Hypertensive'], bins=30, kde=True)
sns.histplot(nhanes_data['Fasting_Blood_Sugar'], bins=30, kde=True)	

```

![Cardio Duration](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Cardio_Duration_histogram.png)

![Change in Heart Rate](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Change_in_Heart_Rate_histogram.png)

![Smoking](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Smoking_histogram.png)

![Systolic BP](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Systolic_BP_histogram.png)

![Diastolic BP](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Diastolic_BP_histogram.png)

![Caffeine Usage (mg)](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Caffeine_Usage_mg_histogram.png)

![Anti-Hypertensive](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Anti_Hypertensive_histogram.png)

![Fasting Blood Sugar](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/Fasting_Blood_Sugar_histogram.png)

It can be seen that the values collected for Cardio Duration, Change in RHR, Blood Pressure, and Blood Sugar follow an approximatley normal distribution, while Caffiene Usage is heavily right tailed.

### Pairwise Relationships

By plotting each of the possible variables against RHR changes, we can visualize possible relationships between potential confounders and colliders.

```python
# Pairplot code
sns.pairplot(nhanes_data[Change in Heart Rate vs Smoking], kind='scatter', diag_kind='kde')
sns.pairplot(nhanes_data[Change in Heart Rate vs Systolic BP], kind='scatter', diag_kind='kde')
sns.pairplot(nhanes_data[Change in Heart Rate vs Diastolic BP], kind='scatter', diag_kind='kde')
sns.pairplot(nhanes_data[Change in Heart Rate vs Caffeine Usage (mg)], kind='scatter', diag_kind='kde')
sns.pairplot(nhanes_data[Change in Heart Rate vs Anti-Hypertensive], kind='scatter', diag_kind='kde')
sns.pairplot(nhanes_data[Change in Heart Rate vs Fasting Blood Sugar], kind='scatter', diag_kind='kde')

```

![Master Scatterplot](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/master_scatterplot_image.png)




## Methodology

In order to account for the impacts of the various confounding variables, a multivariable linear regression can be used to model how specifically changes in cardio duration affect RHR where all of the other variables can be accounted for using averages gleaned from the NHAMES data set.

#### Model Equation

Here is the equation that can be used to model the relationship.

\[
	ext{Change\_in\_Heart\_Rate} = \beta_0 + \beta_1 \times 	ext{Cardio\_Duration} + \beta_2 \times 	ext{Systolic\_BP} + \ldots
\]

```python
# Code to perform multiple linear regression
model = LinearRegression()
model.fit(X_train, y_train)
```

To determine the validity of our model equation we can plot the real values gathered from the data set and the predicted values set via the equation. The plot is shown below.

![Multivariable Regression Scatterplot](https://github.com/Cjcapiola/Causal-Question-RHR-and-exercise/raw/main/multivariable_regression_plot.png)

The statistcs of this equation model are shown:
Mean Squared Error (MSE): 1.55
R Squared Score: 0.90
The R Squared score is quite high, indicating a good fit of the model to the data. The model explains a large portion of the variance in the output variable, which in this case is the change in heart rate.

## Results

### Model Coefficients

- **Cardio_Duration**: \(-0.17\)
- **Systolic_BP**: \(0.0043\)
- **Diastolic_BP**: \(-0.002\)
- **Intercept**: \(8.72\)

```python
# Code snippet to extract coefficients
model.coef_, model.intercept_
```

### Model Evaluation

- **MSE**: \(1.55\)
- **\( R^2 \)**: \(0.90\)

```python
# Code to calculate MSE and R2
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
```

## Discussion

The \( R^2 \) value of 0.90 suggests that approximately 90% of the variability in the change in heart rate can be explained by the model. The negative coefficient for `Cardio_Duration` strongly suggests that longer durations of cardiovascular exercise are associated with a lower resting heart rate.

### Limitations and Future Work

1. **Unmeasured Confounders**: Factors like genetic predispositions were not considered.
2. **Causality**: The model establishes correlation but not causation.

## Conclusion

This study provides compelling evidence that the duration of cardiovascular exercise has a significant impact on an individual's resting heart rate.
