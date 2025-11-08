# Which-country-or-city-has-a-higher-salary-or-score-
Which country or city has a higher salary or score? ,Python, Data cleaning, Corelation
_This analysis explores the relationships between key variables — Age, Rating, Established Year, and Average Salary (Salary_mid) — to identify any meaningful patterns in the data.
The correlation matrix shows that there are no strong linear relationships among these variables. Age and salary have almost no correlation, meaning age does not influence pay levels. The only notable finding is a weak negative correlation between Company Rating and Salary (r = -0.34), suggesting that higher-rated companies might offer slightly lower salaries, possibly compensating employees through other benefits or work culture.
Overall, the data indicates that salary levels are mostly independent of company age, employee age, or rating scores, showing a balanced and diverse structure within the dataset.___________________________
# DATA CLEANING PIPELINE
# ======================================================
# 1️⃣ Import Libraries
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# ------------------------------------------------------
# 2️⃣ Load Dataset
# ------------------------------------------------------
df = pd.read_csv(r'C:\Users\User\OneDrive\Desktop\data\cleaned_data.csv')

# Quick look
df.head()
df.dtypes

# ------------------------------------------------------
# 3️⃣ Clean 'Easy Apply' Column
# ------------------------------------------------------
# Convert all values to string for consistency
df['Easy Apply'] = df['Easy Apply'].astype(str)

# Map TRUE → True, all other values → False
df['Easy Apply'] = df['Easy Apply'].apply(lambda x: True if x.upper() == 'TRUE' else False)

# Check results
df['Easy Apply'].unique()
df.head()

# ------------------------------------------------------
# 4️⃣ Explore 'Established' Column
# ------------------------------------------------------
# Check unique values and missing ratio
df['Established'].unique()
df['Established'].isna().mean() * 100

# Calculate how many '-1' values exist
(df['Established'] == -1).mean() * 100

# ------------------------------------------------------
# 5️⃣ Check Missing Data in All Columns
# ------------------------------------------------------
df.isna().mean() * 100
# Expected:
# Index           0.000000
# Age            24.137931
# Salary          0.000000
# Rating          3.448276
# etc.

# ------------------------------------------------------
# 6️⃣ Handle Missing Data
# ------------------------------------------------------

# (a) Drop rows where Rating is NaN
df = df.dropna(subset=['Rating'])

# (b) Fill missing Age values with median
df['Age'] = df['Age'].fillna(df['Age'].median())

# (c) Replace -1 with NaN in 'Established'
df['Established'] = df['Established'].replace(-1, np.nan)

# (d) Fill missing 'Established' values with median
df['Established'] = df['Established'].fillna(df['Established'].median())

# ------------------------------------------------------
# 7️⃣ Convert 'Established' to Integer
# ------------------------------------------------------
# (1999.0 → 1999)
df['Established'] = df['Established'].astype(int)

# ------------------------------------------------------
# 8️⃣ Final Checks
# ------------------------------------------------------
print("✅ Missing Value Percentages:")
print(df.isna().mean() * 100)

print("\n✅ Data Types:")
print(df.dtypes)
df.head()

# "$" ve "k" karakterlerini kaldır
df['Salary'] = df['Salary'].str.replace('$', '', regex=False)
df['Salary'] = df['Salary'].str.replace('k', '', regex=False)
# Örneğin "44-99" → iki sütuna ayır
df[['Salary_min', 'Salary_max']] = df['Salary'].str.split('-', expand=True)
df['Salary_min'] = pd.to_numeric(df['Salary_min'], errors='coerce')
df['Salary_max'] = pd.to_numeric(df['Salary_max'], errors='coerce')
df['Salary_mid'] = (df['Salary_min'] + df['Salary_max']) / 2
df.head()

<img width="628" height="646" alt="111" src="https://github.com/user-attachments/assets/a074d28a-8262-45ee-9d91-15f9ffdd4b53" />
corr = df[['Age', 'Rating', 'Established', 'Salary_mid']].corr()
plt.figure(figsize=(8,6))
sns.heatmap(corr, annot=True, cmap='coolwarm')
plt.title('Korelasyon Matrisi (Age, Rating, Established, Salary_mid)')
plt.show()
<img width="837" height="647" alt="22" src="https://github.com/user-attachments/assets/47b3ec10-f692-404c-b7c6-ae6aff5314a0" />




