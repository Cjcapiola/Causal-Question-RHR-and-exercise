
# Relationship Between Cardiovascular Exercise and Resting Heart Rate: A Quantitative Analysis

## Abstract

Cardiovascular exercise is widely recognized for its beneficial effects on longevity and overall health, a view supported by recommendations from the Centers for Disease Control and Prevention (CDC), which advise adults aged 18 and older to engage in at least 150 minutes of moderate-intensity cardiovascular activity per week. However, these guidelines are based on broad epidemiological studies that may not account for individual variations. One potentially important biometric indicator of longevity and cardiovascular health is an individual's resting heart rate. Despite its significance, the direct relationship between the duration of moderate-intensity cardiovascular exercise and its impact on resting heart rate has not been quantified in the absence of confounding variables. This paper looks to define a relationship between time spent undergoing cardiovascular activity and resting heart rate in order to provide a more concrete biometric based recommendation for moderate-intensity cardiovascular exercise duration.  Using data sets from the National Health and Nutrition Examination Survey (NHANES) we will be able to extract resting heart rates and average duration of moderate-intensity cardiovascular exercise and define a relationship between the two that will enable an individual to use their current resting heart rate and generate a weekly cardiovascular duration that will allow them to lower their resting heart rate to a range that is optimal for longevity. The model provided a high notable R^2 value of 0.90, indicating a strong fit to the data. For an average person in our dataset, approximately 16.06 minutes of moderate-intensity cardiovascular exercise is estimated to be required to lower the resting heart rate from 74 to 68 beats per minute. The results suggest that individualized cardio duration recommendations could be more effective than general guidelines. Lowering the resting heart rate from 74 to 68 bpm is associated with a 28% reduction in all-cause mortality risk, making this a significant public health finding. Our study demonstrates the potential for data-driven, personalized health recommendations but should be interpreted with caution as it cannot replace professional medical advice.


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

The exact results of this model are shown:

**Cardio_Duration**: A negative coefficient suggests that as the duration of cardiovascular exercise increases, the change in heart rate is more likely to decrease (indicating a potentially lower resting heart rate). Specifically, for each additional minute of exercise, the change in heart rate is expected to decrease by approximately 0.17 units.
**Systolic_BP & Diastolic_BP**: These have small coefficients, suggesting a minor influence on the change in heart rate.
**Caffeine_Usage_mg**: A small positive coefficient suggests that increased caffeine usage could slightly increase the change in heart rate, but the effect is minimal.
**Fasting_Blood_Sugar**: A small positive coefficient indicates a minor impact on the change in heart rate.
**Smoking**: A positive coefficient of 0.13 indicates that smoking could result in a less negative or a more positive change in heart rate.
**Anti_Hypertensive**: A negative coefficient suggests that using anti-hypertensive medication is associated with a decrease in the change in heart rate, potentially indicating a lower resting heart rate.

To purify the model we can extract only the coefficents that provide a impactful change on the final variable and therefore simplify the model for ease of use.

- **Cardio_Duration**: \(-0.17\)
- **Systolic_BP**: \(0.0043\)
- **Diastolic_BP**: \(-0.002\)
- **Intercept**: \(8.72\)

Using these values we can use the model to predict changes in heart rate based on any given Blood Pressure and weekly cardio duration.

## Discussion

The \( R^2 \) value of 0.90 suggests that approximately 90% of the variability in the change in heart rate can be explained by the model. The negative coefficient for `Cardio_Duration` strongly suggests that longer durations of cardiovascular exercise are associated with a lower resting heart rate. However, due to the presence of confounding variables, the effect on RHR change could not be predicted purley based on cardio duration. Therefore, the model had to be refitted to include the impact of blood pressure as well. 

Here we can model how this new relationship can be used to assist an "average person".
Using the average values gleaned from the NHAMES data set, a representative average individual was created. To predict how much cardio the average person needs to do to bring down their resting heart rate from 74 to 68, we used our trained model. A reduction from a RHR > 72 to a RHR < 69 has been shown to decrease all-cause mortality risk by 28% based on epidemological data gathered from Zhao et al. 
Citation: Zhao MX, Zhao Q, Zheng M, Liu T, Li Y, Wang M, Yao S, Wang C, Chen YM, Xue H, Wu S. Effect of resting heart rate on the risk of all-cause death in Chinese patients with hypertension: analysis of the Kailuan follow-up study. BMJ Open. 2020 Mar 10;10(3):e032699. doi: 10.1136/bmjopen-2019-032699. PMID: 32161155; PMCID: PMC7066611.

``` python
# Code snippet to calculate required cardio for average person
required_change = 74 - 68  # 6 bpm
m = model.coef_[0]  # Coefficient for 'Cardio_Duration'
c = model.intercept_  # Intercept
required_cardio = (required_change - c) / m
```

The calculated 'required_cardio' gives us an estimate of the amount of cardio an average person needs to do to reduce their heart rate from 74 to 68. This 28% reduction in all-cause mortality risk represents a significant benefit, making the extra cardio a worthwhile investment in long-term health. According to the model, the average person would need to engage in approximately 
16.06 minutes of moderate-intensity cardiovascular exercise. The code for this calculation is shown below:

``` python
# Run the multiple linear regression
X = nhanes_filtered[['Cardio_Duration', 'Systolic_BP', 'Diastolic_BP', 'Caffeine_Usage_mg', 'Fasting_Blood_Sugar', 'Smoking', 'Anti_Hypertensive']]
y = nhanes_filtered['Change_in_Heart_Rate']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
model = LinearRegression()
model.fit(X_train, y_train)
# Calculate the average values for each variable to represent the 'average person'
average_person = nhanes_filtered.mean()
# Compute the required change in heart rate
required_change = 74 - 68  # 6 beats per minute
# Coefficient for 'Cardio_Duration' and intercept from the trained model
m = model.coef_[0]
c = model.intercept_
# Calculate required 'Cardio_Duration' for the average person to achieve the target heart rate change
required_cardio = (required_change - c) / m
required_cardio
```

## Conclusion

This study provides compelling evidence that the duration of cardiovascular exercise has a significant impact on an individual's resting heart rate. By combining careful statistical modeling with clinical insights, we can generate actionable recommendations that have the potential to significantly improve public health. Our model suggests that individualized cardio duration recommendations can be more effective than one-size-fits-all guidelines.

## Identifying Colliders
In the context of this study, a collider would be a variable that is influenced by both the "Cardio Duration" and "Change in Heart Rate."
For example, if "Fitness Level" is included as a variable in the model. "Fitness Level" could be a collider if both "Cardio Duration" and "Change in Heart Rate" influence it. That is, people who do more cardio are likely fitter, and fitter people generally have a different resting heart rate than less fit people. Colliders can be managed by removing them from the data set preventing any inclusion of possible bias. Conversly, the data could be stratifed until the colliders are segreagated, but in this model that strategy would result in a less precise estimate. Based on the avaliable NHAMES data set, this regression model is the most accurate predictor of how cardio duration affects resting heart rate.
