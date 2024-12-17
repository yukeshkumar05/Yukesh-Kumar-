# Yukesh-Kumar-

import pandas as pd
import matplotlib.pyplot as plt
import streamlit as st

# Title for the App
st.title("Personal Finance Analysis Tool")

# User Inputs Section
st.header("Input Your Financial Data")
income = st.number_input("Enter your monthly income (in $)", min_value=0.0, format="%.2f")
expense_input = st.text_area("Enter your expenses (format: category, amount)", 
                             "Example: Food, 200\nRent, 500\nEntertainment, 100")

# Button to process the data
if st.button("Analyze"):
    # Processing expense input data
    try:
        # Converting input text to DataFrame
        data = [line.split(",") for line in expense_input.split("\n") if line.strip()]
        df = pd.DataFrame(data, columns=["Category", "Amount"])
        df["Amount"] = pd.to_numeric(df["Amount"], errors="coerce")

        # Calculating total expenses and savings
        total_expense = df["Amount"].sum()
        savings = income - total_expense

        # Displaying Results
        st.subheader("Analysis Results")
        st.write(f"**Total Income:** ${income:.2f}")
        st.write(f"**Total Expense:** ${total_expense:.2f}")
        st.write(f"**Remaining Savings:** ${savings:.2f}")

        # Visualizing Expenses
        st.subheader("Expense Breakdown")
        if not df.empty:
            fig, ax = plt.subplots()
            ax.pie(df["Amount"], labels=df["Category"], autopct="%1.1f%%", startangle=90)
            ax.axis("equal")  # Equal aspect ratio ensures the pie is drawn as a circle.
            st.pyplot(fig)

        # Insights Section
        st.subheader("Insights")
        if savings < 0:
            st.error("You are overspending! Try to cut down on unnecessary expenses.")
        elif savings > 0:
            st.success(f"Good job! You have saved ${savings:.2f} this month. Keep it up!")
        else:
            st.info("You have perfectly balanced your income and expenses.")
    except Exception as e:
        st.error(f"Error processing input: {e}")
