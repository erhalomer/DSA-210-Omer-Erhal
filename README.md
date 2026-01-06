# DSA 210 Project 
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
| Lag | Correlation | p-value | Interpretation |
|-----|-------------|---------|----------------|
| 7-day lag  | -0.1026 | 0.1093 | Not significant |
| 21-day lag | -0.0231 | 0.7266 | Not significant |
| 30-day lag | 0.0475  | 0.4809 | Not significant |

Conclusion:  
None of the tested daily lag values show a statistically significant linear relationship with future sales.


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

-  Daily lagged sales quantities do not show statistically significant linear relationships with future sales
- Price does not affect quantity  
- Purchases do not affect sales  
- USD/TRY strongly affects TRY prices  
- USD/TRY does not affect quantity  
- Weak monthly seasonality exists

 7. Machine Learning Methods

To complement statistical analysis, supervised machine learning methods covered in the course were applied:

- Decision Tree Regression
- k-Nearest Neighbors (k-NN) Regression
- Random Forest Regression

Input features included lagged sales quantities (1, 7, and 30 days), daily average sales price, and daily purchase quantity.

Models were evaluated using **TimeSeriesSplit (5-fold cross-validation)**. Performance was assessed using **Average Prediction Error**, defined as the average absolute difference between predicted and actual sales values.

---

 8. Machine Learning Results

Horizon = 1 Day

| Model | Average Prediction Error |
|----|----|
| Lag-1 Baseline | 1032.12 |
| Lag-30 Baseline | 1050.11 |
| Decision Tree | 853.25 |
| k-NN | 843.78 |
| **Random Forest** | **767.25** |

Random Forest achieves the best short-term forecasting performance, substantially improving upon both baseline rules and the single Decision Tree model.

---

 Horizon = 30 Days

| Model | Average Prediction Error |
|----|----|
| Lag-1 Baseline | 1021.97 |
| Lag-30 Baseline | 976.12 |
| Decision Tree | 878.78 |
| **k-NN** | **730.20** |
| Random Forest | 792.90 |

For longer-term forecasting, k-NN provides the strongest performance, indicating that monthly sales patterns are effectively captured by similarity-based methods.

---

9. Discussion

The numerical results demonstrate that although individual lagged variables do not exhibit strong linear correlations with future sales, combining multiple lagged features within non-linear machine learning models significantly improves forecasting accuracy.

Random Forest reduces the average prediction error by approximately 26% compared to the Lag-1 baseline for one-day-ahead forecasts (from 1032.12 to 767.25). For 30 day ahead forecasts, k-NN reduces the average prediction error by approximately 25% compared to the Lag-30 baseline (from 976.12 to 730.20).

These findings confirm that ensemble and similarity based methods outperform simple linear or rule based approaches in this dataset.

---

10. Conclusion

The purpose of this project was to understand whether future sales performance can be predicted using historical company transaction data and to identify which operational and economic factors meaningfully influence sales behavior. The results show that daily sales dynamics in the iron and steel industry are highly volatile and cannot be explained through simple linear relationships alone. While short-term linear dependencies between past and future sales are weak, meaningful patterns emerge at appropriate time scales and through non-linear modeling approaches. In particular, the strong relationship between monthly purchase quantities and next-month sales highlights the importance of inventory planning for medium-term sales performance. Additionally, the findings confirm that sales volume is largely price-inelastic within the observed range, while exchange rate fluctuations are primarily reflected in prices rather than quantities. Machine learning models such as Decision Tree, k-NN, and Random Forest successfully capture these complex patterns and substantially outperform simple lag-based baselines. Overall, this project demonstrates how combining statistical analysis with machine learning can support more informed demand forecasting, inventory planning, and pricing strategies in real-world industrial settings.

IMPORTANT NOTE
Due to the commercially sensitive nature of the company’s transaction data, the number and level of detail of visualizations and plots included in this project were intentionally limited. This approach ensures the protection of confidential business information while still allowing meaningful exploratory data analysis and interpretation of the underlying sales patterns.


DSA 210 – Introduction to Data Science

Final Project Report
2025–2026 Fall Term

Student Name: Ömer Erhal
Student ID: 31032

1. Motivation

The motivation behind this project is to understand whether future sales performance can be predicted using historical company transaction data and to identify which factors meaningfully influence sales behavior. As the dataset comes from a real company operating in the iron and steel industry, accurate demand forecasting is critical for inventory planning, pricing decisions, and operational efficiency.

Through this project, my goal was to apply the full data science pipeline learned in DSA 210 to a real-world problem and to evaluate both statistical and machine learning approaches for sales forecasting. By analyzing historical sales, purchase records, and exchange rate data, I aimed to explore whether past behavior contains predictive information that can support data-driven business decisions.

2. Data Source and Data Collection

The primary dataset used in this project consists of internal company transaction records, including:

Sales transaction data (date, product type, quantity, price)

Purchase (inventory intake) transaction data

Daily USD/TRY exchange rate data collected from a public financial source

The sales and purchase data were obtained directly from the company’s internal systems. Due to the commercially sensitive nature of this information, the raw dataset cannot be shared publicly. To address this limitation while maintaining transparency, mock sample rows were included in the report to demonstrate the structure of the data.

The exchange rate data was used to enrich the internal dataset and to examine the effect of macroeconomic conditions on pricing and sales behavior, as encouraged by the project guidelines.

3. Data Analysis Pipeline

This project follows the complete data science pipeline:

Data Cleaning and Preparation

Converted all date fields to datetime format

Corrected numeric formatting issues caused by decimal separators

Removed invalid or non-positive values

Forward-filled missing prices

Aggregated data to daily and monthly levels

Merged sales data with purchase and USD/TRY exchange rate data

Corrected extreme outlier entries caused by unit inconsistencies

Exploratory Data Analysis (EDA)

To understand the structure and behavior of the data, exploratory analysis was conducted using boxplots and scatter plots:

Distribution of sales quantities

Distribution of sales prices

Relationship between sales price and sales quantity

Relationship between purchases and sales

These visualizations revealed strong skewness in sales quantities, stable pricing behavior, and weak visible relationships between prices, purchases, and sales volume.

Hypothesis Testing

Statistical hypothesis testing was performed using Pearson correlation and linear regression to evaluate the significance of relationships between variables. All tests were reported using correlation coefficients and p-values.

Machine Learning

To complement statistical analysis, supervised machine learning models covered in the course were applied:

Decision Tree Regression

k-Nearest Neighbors (k-NN) Regression

Random Forest Regression

Lagged sales values (1, 7, and 30 days), daily average price, and daily purchase quantity were used as input features. Models were evaluated using TimeSeriesSplit cross-validation to ensure realistic forecasting without data leakage.

4. Findings

The main findings of the project can be summarized as follows:

Daily lagged sales quantities do not show statistically significant linear relationships with future sales

Sales prices do not significantly affect sales volume, indicating price-inelastic behavior within the observed range

Daily purchase quantities do not significantly influence short-term sales

Monthly purchase quantities show a positive but statistically weak relationship with next-month sales

USD/TRY exchange rate strongly affects sales prices but does not significantly affect sales quantities

Non-linear machine learning models substantially outperform simple lag-based baselines in forecasting future sales

In particular, Random Forest achieved the best short-term forecasting performance, while k-NN performed best for longer-term (30-day) forecasts. These results demonstrate that although simple linear relationships are weak, historical transaction data contains meaningful predictive information when modeled appropriately.

5. Limitations and Future Work

This project has several limitations. First, the dataset is limited to a single company and a specific time period, which may restrict generalizability. Second, due to data confidentiality concerns, the number and level of detail of visualizations were intentionally limited, which constrained deeper exploratory analysis. Additionally, external factors such as customer-specific attributes, market demand indicators, and production constraints were not available.

Future work could extend this project by incorporating customer-level features, longer historical time spans, additional macroeconomic indicators, and alternative forecasting techniques such as classification-based demand segmentation or probabilistic forecasting models. With richer data, more advanced modeling approaches could further improve predictive performance and business insights.

6. Ethical Considerations

All analyses were conducted with careful consideration of data privacy and confidentiality. Sensitive company data was not shared publicly, and visualizations were deliberately simplified to prevent disclosure of confidential business information. This approach ensures responsible and ethical use of real-world industrial data while still enabling meaningful data science analysis.

7. Conclusion

This project demonstrates that future sales cannot be reliably predicted using simple linear relationships alone, but meaningful forecasts can be achieved by combining historical sales, pricing, and purchase information within non-linear machine learning models. By applying the concepts learned throughout the DSA 210 course, this project highlights the practical value of data science methods in real-world industrial settings and shows how statistical analysis and machine learning can be jointly used to support informed decision-making.










