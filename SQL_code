  --DATA CLEANING--

--Create a new table combining the 4 description tables into one

CREATE TABLE appleStore_description_combined AS 

SELECT * 
FROM appleStore_description1

UNION ALL

SELECT * 
FROM appleStore_description2

UNION ALL

SELECT * 
FROM appleStore_description3

UNION ALL

SELECT * 
FROM appleStore_description4;

-------------------------------------------------------------------------------------------------------------

  --EXPLORATORY DATA ANALYSIS--

--Check the number of unique apps in both AppleStore tables

SELECT
  COUNT(DISTINCT id) AS UniqueAppIDs
FROM AppleStore;

SELECT
  COUNT(DISTINCT id) AS UniqueAppIDs
FROM appleStore_description_combined;

--Check for missing values in key fields

SELECT 
  COUNT(*) AS MissingValues
FROM AppleStore
WHERE track_name is null or user_rating is null or prime_genre is null;

SELECT 
  COUNT(*) AS MissingValues
FROM appleStore_description_combined
WHERE app_desc is null;

--Find out the number of apps per genre

SELECT 
  prime_genre,
  COUNT(*) AS NumApps
FROM AppleStore
GROUP BY prime_genre
ORDER BY NumApps DESC;

--Get an overview of the apps' ratings

SELECT 
  max(user_rating) AS Max_Rating,
  min(user_rating) AS Min_Rating,
  Avg(user_rating) AS Avg_Rating
FROM AppleStore

--Get the distribution of app prices 
SELECT 
  (price/2)*2 AS PriceRangeStart,
  ((price/2)*2)+2 AS PriceRangeEnd,
  COUNT(*) AS NumApps
FROM AppleStore
GROUP BY PriceRangeStart
ORDER BY PriceRangeEnd;

-------------------------------------------------------------------------------------------------------------

  --DATA ANALYSIS--

--Check if free apps have higher ratings than free apps

SELECT 
  CASE
    WHEN price > 0 THEN 'Paid'
    ELSE 'Free'
  END AS App_Type,
  Avg(user_rating) AS Avg_Rating
FROM AppleStore
GROUP BY App_Type;

--Check if apps with more supported languages have higher ratings

SELECT 
  CASE
    WHEN lang_num < 10 THEN '<10 languages'
    WHEN lang_num BETWEEN 10 and 30 THEN '10-30 languages'
    ELSE '>30 languages'
  END AS Languages_Range,
  Avg(user_rating) AS Avg_Rating
FROM AppleStore
GROUP BY Languages_Range
ORDER BY Avg_Rating DESC;

--Check for genres with low ratings

SELECT 
  prime_genre,
  Avg(user_rating) AS Avg_Rating
FROM AppleStore
GROUP BY prime_genre
ORDER BY Avg_Rating ASC
LIMIT 10;

--Check if there is a correlation between app description length and user rating

SELECT 
  CASE 
    WHEN length(b.app_desc) <500 THEN 'Short'
    WHEN length(b.app_desc) BETWEEN 500 AND 1000 THEN 'Medium'
    ELSE 'Long'
  END AS Description_Length_Range,
  Avg(a.user_rating) AS Average_Rating

FROM 
  AppleStore AS a
JOIN 
  appleStore_description_combined AS b
ON
  a.id - b.id

GROUP BY Description_Length_Range
ORDER BY Average_Rating DESC;

--Check the top rated app for each genre

SELECT 
  prime_genre,
  track_name,
  user_rating
FROM
  (
    SELECT 
      prime_genre,
      track_name,
      user_rating
      RANK() OVER(
        PARTITION BY 
          prime_genre 
        ORDER BY 
          user_rating DESC,
          rating_count_tot DESC
          ) AS rank
    FROM AppleStore
  ) AS a
WHERE 
a.rank = 1;