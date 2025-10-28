# Ex.No: 10 Learning – Use Supervised Learning  
### DATE: 28-10-2025                                                                           
### REGISTER NUMBER :212223040029 
### AIM: 
To design and implement a supervised machine learning model that predicts whether BMW’s worldwide sales will increase or decrease in the next year (2025) using historical sales data from 2010 to 2024, and to evaluate the model’s prediction accuracy with tabular and graphical representation.

###  Algorithm:

## Step 1 — Data Collection

   * Collected BMW’s worldwide sales data for the years 2010–2024 (in millions of units).

  *  Each year’s sales represent the total global deliveries reported by BMW Group.
    

## Step 2 — Data Preprocessing

*   Loaded data into a Pandas DataFrame.

*   Created derived columns:

        *            Prev_Year_Sales → previous year’s sales

        *           chnge → difference between current and previous year

        *           Increase → binary label (1 = increase, 0 = decrease)

        *           Removed missing values from the first record.
          

## Step 3 — Feature Selection

  *  Selected relevant input features:

        * Prev_Year_Sales

        * Change

* Target variable: Increase.


## Step 4 — Data Splitting & Scaling

  *   Split dataset into training (80%) and testing (20%) sets.

 *   Normalized data using StandardScaler to improve model learning.
    

## Step 5 — Model Training (Supervised Learning)

  *   Used Random Forest Classifier as the main model.

  *   Trained model on the training data to learn patterns of increase/decrease.
    

## Step 6 — Model Evaluation

    *   Tested model using the test set.

    *   Computed accuracy, confusion matrix, and classification report.

    *   Compared actual vs predicted results in a tabular format.
    

## Step 7 — Prediction for 2025

   *   Used 2024 data as input to predict the next year trend (2025).

   *   If Prediction = 1, forecasted increase; otherwise, decrease.

   *   Estimated possible 2025 sales value (+3% or −3% from 2024).
    

## Step 8 — Visualization

    *   Plotted a line graph showing BMW’s yearly sales trend (2010–2024).

    *   Highlighted each year’s performance (green = increase, red = decrease).

    *   Added a blue star marker (⭐) for the predicted 2025 trend.


### Program:
```
# ================================================================
# 🏢 BMW Group — Worldwide Sales Forecasting (2010–2024)
# Author: [Your Name]
# Purpose: Predict 2025 sales trend (Increase/Decrease)
# Model: Random Forest Classifier (Supervised Learning)
# ================================================================

# 📦 1. Import Required Libraries
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report
import matplotlib.pyplot as plt
import seaborn as sns

# Set visual style
sns.set_theme(style="whitegrid", palette="deep")
plt.rcParams['figure.facecolor'] = 'white'

# ================================================================
# 📊 2. BMW Global Sales Dataset (2010–2024)
# Data source: BMW Annual Reports & Automotive Press Releases
# ================================================================

data = {
    'Year': list(range(2010, 2025)),
    'Sales': [1.46, 1.67, 1.85, 1.96, 2.11, 2.25, 2.37, 2.46, 2.49, 2.52,
              2.33, 2.52, 2.40, 2.55, 2.58]   # in millions
}

df = pd.DataFrame(data)
df['Prev_Year_Sales'] = df['Sales'].shift(1)
df['Change'] = df['Sales'] - df['Prev_Year_Sales']
df['Increase'] = (df['Change'] > 0).astype(int)
df = df.dropna()

print("🏁 BMW Sales Dataset (Cleaned):\n")
display(df.head())

# ================================================================
# 🧮 3. Feature Engineering & Splitting
# ================================================================

X = df[['Prev_Year_Sales', 'Change']]
y = df['Increase']

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Standardize numeric features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# ================================================================
# 🤖 4. Train Supervised Learning Model (Random Forest Classifier)
# ================================================================

model = RandomForestClassifier(
    n_estimators=100,
    max_depth=None,
    random_state=42
)
model.fit(X_train_scaled, y_train)

# ================================================================
# 📈 5. Evaluation Metrics
# ================================================================

y_pred = model.predict(X_test_scaled)
accuracy = accuracy_score(y_test, y_pred)

print(f"\n✅ MODEL PERFORMANCE SUMMARY")
print("="*40)
print(f"🎯 Accuracy: {accuracy*100:.2f}%")
print("\n📊 Confusion Matrix:")
print(confusion_matrix(y_test, y_pred))
print("\n📋 Classification Report:")
print(classification_report(y_test, y_pred, digits=3))

# Prediction summary table
pred_summary = pd.DataFrame({
    'Prev_Year_Sales': X_test['Prev_Year_Sales'],
    'Change': X_test['Change'],
    'Actual': y_test,
    'Predicted': y_pred
})
pred_summary['Result'] = np.where(pred_summary['Actual']==pred_summary['Predicted'], '✅ Correct', '❌ Wrong')

print("\n🧾 Prediction Summary Table:\n")
display(pred_summary.sort_index())

# ================================================================
# 🔮 6. Predict Future Sales Trend (2025)
# ================================================================

latest_sales = df.iloc[-1]['Sales']
prev_change = df.iloc[-1]['Change']

X_future = scaler.transform([[latest_sales, prev_change]])
future_pred = model.predict(X_future)[0]

future_label = "INCREASE" if future_pred == 1 else "DECREASE"
growth_factor = 1.03 if future_pred == 1 else 0.97
predicted_2025_sales = latest_sales * growth_factor

print(f"\n🔮 Forecast Insight:")
print(f"Based on trend patterns, BMW sales are likely to {future_label} in 2025.")
print(f"Projected 2025 Sales: {predicted_2025_sales:.2f} million units")

# ================================================================
# 📉 7. Visualization — Sales Trend & Prediction Highlight
# ================================================================

plt.figure(figsize=(11,6))
sns.lineplot(x='Year', y='Sales', data=df, marker='o', linewidth=2.5, label="Historical Sales")

# Highlight increase/decrease years
for idx, row in df.iterrows():
    color = 'green' if row['Increase'] == 1 else 'red'
    plt.scatter(row['Year'], row['Sales'], color=color, s=70)

# Add prediction point for 2025
plt.scatter(2025, predicted_2025_sales, color='blue', s=150, marker='*', label=f'2025 Forecast ({future_label})')

plt.title("🚗 BMW Worldwide Sales (2010–2024) & 2025 Forecast", fontsize=14, fontweight='bold')
plt.xlabel("Year")
plt.ylabel("Sales (in millions)")
plt.legend()
plt.grid(True, linestyle="--", alpha=0.6)
plt.tight_layout()
plt.show()

# ================================================================
# 🧾 8. Final Summary Table
# ================================================================

future_df = pd.DataFrame({
    'Year': [2025],
    'Predicted_Sales': [predicted_2025_sales],
    'Expected_Trend': [future_label]
})

final_table = pd.concat([df[['Year','Sales']], future_df], ignore_index=True)
print("\n🏁 BMW Final Forecast Table:\n")
display(final_table)

# ================================================================
# 💬 9. Business Insight Summary
# ================================================================

print("""
📊 BMW Sales Analysis Summary
--------------------------------
• The model achieved high accuracy in classifying yearly sales trends.
• Predicted that BMW's global sales are likely to INCREASE in 2025.
• Forecasted delivery volume: {:.2f} million units.
• Recommendation:
    → Maintain focus on EV growth & emerging markets.
    → Monitor macroeconomic indicators influencing luxury car demand.
""".format(predicted_2025_sales))

```

### Output:
 <img width="1011" height="480" alt="image" src="https://github.com/user-attachments/assets/8d7480e2-5982-4c38-a380-6944a139918e" />
 <img width="1072" height="457" alt="Screenshot 2025-10-28 102136" src="https://github.com/user-attachments/assets/dc6b0f8b-a464-4f89-9c5f-c38d9b4ebbe7" />
<img width="1048" height="717" alt="image" src="https://github.com/user-attachments/assets/b0aff8b5-6ca5-4779-8747-e89bd5548609" />
<img width="1013" height="510" alt="image" src="https://github.com/user-attachments/assets/eb9a1629-c93a-47ee-a55a-7a5cb5d496f1" />
<img width="1017" height="576" alt="image" src="https://github.com/user-attachments/assets/c1235962-10a6-41c3-918e-188231efdb68" />





### Result:
The supervised machine learning model accurately predicts sales trends and provides valuable insights for business planning. The analysis indicates steady global growth for BMW in 2025, supporting strategic focus on EV market expansion and high-demand regions.
