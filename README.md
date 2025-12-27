# DSA 210 Project Proposal  
 Forecasting Future Sales Using Company Transaction Data  

Name: Ömer erhal
ID: 31032

This project focuses on forecasting future sales using the company’s internal transaction records, which include both sales and purchase data 
The main goal is to analyze how historical trends, customer behavior, and operational factors such as stock intake and pricing influence future sales volumes.  
 the project seeks to answer:  
 “Can we predict future sales performance based on historical purchase and sales behaviors?”

The findings will support business planning, optimize inventory levels, and help the company better anticipate demand patterns in the iron and steel industry.  


---

1️- Project Motivation  
The purpose of this project is to use our company’s historical sales and purchase transaction data to forecast future sales trends and support data-driven decision-making. This analysis aims to improve inventory management and demand forecasting accuracy. This project provides an opportunity to apply the complete data science pipeline — data cleaning, analysis, hypothesis testing, and forecasting using a real industrial dataset.

---

2- Data Description  

The dataset consists of sales and purchase records obtained from the company’s internal systems.  
Because the data contains commercially sensitive information, the raw dataset cannot be shared publicly.  
Below are mock sample rows (non-real) for demonstration of the Data:

| Date | Customer | Product_Type | Quantity | Price |
|------|-----------|--------------|-----------|--------|
| 2024-06-15 | Customer_A | THICK | 1200 | 14500 |
| 2024-06-16 | Customer_B | THIN | 800 | 14850 |

| Date | Supplier | Product_Type | Quantity | Price |
|------|-----------|--------------|-----------|--------|
| 2024-06-14 | Supplier_X | KÜTÜK | 1500 | 9200 |
| 2024-06-17 | Supplier_Y | KÜTÜK | 2000 | 9400 |

---

3- Data Cleaning and Preparation and Graphs

Data preprocessing will include the following steps:

- Convert all date fields to datetime format  
- Standardize numeric columns (fix “.” / “,” inconsistencies)  
- Handle missing values: 
  - Price: Forward-fill (use previous valid value)  
  - Purchase quantity: Assume `0` for non-purchase days  
- Normalize text fields (uppercase, stripped whitespace)
- <img width="751" height="451" alt="image" src="https://github.com/user-attachments/assets/554b1fd5-fe64-4bcd-8251-6fd6b2ac2663" />
The boxplot of sales quantities indicates that the majority of transactions are concentrated below approximately 400–500 Tons, with the median located close to this lower range. In contrast, several observations exceed 1,000 units, and a small number of extreme values reach levels above 4,000 units. This substantial gap between typical sales volumes and maximum observed values demonstrates a strongly right-skewed distribution. Such a distribution is consistent with the iron and steel industry, where frequent low-volume sales coexist with occasional high-volume bulk orders, and therefore does not indicate a structural problem in the dataset.
<img width="751" height="451" alt="image" src="https://github.com/user-attachments/assets/bc197bfa-4c6c-4161-a46b-e6968a268ded" />

The boxplot of sales prices shows that the majority of transactions are concentrated within a narrow range of approximately 25,000 to 30,000 TRY, with the median located inside this interval. This indicates a relatively stable pricing structure for most sales. However, a small number of extreme outliers are observed, including one transaction exceeding 240,000 TRY and another close to zero. These values likely correspond to exceptional transactions or special pricing cases and do not represent the typical sales price level. Overall, the distribution is right-skewed and consistent with pricing behavior in the iron and steel industry.
<img width="752" height="452" alt="image" src="https://github.com/user-attachments/assets/29be2672-768a-4734-93ed-38a7f8a768cb" />
The scatter plot of sales price versus sales quantity shows that most transactions are concentrated within a narrow price range of approximately 22,000 to 32,000 TRY across a wide range of quantities, extending up to around 1,500 Tons. No clear upward or downward trend is observed, and the data points form a largely horizontal cloud. Although a small number of extreme outliers are present, these observations are rare and do not affect the overall pattern. This visual evidence supports the statistical finding of a very weak and insignificant relationship between sales price and sales quantity.





---

4- Research Questions  

1. Do past sales quantities contain predictive information about future sales?  
2. Do changes in sales price significantly affect sales volume?  
3. Does the purchase (stock intake) quantity affect sales performance after a lag period?
4. Are there seasonal or periodic sales patterns throughout the year?
5. How does the USD/TRY exchange rate affect the company’s sales volume and sales prices over time?
---

5️- Hypotheses  

H₀₁: Past sales quantities have no significant relationship with future sales.  
H₁₁: Past sales quantities significantly affect future sales.

H₀₂: Purchase quantities do not affect future sales.  
H₁₂: Purchase quantities significantly influence future sales.

H₀₃: Sales price changes have no impact on sales volume.  
H₁₃: Sales price changes significantly affect sales volume.

2. Data Collection

The dataset consists of two internal sources:

- Company sales transaction records  
- Company purchase (KÜTÜK) transaction records  

Additionally, external macroeconomic data was collected:

- Daily USD/TRY exchange rate history (2024–2025)

Raw data cannot be shared publicly. Below are mock samples showing the structure of the dataset.

Sales Data (Mock Example)

Sales Data (Mock Example)

| Date       | Customer    | Product_Type | Quantity | Price |
|------------|-------------|--------------|----------|--------|
| 2024-06-15 | Customer_A  | THICK        | 1200     | 14500 |
| 2024-06-16 | Customer_B  | THIN         | 800      | 14850 |

Purchase Data (Mock Example)

| Date       | Supplier    | Product_Type | Quantity | Price |
|------------|-------------|--------------|----------|--------|
| 2024-06-14 | Supplier_X  | SCRAP        | 1500     | 9200  |
| 2024-06-17 | Supplier_Y  | SCRAP        | 2000     | 9400  |

Merged Sales + USD/TRY Exchange Rate Data (Mock Example)

| Tarih       | Daily_Quantity (ton) | Daily_Price (TL/ton) | USD_TRY |
|-------------|-----------------------|------------------------|---------|
| 2024-11-01  | 5728.0                | 26528.57               | 34.3020 |
| 2024-11-02  | 137.5                 | 26600.00               | 34.3020 |
| 2024-11-04  | 145.0                 | 26600.00               | 34.3337 |
| 2024-11-05  | 425.0                 | 26425.00               | 34.3118 |
| 2024-11-06  | 357.5                 | 25757.14               | 34.1862 |


Data Cleaning Steps

- Converted all date fields into datetime format  
- Corrected numeric formatting (decimal inconsistencies)  
- Normalized text fields (capitalization, trimming)  
- Forward-filled missing sales prices    
- Created daily and monthly aggregated tables  
- Merged sales data with USD/TRY exchange rate  
- Corrected an outlier entry (114000 tons → 114 tons)

---

4. Exploratory Data Analysis (EDA)

Key findings from EDA:

- Daily sales are irregular but show a weak monthly cycle  
- İNCE product type dominates total sales  
- TRY-based sale prices steadily increase over time  
- TRY prices closely follow USD/TRY movements  
- No visible relationship between price and sales quantity  
- No visible relationship between purchases and sales  
- Weak monthly seasonality exists (supports lag-30 hypothesis)

These observations guided the hypothesis tests below.

---

5. Hypothesis Testing

All hypotheses were evaluated using linear regression, Pearson correlation, and p-value significance testing.

---

Hypothesis 1: Past Sales → Future Sales

Null Hypothesis (H01): Past sales quantities have no relationship with future sales.  
Alternative Hypothesis (H11): Past sales quantities significantly affect future sales.

Test Results:

| Lag | Correlation | R2 | p-value | Interpretation |
|-----|-------------|----|---------|----------------|
| 7-day lag  | -0.1192 | 0.0142 | 0.0609 | Not significant |
| 21-day lag | -0.0049 | 0.0000 | 0.9403 | No relationship |
| 30-day lag | 0.1374  | 0.0189 | 0.0395 | Significant |

Conclusion:  
Only the 30-day lag is significant.  
Past monthly sales help predict future sales.

---

Hypothesis 2: Purchases → Future Sales

Null Hypothesis (H02): Purchase quantities do not affect future sales.  
Alternative Hypothesis (H12): Purchase quantities significantly affect future sales.

Daily Purchases → Future Sales:  
Correlation = -0.0515, p = 0.4193 (not significant)

Monthly Purchases → Next Month Sales:  
Correlation = 0.3296, p = 0.3223 (not significant)

Conclusion:  
Purchases do not influence sales performance.

---

 Hypothesis 3: Price → Sales Quantity

Null Hypothesis (H03): Sales price does not affect sales quantity.  
Alternative Hypothesis (H13): Sales price significantly affects sales quantity.

Results:  
Correlation = -0.0180  
p-value = 0.4913 (not significant)

Conclusion:  
Customers are not price-sensitive.

---

 Hypothesis 4a: USD/TRY → Sales Price

Null Hypothesis (H04a): USD/TRY changes do not affect sales price.  
Alternative Hypothesis (H14a): USD/TRY significantly affects sales price.

Results:  
Correlation = 0.3000  
p-value = 0.000001 (significant)

Conclusion:  
TRY prices are strongly influenced by the USD/TRY exchange rate.

---

Hypothesis 4b: USD/TRY → Sales Quantity

Null Hypothesis (H04b): USD/TRY has no effect on sales quantity.  
Alternative Hypothesis (H14b): USD/TRY significantly affects sales quantity.

Results:  
Correlation = -0.0746  
p-value = 0.2354 (not significant)

Conclusion:  
Sales volume is not affected by the exchange rate.

---

6. Summary of Findings

- The only meaningful predictor of future sales is the past 30-day sales volume  
- Price does not affect quantity  
- Purchases do not affect sales  
- USD/TRY strongly affects TRY prices  
- USD/TRY does not affect quantity  
- Weak monthly seasonality exists

7. Machine Learning Methods and Results

This section presents the machine learning methods applied to the dataset in order to evaluate the predictive performance of key variables and to complement the statistical hypothesis testing results.

Supervised Learning: Sales Forecasting

Supervised regression models were used to forecast future daily sales quantities based on historical transaction data. The feature set includes lagged sales quantities, rolling averages, daily average prices, purchase quantities, and calendar-based variables. To prevent data leakage, all models were evaluated using 5-fold TimeSeriesSplit cross-validation.

Baseline Models
Baseline	MAE	RMSE
Lag-1 (Yesterday = Today)	1032.12	1685.71
Lag-30 (30 Days Ago = Today)	1050.11	1635.61

These baselines represent simple forecasting strategies without any learning mechanism and serve as reference points for model comparison.

ML Model Performance (Horizon = 1 Day)
Model	MAE	RMSE
Gradient Boosting Regressor	846.20	1280.73
Random Forest Regressor	892.95	1262.35
Linear Regression	1554.82	2099.64

For one-day-ahead forecasting, ensemble-based models significantly outperform the baseline approaches. Gradient Boosting achieves the lowest MAE, while Random Forest produces the lowest RMSE.

ML Model Performance (Horizon = 30 Days)
Model	MAE	RMSE
Random Forest Regressor	739.17	1024.80
Gradient Boosting Regressor	767.24	1149.53
Lag-30 Baseline	976.12	1636.97
Linear Regression	2648.29	3398.05

For longer-term forecasting, Random Forest demonstrates the best predictive performance, improving MAE by approximately 24% compared to the Lag-30 baseline.

Unsupervised Learning: Customer Segmentation

To explore structural patterns in customer purchasing behavior, K-Means clustering (k = 4) was applied to customer-level aggregated features, including total sales quantity, average price, number of transactions, and number of active days.

Cluster Summary
Cluster	Customers	Avg Total Qty	Avg Price (TRY)	Avg Transactions	Avg Active Days
0	54	276.51	27757.58	3.56	2.85
1	6	5875.75	26467.72	81.17	62.17
2	65	456.80	25039.80	4.20	3.57
3	20	6435.52	26081.48	24.95	22.95

The clustering results reveal distinct customer segments ranging from small-volume, infrequent buyers to high-volume, highly active customers. This segmentation provides additional insight into demand structure and customer behavior.
   









