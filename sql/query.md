
select * from zomato;


--1. What is the average rating and average cost for each location, and how many restaurants does each have?
select
    location,
    count(*)                    as total_restaurants,
    round(avg(rate), 2)         as avg_rating,
    round(avg(cost_for_two), 0) as avg_cost
from zomato
where location is not null
group by location
order by total_restaurants desc;


-- 2.Which locations have more than 50 restaurants?
select 
location,
count(*) as total_resturant,
    ROUND(CAST(AVG(rate)AS numeric), 2)         AS avg_rating
from zomato
where location is not null
group by location
having count(*)>50
order by avg_rating desc;


--3. what percentage of restaurants in each location
--     accept online orders?
select 
location,
round(sum(case
when online_order = 'Yes' then 1 else 0 end)*100.0/count(*),1) as online_order_percentage
from zomato
where location is not null
group by location
order by online_order_percentage desc;


-- 4.what is the difference in average rating between restaurants
--     that offer online ordering and those that don't?
select
    online_order,
    count(*)             as total_restaurants,
    round(cast(avg(rate)as numeric), 2)  as avg_rating
from zomato
where rate is not null
group by online_order;


 -- 5.which restaurants have a rating above 4.0
--  and more than 500 votes?
select 
name,
rate,
votes 
from zomato
where rate > 4.0 and votes >500;


-- 6.what is the average rating for each combination of
--     online_order and book_table (yes/yes, yes/no, no/yes, no/no)?
select
    online_order,
    book_table,
    count(*)             as total_restaurants,
    round(cast(avg(rate)as numeric), 2)  as avg_rating
from zomato
where rate is not null
group by online_order, book_table
order by avg_rating desc;


 -- 7.what are the top 10 cuisines by number of restaurants?
 select
    cuisines,
    count(*) as total_restaurants
from zomato
where cuisines is not null
group by cuisines
order by total_restaurants desc
limit 10;


-- 8.which cuisine has the highest average cost for two people?
SELECT
    cuisines,
    ROUND(
        AVG(REPLACE(cost_for_two, ',', '')::numeric),
        2
    ) AS avg_cost
FROM zomato
WHERE cost_for_two IS NOT NULL
GROUP BY cuisines;

-- 9.for each cuisine, what is the average rating
--     and average number of votes?
select
    cuisines,
    count(*)             as total_restaurants,
    round(cast(avg(rate)as numeric), 2)  as avg_rating,
    round(cast(avg(votes)as numeric), 0) as avg_votes
from zomato
where cuisines is not null
group by cuisines
order by avg_rating desc;


-- 10.how many restaurants fall into each price bracket
--      (under 200, 200-500, 500-1000, 1000+)?
SELECT
    CASE
        WHEN REPLACE(cost_for_two, ',', '')::numeric < 200
            THEN 'Under 200'
        WHEN REPLACE(cost_for_two, ',', '')::numeric BETWEEN 200 AND 500
            THEN '200-500'
        WHEN REPLACE(cost_for_two, ',', '')::numeric BETWEEN 501 AND 1000
            THEN '501-1000'
        ELSE '1000+'
    END AS price_bracket,
    COUNT(*) AS total_restaurants
FROM zomato
WHERE cost_for_two IS NOT NULL
GROUP BY price_bracket;


 -- 11.what is the average rating for restaurants
--      in each price bracket?

select
    case
         WHEN REPLACE(cost_for_two, ',', '')::numeric < 200
            THEN 'Under 200'
        WHEN REPLACE(cost_for_two, ',', '')::numeric BETWEEN 200 AND 500
            THEN '200-500'
        WHEN REPLACE(cost_for_two, ',', '')::numeric BETWEEN 501 AND 1000
            THEN '501-1000'
        ELSE '1000+'
    end                  as price_bracket,
    count(*)             as total_restaurants,
    round(cast(avg(rate)as numeric), 2)  as avg_rating
from zomato
where cost_for_two is not null
  and rate is not null
group by price_bracket
order by price_bracket;

-- 12.which restaurants have a cost for two under 300
--      but a rating above 4.0?

SELECT
    name,
    location,
    cost_for_two,
    rate,
    votes,
    cuisines
FROM zomato
WHERE REPLACE(cost_for_two, ',', '')::numeric < 300
  AND rate > 4.0
ORDER BY
    rate DESC,
    REPLACE(cost_for_two, ',', '')::numeric ASC;

-- 13.what is the most common restaurant type (rest_type),
--      and what is its average rating?
select
    rest_type,
    count(*)             as total_restaurants,
    round(cast(avg(rate)as numeric), 2)  as avg_rating
from zomato
where rest_type is not null
group by rest_type
order by total_restaurants desc
limit 5;

 -- 14.which restaurant type has the highest average cost for two?
 select
    rest_type,
    count(*)                     as total_restaurants,
    round(avg(REPLACE(cost_for_two, ',', '')::numeric), 0)  as avg_cost
from zomato
where rest_type is not null
  and cost_for_two is not null
group by rest_type
having count(*) >= 10
order by avg_cost desc
limit 10;

-- 15.how many restaurants have zero or very few votes (under 10)?
select 
count(*) as total_resturant
from zomato
where votes <10;


 -- 16.which location has the highest number of restaurants offering
--      both online ordering and table booking?

select
    location,
    count(*) as restaurants_with_both_features
from zomato
where online_order = 'Yes'
  and book_table   = 'Yes'
  and location is not null
group by location
order by restaurants_with_both_features desc
limit 10;

 -- 17.which 10 restaurants have the highest number of votes,
--      along with their rating and location?

select
    name,
    location,
    votes,
    rate,
    cost_for_two,
    cuisines
from zomato
order by votes desc
limit 10;


