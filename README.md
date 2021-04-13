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


2. What was the highest single-day increase in Apple's share value?

select max(close - open) from tutorial.aapl_historical_stock_price; 


#### SQL Aggregate Functions - AVG

1. Write a query that calculates the average daily trade volume for Apple stock.

select avg(volume) as avg_volume from tutorial.aapl_historical_stock_price; 


#### SQL GROUP BY

1. Calculate the total number of shares traded each month. Order your results chronologically.

select year, month, sum(volume) as total_volume from tutorial.aapl_historical_stock_price group by year, month order by year, month; 


2. Write a query to calculate the average daily price change in Apple stock, grouped by year.

select year, avg(close - open) as avg_daily_change from tutorial.aapl_historical_stock_price group by year order by year;


3. Write a query that calculates the lowest and highest prices that Apple stock achieved each month.

select year, month, min(low) as min_low, max(high) as max_high from tutorial.aapl_historical_stock_price group by year, month order by year, month; 


#### SQL CASE

SELECT * FROM benn.college_football_players 

1. Write a query that includes a column that is flagged "yes" when a player is from California, and sort the results with those players first.

SELECT player_name, CASE WHEN state = 'CA' THEN 'yes' ELSE NULL END as is_from_CA from benn.college_football_players order by 2

2. Write a query that includes players' names and a column that classifies them into four categories based on height. Keep in mind that the answer we provide is only one of many possible answers, since you could divide players' heights in many ways.

SELECT player_name, height, 
CASE when height <= 70 then 'short' when height > 70 and height < 73 then 'medium' 
     when height >= 73 and height < 76 then 'tall' else 'very_tall' end as height_classification 
from benn.college_football_players; 

3. Write a query that selects all columns from benn.college_football_players and adds an additional column that displays the player's name if that player is a junior or senior.

select * , CASE when year = 'JR' or year = 'SR' then player_name else NULL end as is_jr_or_sr from benn.college_football_players;

4. Write a query that counts the number of 300lb+ players for each of the following regions: West Coast (CA, OR, WA), Texas, and Other (Everywhere else).

select case when state = 'TX' then 'Texas' when state in ('CA','OR','WA') then 'West' else 'Other' END as region, 
       count(1) as players
from benn.college_football_players
where weight >= 300
group by 1
order by 2;

5. Write a query that calculates the combined weight of all underclass players (FR/SO) in California as well as the combined weight of all upperclass players (JR/SR) in California.

select case when year in ('JR','SR') then 'upperclassman' else 'lowerclassman' end as year_classification,
sum(weight) as sum_weight
from benn.college_football_players
where state = 'CA'
group by 1
order by 2;

6. Write a query that displays the number of players in each state, with FR, SO, JR, and SR players in separate columns and another column for the total number of players. Order results such that states with the most players come first.

SELECT state,
       COUNT(CASE WHEN year = 'FR' THEN 1 ELSE NULL END) AS fr_count,
       COUNT(CASE WHEN year = 'SO' THEN 1 ELSE NULL END) AS so_count,
       COUNT(CASE WHEN year = 'JR' THEN 1 ELSE NULL END) AS jr_count,
       COUNT(CASE WHEN year = 'SR' THEN 1 ELSE NULL END) AS sr_count,
       COUNT(1) AS sum_players
  FROM benn.college_football_players
 GROUP BY state
 ORDER BY 6 DESC;

7. Write a query that shows the number of players at schools with names that start with A through M, and the number at schools with names starting with N - Z.

SELECT CASE WHEN school_name < 'n' THEN 'A-M'
            WHEN school_name >= 'n' THEN 'N-Z'
            ELSE NULL END AS group_school_name,
       COUNT(1) AS total_players
  FROM benn.college_football_players
 GROUP BY 1;


#### SQL DISTINCT

1. Write a query that returns the unique values in the year column, in chronological order.

select distinct year as distinct_year from tutorial.aapl_historical_stock_price order by 1;


2. Write a query that counts the number of unique values in the month column for each year.

select year, count(distinct month) as distinct_month from tutorial.aapl_historical_stock_price group by year order by year;


3. Write a query that separately counts the number of unique values in the month column and the number of unique values in the `year` column.

select count(distinct year) as distinct_year, count(distinct month) as distinct_month from tutorial.aapl_historical_stock_price;



#### SQL JOIN

1. Write a query that selects the school name, player name, position, and weight for every player in Georgia, ordered by weight (heaviest to lightest). Be sure to make an alias for the table, and to reference all column names in relation to the alias.

  select players.school_name, players.player_name, players.position, players.weight
  FROM benn.college_football_players as players
    where players.state = 'GA'
    order by players.weight DESC;

#### SQL INNER JOIN

1. Write a query that displays player names, school names and conferences for schools in the "FBS (Division I-A Teams)" division.

select players.school_name, players.player_name, teams.conference
  FROM benn.college_football_players as players
  join benn.college_football_teams as teams
  ON players.school_name = teams.school_name
    where teams.division = 'FBS (Division I-A Teams)';


#### SQL LEFT JOIN

1. Write a query that performs an inner join between the tutorial.crunchbase_acquisitions table and the tutorial.crunchbase_companies table, but instead of listing individual rows, count the number of non-null rows in each table.

select count(1) as companies_permalink,
count(1) as acq_permalink
from tutorial.crunchbase_acquisitions as acquisitions
join tutorial.crunchbase_companies as companies
on companies.permalink = acquisitions.company_permalink;

2. Modify the query above to be a LEFT JOIN. Note the difference in results.


select count(1) as companies_permalink,
count(1) as acq_permalink
from tutorial.crunchbase_acquisitions as acquisitions
left join tutorial.crunchbase_companies as companies
on companies.permalink = acquisitions.company_permalink;


3. Count the number of unique companies (don't double-count companies) and unique acquired companies by state. Do not include results for which there is no state data, and order by the number of acquired companies from highest to lowest.

select acquisitions.company_state_code as state, count(distinct acquisitions.company_permalink) as count_acquired_companies,
count(distinct companies.permalink) as count_companies
from tutorial.crunchbase_companies as companies
left join tutorial.crunchbase_acquisitions as acquisitions
on companies.permalink = acquisitions.company_permalink
where acquisitions.company_state_code is not null and 
acquisitions.acquirer_state_code is not NULL and 
companies.state_code is not null
group by 1
order by 2 desc; 



#### SQL RIGHT JOIN


1. Rewrite the previous practice query in which you counted total and acquired companies by state, but with a RIGHT JOIN instead of a LEFT JOIN. The goal is to produce the exact same results.

select acquisitions.company_state_code as state, count(distinct acquisitions.company_permalink) as count_acquired_companies,
count(distinct companies.permalink) as count_companies
from  tutorial.crunchbase_acquisitions as acquisitions 
right join tutorial.crunchbase_companies as companies
on companies.permalink = acquisitions.company_permalink
where acquisitions.company_state_code is not null and 
acquisitions.acquirer_state_code is not NULL and 
companies.state_code is not null
group by 1
order by 2 desc;










