## SQL Mode Course Answer Key 
Answers to https://mode.com/sql-tutorial/


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



#### SQL Joins Using WHERE or ON

1. Write a query that shows a company's name, "status" (found in the Companies table), and the number of unique investors in that company. Order by the number of investors from most to fewest. Limit to only companies in the state of New York.


select companies.name, companies.status, count(distinct investments.investor_name) as count_investors
from tutorial.crunchbase_investments as investments 
right join tutorial.crunchbase_companies as companies
on investments.company_permalink = companies.permalink
where companies.state_code = 'NY'
group by 1, 2
order by 3 desc; 


2. Write a query that lists investors based on the number of companies in which they are invested. Include a row for companies with no investor, and order from most companies to least.

SELECT case when investments.investor_name is NULL then 'No investors' else investments.investor_name end as count_investors,
      count(distinct companies.permalink)
  FROM tutorial.crunchbase_companies as companies 
  left join tutorial.crunchbase_investments as investments
  on companies.permalink = investments.company_permalink
 GROUP BY 1
 ORDER BY 2 DESC;


#### SQL FULL OUTER JOIN

 1. Write a query that joins tutorial.crunchbase_companies and tutorial.crunchbase_investments_part1 using a FULL JOIN. Count up the number of rows that are matched/unmatched as in the example above.
	
 SELECT COUNT(CASE WHEN companies.permalink IS NOT NULL AND investments.company_permalink IS NULL
                      THEN companies.permalink ELSE NULL END) AS companies_only,
           COUNT(CASE WHEN companies.permalink IS NOT NULL AND investments.company_permalink IS NOT NULL
                      THEN companies.permalink ELSE NULL END) AS both_tables,
           COUNT(CASE WHEN companies.permalink IS NULL AND investments.company_permalink IS NOT NULL
                      THEN investments.company_permalink ELSE NULL END) AS investments_only
      FROM tutorial.crunchbase_companies companies
      FULL JOIN tutorial.crunchbase_investments_part1 investments
        ON companies.permalink = investments.company_permalink;


#### SQL UNION

1. Write a query that appends the two crunchbase_investments datasets above (including duplicate values). Filter the first dataset to only companies with names that start with the letter "T", and filter the second to companies with names starting with "M" (both not case-sensitive). Only include the company_permalink, company_name, and investor_name columns.

SELECT company_permalink, company_name, investor_name
  FROM tutorial.crunchbase_investments_part1
  where company_name like 'T%'
 UNION ALL
 SELECT company_permalink, company_name, investor_name
   FROM tutorial.crunchbase_investments_part2
  where company_name like 'M%';


2. Write a query that shows 3 columns. The first indicates which dataset (part 1 or 2) the data comes from, the second shows company status, and the third is a count of the number of investors.

Hint: you will have to use the tutorial.crunchbase_companies table as well as the investments tables. And you'll want to group by status and dataset.

SELECT 'investor_dataset_1' as investor_dataset,
    companies.status,
    count(distinct part1.investor_name) as count_investors
    from tutorial.crunchbase_companies as companies
    left join tutorial.crunchbase_investments_part1 as part1
    on companies.permalink = part1.company_permalink
    group by 1, 2
    
UNION ALL

SELECT 'investor_dataset_2' as investor_dataset,
    companies.status,
    count(distinct part2.investor_name) as count_investors
    from tutorial.crunchbase_companies as companies
    left join tutorial.crunchbase_investments_part2 as part2
    on companies.permalink = part2.company_permalink
    group by 1, 2;




### Advanced


#### SQL Data Types

1. Convert the funding_total_usd and founded_at_clean columns in the tutorial.crunchbase_companies_clean_date table to strings (varchar format) using a different formatting function for each one.

select cast(funding_total_usd as varchar(1024)), founded_at_clean::varchar(1024) 
from tutorial.crunchbase_companies_clean_date;


#### SQL Datetime Format

1. Write a query that counts the number of companies acquired within 3 years, 5 years, and 10 years of being founded (in 3 separate columns). Include a column for total companies acquired as well. Group by category and limit to only rows with a founding date.



SELECT companies.category_code, 
      count(case when acquisitions.acquired_at_cleaned - companies.founded_at_clean::timestamp < '1095 days' then 'within_3y' else NULL end) as within_3y,
      count(case when acquisitions.acquired_at_cleaned - companies.founded_at_clean::timestamp < '1825 days' then 'within_5y' else NULL end) as within_5y,
      count(case when acquisitions.acquired_at_cleaned - companies.founded_at_clean::timestamp < '3650 days' then 'within_10y' else NULL end) as within_10y,
      count(distinct companies.permalink) as count_companies
  FROM tutorial.crunchbase_companies_clean_date companies
  join tutorial.crunchbase_acquisitions_clean_date acquisitions
  on companies.permalink = acquisitions.company_permalink
 WHERE founded_at_clean IS NOT NULL
 group by 1
 order by 5 DESC;


#### SQL Functions to Clean Data

1. Write a query that separates the `location` field into separate fields for latitude and longitude. You can compare your results against the actual `lat` and `lon` fields in the table.


 SELECT location,
       TRIM(leading '(' FROM LEFT(location, POSITION(',' IN location) - 1)) AS lat,
       TRIM(trailing ')' FROM RIGHT(location, LENGTH(location) - POSITION(',' IN location) ) ) AS long
  FROM tutorial.sf_crime_incidents_2014_01;


2. Concatenate the lat and lon fields to form a field that is equivalent to the location field. (Note that the answer will have a different decimal precision.)

SELECT concat('(', lat, ', ', lon, ')') as location
  FROM tutorial.sf_crime_incidents_2014_01; 

3. Create the same concatenated location field, but using the || syntax instead of CONCAT.

SELECT '(' || lat || ', ' || lon || ')' as location
  FROM tutorial.sf_crime_incidents_2014_01; 


4. Write a query that creates a date column formatted YYYY-MM-DD.

select substr(date, 7, 4) || '-' || left(date, 2) || '-' || substr(date, 4, 2)  as date_reformatted
from tutorial.sf_crime_incidents_2014_01;


5. Write a query that returns the `category` field, but with the first letter capitalized and the rest of the letters in lower-case.


select upper(left(category, 1)) || lower(substr(category, 2)) as capitalized 
from tutorial.sf_crime_incidents_2014_01;


6. Write a query that counts the number of incidents reported by week. Cast the week as a date to get rid of the hours/minutes/seconds.

select cast(date_trunc('week' , incidents.cleaned_date) as date) as week,
count(distinct incidents.incidnt_num) as num_incidents
from tutorial.sf_crime_incidents_cleandate as incidents
group by week 
order by 2 desc;

7. Write a query that shows exactly how long ago each indicent was reported. Assume that the dataset is in Pacific Standard Time (UTC - 8).

select incidnt_num,
NOW() AT TIME ZONE 'PST' - incidents.cleaned_date as time_ago
from tutorial.sf_crime_incidents_cleandate as incidents;



#### SQL Subqueries


1. Write a query that selects all Warrant Arrests from the tutorial.sf_crime_incidents_2014_01 dataset, then wrap it in an outer query that only displays unresolved incidents.


select sub.* from
(select * from tutorial.sf_crime_incidents_2014_01
where descript = 'WARRANT ARREST') sub
where sub.resolution = 'NONE';


2. Write a query that displays the average number of monthly incidents for each category. Hint: use tutorial.sf_crime_incidents_cleandate to make your life a little easier.


select sub.category, avg(sub.incidents)
from
(select EXTRACT('month'  FROM cleaned_date) AS month, category,
count(distinct incidnt_num) as incidents 
from tutorial.sf_crime_incidents_cleandate 
group by 1, 2) sub
group by 1;



#### SQL Window Functions


1. Write a query modification of the above example query that shows the duration of each ride as a percentage of the total time accrued by riders from each start_terminal

SELECT start_terminal,
       duration_seconds / SUM(duration_seconds) OVER
         (PARTITION BY start_terminal) * 100 AS perc_terminal_total
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 order by 1, 2 DESC;


 2. Write a query that shows a running total of the duration of bike rides (similar to the last example), but grouped by end_terminal, and with ride duration sorted in descending order.

SELECT end_terminal,
       duration_seconds,
       SUM(duration_seconds) OVER
         (PARTITION BY end_terminal ORDER BY duration_seconds desc)
         AS running_total
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08';


 3. Write a query that shows the 5 longest rides from each starting terminal, ordered by terminal, and longest to shortest rides within each terminal. Limit to rides that occurred before Jan. 8, 2012.


 SELECT *
  FROM (
        SELECT start_terminal,
               start_time,
               duration_seconds AS trip_time,
               RANK() OVER (PARTITION BY start_terminal ORDER BY duration_seconds DESC) AS rank
          FROM tutorial.dc_bikeshare_q1_2012
         WHERE start_time < '2012-01-08'
               ) sub
 WHERE sub.rank <= 5;


 4. Write a query that shows only the duration of the trip and the percentile into which that duration falls (across the entire dataset—not partitioned by terminal).


 SELECT duration_seconds,
       NTILE(100) OVER (ORDER BY duration_seconds)
         AS percentile
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 ORDER BY 1 DESC;





## END













