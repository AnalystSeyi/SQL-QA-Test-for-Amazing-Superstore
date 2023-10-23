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
#### 1. The dataset was on two different sheets and had to be made into a CSV file before being connected to MySQL database.
   
   **Vlookup** was extensively used for this transformation. This transformation was done on Excel before loading into MySql.
   
      **=VLOOKUP(lookup_value, table_array, Match, [range_lookup])**

##
   - Table 1 <kbd>ListOfOrders</kbd>
##
![](Table1.png)
##
   -  Table 2 <kbd>OrderBreakdown</kbd>
##
   
![](Table2.png)

##
   -  <kbd>Merged_Table</kbd>
##
   
![](Merged_Table.png)

##
   -  **Coverted to CSV**
##

![](Merged_CSV.png)

**The CSV File was connected to MYSQL Server**

##

#### 2. Date Function Transformation: Date Transformation was a must because we will be carrying out **Time Inetlligence Functions.**

The date is in the format of <kbd>DD-MM-YY</kbd> **Mysql** can only process dates in this format <kbd>YY-MM-DD.</kbd> 
Therefore, date conversion is inevitable.
This query is a modification query.

![](Date_conversion.png)







