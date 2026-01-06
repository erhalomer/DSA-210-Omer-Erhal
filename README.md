DSA 210 – Introduction to Data Science  
Final Project Report (2025–2026 Fall Term)

Student Name: Ömer Erhal  
Student ID: 31032  

---

1. Motivation

The motivation behind this project is to understand whether future sales performance can be predicted using historical company transaction data and to identify which factors meaningfully influence sales behavior. As the dataset comes from a real company operating in the iron and steel industry, accurate demand forecasting is critical for inventory planning, pricing decisions, and operational efficiency.

Through this project, my goal was to apply the full data science pipeline learned in DSA 210 to a real-world problem and to evaluate both statistical and machine learning approaches for sales forecasting. By analyzing historical sales, purchase records, and exchange rate data, I aimed to explore whether past behavior contains predictive information that can support data-driven business decisions.

---

2. Data Source and Data Collection

The primary dataset used in this project consists of internal company transaction records, including:

- Sales transaction data (date, product type, quantity, price)
- Purchase (inventory intake) transaction data
- Daily USD/TRY exchange rate data collected from a public financial source

The sales and purchase data were obtained directly from the company’s internal systems. Due to the commercially sensitive nature of this information, the raw dataset cannot be shared publicly. To address this limitation while maintaining transparency, mock sample rows were included in the report to demonstrate the structure of the data.

Below are mock sample rows (non-real) for demonstration of the Data:

| Date | Customer | Product_Type | Quantity | Price |
|------|-----------|--------------|-----------|--------|
| 2024-06-15 | Customer_A | THICK | 1200 | 14500 |
| 2024-06-16 | Customer_B | THIN | 800 | 14850 |

| Date | Supplier | Product_Type | Quantity | Price |
|------|-----------|--------------|-----------|--------|
| 2024-06-14 | Supplier_X | BILLET | 1500 | 9200 |
| 2024-06-17 | Supplier_Y | BILLET | 2000 | 9400 |

The exchange rate data was used to enrich the internal dataset and to examine the effect of macroeconomic conditions on pricing and sales behavior, as encouraged by the project guidelines.

Merged Sales + USD/TRY Exchange Rate Data (Mock Example)

| Tarih       | Daily_Quantity (ton) | Daily_Price (TL/ton) | USD_TRY |
|-------------|-----------------------|------------------------|---------|
| 2024-11-01  | 5728.0                | 26528.57               | 34.3020 |
| 2024-11-02  | 137.5                 | 26600.00               | 34.3020 |
| 2024-11-04  | 145.0                 | 26600.00               | 34.3337 |
| 2024-11-05  | 425.0                 | 26425.00               | 34.3118 |
| 2024-11-06  | 357.5                 | 25757.14               | 34.1862 |

---

3. Data Analysis Pipeline

This project follows the complete data science pipeline:

Data Cleaning and Preparation
- Converted all date fields to datetime format
- Corrected numeric formatting issues caused by decimal separators
- Removed invalid or non-positive values
- Forward-filled missing prices
- Aggregated data to daily and monthly levels
- Merged sales data with purchase and USD/TRY exchange rate data
- Corrected extreme outlier entries caused by unit inconsistencies

Exploratory Data Analysis (EDA)
To understand the structure and behavior of the data, exploratory analysis was conducted using boxplots and scatter plots:
- Distribution of sales quantities
- Distribution of sales prices
- Relationship between sales price and sales quantity
- Relationship between purchases and sales

These visualizations revealed strong skewness in sales quantities, stable pricing behavior, and weak visible relationships between prices, purchases, and sales volume.

Hypothesis Testing
Statistical hypothesis testing was performed using Pearson correlation and linear regression to evaluate the significance of relationships between variables. All tests were reported using correlation coefficients and p-values.

Machine Learning
To complement statistical analysis, supervised machine learning models covered in the course were applied:
- Decision Tree Regression
- k-Nearest Neighbors (k-NN) Regression
- Random Forest Regression

Lagged sales values (1, 7, and 30 days), daily average price, and daily purchase quantity were used as input features. Models were evaluated using TimeSeriesSplit cross-validation to ensure realistic forecasting without data leakage.

---

4. Findings

The main findings of the project can be summarized as follows:

- Daily lagged sales quantities do not show statistically significant linear relationships with future sales  
- Sales prices do not significantly affect sales volume, indicating price-inelastic behavior within the observed range  
- Daily purchase quantities do not significantly influence short-term sales  
- Monthly purchase quantities show a positive but statistically weak relationship with next-month sales  
- USD/TRY exchange rate strongly affects sales prices but does not significantly affect sales quantities  
- Non-linear machine learning models substantially outperform simple lag-based baselines in forecasting future sales  

In particular, Random Forest achieved the best short-term forecasting performance, while k-NN performed best for longer-term (30-day) forecasts. These results demonstrate that although simple linear relationships are weak, historical transaction data contains meaningful predictive information when modeled appropriately.

---

5. Limitations and Future Work

This project has several limitations. First, the dataset is limited to a single company and a specific time period, which may restrict generalizability. Second, due to data confidentiality concerns, the number and level of detail of visualizations were intentionally limited, which constrained deeper exploratory analysis. Additionally, external factors such as customer-specific attributes, market demand indicators, and production constraints were not available.

Future work could extend this project by incorporating customer-level features, longer historical time spans, additional macroeconomic indicators, and alternative forecasting techniques such as classification-based demand segmentation or probabilistic forecasting models. With richer data, more advanced modeling approaches could further improve predictive performance and business insights.

---

6. Conclusion

This project demonstrates that future sales cannot be reliably predicted using simple linear relationships alone, but meaningful forecasts can be achieved by combining historical sales, pricing, and purchase information within non-linear machine learning models. By applying the concepts learned throughout the DSA 210 course, this project highlights the practical value of data science methods in real-world industrial settings and shows how statistical analysis and machine learning can be jointly used to support informed decision-making.











