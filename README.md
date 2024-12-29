# TripleTen_Projects
Task 1
Print the company_name field. Find the number of taxi rides for each taxi company for November 15-16, 2017,
name the resulting field trips_amount and print it, too. Sort the results by the trips_amount field in descending order.

SELECT 
    cabs.company_name, 
    COUNT(trips.trip_id) AS trips_amount
FROM 
    trips
INNER JOIN 
    cabs ON trips.cab_id = cabs.cab_id
WHERE 
    CAST(trips.start_ts AS DATE) BETWEEN '2017-11-15' AND '2017-11-16'
GROUP BY 
    cabs.company_name
ORDER BY 
    trips_amount DESC;
    
Task 1 screenshot: ![image](https://github.com/user-attachments/assets/39cc1cfa-8075-412c-8057-6d9ce52db1fe)
--------------------------------------------------------------------------------------------------------------------------

Task 2

Find the number of rides for every taxi companies whose name contains the words "Yellow" or "Blue" for November 1-7, 2017. 
Name the resulting variable trips_amount. Group the results by the company_name field.

SELECT 
    cabs.company_name, 
    COUNT(trips.trip_id) AS trips_amount
FROM 
    trips
INNER JOIN 
    cabs ON trips.cab_id = cabs.cab_id
WHERE 
    CAST(trips.start_ts AS DATE) BETWEEN '2017-11-01' AND '2017-11-07'
    AND (cabs.company_name LIKE '%Yellow%' OR cabs.company_name LIKE '%Blue%')
GROUP BY 
    cabs.company_name
ORDER BY 
    trips_amount DESC; 
    
  Task 2 screenshot:  ![image](https://github.com/user-attachments/assets/498c3847-e8ec-4423-a821-a4c25a90d8d5) 
  ---------------------------------------------------------------------------------------------------------------------------------------------
 
  Task 3

For November 1-7, 2017, the most popular taxi companies were Flash Cab and Taxi Affiliation Services. 
Find the number of rides for these two companies and name the resulting variable trips_amount. 
Join the rides for all other companies in the group "Other." Group the data by taxi company names. 
Name the field with taxi company names company. Sort the result in descending order by trips_amount.

SELECT 
    CASE 
        WHEN cabs.company_name IN ('Flash Cab', 'Taxi Affiliation Services') THEN cabs.company_name
        ELSE 'Other'
    END AS company, 
    COUNT(trips.trip_id) AS trips_amount
FROM 
    trips
INNER JOIN 
    cabs ON trips.cab_id = cabs.cab_id
WHERE 
    CAST(trips.start_ts AS DATE) BETWEEN '2017-11-01' AND '2017-11-07'
GROUP BY 
    CASE 
        WHEN cabs.company_name IN ('Flash Cab', 'Taxi Affiliation Services') THEN cabs.company_name
        ELSE 'Other'
    END
ORDER BY 
    trips_amount DESC;

Task 3 screenshot: ![image](https://github.com/user-attachments/assets/1b644288-9d31-4481-8e49-74fd7e9ca669)
------------------------------------------------------------------------------------------------------------------------------------------------------

Task 4

Retrieve the identifiers of the O'Hare and Loop neighborhoods  from the neighborhoods table.

SELECT 
    neighborhood_id, 
    name
FROM 
    neighborhoods
WHERE 
    name LIKE '%Hare' 
    OR name LIKE 'Loop';

Task 4 screenshot: ![image](https://github.com/user-attachments/assets/b459eb2d-d0af-4d6a-862a-1e5022208a62)
--------------------------------------------------------------------------------------------------------------------------------------------------------

Task 5

For each hour, retrieve the weather condition records from the weather_records table.
Using the CASE operator, break all hours into two groups: Bad if the description field contains the words rain or storm,
and Good for others.
Name the resulting field weather_conditions.
The final table must include two fields: date and hour (ts) and weather_conditions.

SELECT 
    ts, 
    CASE 
        WHEN description LIKE '%rain%' 
        OR description LIKE '%storm%' THEN 'Bad'
        ELSE 'Good'
    END AS weather_conditions
FROM 
    weather_records;

Task 5 screenshot: ![image](https://github.com/user-attachments/assets/76eea557-92b4-4d49-ad86-93de6be6d93d)

--------------------------------------------------------------------------------------------------------------------------------------------------------------

Task 6

Retrieve from the trips table all the rides that started in the Loop (pickup_location_id: 50)
on a Saturday and ended at O'Hare (dropoff_location_id: 63). 
Get the weather conditions for each ride. 
Use the method you applied in the previous task. 
Also, retrieve the duration of each ride.
Ignore rides for which data on weather conditions is not available.

SELECT 
    t.start_ts, 
    CASE 
        WHEN w.description LIKE '%rain%' 
        OR w.description LIKE '%storm%' THEN 'Bad'
        ELSE 'Good'
    END AS weather_conditions,
    t.duration_seconds
FROM 
    trips t
INNER JOIN 
    weather_records w ON DATE_TRUNC('hour', t.start_ts) = w.ts
WHERE 
    EXTRACT(DOW FROM t.start_ts) = 6  -- Saturday
    AND t.pickup_location_id = 50  -- Loop
    AND t.dropoff_location_id = 63  -- O'Hare
ORDER BY 
    t.trip_id;

Task 6 screenshot: ![image](https://github.com/user-attachments/assets/ea1aaa8d-6258-476f-9cd8-6f3a69b6c4c8)



    

