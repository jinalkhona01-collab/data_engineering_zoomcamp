Question 3. Counting short trips
For the trips in November 2025 (lpep_pickup_datetime between '2025-11-01' and '2025-12-01', exclusive of the upper bound), how many trips had a trip_distance of less than or equal to 1 mile?

SELECT COUNT(*) AS total_trips
FROM green_taxi_data
WHERE lpep_pickup_datetime >= '2025-11-01' AND
      lpep_pickup_datetime < '2025-12-01' AND
      trip_distance <= 1;

![alt text](image.png)

Question 4. Longest trip for each day
Which was the pick up day with the longest trip distance? Only consider trips with trip_distance less than 100 miles (to exclude data errors).

Use the pick up time for your calculations.

SELECT lpep_pickup_datetime
FROM green_taxi_data
WHERE trip_distance < 100
ORDER BY  trip_distance DESC
LIMIT 1;

![alt text](image-1.png)

Question 5. Biggest pickup zone
Which was the pickup zone with the largest total_amount (sum of all trips) on November 18th, 2025?

SELECT z."Zone" AS pickup_zone, SUM(g."total_amount") AS total_amount
FROM green_taxi_data AS g
LEFT JOIN taxi_zones AS z
ON g."PULocationID" = z."LocationID"
WHERE lpep_pickup_datetime >= '2025-11-18' AND
      lpep_pickup_datetime < '2025-11-19'
GROUP BY pickup_zone
ORDER BY total_amount DESC
LIMIT 1;

![alt text](image-2.png)


Question 6. Largest tip
For the passengers picked up in the zone named "East Harlem North" in November 2025, which was the drop off zone that had the largest tip?

Note: it's tip , not trip. We need the name of the zone, not the ID.

SELECT zo."Zone" AS dropoff_zone, MAX(g."tip_amount") AS max_tip_amount
FROM green_taxi_data AS g
JOIN taxi_zones AS z
ON g."PULocationID" = z."LocationID"
JOIN taxi_zones AS zo
ON g."DOLocationID" = zo."LocationID"
WHERE lpep_pickup_datetime >= '2025-11-01' AND
      lpep_pickup_datetime < '2025-12-01' AND
	  z."Zone" LIKE 'East Harlem North'
GROUP BY dropoff_zone
ORDER BY max_tip_amount DESC
LIMIT 1;

![alt text](image-3.png)


