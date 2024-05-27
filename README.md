# Pizza_Sale_Data_Analysis

Sure! Here is a revised README section for your GitHub repository without categorizing the queries as basic, intermediate, or advanced.

---

# Pizza Sale Data Analysis

This project involves analyzing pizza sales data using SQL queries to uncover valuable insights. The analysis includes various queries to provide a comprehensive understanding of sales patterns and trends.

## Table of Contents
1. [Introduction](#introduction)
2. [Queries and Results](#queries-and-results)


## Introduction
The purpose of this project is to analyze pizza sales data to derive meaningful insights that can help in making data-driven decisions. This analysis covers a range of queries to provide a detailed understanding of the sales data.

## Queries and Results

1. **Total Number of Orders Placed**
    ```sql
    SELECT 
    COUNT(order_id) AS total_orders
FROM
    orders;
    ```
    - **Result:** Total number of orders placed - 21350.

2. **Total Revenue Generated from Pizza Sales**
    ```sql
  SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_sale
FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id;
    ```
    - **Result:** Total revenue generated- 817860.05.

3. **Highest-Priced Pizza**
    ```sql
   SELECT 
    pizza_types.name, pizzas.price AS highest_priced_pizza
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
    ```
    - **Result:** Highest-priced pizza and its price - The Greek Pizaa - 35.95.

4. **Most Common Pizza Size Ordered**
    ```sql
   SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;
    ```
    - **Result:** Most common pizza size ordered = L - 18526.

5. **Top 5 Most Ordered Pizza Types**
    ```sql
SELECT 
    pizza_types.name, SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
    ```
    - **Result:** List of the top 5 most ordered pizza types and their quantities.

6. **Total Quantity of Each Pizza Category Ordered**
    ```sql
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
    ```
    - **Result:** Total quantity ordered for each pizza category.

7. **Distribution of Orders by Hour of the Day**
    ```sql
  SELECT 
    HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM
    orders
GROUP BY hour;
    ```
    - **Result:** Number of orders placed each hour.

8. **Category-wise Distribution of Pizzas**
    ```sql
SELECT 
    category, COUNT(pizza_type_id) AS quantity
FROM
    pizza_types
GROUP BY category
ORDER BY quantity DESC; 
    ```
    - **Result:** Number of orders for each pizza category.

9. **Average Number of Pizzas Ordered Per Day**
    ```sql
SELECT 
    ROUND(AVG(quantity), 0) AS avg_pizza_order_per_day
FROM
    (SELECT 
        orders.order_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date) AS order_quantity;
    ```
    - **Result:** Average number of pizzas ordered per day.

10. **Top 3 Most Ordered Pizza Types Based on Revenue**
    ```sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
    ```
    - **Result:** Top 3 most ordered pizza types by revenue.

11. **Percentage Contribution of Each Pizza Type to Total Revenue**
    ```sql
select pizza_types.category,
round(sum(order_details.quantity * pizzas.price) /(SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_sale
FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id) *100,2) as revenue_percentage
from pizza_types join pizzas
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join order_details
on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.category order by revenue_percentage desc;
    ```
    - **Result:** Percentage contribution of each pizza type to total revenue.

12. **Cumulative Revenue Generated Over Time**
    ```sql
select order_date,
round(sum(revenue) over (order by order_date),2) as cum_revenue
from 
(select orders.order_date, 
sum(order_details.quantity * pizzas.price) as revenue
from order_details join pizzas 
on order_details.pizza_id = pizzas.pizza_id
join orders on orders.order_id = order_details.order_id
group by orders.order_date) as sales;
    ```
    - **Result:** Cumulative revenue generated over time.

13. **Top 3 Most Ordered Pizza Types by Revenue for Each Category**
    ```sql
select name, revenue
from
(select category, name, revenue,
rank() over (partition by category order by revenue desc) as rn
from
(select pizza_types.category,pizza_types.name,
sum((order_details.quantity) * pizzas.price) as revenue
from pizza_types join pizzas
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join order_details
on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.category,pizza_types.name) as a) as b
where rn <= 3;
    ```
    - **Result:** Top 3 most ordered pizza types by revenue for each category.


