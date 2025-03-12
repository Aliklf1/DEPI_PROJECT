select * from Sheet1$
select * into buckup from Sheet1$ 

-- **********************************************************************************************
-- Answer business Questions

--******************Salles****************************
-- 1 ) best Selling Products
select max(count_Product_Order) from
(
select PRODUCTNAME, SUM(QUANTITYORDERED) as count_Product_Order
from Sheet1$ group by PRODUCTNAME
)as best_Selling

-- 2)total sales revenue for each Product?
select PRODUCTNAME, sum(SALES)as SumSales from Sheet1$ group by PRODUCTNAME

-- 3 )total sales revenue for each year , mounth,qurter?
select YEAR_ID ,QTR_ID,MONTH_ID,SUM(SALES) AS TOTAL_SALES
from Sheet1$ group by YEAR_ID , QTR_ID,MONTH_ID order by YEAR_ID,QTR_ID,MONTH_ID

-- 4 ) Total Sales for each Country and Each city
select COUNTRY,CITY,SUM(SALES) AS TOTAL_SALES
from Sheet1$ group by COUNTRY,CITY order by COUNTRY , CITY

-- 5 ) State Sales
select STATE, sum(SALES)as TOTAL_SALES from Sheet1$ Group by STATE

-- 6 ) Sum of Sales by Product line
select PRODUCTLINE,sum(SALES)as TOTAL_SALES from Sheet1$ Group by PRODUCTLINE


--******************Orders operations**************************
-- 7 ) Count of orders operations which maked in Each Country 
select Country , Count(ORDERNUMBER)as COUNT_OF_ORDERS_Operations from
(
select distinct(ORDERNUMBER),Country from Sheet1$
)as dd
group by COUNTRY

-- 8 )Count of orders operations which maked in Each CITY
select CITY , Count(ORDERNUMBER)as COUNT_OF_ORDERS_Operations from
(
select distinct(ORDERNUMBER),CITY from Sheet1$
)as dd
group by CITY

-- 9 )Count of orders operations which maked in Each State
select STATE , Count(ORDERNUMBER)as COUNT_OF_ORDERS_Operations from
(
select distinct(ORDERNUMBER),STATE from Sheet1$
)as dd
group by STATE

-- 10 )Count of Orders Operation which maked in Each Year, QTR , MONTH
select YEAR_ID,QTR_ID,MONTH_ID , Count(ORDERNUMBER)as COUNT_OF_ORDERS_Operations from
(
select distinct(ORDERNUMBER),YEAR_ID,QTR_ID,MONTH_ID from Sheet1$
)as dd
group by YEAR_ID,QTR_ID,MONTH_ID


--******************Quantity Orders**********************************
-- 11 ) Create Table of SUM Quantity Orders and Select from it according to Year , mounth , Qtr
select PRODUCTNAME,YEAR_ID , QTR_ID , MONTH_ID , sum(QUANTITYORDERED) as Sum_Quantity_Order
into Table_Of_Sum_QuantityOrders
from Sheet1$ Group by PRODUCTNAME , YEAR_ID , QTR_ID ,MONTH_ID

select * from Table_Of_Sum_QuantityOrders
select sum(Sum_Quantity_Order) from Table_Of_Sum_QuantityOrders where YEAR_ID=2003
select sum(Sum_Quantity_Order) from Table_Of_Sum_QuantityOrders where YEAR_ID=2003 and MONTH_ID=1
select sum(Sum_Quantity_Order) from Table_Of_Sum_QuantityOrders where YEAR_ID=2003 and MONTH_ID=1 and PRODUCTNAME = 'Classic Cruiser' 
select sum(Sum_Quantity_Order) from Table_Of_Sum_QuantityOrders where YEAR_ID=2003 and QTR_ID=1


-- 12 ) Create Table of SUM Quantity Orders and Select from it according to COUNTRY and CITY
select PRODUCTNAME,Country ,City, sum(QUANTITYORDERED) as Sum_Quantity_Order
into Table_Of_Sum_QuantityOrders_by_Country_and_city
from Sheet1$ Group by PRODUCTNAME , COUNTRY , City 

select * from Table_Of_Sum_QuantityOrders_by_Country_and_city
select sum(Sum_Quantity_Order) from Table_Of_Sum_QuantityOrders_by_Country_and_city where Country='Australia'
select sum(Sum_Quantity_Order) from Table_Of_Sum_QuantityOrders_by_Country_and_city where Country='Australia' and City='Melbourne'


--******************DEALSIZE******************************************
-- 13 )Sum of sales according to dealsize
select DEALSIZE,sum(SALES)as sum_Sales from Sheet1$ group by DEALSIZE

-- 14 ) count of Orders according to dealsize
select DEALSIZE,count(distinct ORDERNUMBER)as count from Sheet1$ group by DEALSIZE

-- 15 ) sum of Orders according to dealsize
select PRODUCTNAME,DEALSIZE,count(distinct ORDERNUMBER)as count from Sheet1$ group by PRODUCTNAME, DEALSIZE order by PRODUCTNAME,DEALSIZE


--******************STATUE**********************************************
-- 16 ) count of Orders according To Status
select  STATUS,count(distinct ORDERNUMBER)as count from Sheet1$ group by STATUS
-- 17) count of Orders according Name To Status
select  PRODUCTNAME,STATUS,count( STATUS)as count from Sheet1$ group by PRODUCTNAME,STATUS


-- ********************Create Tables***************************************
-- Create table for Customers
Select  Distinct ORDERNUMBER,CUSTOMERNAME,ADDRESSLINE1,CITY,POSTALCODE,COUNTRY,CONTACTFIRSTNAME,CONTACTLASTNAME
into Customers
from Sheet1$
select * from Customers
select COUNTRY,count(ORDERNUMBER)as NUMBEROFORDERS from Customers Group by COUNTRY
/*****************************/
-- Create table for Orders
Select ORDERNUMBER,PRODUCTNAME,ORDERLINENUMBER,SALES,ORDERDATE,STATUS,PRODUCTLINE,MSRP
into Orders
from Sheet1$
select * from Orders