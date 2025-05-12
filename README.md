# Task_1-Data-Cleaning
Data cleaning code
import pandas as pd

# Load dataset
df = pd.read_csv("netflix.csv")

# Display the first few rows
print("Initial Dataset Preview:")
print(df.head())

# 1. Check and show missing values
print("\nMissing Values Before Cleaning:")
print(df.isnull().sum())

# 2. Handle missing values
# Drop rows with too many missing fields (optional: adjust strategy as needed)
df = df.dropna(subset=['title', 'date_added', 'country'])  # Drop rows missing key info

# 3. Remove duplicate rows
df = df.drop_duplicates()

# 4. Strip whitespace and standardize text fields
text_cols = ['type', 'title', 'director', 'cast', 'country', 'rating', 'duration', 'listed_in']
for col in text_cols:
    df[col] = df[col].astype(str).str.strip().str.lower()

# 5. Convert 'date_added' to datetime format
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')

# 6. Rename columns to lowercase with underscores
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')

# 7. Check and fix data types
df['release_year'] = df['release_year'].astype(int)

# Final check
print("\nMissing Values After Cleaning:")
print(df.isnull().sum())

print("\nData Types:")
print(df.dtypes)

# 8. Save the cleaned dataset
df.to_csv("netflix_cleaned.csv", index=False)
print("\nCleaning complete. Cleaned file saved as 'netflix_cleaned.csv'")
