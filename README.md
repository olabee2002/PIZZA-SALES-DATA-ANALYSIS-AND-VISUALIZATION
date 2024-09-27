# Pizza Sales Data Analysis and Visualization

### Project Overview
 The goal of this project is to analyze key performance indicators (KPIs) from our pizza sales data to derive insights into business performance. Additionally, we aim to create various visualizations to track trends, understand customer preferences, and identify the most and least popular pizza offerings.

### Data Source
Pizza Sales Data: The primary dataset used for this analysis is the "pizza_sales.csv" file, contaning detailed records of customer transactions.

### Tools
- Excel - Data Cleaning, Excel Formulas (IF etc.), Pivot Tables/Charts, Data Visualization, Dashboard Building, Timeline.  [Download here](https://microsoft.com)
- SQL Server - Data Analysis  [Download here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)

### Data Cleaning/Preparation
1. Removing Duplicates
2. Handling Missing Values
3. Aggregation for KPIs
4. Preparing Data for Charts

## Data Analysis   
### KPI Requirements
We will calculate the following metrics to evaluate business performance:

1. Total Revenue: The total price of all pizza orders.
~~~ sql
select sum (total_price) as Total_Revenue from  pizza_sales
~~~
![image](https://github.com/user-attachments/assets/237b9539-6993-468f-a0d6-303ef23a1eb1)

2. Average Order Value: The average amount spent per order, calculated by dividing the total revenue by the total number of orders.
~~~ sql
select sum (total_price) / count ( distinct order_id) as Average_Order_Value from pizza_sales
~~~

![image](https://github.com/user-attachments/assets/fafae70e-c653-469e-b7cb-df798550abc4)

3. Total Pizzas Sold: The sum of all pizzas sold.
~~~ sql
select sum (quantity) as Total_Pizza_Sold from pizza_sales
~~~
![image](https://github.com/user-attachments/assets/e0b4610d-bcce-43ba-9f43-00f8ebccb547)

4. Total Orders: The total number of orders placed.
~~~ sql
select count (distinct order_id) as Total_Order from pizza_sales
~~~
![image](https://github.com/user-attachments/assets/66464f08-cb15-492d-bf49-4d0e1c0a85de)


5. Average Pizzas Per Order: The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.
~~~ sql
select cast (cast(sum(quantity) as decimal(10,2))/ 
cast (count (distinct order_id)as decimal(10,2)) as decimal(10,2)) as Avg_Pizzas_Per_Order from pizza_sales
~~~
![image](https://github.com/user-attachments/assets/7b71874b-39ea-4eec-88a6-02beeacf894e)

### Charts Requirements
To visualize insights from the pizza sales data, the following charts will be created:
1. Daily Trend for Total Orders
~~~sql
select DATENAME (DW, order_date) as order_day, COUNT (distinct order_id) as Total_orders 
from pizza_sales
group by DATENAME (DW, order_date)
~~~
![image](https://github.com/user-attachments/assets/dd36448a-f50c-4b86-b11b-9b92243ba680)

2. Hourly Trend for Total Orders
~~~ sql
select DATEPART(hour, order_time) as Order_hours, count (distinct order_id) as Total_orders
from pizza_sales
group by DATEPART(hour, order_time)
order by DATEPART(hour, order_time)
~~~
![image](https://github.com/user-attachments/assets/1ff3aba3-a3d7-42bc-bf92-b35685e44f06)

3. Percentage of Sales by Pizza Category
~~~sql
select pizza_category, sum(total_price) as Total_Sales, sum(total_price) * 100 /
(select sum(total_price) from pizza_sales where month (order_date) = 1) as PCT
from pizza_sales 
where month (order_date) = 1
group by pizza_category
~~~
![image](https://github.com/user-attachments/assets/cbb07a7d-9532-46ef-bf2b-b8a3769c081e)

4. Percentage of Sales by Pizza Size
~~~ sql
select pizza_size, cast (sum(total_price) as decimal (10,2)) as Total_Sales, cast (sum(total_price) * 100 /
(select sum(total_price) from pizza_sales) as decimal (10,2)) as  PCT
from pizza_sales 
group by pizza_size
order by PCT DESC
~~~
![image](https://github.com/user-attachments/assets/b479cdb8-92fe-44dd-a2d0-1806487f97f0)

5. Total Pizza Sold by Pizza Category
~~~sql
select pizza_category, sum (quantity) as  Total_Pizza_Sold
from pizza_sales
group by pizza_category
~~~
![image](https://github.com/user-attachments/assets/ba132676-af2f-4d18-82d0-39db6631a6b8)

6. Top 5 Best Seller by Total  Pizza Sold
~~~sql
select Top 5 pizza_name,sum(quantity) as Total_Pizza_Sold
from pizza_sales
group by pizza_name
order by sum(quantity) desc
~~~
![image](https://github.com/user-attachments/assets/173948c0-7b6f-47a4-946b-c9dd0c132e04)

7. Buttom 5 Best Seller by Total  Pizza Sold
~~~sql
select Top 5 pizza_name,sum(quantity) as Total_Pizza_Sold
from pizza_sales
group by pizza_name
order by sum(quantity)asc
~~~
![image](https://github.com/user-attachments/assets/3a7a2d15-a45b-4b9a-a961-b9c34c3e8f63)

## Detailed Overview Of The Analysis
1. Daily Trend for Total Orders:
   
Chart Type: Bar Chart

Purpose: Display the daily trend of total orders over a specific time period to identify patterns or fluctuations in order volumes.
![image](https://github.com/user-attachments/assets/07d7084a-e3b7-4666-bb0c-ac7a26acbd38)

