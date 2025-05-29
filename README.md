import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import missingno as msno
import streamlit as st
import plotly.express as px

# First, load your data into a DataFrame
# Replace 'your_data_file.csv' with your actual data file path
df = pd.read_csv('your_data_file.csv')  # Add this line to load your data

# Display first few rows
print(df.head())
# Handling missing values
msno.matrix(df)  # Visualize missing data
df.fillna(df.mean(), inplace=True)  # Fill numeric missing values with mean
df.drop_duplicates(inplace=True)  # Remove duplicates

# Standardizing categorical data
df["product_category"] = df["product_category"].str.lower().str.replace(" ", "_")

# Convert date column to datetime format
df["sale_date"] = pd.to_datetime(df["sale_date"])
# Sales trends over time
plt.figure(figsize=(10, 5))
sns.lineplot(x="sale_date", y="sales_amount", data=df)
plt.title("Monthly Sales Trends")
plt.show()

# Sales by product category
plt.figure(figsize=(10, 5))
sns.barplot(x="product_category", y="sales_amount", data=df)
plt.xticks(rotation=45)
plt.title("Sales by Product Category")
plt.show()

# Store-wise sales analysis using pivot table
store_sales = df.pivot_table(index="store_location", values="sales_amount", aggfunc="sum")
print(store_sales.sort_values(by="sales_amount", ascending=False))
st.title("Sales Performance Analysis Dashboard")

# Regional Sales Distribution
fig = px.bar(df, x="store_location", y="sales_amount", title="Store-wise Sales Performance")
st.plotly_chart(fig)

# Seasonal Trends
