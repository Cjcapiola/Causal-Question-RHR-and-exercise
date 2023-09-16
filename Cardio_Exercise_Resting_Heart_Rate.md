
# Unveiling the Relationship Between Cardiovascular Exercise and Resting Heart Rate: A Quantitative Analysis

## Abstract

This paper aims to quantitatively investigate the effect of the duration of moderate-intensity cardiovascular exercise on an individual's resting heart rate. Using data from the National Health and Nutrition Examination Survey (NHANES), a multiple linear regression model was developed to analyze this relationship while controlling for confounding variables. The study reveals a statistically significant negative correlation between exercise duration and resting heart rate.

## Introduction

### Background

Cardiovascular diseases remain a leading cause of mortality worldwide. Exercise, particularly of cardiovascular nature, has been endorsed for its protective effects against cardiovascular diseases. However, generalized exercise guidelines do not consider individual variability, such as resting heart rate, that could modulate the benefits of exercise.

### Objective

The primary objective is to establish a quantifiable relationship between the duration of moderate-intensity cardiovascular exercise and its impact on resting heart rate, considering potential confounding variables.

## Data Source and Preprocessing

### Data Collection

The data utilized in this analysis originates from the National Health and Nutrition Examination Survey (NHANES).

### Variables

1. **Dependent Variable**: Change in Heart Rate
2. **Independent Variables**: Cardio Duration, Age, Smoking, Systolic and Diastolic Blood Pressure, Caffeine Usage, Anti-Hypertensive medication, Fasting Blood Sugar

### Data Preprocessing

```python
# Code for data preprocessing
nhanes_data['Smoking'] = nhanes_data['Smoking'].map({'No': 0, 'Yes': 1})
nhanes_data['Anti_Hypertensive'] = nhanes_data['Anti_Hypertensive'].map({'No': 0, 'Yes': 1})
```

## Exploratory Data Analysis (EDA)

### Data Distribution

```python
# Histogram code
sns.histplot(nhanes_data['Cardio_Duration'], bins=30, kde=True)
```

### Pairwise Relationships

```python
# Pairplot code
sns.pairplot(nhanes_data[numerical_columns], kind='scatter', diag_kind='kde')
```

## Methodology

### Statistical Model

The model was implemented using Python's scikit-learn library.

#### Model Equation

\[
	ext{Change\_in\_Heart\_Rate} = \beta_0 + \beta_1 \times 	ext{Cardio\_Duration} + \beta_2 \times 	ext{Systolic\_BP} + \ldots
\]

```python
# Code to perform multiple linear regression
model = LinearRegression()
model.fit(X_train, y_train)
```

### Confounding Variables

- **Age**
- **Smoking**
- **Systolic and Diastolic Blood Pressure**
- **Caffeine Usage**
- **Anti-Hypertensive Medication**
- **Fasting Blood Sugar**

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
