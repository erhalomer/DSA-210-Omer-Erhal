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
| 2024-06-14 | Supplier_X | SCRAP | 1500 | 9200 |
| 2024-06-17 | Supplier_Y | SCRAP | 2000 | 9400 |

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





