# E-Commerce Analysis Dashboard 

# **Overview:** 

* This project presents a detailed data analysis of an eCommerce business, built using Power BI. The dashboard is designed to provide interactive insights into sales performance, customer behavior, product analytics, and revenue trends across categories and regions.
* The primary goal of this dashboard is to enable business stakeholders to track KPIs, identify sales trends, and discover opportunities to optimize product offerings and marketing efforts.

# **Objectives:**

* Monitor key sales performance metrics in real time.
* Identify top-performing products and categories.
* Analyze customer segmentation and geographic behavior.
* Deliver data-driven recommendations for growth and optimization.

# **Dataset:**
![image_alt](https://github.com/huymtran0502/Ecommerce-Analysis/blob/main/Model%20View.png)
The analysis was built from a relational model of multiple CSV files, imported and transformed in Power BI. The tables include:
* Ecommerce_data.csv: This is the main transactional dataset, encompassing customer IDs, order dates, revenue, item quantity per transaction, product names, IDs, categories, prices, payment details, and geographical information (Country and Continent).
* Product Images.csv: Contains product names mapped to their respective image URLs.
* Country Flags.csv: Provides country names mapped to their corresponding flag image URLs.

# **Dashboard:**
![image_alt](https://github.com/huymtran0502/Ecommerce-Analysis/blob/main/Dashboard.png)
  ## KPI Summary:
  * Total Revenue: $2.1M
  * Total Orders: 9,300
  * Average Order Value (AOV): $226
  * Top Category Revenue Share:
    Electronics: 68%,
    Computing: 51%
  * Returning Customers Rate: 34%

  ## Trend Analysis:
  * Monthly Sales Trend: Reveals strong peaks in Q4 and notable dips in Q2, reflecting possible seasonality.
  * Customer Growth: Consistent increase in new customers over the year with improved retention in Q3.
  * Product Trends: Clear spikes in sales for Electronics during promotional periods.

  ## Geographic Insights:
  * Regional Revenue Performance:
      * USA: $850K
      * Canada: $300K
      * Australia: $250K
  * Delivery Speed by Region:
      * USA: 1–2 days (Excellent)
      * Australia: ~10 days (Needs Improvement)
   

# **DAX Measures Used:**
  * Core Measures:
      * Sales = SUM(FactEcommerce[Total Sale Amount])
      * Sales PM = CALCULATE([Sales], PREVIOUSMONTH('Calendar'[Date]))
      * Sales Growth = DIVIDE([Sales] - [Sales PM], [Sales PM])
      * Sales Colours = IF([Sales Growth] >= 0, "Green", "Red")

  
