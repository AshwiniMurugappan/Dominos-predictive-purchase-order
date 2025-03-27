<div align="center">

# 🍕 Dominos Predictive Purchase Order System

</div>

## 📈 Problem Statement

To improve Domino's ingredient management, this project aims to create a system that predicts pizza demand and automatically generates purchase orders. By analyzing past sales, we'll develop a model to accurately forecast future orders, ensuring optimal stock levels, reducing waste, and avoiding shortages.

---
## 🎯 Objective:

- Build a sales forecasting model for pizza orders.
- Design an automated purchase order system that determines ingredient needs based on the sales forecast.
  
---
## 🔍 Dataset Overview

The project involves two datasets: **Pizza Sales** and **Pizza Ingredients**. 

- The **Pizza Sales Dataset** comprises 48,620 entries, each detailing an individual sale. This includes information such as `pizza_id` (a unique identifier for the sale), `order_id` (linking to a specific order), `pizza_name_id` (a unique identifier for each pizza), `quantity` (number of pizzas sold), `order_date` and `order_time` (when the sale occurred), `unit_price` and `total_price` (pricing details), as well as `pizza_size` and `pizza_category` (size and type of pizza). This dataset offers a thorough view of sales, covering pricing, timing, and pizza characteristics. 

- The **Pizza Ingredients Dataset** consists of 518 entries that describe the ingredients for various pizzas. It includes `pizza_name_id` (a unique identifier for each pizza), `pizza_name` (name of the pizza), `pizza_ingredients` (list of ingredients), and `Items_Qty_In_Grams` (the quantity of each ingredient used). This dataset provides detailed insights into the composition of each pizza and the amounts of ingredients required.
---

## 💡 Business Use Cases

- ***Inventory Efficiency:*** Maintain precise stock levels to satisfy customer demand and prevent excess inventory.
- ***Financial Savings:*** Reduce losses from expired or surplus ingredients, maximizing profitability.
- ***Strategic Sales Planning:*** Utilize accurate sales forecasts to guide business decisions and promotional activities.
- ***Seamless Supply Chain:*** Optimize ingredient ordering to match predicted sales, ensuring uninterrupted operations.
---

## 🛠️ Approach

### I. Data Preprocessing 

**Data Cleaning** ensures the dataset's accuracy and consistency through:

- **Handling Missing Data**:
  - Detected missing values.
  - Replaced missing values using mean, median, mode, or placeholders.

- **Removing Inconsistent Data**:
  - Checked for format consistency and valid ranges.
  - Fixed inconsistencies, such as standardizing text and correcting typos.
 
### II. Exploratory Data Analysis (EDA)

**Exploratory Data Analysis (EDA)** discovers patterns, relationships, and anomalies in the data.

- Pizza size distribution, Pizza category distribution
- Daily Sales Trends
- Top 10 Most popular Pizzas
- Pie chart of Quantity ordered
- Pairplot of quantity, unit_price, total_price, Items_Qty_In_Grams
- Correlation Matrix

### III. Sales Prediction

Sales Prediction involves **Time Series Forecasting**, a technique used to predict future values based on historical data collected over time. The process includes the following steps:

#### i) Feature Engineering

Created new variables from the raw sales data to improve the model’s performance, such as:

- **Day of the Week**: Extracted the day of the week from the sales date to capture weekly variations.
- **Month**: Extracted the month from the sales date to account for monthly trends and seasonal patterns.
- **Holiday Effects**: Identified and included features for holidays or special events that can impact sales patterns.

#### ii) Model Selection

Model Selection involves choosing the most suitable forecasting model for our sales data:

- ARIMA (AutoRegressive Integrated Moving Average): Captures trends and autocorrelations in non-seasonal data.
- SARIMA (Seasonal ARIMA): Extends ARIMA to handle seasonality.

#### iii) Model Training

Model Training involves fitting the chosen model to historical sales data:

- Split the data into training and test sets to evaluate model performance. 
- Trained the model on the training set by adjusting parameters to minimize prediction errors.
- Optimized model performance by tuning hyperparameters using techniques like cross-validation or grid search.

### iv) 📊 Model Evaluation

### Pizza Sales by Week

- This process begins by aggregating pizza sales on a weekly basis, converting the `order_date` to a datetime format for accurate grouping.
- The data is then split into training (80%) and testing (20%) sets to prepare for model evaluation.
- The Mean Absolute Percentage Error (MAPE) function is defined to assess model performance by comparing actual and predicted values.

#### ARIMA Model Tuning

- The ARIMA model is tuned using a grid search over specified values of p, d, and q parameters to find the optimal configuration.
- The model forecasts sales for the test set, and the best MAPE score and corresponding parameters are printed.
- The predicted values are formatted for display, allowing for easy comparison with actual sales.
- Finally, a line plot visualizes the actual vs. predicted weekly sales, helping to evaluate the ARIMA model's forecasting performance.


#### Best SARIMA Model Training and Output

- The SARIMA model is trained with orders (1, 1, 3) for ARIMA and seasonal components.
- It forecasts sales for the test set, calculating the Mean Absolute Percentage Error (MAPE) for accuracy assessment.
- The best MAPE score is printed to evaluate model performance.
- Predictions are formatted for easy comparison with actual sales.
- A line plot visualizes actual vs. predicted weekly sales, aiding in performance evaluation.


#### Prophet Model Forecasting
- The order_date column is converted to datetime format and renamed to 'ds' for dates and 'y' for target values.
- The Prophet model is fitted to the prepared data, with US country holidays included for enhanced accuracy.
- Future dates for the next 7 days are generated to predict sales.
- The forecast results are displayed using Prophet's built-in plotting functionality.

#### Exponential Smoothing Model
- The Exponential smoothing gives more weight to recent sales data
- It uses itertools.product to generate all possible combinations of trend, seasonal, and seasonal_period values
- It converts the resulting ets_predictions to a Pandas Series with the correct index from the test data.
- The forecast results are displayed as ETS predictions.

---

### 🧩 Model Comparison: MAPE Scores

Performance Overview: The table below summarizes the Mean Absolute Percentage Error (MAPE) scores of different forecasting models, highlighting their ranking and performance.

| Model      | MAPE   | Rank | Best/Worst |
|------------|--------|------|------------|
| SARIMA     | 0.1841 | 1    | Best       |
| ETS        | 0.1889 | 2    |            |
| ARIMA      | 0.1982 | 3    |            |
| Prophet    | 0.2155 | 4    |            |


## Conclusion
The SARIMA model outperformed other models, providing the most accurate sales predictions, while Prophet yielded the least accurate results. This project lays the groundwork for improving inventory management through data-driven forecasting techniques.


### SARIMA Model Forecasting

This section demonstrates how to load the best-performing SARIMA model and use it to forecast future sales.

1. **Model Loading**: The SARIMA model, previously trained is used for forecasting.

2. **Forecasting**: The model predicts sales for the next 7 days (`n_forecast = 7`).

3. **Visualization**: A plot is generated to compare the training data with the forecasted sales, clearly illustrating expected future sales trends. The forecast is displayed in orange, while the training data is shown in blue.


### Predicted Ingredient Quantities

This section calculates the total quantity of ingredients needed based on predicted pizza sales for the upcoming week.

- 1. Mapping Predicted Sales: The Ingredients_dataset maps predicted sales to corresponding ingredients from the next_week_pizza_sales_forecasts_arima.

- 2. Calculating Total Quantity: The total quantity for each ingredient is computed by multiplying the grams needed per item (Items_Qty_In_Grams) by the predicted quantity of pizzas sold.

- 3. Summarizing Totals: The summed total quantities for each ingredient are displayed as a dictionary, giving a clear overview of the ingredient requirements for the upcoming week.

- 4. Saving Results: The ingredient totals are saved to a CSV file, predicted_ingredient_totals.csv, for future reference and easy sharing.


---

## 🏆 Results

The project delivers highly accurate pizza sales forecasts, providing precise predictions for the upcoming week. These forecasts enable better planning and optimized inventory management. Based on the predicted sales, a comprehensive purchase order is generated, detailing the exact quantities of each ingredient required. This ensures that the necessary ingredients are stocked efficiently, minimizing waste and avoiding shortages. The result supports seamless operations by aligning supply with demand, improving both supply chain efficiency and overall business performance.

---
