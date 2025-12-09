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

3- Data Cleaning and Preparation  

Data preprocessing will include the following steps:

- Convert all date fields to datetime format  
- Standardize numeric columns (fix “.” / “,” inconsistencies)  
- Handle missing values: 
  - Price: Forward-fill (use previous valid value)  
  - Purchase quantity: Assume `0` for non-purchase days  
- Normalize text fields (uppercase, stripped whitespace)

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









