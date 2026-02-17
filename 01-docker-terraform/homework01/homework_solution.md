## 📝 Homework

### Question 1. Understanding Docker Images 
> Q1: Run docker with the python:3.13 image. Use an entrypoint bash to interact with the container.
> What's the version of pip in the image?
- 25.3
- 24.3.1
- 24.2.1
- 23.3.1
  
1. Creating a Docker image Python 3.13
```bash
docker run -it --rm \
  -v $(pwd):/app \
  --entrypoint=bash \
  python:3.13

```
2. Check the pip version
```bash
pip --version
```

**✅ Answer**: <br>
The pip version of the Python 3.13 Docker image is **25.3**

---
### Question 2. Understanding Docker Networking and Docker-Compose
> Given the following docker-compose.yaml, 
> what is the hostname and port that pgadmin should use to connect to the postgres database?
- postgres:5433
- localhost:5432
- db:5433
- postgres:5432
- db:5432
  
1. Create ``docker-compose.yaml``
fill the file with the existing script from the question
```bash
services:
 # this is container for postgres (the database)
  db: #hostname
    container_name: postgres
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'postgres'
      POSTGRES_DB: 'ny_taxi'
    ports:
      - '5433:5432' #host:container
    volumes:
      - vol-pgdata:/var/lib/postgresql/data

 # this is container for pgadmin (GUI database)
  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
      PGADMIN_DEFAULT_PASSWORD: "pgadmin"
    ports:
      - "8080:80" # this is port to access to our pgadmin, http://localhost:8080
    volumes:
      - vol-pgadmin_data:/var/lib/pgadmin

volumes:
  vol-pgdata:
    name: vol-pgdata
  vol-pgadmin_data:
    name: vol-pgadmin_data
```
2. Run the docker-compose.yaml
```bash
docker compose up -d
```
3. Run pgadmin4 
Access pgAdmin in browser by browsing to `http://localhost:8080`, and login into pgadmin using `PGADMIN_DEFAULT_EMAIL` and `PGADMIN_DEFAULT_PASSWORD` state in the `docker-compose.yaml` file.

4. Create new server
Register the server connection refers to our configuration on the `docker-compose.yaml`:
- hostname : `db`
- port : `5432`
- username : `postgres`
- password : `postgres`

**✅ Answer**: <br>
Based on the docker-compose.yaml, the right choice is `db:5432`

---
### Question 3. Counting short trips

For the trips in November 2025 (lpep_pickup_datetime between '2025-11-01' and '2025-12-01', exclusive of the upper bound), how many trips had a trip_distance of less than or equal to 1 mile?

- 7,853
- 8,007
- 8,254
- 8,421
  
```bash
SELECT COUNT(*) AS total_trips
FROM green_taxi_data
WHERE lpep_pickup_datetime >= '2025-11-01' AND
      lpep_pickup_datetime < '2025-12-01' AND
      trip_distance <= 1;
```

**✅ Answer**: <br> ![alt text](image.png)

---
### Question 4. Longest trip for each day

Which was the pick up day with the longest trip distance? Only consider trips with trip_distance less than 100 miles (to exclude data errors).

Use the pick up time for your calculations.

- 2025-11-14
- 2025-11-20
- 2025-11-23
- 2025-11-25
  
```bash
SELECT lpep_pickup_datetime
FROM green_taxi_data
WHERE trip_distance < 100
ORDER BY  trip_distance DESC
LIMIT 1;
```
**✅ Answer**: <br> ![alt text](image-1.png)

---
### Question 5. Biggest pickup zone

Which was the pickup zone with the largest total_amount (sum of all trips) on November 18th, 2025?

- East Harlem North
- East Harlem South
- Morningside Heights
- Forest Hills

```bash
SELECT z."Zone" AS pickup_zone, SUM(g."total_amount") AS total_amount
FROM green_taxi_data AS g
LEFT JOIN taxi_zones AS z
ON g."PULocationID" = z."LocationID"
WHERE lpep_pickup_datetime >= '2025-11-18' AND
      lpep_pickup_datetime < '2025-11-19'
GROUP BY pickup_zone
ORDER BY total_amount DESC
LIMIT 1;
```
**✅ Answer**: <br> ![alt text](image-2.png)

---
### Question 6. Largest tip

For the passengers picked up in the zone named "East Harlem North" in November 2025, which was the drop off zone that had the largest tip?

Note: it's tip , not trip. We need the name of the zone, not the ID.

- JFK Airport
- Yorkville West
- East Harlem North
- LaGuardia Airport
  
```bash
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
```
**✅ Answer**: <br> ![alt text](image-3.png)

---
### Question 7. Terraform Workflow

Which of the following sequences, respectively, describes the workflow for:
1. Downloading the provider plugins and setting up backend,
2. Generating proposed changes and auto-executing the plan
3. Remove all resources managed by terraform`

Choices:
- terraform import, terraform apply -y, terraform destroy
- teraform init, terraform plan -auto-apply, terraform rm
- terraform init, terraform run -auto-approve, terraform destroy
- terraform init, terraform apply -auto-approve, terraform destroy
- terraform import, terraform apply -y, terraform rm


**✅ Answer**: <br>
The correct sequence is: terraform init, terraform apply -auto-approve, terraform destroy

**Explanation:**

-terraform init: Initializes the working directory, downloads the necessary provider plugins, and configures the backend for state storage.

-terraform apply -auto-approve: Scans the configuration to generate proposed changes and executes them immediately, skipping the manual "yes" confirmation prompt.

-terraform destroy: Identifies all resources currently managed in the state file and removes them from the infrastructure provider.

