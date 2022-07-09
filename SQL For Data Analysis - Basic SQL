-- 1. Basics of SQL - SELECT, FROM, WHERE,HAVING, ORDER BY, LIMIT, Aliases and OPERATORS


SELECT *
FROM orders

 SELECT id, occurred_at, total_amt_usd
  FROM  orders
ORDER BY occurred_at
LIMIT 10

SELECT id, account_id, total_amt_usd
  FROM  orders
ORDER BY total_amt_usd DESC
LIMIT 5

SELECT id, account_id, total_amt_usd
  FROM  orders
ORDER BY total_amt_usd
LIMIT 20

SELECT id, account_id, total_amt_usd
  FROM orders
ORDER BY account_id, total_amt_usd DESC

SELECT *
  FROM orders
   WHERE gloss_amt_usd >= 1000
    ORDER BY gloss_amt_usd DESC
LIMIT 5

SELECT name, website, primary_poc
  FROM accounts
   WHERE name = 'Exxon Mobil'

SELECT id, account_id, (standard_amt_usd/standard_qty) AS unit_price
  FROM orders
LIMIT 10

SELECT id, account_id,
       poster_amt_usd/(standard_amt_usd + gloss_amt_usd + poster_amt_usd) AS post_per
  FROM orders
LIMIT 10;

SELECT *
  FROM accounts
WHERE name LIKE 'C%'

SELECT *
  FROM accounts
WHERE name LIKE '%s'

SELECT *
  FROM accounts
WHERE name NOT LIKE '%s'

SELECT name, primary_poc, sales_rep_id
  FROM accounts
WHERE name IN ('Walmart','Target', 'Nordstrom')

SELECT name, primary_poc, sales_rep_id
  FROM accounts
WHERE name NOT IN ('Walmart','Target', 'Nordstrom')

SELECT *
  FROM web_events
WHERE channel IN ('organic','adwords')

SELECT *
  FROM orders
WHERE standard_qty > 1000 AND poster_qty = 0 AND gloss_qty = 0

SELECT *
FROM accounts
WHERE name NOT LIKE 'C%' AND name LIKE '%s'

SELECT occurred_at, gloss_qty
FROM orders
WHERE gloss_qty BETWEEN 24 AND 29

SELECT *
FROM web_events  -- To query any order that occur anytime in 2015 and channel name is either organic or adwords
WHERE channel IN ('organic', 'adwords') AND occured_at BETWEEN '2015-01-01' AND '2016-01-01'

SELECT *
FROM web_events
WHERE channel IN ('organic', 'adwords') AND
        occurred_at BETWEEN '2016-01-01' AND '2017-01-01' -- To query any order that occur anytime in 2016
ORDER BY occurred_at DESC

SELECT id, gloss_qty, poster_qty
FROM orders
WHERE gloss_qty = 4000 OR poster_qty > 4000

SELECT *
  FROM orders
  WHERE (gloss_qty > 1000 OR poster_qty > 1000)
   AND standard_qty = 0

SELECT *
  FROM accounts
  WHERE (name LIKE 'C%' OR name LIKE 'W%')
     	AND ((primary_poc LIKE '%ana%' OR primary_poc LIKE '%Ana%')
        AND primary_poc NOT LIKE '%eana%')

-- 2. Dealing With JOINS

SELECT *
FROM orders AS oh
INNER JOIN accounts AS ac
ON oh.account_id = ac.id

SELECT oh.standard_qty, oh.gloss_qty, oh.poster_qty, ac.website, ac.primary_poc
FROM orders AS oh
INNER JOIN accounts AS ac
ON oh.account_id = ac.id

SELECT ac.primary_poc, we.occurred_at, we.channel, ac.name
FROM web_events AS we
INNER JOIN accounts AS ac
ON ac.id = we.account_id
WHERE ac.name = 'Walmart'

SELECT ac.name AS account, sr.name AS sales_rep, re.name AS region
FROM sales_reps AS sr
INNER JOIN accounts AS ac
ON ac.sales_rep_id = sr.id
INNER JOIN region AS re
ON re.id= sr.region_id
ORDER BY ac.name

SELECT sr.name AS sales_rep, ac.name AS account, re.name AS region, (oh.total_amt_usd/(total+0.01)) AS unit_price
FROM sales_reps AS sr
INNER JOIN accounts AS ac
ON ac.sales_rep_id = sr.id
INNER JOIN region AS re
ON re.id= sr.region_id
INNER JOIN orders AS oh
ON ac.id = oh.account_id


SELECT re.name AS region, sr.name sales_rep, ac.name AS account_name
FROM sales_reps AS sr
INNER JOIN region AS re
ON re.id = sr.region_id
INNER JOIN accounts ac
ON sr.id = ac.sales_rep_id
AND re.name = 'Midwest'
ORDER BY ac.name

SELECT re.name AS region, sr.name sales_rep, ac.name AS account_name
FROM sales_reps AS sr
INNER JOIN region AS re
ON re.id = sr.region_id
INNER JOIN accounts ac
ON sr.id = ac.sales_rep_id
AND (re.name = 'Midwest') AND (sr.name LIKE 'S%')
ORDER BY ac.name

SELECT re.name AS region, sr.name sales_rep, ac.name AS account_name
FROM sales_reps AS sr
INNER JOIN region AS re
ON re.id = sr.region_id
INNER JOIN accounts ac
ON sr.id = ac.sales_rep_id
AND (re.name = 'Midwest') AND (sr.name LIKE '% K%')
ORDER BY ac.name

SELECT re.name AS region, ac.name AS account_name, (oh.total_amt_usd/oh.total + 0.01) AS unit_price
FROM sales_reps AS sr
INNER JOIN region AS re
ON re.id = sr.region_id
INNER JOIN accounts ac
ON sr.id = ac.sales_rep_id
INNER JOIN orders AS oh
ON oh.account_id = ac.id
AND (oh.standard_qty > 100) AND (oh.poster_qty > 50)
ORDER BY unit_price ASC

SELECT DISTINCT ac.name AS account_name, we.channel, ac.id AS account_id
FROM accounts AS ac
INNER JOIN web_events AS we
ON ac.id = we.account_id
WHERE account_id = 1001

SELECT DISTINCT oh.occurred_at, ac.name AS account_name, oh.total, oh.total_amt_usd
FROM orders AS oh
INNER JOIN accounts AS ac
ON ac.id = oh.account_id
WHERE occurred_at BETWEEN '2015-01-01'
     AND '2016-01-01'
ORDER BY occurred_at DESC

-- 3. SQL AGGREATIONS AND FUNCTIONS
SELECT SUM(poster_qty) AS Total_poster_qty
FROM orders

SELECT SUM(standard_amt_usd)/SUM(standard_qty) AS unit_price_per_qty
FROM orders;

SELECT MIN(occurred_at)
FROM  orders

SELECT occurred_at   -- Performs same function as previous query to retrieve earliest order date
FROM  orders
ORDER BY occurred_at ASC
LIMIT 1

SELECT occurred_at
FROM  web_events
ORDER BY occurred_at DESC
LIMIT 1

SELECT MAX(occurred_at)
FROM  web_events   -- Performs same function as previous query to retrieve latest date in the events table

SELECT AVG(standard_qty) AS mean_std_qty, AVG(gloss_qty) AS mean_gls_qty, AVG(poster_qty) AS mean_pst_qty,
 AVG(standard_amt_usd) AS mean_std_amt, AVG(gloss_amt_usd) AS mean_gls_amt, AVG(poster_amt_usd) AS _mean_pst_amt
FROM  orders;

SELECT ac.name, MIN(oh.occurred_at) as Earliest_order_date
FROM accounts AS ac
INNER JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.name
ORDER BY Earliest_order_date ASC

SELECT ac.name, SUM(total_amt_usd) AS total_sales
FROM accounts AS ac
INNER JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.name
ORDER BY total_sales DESC

SELECT ac.name, we.channel, we.occurred_at AS Latest_web_event
FROM accounts AS ac
INNER JOIN web_events as we
ON ac.id = we.account_id
ORDER BY Latest_web_event DESC
LIMIT 1


SELECT ac.name, we.channel, MAX(we.occurred_at) AS Latest_web_event
FROM accounts AS ac
INNER JOIN web_events as we
ON ac.id = we.account_id   -- Performs same function as previous query to retrieve latest date in the events table
GROUP BY ac.name, we.channel
ORDER BY Latest_web_event DESC
LIMIT 1

SELECT ac.name, ac.primary_poc, we.channel, MIN(we.occurred_at) AS earliest_web_event
FROM accounts AS ac
INNER JOIN web_events as we
ON ac.id = we.account_id
GROUP BY ac.name, we.channel, ac.primary_poc
ORDER BY earliest__web_event ASC
LIMIT 1

SELECT COUNT(*) AS no_of_channel_1, COUNT(channel) AS no_of_channel_2
FROM web_events
ORDER BY no_of_channel_2 DESC

SELECT channel, COUNT(*) AS no_of_channel_1, COUNT(channel) AS no_of_channel_2
FROM web_events
GROUP BY channel
ORDER BY no_of_channel_1 DESC

SELECT ac.name, oh.total_amt_usd
FROM accounts AS ac
INNER JOIN orders AS oh
ON ac.id = oh.account_id
WHERE ac.name = 'EOG Resources'
ORDER BY oh.total_amt_usd DESC

SELECT re.name, COUNT(sa.name) AS no_of_sales_rep
FROM region AS re
INNER JOIN sales_reps AS sa
ON re.id = sa.region_id
GROUP BY re.name
ORDER BY no_of_sales_rep ASC

SELECT ac.name, AVG(oh.standard_amt_usd) AS avg_standard_amt,
        AVG(oh.gloss_amt_usd) AS avg_gloss_amt, AVG(oh.poster_amt_usd) AS avg_poster_amt
FROM accounts AS ac
INNER JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.name

SELECT sr.name, we.channel, count(we.channel) AS channel_occurence
FROM web_events AS we
INNER JOIN accounts AS ac
ON we.account_id = ac.id
INNER JOIN sales_reps AS sr
ON ac.sales_rep_id = sr.id
GROUP BY sr.name, we.channel
ORDER BY channel_occurence DESC

SELECT re.name, we.channel, count(we.channel) AS num_occurence
FROM web_events AS we
INNER JOIN accounts AS ac
ON we.account_id = ac.id
INNER JOIN sales_reps AS sr
ON ac.sales_rep_id = sr.id
INNER JOIN region as re
ON sr.region_id = re.id
GROUP BY re.name, we.channel
ORDER BY re.name, num_occurence DESC


-- USING HAVING CLAUSE
SELECT DISTINCT id, name
FROM sales_reps

SELECT sr.id, sr.name, COUNT(*) AS num_accounts
FROM accounts AS ac
JOIN sales_reps AS sr
ON sr.id = ac.sales_rep_id
GROUP BY sr.id, sr.name
HAVING COUNT(*) > 5
ORDER BY num_accounts;

SELECT ac.id, ac.name, COUNT(*) AS num_accounts
FROM accounts AS ac
JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.id, ac.name
HAVING COUNT(*) > 20
ORDER BY num_accounts;

SELECT ac.id, ac.name, COUNT(*) AS num_accounts
FROM accounts AS ac
JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.id, ac.name
ORDER BY num_accounts DESC
LIMIT 1

SELECT ac.id, ac.name, SUM(oh.total_amt_usd) AS total_spent
FROM accounts AS ac
JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.id, ac.name
HAVING SUM(oh.total_amt_usd)> 30000
ORDER BY total_spent

SELECT ac.id, ac.name, SUM(oh.total_amt_usd) AS total_spent
FROM accounts AS ac
JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.id, ac.name
HAVING SUM(oh.total_amt_usd) < 1000
ORDER BY total_spent DESC

SELECT ac.id, ac.name, SUM(oh.total_amt_usd) AS total_spent
FROM accounts AS ac
JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.id, ac.name
ORDER BY total_spent DESC
LIMIT 1

SELECT ac.id, ac.name, SUM(oh.total_amt_usd) AS total_spent
FROM accounts AS ac
JOIN orders AS oh
ON ac.id = oh.account_id
GROUP BY ac.id, ac.name
ORDER BY total_spent ASC
LIMIT 1

SELECT ac.id, ac.name, we.channel, COUNT(*) AS use_of_channel
FROM accounts AS ac
JOIN web_events AS we
ON ac.id = we.account_id
GROUP BY ac.id, ac.name, we.channel
HAVING COUNT(*) > 6 AND we.channel = 'facebook'
ORDER BY use_of_channel

SELECT ac.id, ac.name, we.channel, COUNT(*) AS use_of_channel
FROM accounts AS ac
JOIN web_events AS we
ON ac.id = we.account_id
WHERE we.channel = 'facebook'
GROUP BY ac.id, ac.name, we.channel
ORDER BY use_of_channel DESC
LIMIT 1

SELECT ac.id, ac.name, we.channel, COUNT(*) AS use_of_channel
FROM accounts AS ac
JOIN web_events AS we
ON ac.id = we.account_id
GROUP BY ac.id, ac.name, we.channel
ORDER BY use_of_channel DESC
LIMIT 5

-- DATE FUNCTIONS
SELECT DATE_PART('year',occurred_at) AS  year, SUM(total_amt_usd) AS total_sales
FROM orders
GROUP BY year
ORDER BY total_sales DESC


SELECT DATE_PART('month',occurred_at) AS  month, SUM(total_amt_usd) AS total_sales
FROM orders
WHERE occurred_at BETWEEN '2014-01-01' AND '2017-01-01'
GROUP BY month   -- execluding some years to accommodate fairness
ORDER BY total_sales DESC

SELECT DATE_PART('year',occurred_at) AS  year, COUNT(*) AS total_sales
FROM orders
GROUP BY year
ORDER BY total_sales DESC

SELECT DATE_PART('month',occurred_at) AS  month, COUNT(*) AS total_sales
FROM orders
WHERE occurred_at BETWEEN '2014-01-01' AND '2017-01-01'
GROUP BY month  -- execluding some years to accommodate fairness
ORDER BY total_sales DESC

SELECT DATE_PART('month',oh.occurred_at) AS  month, ac.name, SUM(oh.gloss_amt_usd) AS total_gloss_sales
FROM orders AS oh
INNER JOIN accounts AS ac
ON ac.id = oh.account_id
WHERE ac.name = 'Walmart'
GROUP BY month, ac.name
ORDER BY total_gloss_sales DESC
LIMIT 1

-- CASE STATEMENT
SELECT account_id, total_amt_usd,
		CASE WHEN total_amt_usd >= 3000 THEN 'Large'
          ELSE 'Small'
    END AS order_level
FROM orders
ORDER BY total_amt_usd DESC
LIMIT 20

SELECT
   CASE WHEN total >= 2000 THEN 'At Least 2000'
        WHEN total >= 1000 AND total < 2000 THEN 'Between 1000 and 2000'
      ELSE 'Less than 1000'
   END AS Level, COUNT(*) as order_count
FROM orders
GROUP BY 1
ORDER BY order_count DESC

SELECT  ac.name, SUM(oh.total_amt_usd) as total_sales,
   CASE
       WHEN SUM(oh.total_amt_usd) > 200000 THEN 'Greater than 200000'
       WHEN SUM(oh.total_amt_usd) >= 100000 AND SUM(oh.total_amt_usd) < 200000 THEN 'Between 100000 and 200000'
    ELSE 'Under 100000'
  END AS customer_level
FROM orders AS oh
INNER JOIN accounts AS ac
ON ac.id = oh.account_id
GROUP BY ac.name
ORDER BY total_sales DESC

SELECT  ac.name, SUM(oh.total_amt_usd) as total_sales,
   CASE
      WHEN SUM(oh.total_amt_usd) > 200000 THEN 'Greater than 200000'
      WHEN SUM(oh.total_amt_usd) >= 100000 AND SUM(oh.total_amt_usd) < 200000 THEN 'Between 100000 and 200000'
   ELSE 'Under 100000'
  END AS customer_level
FROM orders AS oh
INNER JOIN accounts AS ac
ON ac.id = oh.account_id
WHERE DATE_PART('year', oh.occurred_at) BETWEEN '2016' AND '2017'
GROUP BY ac.name
ORDER BY total_sales DESC

SELECT  sr.name, COUNT(*) as num_of_orders,
   CASE
     WHEN COUNT(*) > 200 THEN 'Top'
    ELSE 'Not'
   END AS top_not
FROM orders AS oh
INNER JOIN accounts AS ac
ON ac.id = oh.account_id
INNER JOIN sales_reps AS sr
ON ac.sales_rep_id = sr.id
GROUP BY sr.name
ORDER BY num_of_orders DESC

SELECT  sr.name, COUNT(*) AS num_of_orders, SUM(total_amt_usd) AS total_sales,
   CASE
     WHEN COUNT(*) > 200 OR SUM(total_amt_usd) > 750000 THEN 'Top'
     WHEN COUNT(*) > 150 OR COUNT(*) <= 200 AND SUM(total_amt_usd) > 500000 THEN 'Middle'
    ELSE 'Not'
  END AS sales_rep_type
FROM orders AS oh
INNER JOIN accounts AS ac
ON ac.id = oh.account_id
INNER JOIN sales_reps AS sr
ON ac.sales_rep_id = sr.id
GROUP BY sr.name
ORDER BY total_sales DESC,num_of_orders DESC

-- 4. SUBQUERIES and COMMON TABLE EXPRESSIONS(CTEs)
SELECT channel, AVG(event_count) avg_event_count
FROM
     (SELECT DATE_TRUNC('day', occurred_at) AS day, channel,
             COUNT(*) AS event_count
      FROM web_events
      GROUP BY 1,2) AS sub
GROUP BY 1  -- Remember to Format and Document your SQL Code especially SUB-QUERIES
ORDER BY 2 DESC

SELECT AVG(standard_qty) AS avg_std_qty, AVG(gloss_qty) AS avg_glo_qty,
        AVG(poster_qty) AS avg_pos_qty, SUM(total_amt_usd) AS avg_total_amt
FROM orders
WHERE DATE_TRUNC('month', occurred_at) =
              (SELECT MIN(DATE_TRUNC('month', occurred_at))
               FROM orders)


-- COMMON TABLE EXPRESSIONS CTEs
-- You need to find the average number of events for each channel per day.
WITH events AS (
          SELECT DATE_TRUNC('day',occurred_at) AS day,
                 channel, COUNT(*) as events
          FROM web_events
          GROUP BY 1,2)

SELECT channel, AVG(events) AS average_events
FROM events
GROUP BY channel
ORDER BY 2 DESC;

-- Question 1. Provide the name of the sales_rep in each region with the largest amount of total_amt_usd sales.

SELECT t3.rep_name, t3.region_name, t3.total_amt
FROM(SELECT region_name, MAX(total_amt) total_amt
     FROM(SELECT s.name rep_name, r.name region_name,
                  SUM(o.total_amt_usd) total_amt, COUNT(*)
             FROM sales_reps s
             JOIN accounts a
             ON a.sales_rep_id = s.id
             JOIN orders o
             ON o.account_id = a.id
             JOIN region r
             ON r.id = s.region_id
             GROUP BY 1, 2) t1
     GROUP BY 1) t2
JOIN (SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
     FROM sales_reps s
     JOIN accounts a
     ON a.sales_rep_id = s.id
     JOIN orders o
     ON o.account_id = a.id
     JOIN region r
     ON r.id = s.region_id
     GROUP BY 1,2
     ORDER BY 3 DESC) t3
ON t3.region_name = t2.region_name AND t3.total_amt = t2.total_amt

SELECT t3.rep_name, t3.region_name, t3.total_amt, t2.num_of_orders
FROM(SELECT region_name, MAX(total_amt) total_amt
     FROM(SELECT s.name rep_name, r.name region_name, num_of_orders
                  SUM(o.total_amt_usd) total_amt, COUNT(*) AS num_of_orders
             FROM sales_reps s
             JOIN accounts a
             ON a.sales_rep_id = s.id
             JOIN orders o
             ON o.account_id = a.id
             JOIN region r
             ON r.id = s.region_id
             GROUP BY 1, 2) t1
     GROUP BY 1) t2
JOIN (SELECT s.name rep_name, r.name region_name, num_of_orders
                  SUM(o.total_amt_usd) total_amt, COUNT(*) AS num_of_orders
     FROM sales_reps s
     JOIN accounts a
     ON a.sales_rep_id = s.id
     JOIN orders o
     ON o.account_id = a.id
     JOIN region r
     ON r.id = s.region_id
     GROUP BY 1,2
     ORDER BY 3 DESC) t3
ON t3.region_name = t2.region_name AND t3.total_amt = t2.total_amt


-- 5. SQL Data Cleansing

--  Pull these extensions and provide how many of each website type exist in the accounts table.
SELECT RIGHT(website, 3) AS extension, COUNT(*) AS num_ext
FROM accounts
GROUP BY 1
ORDER BY 2 DESC

/*Use the accounts table to pull the first letter of each
company name to see the distribution of company names that begin with each letter (or number).*/
SELECT LEFT(UPPER(name), 1) as company_alias,  COUNT(*) AS num_company_alias
FROM accounts
GROUP BY 1
ORDER BY 2 DESC

/* one group of company names that start with a number and a second group of
those company names that start with a letter. What proportion of company
names start with a letter?*/ -- ANS = 99.7%
SELECT CASE
    WHEN name >= '0' AND name <= '9' THEN 'number'
      ELSE 'Letter'
    END AS name_type, COUNT(*) AS num_com
FROM accounts
GROUP BY name_type
ORDER BY num_com DESC

/* Consider vowels as a, e, i, o, and u. What proportion of company names start
 with a vowel, and what percent start with anything else?*/ -- ANS = 29.5%
SELECT CASE
    WHEN LEFT(UPPER(name),1) = 'A' OR LEFT(UPPER(name),1) = 'E' OR LEFT(UPPER(name),1) = 'I'
        OR LEFT(UPPER(name),1) = 'O' OR LEFT(UPPER(name),1) = 'U' THEN 'vowels'
     ELSE 'Others'
    END AS name_type, COUNT(*) AS num_com
FROM accounts
GROUP BY name_type
ORDER BY num_com DESC

/* Use the accounts table to create first and last name columns that hold the
   first and last names for the primary_poc. */
SELECT primary_poc,
POSITION(' ' IN primary_poc) AS pos_frm_left,
LENGTH(primary_poc)-POSITION(' ' IN primary_poc) AS pos_frm_right,
LEFT(primary_poc, POSITION(' ' IN primary_poc)-1) AS first_name,
RIGHT(primary_poc, LENGTH(primary_poc)-POSITION(' ' IN primary_poc)) AS last_name
FROM accounts

/* Use the accounts table to create first and last name columns that
   hold the first and last names for the primary_poc. */
SELECT name,
LEFT(name, POSITION(' ' IN name)-1) AS first_name,
RIGHT(name, LENGTH(name)-POSITION(' ' IN name)) AS last_name
FROM sales_reps

/* Each company in the accounts table wants to create an email address for each primary_poc.
   The email address should be the first name of the primary_poc . last name primary_poc
   @ company name .com.*/
SELECT company_name, first_name || '.'|| last_name || '@' || company_name || '.com' AS email_address
FROM  (SELECT name AS company_name, primary_poc, LEFT(primary_poc, POSITION(' ' IN primary_poc)-1)
          AS first_name, RIGHT(primary_poc, LENGTH(primary_poc)-POSITION(' ' IN primary_poc))
          AS last_name
       FROM accounts) temp_1

/* See if you can create an email address that will work by removing all of the spaces in the account name,
  but otherwise your solution should be just as in question 1. */
SELECT company_name, first_name || '.'|| last_name || '@' || company_name || '.com' AS email_address
FROM   (SELECT REPLACE(name,' ','') AS company_name, primary_poc,
         LEFT(primary_poc, POSITION(' ' IN primary_poc)-1) AS first_name, RIGHT(primary_poc,
          LENGTH(primary_poc)-POSITION(' ' IN primary_poc)) AS last_name
        FROM accounts) AS temp_1

/* We would also like to create an initial password, which they will change after their first log in.
  The first password will be the first letter of the primary_poc's first name (lowercase),
  then the last letter of their first name (lowercase), the first letter of their last name (lowercase),
  the last letter of their last name (lowercase), the number of letters in their first name,
  the number of letters in their last name, and then the name of the company they are working with,
  all capitalized with no spaces. */

SELECT first_name, last_name, CONCAT(first_letter_first_name,last_letter_first_name,first_letter_last_name,
      last_letter_last_name,len_first_name, len_last_name,company_name) AS password,
      first_name || '.'|| last_name || '@' || company_name || '.com' AS email_address
FROM  (SELECT first_name, last_name, SUBSTRING(first_name,1,1) AS first_letter_first_name,
       RIGHT(first_name, 1) AS last_letter_first_name, LEFT(last_name,1) AS first_letter_last_name,
       RIGHT(last_name, 1) AS last_letter_last_name, -- Both SUBSTRING and LEFT/RIGHT fx can be used here
       LENGTH(first_name) AS len_first_name, LENGTH(last_name) AS len_last_name,
       UPPER(company_name) AS company_name
       FROM   (SELECT REPLACE(name,' ','') AS company_name, primary_poc,
               LOWER(LEFT(primary_poc, POSITION(' ' IN primary_poc)-1)) AS first_name, LOWER(RIGHT(primary_poc,
               LENGTH(primary_poc)-POSITION(' ' IN primary_poc))) AS last_name
              FROM accounts) AS temp_1) AS temp_2

/* Write a query to change the date into a correct SQL date format using CTEs */
WITH temp AS
      (SELECT date, SUBSTRING(date,7,4) AS year, SUBSTRING(date,4,2) AS month, SUBSTRING(date,1,2) AS day
       FROM sf_crime_data)      -- using CTE, SUBSTRING, CAST, CONCAT
SELECT date AS org_date, CAST(CONCAT(year,'-',day,'-',month) AS date) AS formatted_date
FROM temp
LIMIT 10

/* Write a query to change the date into a correct SQL date format using SUB-QUERIES */
SELECT date AS org_date, (year || '-' || day ||'-' || month)::DATE AS formatted_date
FROM  (SELECT date, SUBSTR(date,7,4) AS year, SUBSTR(date,4,2) AS month, SUBSTR(date,1,2) AS day
      FROM sf_crime_data) AS temp  -- using SUB-QUERIES, SUBSTRING, PIPING
LIMIT 10

-- 6. WINDOW Functions

/*  Create a running total of standard_amt_usd (in the orders table) over
    order time with no date truncation. */
SELECT standard_amt_usd,
       SUM(standard_amt_usd) OVER(ORDER BY occurred_at) AS ruuning_std_usd_amt
FROM orders

/* Create a running total of standard_amt_usd (in the orders table) over order time, but this time, date truncate
   occurred_at by year and partition by that same year-truncated occurred_at variable. */
SELECT standard_amt_usd, DATE_TRUNC('year',occurred_at) AS year,
       SUM(standard_amt_usd) OVER(PARTITION BY DATE_TRUNC('year',occurred_at)
       ORDER BY occurred_at DESC) AS ruuning_std_usd_amt
FROM orders

/* Create a running total of standard_amt_usd (in the orders table) over order time, but this time, date truncate
   occurred_at by month and partition by that same year-truncated occurred_at variable. */
SELECT standard_amt_usd, DATE_TRUNC('month',occurred_at) AS month,
       SUM(standard_amt_usd) OVER(PARTITION BY DATE_TRUNC('month',occurred_at)
       ORDER BY occurred_at) AS ruuning_std_usd_amt
FROM orders

/* Select the id, account_id, and total variable from the orders table, then create a column called
   total_rank that ranks this total amount of paper ordered (from highest to lowest)
   for each account using a partition */
SELECT id, account_id, total,
ROW_NUMBER() OVER(PARTITION BY account_id ORDER BY total DESC
		) AS total_rank_using_row_num,
RANK() OVER(PARTITION BY account_id ORDER BY total DESC
		) AS total_rank_using_rank,
DENSE_RANK() OVER(PARTITION BY account_id ORDER BY total DESC
		) AS total_rank_using_dense_rank
FROM orders

/* NOTE: The ORDER BY clause is one of two clauses integral to window functions. The ORDER
  and PARTITION define what is referred to as the “window” — the ordered subset of
  data over which calculations are made. Removing ORDER BY just leaves an unordered partition */

/* WINDOW Functions can be Aliased using a WINDOW Clause and it goes immediately
   after WHERE Clause and before GROUP BY Clause */

SELECT id,
       account_id,
       DATE_TRUNC('year',occurred_at) AS year,
       DENSE_RANK() OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at)) AS dense_rank,
       total_amt_usd,
       SUM(total_amt_usd) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at)) AS sum_total_amt_usd,
       COUNT(total_amt_usd) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at)) AS count_total_amt_usd,
       AVG(total_amt_usd) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at)) AS avg_total_amt_usd,
       MIN(total_amt_usd) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at)) AS min_total_amt_usd,
       MAX(total_amt_usd) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at)) AS max_total_amt_usd
FROM orders

/*  create and use an alias to shorten the following query (which is different than
   the one in Derek's previous video) that has multiple window functions. Name the alias account_year_window */
SELECT id,
       account_id,
       DATE_TRUNC('year',occurred_at) AS year,
       DENSE_RANK() OVER account_year_window AS dense_rank,
       total_amt_usd,
       SUM(total_amt_usd) OVER account_year_window AS sum_total_amt_usd,
       COUNT(total_amt_usd) OVER account_year_window AS count_total_amt_usd,
       AVG(total_amt_usd) OVER account_year_window AS avg_total_amt_usd,
       MIN(total_amt_usd) OVER account_year_window AS min_total_amt_usd,
       MAX(total_amt_usd) OVER account_year_window AS max_total_amt_usd
FROM orders
WINDOW account_year_window AS (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at))

-- LAG:  Returns the value from a previous row to the current row in the table.
-- LEAD: Return the value from the row following the current row in the table.
-- USE CASES: You can use LAG and LEAD functions whenever you are trying to compare the
-- values in adjacent rows or rows that are offset by a certain number.

-- LAG
SELECT account_id,
       standard_sum,
       LAG(standard_sum) OVER (ORDER BY standard_sum) AS lag,
       standard_sum - LAG(standard_sum) OVER (ORDER BY standard_sum) AS lag_difference
FROM (
       SELECT account_id,
       SUM(standard_qty) AS standard_sum
       FROM orders
       GROUP BY 1
      ) sub

-- LEAD
SELECT account_id,
       standard_sum,
       LEAD(standard_sum) OVER (ORDER BY standard_sum) AS lead,
       LEAD(standard_sum) OVER (ORDER BY standard_sum) - standard_sum AS lead_difference
FROM (
SELECT account_id,
       SUM(standard_qty) AS standard_sum
       FROM orders
       GROUP BY 1
     ) sub

/*  This technique can be useful when analyzing time-based events. Imagine you're an analyst
    at Parch & Posey and you want to determine how the current order's total revenue
    ("total" meaning from sales of all types of paper) compares to the next order's total revenue. */
SELECT occurred_at,
     	 total_amt_usd,
       LEAD(total_amt_usd) OVER (ORDER BY occurred_at) as lead,
       LEAD(total_amt_usd) OVER (ORDER BY occurred_at)-total_amt_usd AS total_difference
FROM  (SELECT occurred_at,
           SUM(total_amt_usd) AS total_amt_usd
       FROM orders
      GROUP BY occurred_at) AS temp

-- NTILE
/* Use the NTILE functionality to divide the accounts into 4 levels in terms
   of the amount of standard_qty for their orders */
SELECT
       account_id,
       occurred_at,
       standard_qty,
       NTILE(4) OVER (PARTITION BY account_id ORDER BY standard_qty) AS standard_quartile
  FROM orders
 ORDER BY account_id DESC

/* Use the NTILE functionality to divide the accounts into two levels in terms
  of the amount of gloss_qty for their orders */
SELECT
       account_id,
       occurred_at,
       gloss_qty,
       NTILE(2) OVER (PARTITION BY account_id ORDER BY gloss_qty) AS gloss_half
FROM orders
ORDER BY account_id DESC

-- ADVANCED JOINS for EDGE USE CASES
-- INNER JOIN, LEFT OUTER JOIN, RIGHT OUTER JOIN and FULL OUTER JOIN

-- FULL OUTER JOIN
SELECT ac.*, sr.*
FROM accounts AS ac
FULL OUTER JOIN sales_reps AS sr
ON ac.sales_rep_id = sr.id
-- WHERE ac.sales_rep_id IS NULL OR sr.id IS NULL

/* In the following SQL Explorer, write a query that left joins the accounts table and the
   sales_reps tables on each sale rep's ID number and joins it using the < comparison operator
   on accounts.primary_poc and sales_reps.name */
SELECT *
FROM sales_reps AS sr
LEFT OUTER JOIN accounts AS ac
ON sr.id = ac.sales_rep_id
AND ac.primary_poc < sr.name

-- self join Use Case: Hierachical view/ Parent-Child Relationship
SELECT we1.id AS we1_id,
       we1.account_id AS we1_account_id,
       we1.occurred_at AS we1_occurred_at,
       we1.channel AS we1_channel,
       we2.id AS we2_id,
       we2.account_id AS we2_account_id,
       we2.occurred_at AS we2_occurred_at,
       we2.channel AS we2_channel
  FROM web_events as we1
 LEFT JOIN web_events as we2
   ON we1.account_id = we2.account_id
  AND we2.occurred_at > we1.occurred_at
  AND we2.occurred_at <= we1.occurred_at + INTERVAL '1 day'
ORDER BY we1.id, we1.occurred_at

/* UNION OPERATION: Same no of expressions in SELECT statement, Same Data Tyope for Corresponding Expressions in the SELECT
    Statement in the same order. It's like appending/stacking data. Just like MERGE is like JOINSin Power BI */
-- UNION: Removes duplicates
-- UNION ALL: Does not remove duplicates
SELECT *
FROM accounts AS ac1
UNION ALL
SELECT *
FROM accounts AS ac2

SELECT *
FROM accounts AS ac1
WHERE ac1.name = 'Walmart'
UNION ALL    -- This is same as SELECT * FROM accounts WHERE name='Walmart' OR name = 'Disney'
SELECT *
FROM accounts AS ac2
WHERE ac2.name = 'Disney'


WITH double_accounts AS
     (SELECT *
      FROM accounts AS ac1
       UNION ALL
      SELECT *
      FROM accounts AS ac2)

SELECT name, COUNT(name)
FROM double_accounts
GROUP BY 1
ORDER BY 2 DESC

/* PERFORMANCE TUNING: Table size, JOINS and Aggregations will affect query runtime
1. Filtering your data to only relevant data will dramatically improve Query Performance
2. Test query on a subset of data, finalize the query and then removing limitation is a sound strategy
3. using the LIMIT clause during explaration is Great(This doesnt apply to AGGREGATIONS)
4. With SUB-QUERIES, Limit data where it will be executed i.e in the inner query.
  Use this to test query result NOT actual results
5. Reduce the number of rows evaluated during joins by aggregating tables before joining
6. Worry about the accuracy of your query before run speed
7. Add EXPLAIN add the beginning of your query to get a feeling of how long it will take */
