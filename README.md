# Titanic Data Analysis

This project involves performing data cleaning and exploratory data analysis on the Titanic dataset. The goal is to handle missing values, detect and handle outliers, and visualize the data to gain insights.

## Table of Contents
- [Introduction](#introduction)
- [Data Dictionary](#data-dictionary)
- [Initial Observations](#initial-observations)
- [Data Cleaning and Feature Engineering](#data-cleaning-and-feature-engineering)
- [Visualizations](#visualizations)
- [Conclusion](#conclusion)
- [Acknowledgements](#acknowledgements)

## Introduction
The Titanic dataset provides information on the passengers aboard the Titanic, including whether they survived or not. This project focuses on cleaning the data, handling missing values, detecting and handling outliers, and visualizing the data to understand the key factors that influenced survival.

## Data Dictionary
| Variable  | Definition                          | Key                               |
|-----------|-------------------------------------|-----------------------------------|
| survival  | Survival                            | 0 = No, 1 = Yes                   |
| pclass    | Ticket class                        | 1 = 1st, 2 = 2nd, 3 = 3rd         |
| sex       | Sex                                 |                                   |
| Age       | Age in years                        |                                   |
| sibsp     | # of siblings / spouses aboard      |                                   |
| parch     | # of parents / children aboard      |                                   |
| ticket    | Ticket number                       |                                   |
| fare      | Passenger fare                      |                                   |
| cabin     | Cabin number                        |                                   |
| embarked  | Port of Embarkation                 | C = Cherbourg, Q = Queenstown, S = Southampton |

## Initial Observations
### Missing Values:
- **Age**: 714/891 values are present (~20% missing).
- **Cabin**: Only 204/891 values (~77% missing).
- **Embarked**: 889/891 values (~0.2% missing).

### Columns of Interest:
- **Survived**: Target variable.
- **Pclass, Sex, Age, SibSp, Parch, Fare, Embarked**: Potential predictors.
- **Cabin**: Too sparse; might be simplified to indicate presence or absence.

### Feature Types:
- **PassengerId and Name**: Likely identifiers, can be excluded.
- **Ticket**: Mostly unique, may need encoding or simplification.

## Data Cleaning and Feature Engineering
### Handle Missing Values:
- Fill **Age** with median or use predictive imputation.
- Encode **Cabin** as binary (HasCabin).
- Fill **Embarked** with the mode (S).
```python
# handle missing values in 'Age' column
df['Age'].fillna(df['Age'].median(), inplace=True)

# handle missing values in 'Embarked' column
df.Embarked.fillna(df.Embarked.mode()[0], inplace=True)

# Encode 'cabin' column as binary 
df['HasCabin'] = df['Cabin'].notnull().astype(int)

# drop 'Cabin' column
df.drop('Cabin', axis=1, inplace=True)

# convert the Age column to integer
df['Age'] = df['Age'].astype(int)
```

### Feature Engineering:
- Create **FamilySize** = SibSp + Parch.
- Create **Title** from Name.
- Bin **Age** and **Fare** into categories.
```python
# Feature Engineering
# Create 'FamilySize' column
df['FamilySize'] = df['SibSp'] + df['Parch']

# Extract 'Title' from 'Name' column
df['Title'] = df['Name'].str.extract(' ([A-Za-z]+)\.', expand=False)

# Bin 'Age' into categories
bins_age = [0, 12, 20, 40, 60, 80]
labels_age = ['Child', 'Teenager', 'Adult', 'Middle-Aged', 'Senior']
df['AgeBin'] = pd.cut(df['Age'], bins=bins_age, labels=labels_age)

# Bin 'Fare' into categories
bins_fare = [0, 7.91, 14.45, 31.0,512.33]
labels_fare = ['Low', 'Medium', 'High', 'Very High']
df['FareBin'] = pd.cut(df['Fare'], bins=bins_fare, labels=labels_fare)

```

### Encoding:
- One-hot encode **Sex** and **Embarked**.
- Convert **Pclass** to a categorical feature.

## Visualizations
- Boxplots to detect outliers in numerical columns.
- Histograms to visualize the distribution of `Age` and `Fare`.
- Count plots to visualize the distribution of categorical features like `Sex`, `Pclass`, and `Embarked`.

## Conclusion
This project involved cleaning the Titanic dataset by handling missing values and outliers, and performing feature engineering to prepare the data for further analysis. Visualizations were used to gain insights into the data and understand the key factors that influenced survival.

You can check out the full project on GitHub: [ FutureIntern_DA_01](https://github.com/codac-black/FutureIntern_DA_01.git)
## Acknowledgements
Special thanks to @Futureintern for inspiring this project.


#DataAnalysis #Python #Data Cleaning  #futureintern
