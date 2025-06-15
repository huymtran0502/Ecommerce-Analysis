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
        
  * Percentage & Contribution:
      * Country Sales % =
          VAR _allValue =
              CALCULATE([Sales], ALL(dimCountry))
          RETURN DIVIDE([Sales], _allValue)

  * SVG-Based Custom Visual:
      * Continent Rounded Bar Chart = 
    VAR Bar_Fill = ([Country Sales %]) * 500
    VAR Bar_Text = ([Country Sales %])
    VAR Formatted_Bar_text = FORMAT(Bar_Text, "#.0%")
    VAR SVG_Data_URL = "data:image/svg+xml;utf8,"
    VAR SVG_Start = "<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 25'>"
    VAR SVG_Data = 
        "<rect fill = '#8D94C9' x='10' y='5' rx='9' ry='9' width='" & Bar_Fill & "' height='20' />
        <text font-family='Trebuchet MS' font-size='1.5em' fill='white' font-weight='bold' x='" & Bar_Fill + 20 & "' y='20'>" & Formatted_Bar_text & "</text>"
    VAR SVG_End = "</svg>"

    RETURN SVG_Data_URL & SVG_Start & SVG_Data & SVG_End

  * Month-Based Insights & Formatting:
      * Month Switch = 
    SWITCH(
        SELECTEDVALUE('Month Parameter'[Month Parameter]),
        1, "Jan", 2, "Feb", 3, "Mar", 4, "Apr",
        5, "May", 6, "June", 7, "July", 8, "Aug",
        9, "Sep", 10, "Oct", 11, "Nov", 12, "Dec"
    )

      * Previous Month = 
    VAR myprevmonth = CALCULATE(
        SELECTEDVALUE('Calendar'[Month]),
        PREVIOUSMONTH('Calendar'[Date])
    )
    RETURN "Vs. " & myprevmonth & " : "

  *  Conditional Colors & Icons:
      * Icon Sales = 
    VAR PositiveIcon = UNICHAR(9650)
    VAR NegativeIcon = UNICHAR(9660)
    RETURN IF([Sales Growth] > 0, PositiveIcon, NegativeIcon)

      * Month Highlight Colour =  
    VAR _selected_months = VALUES('Month Parameter'[Month Parameter])
    RETURN IF(
        ISFILTERED('Month Parameter'[Month Parameter]) && 
        MAX('Calendar'[Monthnum]) IN _selected_months,
        "#F9C9C6",  // Highlight
        "#8D94C9"   // Default
    )

  * Highlighted Month Details Summary:
      * Highlighted Month Details = 
    VAR _selected_months = VALUES('Month Parameter'[Month Parameter])
    VAR _selected_months_sales = 
        CALCULATE([Sales], 'Calendar'[Monthnum] IN _selected_months)
    VAR _percent_of_total = 
        FORMAT(
            DIVIDE(_selected_months_sales, [Sales], 0),
            "percent"
        )
    VAR _result = IF(
        ISFILTERED('Month Parameter'[Month Parameter]),
        "Highlighted month sales : " & FORMAT(_selected_months_sales, "#,0,,.00 Mi ( ") 
        & _percent_of_total & " ) in Total Sales " & FORMAT([Sales], "#,0,,.00 Mi"),
        "Total Sales : " & FORMAT([Sales], "#,0,,.00 Mi")
    )
    RETURN _result

# **Key Insights:**

* Electronics & Computing drive over 70% of the total revenue — indicating potential for targeted campaigns.
* Returning customers generate higher AOV than new ones — suggesting investment in retention strategies.
* USA leads in revenue & delivery performance, while Australia has opportunities for logistics improvement.
* Sales trends align with major holidays/promos, showing responsiveness to marketing campaigns.
