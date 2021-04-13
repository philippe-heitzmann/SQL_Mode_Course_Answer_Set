## SQL Mode Course Answer Key 


### Intermediate



#### SQL Aggregate Functions - COUNT


1. Write a query to count the number of non-null rows in the low column.


select count(low) from tutorial.aapl_historical_stock_price; 


2. Write a query that determines counts of every single column. Which column has the most null values?


SELECT COUNT(year) AS year,
       COUNT(month) AS month,
       COUNT(open) AS open,
       COUNT(high) AS high,
       COUNT(low) AS low,
       COUNT(close) AS close,
       COUNT(volume) AS volume FROM tutorial.aapl_historical_stock_price;



#### SQL Aggregate Functions - SUM


1. Write a query to calculate the average opening price (hint: you will need to use both COUNT and SUM, as well as some simple arithmetic.).


select sum(open) / count(open) as ave_open 
from tutorial.aapl_historical_stock_price;



#### SQL Aggregate Functions - MIN/MAX


1. What was Apple's lowest stock price (at the time of this data collection)?

select min(low) from tutorial.aapl_historical_stock_price;


1. What was the highest single-day increase in Apple's share value?

select max(close - open) from tutorial.aapl_historical_stock_price; 



