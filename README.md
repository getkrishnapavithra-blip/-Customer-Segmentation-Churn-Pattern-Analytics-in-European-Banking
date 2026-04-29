import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

# Title
st.title("Customer Churn Analysis Dashboard")

# Load data
df = pd.read_excel("European_Bank.xlsx")

# Show dataset
st.subheader("Dataset Preview")
st.write(df.head())

# Overall churn
churn_rate = df["Exited"].mean()*100
st.metric("Overall Churn Rate (%)", round(churn_rate,2))

# Geography vs churn
geo = df.groupby("Geography")["Exited"].mean()*100

st.subheader("Churn Rate by Geography")

fig, ax = plt.subplots()
geo.plot(kind="bar", ax=ax)
st.pyplot(fig)
# Filter
gender = st.selectbox("Select Gender", df["Gender"].unique())

filtered_df = df[df["Gender"] == gender]

st.write(filtered_df)
st.header("Key Insights")

st.write("""
- Germany has highest churn (~32%)
- Age 46-60 shows highest churn
- Inactive customers churn more
- Single product users churn more
""")
age = df.groupby("Age")["Exited"].mean()*100

fig, ax = plt.subplots()
age.plot(ax=ax)
st.pyplot(fig)
active = df.groupby("IsActiveMember")["Exited"].mean()*100

fig, ax = plt.subplots()
active.plot(kind="bar", ax=ax)
st.pyplot(fig)
credit = st.number_input("Credit Score")
age = st.number_input("Age")

if st.button("Predict"):
    st.success("Prediction feature can be added here")
