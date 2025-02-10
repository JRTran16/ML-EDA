# Gen-Z Dating App Data Analysis

This project focuses on analyzing dating app usage patterns among Gen-Z users in India. It involves data cleaning, preprocessing, and visualization to uncover trends related to age, gender, location, and app preferences.

---

## Project Overview

This analysis explores:
- Age and gender activity on dating apps
- Differences between urban and rural users
- Popularity trends of dating apps over time
- Representation and inclusivity within the dataset

---

## Dataset Description

The dataset includes the following key features:

- **User_ID**: Unique identifier for each participant
- **Age**: Age of the user in years
- **Gender**: Gender identity of the user (Male, Female, Non-binary)
- **Location**: City where the user resides (e.g., Delhi, Mumbai)
- **Primary_App**: Main dating app used (Tinder, Bumble, etc.)
- **Secondary_Apps**: Other apps the user engages with
- **Daily_Usage_Time**: Time spent daily on dating apps (e.g., '1 hour', '30 minutes')
- **Satisfaction**: Self-reported satisfaction level with dating apps (scale of 1-10)

---

## Data Preprocessing

### 1. Handling Categorical Variables

We applied **One-Hot Encoding** to transform categorical variables into numeric format:
```python
from sklearn.preprocessing import OneHotEncoder

categorical_columns = ['Gender', 'Location', 'Education', 'Occupation', 'Primary_App', 'Secondary_Apps',
                       'Usage_Frequency', 'Reason_for_Using', 'Challenges', 'Desired_Features',
                       'Preferred_Communication', 'Partner_Priorities']

one_hot_encoder = OneHotEncoder(sparse_output=False, drop='first')
encoded_categorical = one_hot_encoder.fit_transform(date[categorical_columns])
```

### 2. Normalizing Numerical Variables

We normalized numerical features using **MinMaxScaler**:
```python
from sklearn.preprocessing import MinMaxScaler

# Convert Daily_Usage_Time to numerical values
date['Daily_Usage_Time'] = date['Daily_Usage_Time'].replace({
    '30 minutes': 0.5, '1 hour': 1, '1.5 hours': 1.5, '2 hours': 2, '3 hours': 3
})

numerical_columns = ['Age', 'Daily_Usage_Time', 'Satisfaction']
scaler = MinMaxScaler()
normalized_numerical = scaler.fit_transform(date[numerical_columns])
```

### 3. Feature Engineering

We created a new feature **Active_App_Count** to represent the number of apps each user actively uses:
```python
date['Active_App_Count'] = date.apply(lambda row: 2 if row['Primary_App'] != 'None' and row['Secondary_Apps'] != 'None'
                                      else 1 if row['Primary_App'] != 'None' or row['Secondary_Apps'] != 'None'
                                      else 0, axis=1)
```

---

## Visualizations

### 1. Age Distribution Across Genders
```python
sns.histplot(data=date, x='Age', hue='Gender', multiple='stack')
```

### 2. Dating App Usage Over Age Groups
```python
age_app_popularity = date.groupby(['Age_Group', 'Primary_App']).size().reset_index(name='User_Count')

sns.scatterplot(data=age_app_popularity, x='Age_Group', y='Primary_App', size='User_Count', sizes=(50, 500))
```

---

## Key Findings

1. **Younger Gen-Z Users (18-22)** are slightly more active on dating apps compared to older users.
2. **OkCupid** and **Bumble** are more popular among younger age groups, while **Hinge** gains popularity among users aged 23-26.
3. The dataset is **urban-heavy**, with minimal representation from rural users.
4. **Non-binary** and **older Gen-Z (27-30)** users are underrepresented.

---

## Ethical Considerations

- **Representation Bias**: The dataset primarily includes urban, younger Gen-Z users, limiting the generalizability of findings.
- **Privacy**: All user data was anonymized to maintain confidentiality.
- **Inclusivity**: The dataset could benefit from more diverse representation of rural and LGBTQ+ users.

---


## Contributing
Feel free to fork the repository, make changes, and submit a pull request! We welcome contributions to improve the analysis or expand the dataset.


** Written by** : JOHN RAY TRAN
