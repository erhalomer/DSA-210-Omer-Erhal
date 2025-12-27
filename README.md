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

<h1>7. Machine Learning Methods and Results</h1>

<p>
This section presents the machine learning methods applied to the dataset in order to evaluate the predictive performance of key variables and to complement the statistical hypothesis testing results.
</p>

<h2>Supervised Learning: Sales Forecasting</h2>

<h3>Baseline Models</h3>

<table>
  <tr>
    <th>Baseline</th>
    <th>MAE</th>
    <th>RMSE</th>
  </tr>
  <tr>
    <td>Lag-1 (Yesterday = Today)</td>
    <td>1032.12</td>
    <td>1685.71</td>
  </tr>
  <tr>
    <td>Lag-30 (30 Days Ago = Today)</td>
    <td>1050.11</td>
    <td>1635.61</td>
  </tr>
</table>

<h3>ML Model Performance (Horizon = 1 Day)</h3>

<table>
  <tr>
    <th>Model</th>
    <th>MAE</th>
    <th>RMSE</th>
  </tr>
  <tr>
    <td>Gradient Boosting Regressor</td>
    <td><b>846.20</b></td>
    <td>1280.73</td>
  </tr>
  <tr>
    <td>Random Forest Regressor</td>
    <td>892.95</td>
    <td><b>1262.35</b></td>
  </tr>
  <tr>
    <td>Linear Regression</td>
    <td>1554.82</td>
    <td>2099.64</td>
  </tr>
</table>

<h3>ML Model Performance (Horizon = 30 Days)</h3>

<table>
  <tr>
    <th>Model</th>
    <th>MAE</th>
    <th>RMSE</th>
  </tr>
  <tr>
    <td>Random Forest Regressor</td>
    <td><b>739.17</b></td>
    <td><b>1024.80</b></td>
  </tr>
  <tr>
    <td>Gradient Boosting Regressor</td>
    <td>767.24</td>
    <td>1149.53</td>
  </tr>
  <tr>
    <td>Lag-30 Baseline</td>
    <td>976.12</td>
    <td>1636.97</td>
  </tr>
  <tr>
    <td>Linear Regression</td>
    <td>2648.29</td>
    <td>3398.05</td>
  </tr>
</table>

<h2>Unsupervised Learning: Customer Segmentation</h2>

<table>
  <tr>
    <th>Cluster</th>
    <th>Customers</th>
    <th>Avg Total Qty</th>
    <th>Avg Price (TRY)</th>
    <th>Avg Transactions</th>
    <th>Avg Active Days</th>
  </tr>
  <tr>
    <td>0</td>
    <td>54</td>
    <td>276.51</td>
    <td>27757.58</td>
    <td>3.56</td>
    <td>2.85</td>
  </tr>
  <tr>
    <td>1</td>
    <td>6</td>
    <td>5875.75</td>
    <td>26467.72</td>
    <td>81.17</td>
    <td>62.17</td>
  </tr>
  <tr>
    <td>2</td>
    <td>65</td>
    <td>456.80</td>
    <td>25039.80</td>
    <td>4.20</td>
    <td>3.57</td>
  </tr>
  <tr>
    <td>3</td>
    <td>20</td>
    <td>6435.52</td>
    <td>26081.48</td>
    <td>24.95</td>
    <td>22.95</td>
  </tr>
</table>

<p>
The machine learning results are consistent with the statistical hypothesis testing outcomes. Historical sales quantities provide strong predictive power, while price and purchase-related variables contribute limited additional improvement.
</p>









