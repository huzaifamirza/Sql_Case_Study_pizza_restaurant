# Sql_Case_Study_pizza_restaurant
<P>Danny is the owner of pizza_runner restaurant they have business questions but does not know how to answer it so we help danny and solve its business questions using 
  SQL. Solved danny question with the help of query and perform data analysis on the data provided by danny. This answers will helps danny alot to grow its business.</p>
  
<h3> Answering business question using SQL</h3>  
  
  
  <h6>CREATE SCHEMA pizza_runner;</h6>

<h6>use pizza_runner;</h6>
<h6>CREATE TABLE runners (
<h6>runner_id INTEGER,
<h6>registration_date DATE
<h6>);
<h6>INSERT INTO runners
  <h6>(runner_id, registration_date)
<h6>VALUES
  <h6>(1, '2021-01-01'),
  <h6>(2, '2021-01-03'),
  <h6>(3, '2021-01-08'),
  <h6>(4, '2021-01-15');


<h6>CREATE TABLE customer_orders (
  <h6>order_id INTEGER,
  <h6>customer_id INTEGER,
  <h6>pizza_id INTEGER,
  <h6>exclusions VARCHAR(4),
  <h6>extras VARCHAR(4),
  <h6>order_time TIMESTAMP
<h6>);

<h6>INSERT INTO customer_orders
  <h6>(order_id, customer_id, pizza_id, exclusions, extras, order_time)
<h6>VALUES
  <h6>('1', '101', '1', '', '', '2020-01-01 18:05:02'),
  <h6>('2', '101', '1', '', '', '2020-01-01 19:00:52'),
  <h6>('3', '102', '1', '', '', '2020-01-02 23:51:23'),
  <h6>('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
  <h6>('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  <h6>('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  <h6>('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
  <h6>('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
  <h6>('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
  <h6>('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
  <h6>('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
  <h6>('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
  <h6>('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
  <h6>('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');



<h6>CREATE TABLE runner_orders (
  <h6>order_id INTEGER,
  <h6>runner_id INTEGER,
  <h6>pickup_time VARCHAR(19),
  <h6>distance VARCHAR(7),
  <h6>duration VARCHAR(10),
  <h6>cancellation VARCHAR(23)
<h6>);

<h6>INSERT INTO runner_orders
  <h6>(order_id, runner_id, pickup_time, distance, duration, cancellation)
<h6>VALUES
  <h6>('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
  <h6>('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
  <h6>('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
  <h6>('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
  <h6>('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
  <h6>('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
  <h6>('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
  <h6>('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
  <h6>('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
  <h6>('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');


<h6>CREATE TABLE pizza_names (
  <h6>pizza_id INTEGER,
 <h6>pizza_name TEXT
<h6>);
<h6>INSERT INTO pizza_names
  <h6>(pizza_id, pizza_name)
<h6>VALUES
  <h6>(1, 'Meatlovers'),
  <h6>(2, 'Vegetarian');



<h6>CREATE TABLE pizza_recipes (
  <h6>pizza_id INTEGER,
  <h6>toppings TEXT
<h6>);
<h6>INSERT INTO pizza_recipes
  <h6>(pizza_id, toppings)
<h6>VALUES
  <h6>(1, '1, 2, 3, 4, 5, 6, 8, 10'),
  <h6>(2, '4, 6, 7, 9, 11, 12');



<h6>CREATE TABLE pizza_toppings (
  <h6>topping_id INTEGER,
  <h6>topping_name TEXT
<h6>);
<h6>INSERT INTO pizza_toppings
  <h6>(topping_id, topping_name)
<h6>VALUES
  <h6>(1, 'Bacon'),
  <h6>(2, 'BBQ Sauce'),
  <h6>(3, 'Beef'),
  <h6>(4, 'Cheese'),
  <h6>(5, 'Chicken'),
  <h6>(6, 'Mushrooms'),
  <h6>(7, 'Onions'),
  <h6>(8, 'Pepperoni'),
  <h6>(9, 'Peppers'),
  <h6>(10, 'Salami'),
  <h6>(11, 'Tomatoes'),
  <h6>(12, 'Tomato Sauce');
  

  
  # Data Cleaning
  CREATE TEMPORARY TABLE customer_order  
  SELECT order_id, customer_id, pizza_id, 
  CASE 
    WHEN exclusions IS null OR exclusions LIKE 'null' THEN ' '
    ELSE exclusions
    END AS exclusions,
  CASE 
    WHEN extras IS NULL or extras LIKE 'null' THEN ' '
    ELSE extras 
    END AS extras, 
  order_time
#INTO customer_order -- create TEMP TABLE
FROM customer_orders;

# Runners Order clean
Create Temporary Table runner_order
Select order_id, runner_id,
Case
  When pickup_time Is Null or pickup_time Like 'null' Then ' '
  Else pickup_time
  End As pickup_time,
Case
  When distance Like 'null' Then ' '
  When distance Like '%km' Then Trim('km' from distance)
  Else distance
  End As distance,
Case
  When duration Like 'null' Then ' '
  When duration Like '%mins' Then Trim('mins' from duration)
  When duration Like '%minute' Then Trim('minute' from duration)
  When duration Like '%minutes' Then Trim('minutes' from duration)
  Else duration
  End As duration,
Case
  When cancellation Is Null or cancellation Like 'null' Then ' '
  Else cancellation
  End As cancellation
  From runner_orders;
  
  
  # Part A pizza Metrics
  # Ques 1 : How many pizzas were ordered?
  Select Count(order_id) From customer_order;
  
  # Ques 2 : How many unique customer orders were made?
  Select Count(Distinct(order_id)) From customer_order;
  
  # Ques 3 : How many successful orders were delivered by each runner?
  Select runner_id, Count(runner_id) as total_orders From runner_order
  Where duration  not in (' ')
  Group By runner_id;
  
  # Ques 4 : How many of each type of pizza was delivered?
  Select p.pizza_name, Count(c.pizza_id) As total_order
  From customer_order c
  Join pizza_names p
  On c.pizza_id = p.pizza_id
  Join runner_order r
  On r.order_id = c.order_id
  Where r.distance != 0
  Group By p.pizza_name
  Order By 2 Desc;
  
  # Ques 5 : How many Vegetarian and Meatlovers were ordered by each customer?
  Select c.customer_id, p.pizza_name, Count(c.pizza_id) As total_order
  From customer_order c
  Join pizza_names p
  On c.pizza_id = p.pizza_id
  Group By c.customer_id, p.pizza_name
  Order By 3 Desc;
  
 # Ques 6 : What was the maximum number of pizzas delivered in a single order?
 Select order_id, Count(pizza_id) max_order From customer_order
 Group By order_id
 Order By 2 Desc
 Limit 1;
 
 # Ques 7 : For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
 Select c.customer_id,
 Sum(Case
	   When c.exclusions <> ' '  or c.extras <> ' ' Then 1
       Else 0 End) As At_least_one_change,
Sum(Case
     When c.exclusions = ' '  And c.extras = ' ' Then 1
     Else 0 End) As No_canges
From customer_order c
Join runner_order r
On c.order_id = r.order_id
Where r.distance != 0
Group By c.customer_id;

# Ques 8 : How many pizzas were delivered that had both exclusions and extras?
Select Sum(Case
              When exclusions Is Not Null And extras Is Not Null Then 1
              Else 0 End) As delivred
From customer_order c
join runner_order r
On c.order_id = r.order_id
Where r.distance >= 1 And c.exclusions <> ' ' And c.extras <> ' ';

# Ques 9 : What was the total volume of pizzas ordered for each hour of the day?
Select extract(hour from order_time) as hour_of_day, Count(order_id) 
From customer_order
Group By extract(hour from order_time);

# Part B. Runner and Customer Experience

# Ques 1 : How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
Select extract(week from registration_date) As Week_no, Count(runner_id) From runners
Group By extract(week from registration_date);

# Ques 2 : What was the average distance travelled for each customer?
Select c.customer_id, Avg(r.distance) avg_dist From customer_order c
Join runner_order r
On c.order_id = r.order_id
Group By c.customer_id;

# Ques 3 : What was the difference between the longest and shortest delivery times for all orders?
Select Max(duration) - Min(duration) As dff_between_long_short From runner_order
Where duration != ' ';

# Ques 4 :What is the successful delivery percentage for each runner?
Select runner_id, Round(100 * Sum(Case When distance = 0 Then 0 Else 1 End)/Count(*),0)AS success_perc
From runner_order
Group By runner_id;

# C. Ingredient Optimisation

# Ques 1 :
# What are the standard ingredients for each pizza?
Select p.pizza_name, t.topping_name From pizza_names p
Join pizza_recipes r
On p.pizza_id = r.pizza_id
Join pizza_toppings t
On r.toppings = t.topping_id
Group By p.pizza_name; 

# Ques 2 : What was the most commonly added extra?
Select extras, count(extras) From customer_order 
Where extras != 0
Group By extras;

# Ques 3 : What was the most common exclusion?
Select exclusions, count(exclusions) From customer_order 
Where exclusions != 0
Group By exclusions;

# D. Pricing and Ratings

# Ques 1 : If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?
Select p.pizza_name, Count(c.pizza_id) As total_order,
 Case
   when p.pizza_name = 'Meatlovers' Then 12*Count(c.pizza_id)
   when p.pizza_name = 'Vegetarian' Then 10*Count(c.pizza_id) End as total_price
  From customer_order c
  Join pizza_names p
  On c.pizza_id = p.pizza_id
  Join runner_order r
  On r.order_id = c.order_id
  Where r.distance != 0
  Group By p.pizza_name
  Order By 2 Desc;

# Ques 2 : What if there was an additional $1 charge for any pizza extras?
# Add cheese is $1 extra
Select p.pizza_name, Count(c.pizza_id) As total_order,
 Case
   when p.pizza_name = 'Meatlovers' Then 12*Count(c.pizza_id) + 1*Count(c.pizza_id)
   when p.pizza_name = 'Vegetarian' Then 10*Count(c.pizza_id) + 1*Count(c.pizza_id) End as total_price
  From customer_order c
  Join pizza_names p
  On c.pizza_id = p.pizza_id
  Join runner_order r
  On r.order_id = c.order_id
  Where r.distance != 0
  Group By p.pizza_name
  Order By 2 Desc;

             
  
  
  
  
  
  
  
