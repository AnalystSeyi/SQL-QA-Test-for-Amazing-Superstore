##
# Quality Assurance Test For Amazing Superstore Power BI Analysis Using SQL

![](Sql_codes.webp)
<br><br>
## Introduction

**Type of Data Set:**  Sales Dataset 

**Stakeholder Requirement / Problem Statement:** Create a Superstore database in MySql, write SQL queries, and create a test document to QA the Superstore Dashboard developed in Power BI Software.
1. Functional Validation: Test each feature as per the requirement. To verify all the filters and action filters on the report work as per the requirement
2. **Data Validation: Check the accuracy and quality of the data to match the value in the Power BI report with SQL queries.**
3. Test Document: Create a test document that will contain the screenshots and queries used to test the reports.

**Data Structure:** There are 2 tables in the dataset, which are named <kbd> Listoforders </kbd> and <kbd> Orderbreakdown </kbd>.

## Analysis Pipeline Using SQL Queries

**Data Source:** SharePoint

**Data Transformation/Cleaning:** Data cleaning and transformation were carried out using SQL Query.
#### 1. The dataset was on two different sheets and had to be made into a CSV file before being connected to the MySQL database.
   
   **Vlookup** was extensively used for this transformation. This transformation was done on Excel before loading into MySQL.
   
      =VLOOKUP(lookup_value, table_array, Match, [range_lookup])

##
   - Table 1 : The table name is <kbd>ListOfOrders</kbd>
##
![](Table1.png)
##
   -  Table 2: The table name is <kbd>OrderBreakdown</kbd>
##
   
![](Table2.png)

##
   -  <kbd>Merged_Table</kbd> The two tables above are merged into one.
##
   
![](Merged_Table.png)

##
   -  **The Merged table was Converted to CSV**
##

![](Merged_CSV.png)

**The CSV File was connected to MYSQL database Server**

##

#### 2. Date Function Transformation: This is neccessary for **Time Inetlligence Functions.**

The date is in the format of <kbd>DD-MM-YY</kbd>. <br> **Mysql** can only process dates in this format <kbd>YY-MM-DD.</kbd> <br>
Therefore, date conversion is inevitable.<br>
This query is a modification query.

         use amazingmart;
         SELECT * FROM amazingsuperstore;
         alter table amazingsuperstore modify column `Order Date` varchar(20);
         alter table amazingsuperstore modify column `Ship Date` varchar(20);
         desc amazingsuperstore;
         update amazingsuperstore set `Order Date` = str_to_date(`Order Date`, '%m/%d/%Y');
         select date_format(str_to_date(`Order Date`, '%m/%d/%Y'), '%d/%m/%Y') as dateformat from amazingsuperstore;

         update amazingsuperstore set `Ship Date` = str_to_date(`Ship Date`, '%m/%d/%Y');
         select date_format(str_to_date(`Ship Date`, '%m/%d/%Y'), '%d/%m/%Y') as dateformat from amazingsuperstore;
         SELECT * FROM amazingsuperstore;

<br>![](Date_conversion.png)

<br><br>
## Analysis

**Data Validation: QA check for the Power BI live report with SQL queries.**

For Power BI Live Visualization click here <kbd>[Live-Visualization](https://app.powerbi.com/view?r=eyJrIjoiMzczYjA0YzItYTgzZi00MTk0LTk4ZTYtN2U4MDdjYzk2ZjQ3IiwidCI6IjU0OGU5MDRlLTY2MDEtNGQ0My1iZmY3LTYzZGRlZTRjOWVlNiJ9&pageName=ReportSectiona0423d4498c17d5c245d)</kbd>
<br><br>

**SQL Software:** MySQL
##
### A. KPI
##
<br>**1. Total Order** 

     select count(`Order ID`) as Total_Order from amazingsuperstore;
     
**Output:**

![](Total_Order.png)
##
<br>**2. Total Sales** 

     select sum(Sales) as Total_Sales from amazingsuperstore;
     
**Output:**

![](Total_Sales.png)

##
<br>**3. Total Profit** 

     select sum(Profit) as Total_Profit from amazingsuperstore;
     
**Output:**

![](Total_Profit.png)

##
<br>**4. Percentage Profit** 

     select round((sum(Profit) / sum(Sales) ) * 100, 0) as Percentage_Profit FROM amazingsuperstore; 
     Converted to 0 decima
     
**Output:**

![](Percentage_Profit.png)

##
### B. Product Category
##
<br>**5. Office Supply** 

     select count(`Order ID`) as Office_Supplies from amazingsuperstore where Category='Office Supplies';
     
**Output:**

![](Product_category_office_supply.png)

##
<br>**6. Furniture** 

     select count(`Order ID`) as Office_Supplies from amazingsuperstore where Category='Furniture';
     
**Output:**

![](Product_category_Furniture.png)

##
<br>**7. Technology** 

     select count(`Order ID`) as Office_Supplies from amazingsuperstore where Category='Technology';
     
**Output:**

![](Product_category_Technology.png)

##
### C. Performance by Year
##
<br>**8. Total Order by Year** 

     select   year(`Order Date`) as Year, count(`Order ID`) as Total_order from amazingsuperstore group by year(`Order Date`);
     
**Output:**

![](Total_Profit_by_year.png)

##
<br>**9. Total Sales by Year** 

     select   year(`Order Date`) as Year, sum(Sales) as Total_Sales from amazingsuperstore group by year(`Order Date`);
     
**Output:**

![](Total_sales_by_year.png)

##
<br>**10. Total Profit by Year** 

     select   year(`Order Date`) as Year, sum(Profit) as Total_Sales from amazingsuperstore group by year(`Order Date`);
     
**Output:**

![](Total_Profit_by_year.png)

##
<br>**11. Cost of Sales by Year** 

     Calculating cost of sales
     
     select   year(`Order Date`) as Year,  (sum(Sales) - sum(Profit)) as Cost_of_sales from amazingsuperstore group by year(`Order Date`);

     
**Output:**

![](Total_cost_of_sale_by_year.png)

##
<br>**12. Year on Year Growth** 

Calculating cost of sales
     
     select year(`Order Date`) as Current_Year, sum(profit) as Total_Profit from amazingsuperstore group by  year(`Order Date`);
     select sum(profit) as Total_Profit from amazingsuperstore where  year(`Order Date`)='2011';
     select sum(profit) as Total_Profit from amazingsuperstore where  year(`Order Date`)='2012';
     select sum(profit) as Total_Profit from amazingsuperstore where  year(`Order Date`)='2013';
     select sum(profit) as Total_Profit from amazingsuperstore where  year(`Order Date`)='2014'; 

Year on Year Growth for year 2012:
 
     select round( (sum(profit) / (select sum(profit) as Total_Profit from amazingsuperstore where  year(`Order Date`)='2011') - 1) * 100, 2) as YOY_Growth  
     from amazingsuperstore where  year(`Order Date`)='2012';
     
**Output:**

![](2012_Profit_Growth.png)

Year on Year Growth for year 2013:
     
     select round( (sum(profit) / (select sum(profit) as Total_Profit from amazingsuperstore where  year(`Order Date`)='2012') - 1) * 100, 2) as YOY_Growth  
     from amazingsuperstore where  year(`Order Date`)='2013';

**Output:**

![](2013_Profit_Growth.png)

Year on Year Growth for year 2014:

     select round( (sum(profit) / (select sum(profit) as Total_Profit from amazingsuperstore where  year(`Order Date`)='2013') - 1) * 100, 2) as YOY_Growth  
     from amazingsuperstore where  year(`Order Date`)='2014';

     
**Output:**

![](Year_on_Year_Growth.png)

##
### D. Profit with or without Discount
##
<br>**13. Profit with Discount** 

     select sum(Profit) as Profit_before_Discount FROM amazingsuperstore;

     
**Output:**

![](Total_Profit.png)

##
<br>**14. Profit without Discount** <br>

**Statistics:** <br>
cost of sales = Selling price - Profit<br>
Selling price before discount = selling Price / (1 - discount)<br>
The Discount is in decimals, that is why am using 1 and not 100<br>
Profit without discount = Selling price before Discount - Cost of Sales<br>

**Cost of sales:**

      select  sum(`Sales` - `Profit`) as Cost_of_Sales FROM amazingsuperstore;

     
**Output:**

![](cost-of-sales.png)

**Selling Price before Discount:**

    select  sum(`Sales` / (1 - `Discount`)) as Price_Before_Discount FROM amazingsuperstore;

**Output:**

![](Selling_Price_B4_Discount.png)


**Profit before discount:**

     select  round( sum(`Sales` / (1 - `Discount`)) - sum(`Sales` - `Profit`)  , 0) as Profit_before_Discount FROM amazingsuperstore;

**Output:**

![](Profit_without_discount.png)

##
<br>**15. Percentage Profit without Discount** 

    select  round( ((sum(`Sales` / (1 - `Discount`)) - sum(`Sales` - `Profit`)) / sum(sales)) * 100, 0) as Profit_before_Discount FROM amazingsuperstore;
     Converted to 0 decima
     
**Output:**

![](Percentage_Profit_Without_Discount.png)


##
### E. Profit Comparism
##
<br>**16. Percentage of Profit with Discount to Profit without Disco** 

    select  round( ( sum(Profit) / ( sum(`Sales` / (1 - `Discount`)) - sum(`Sales` - `Profit`) )) * 100, 1) as Profit_before_Discount FROM amazingsuperstore;
     Converted to 0 decima
     
**Output:**

![](Percentage_profit_with&without_Discount.png)

##
### F. Profit With & Without Discount By Year
##
<br>**17. Profit with Discount by Year** 

    select  year(`Order Date`) as Year, sum(Profit) as Profit_before_Discount FROM amazingsuperstore group by  year(`Order Date`);
     
**Output:**

![](Total_Profit_by_year.png)

##
<br>**18.Profit Without Discount by Year** 

    select year(`Order Date`) as Year,  round( sum(`Sales` / (1 - `Discount`)) - sum(`Sales` - `Profit`)  , 0) as Profit_before_Discount FROM amazingsuperstore group by year(`Order Date`) ;
     
**Output:**

![](Profit_without_Discount_by_Year.png)

##
### G. Profit by Discount
##
<br>**19. Profit by Discount** 

    select  `Discount`, sum(Profit) as Profit FROM amazingsuperstore group by `Discount` order by `Discount` asc;
     
**Output:**

![](Discount_by_profit.png)

##
<br>**20. Profit made from <= 20% Discount sales** 

    select  sum(Profit) as Profit FROM amazingsuperstore where `Discount` between 0 and 0.2;
     
**Output:**

![](Profit_of_less.png)

##
<br>**21. Total Number from <= 20%** 

    select count(`Order ID`) as Profit FROM amazingsuperstore where `Discount` between 0 and 0.2;
     
**Output:**

![](Total_no_order_less_than.png)

##
<br>**22. Profit made from >= 30% Discount sales** 

    select  sum(Profit) as Profit FROM amazingsuperstore where `Discount` between 0.3 and 1;
     
**Output:**

![](profit_greater_30.png)

##
<br>**23. Total Number of Order >= 30%** 

    select  sum(Profit) as Profit FROM amazingsuperstore where `Discount` between 0.3 and 1;
     
**Output:**

![](Total_order_greater_30.png)

##
### H.19 Total Order, Profit and Max Discount by Country
##
<br>

     select Country, count(`Order ID`) as Total_Order , sum(Profit) as Profit, max(Discount) as Max_Discount FROM amazingsuperstore group by Country order by sum(Profit) desc ;
     

**Output:**


![](Country,Total_order,profit,Max_Disount.png)

##
### I.20 Discount by Profit
##
<br>

     select  Discount, sum(Profit) as Profit FROM amazingsuperstore group by Discount order by Discount asc;
     

**Output:**


![](Discount_by_profit.png)

##
### Result: The report is quality assured.
##
