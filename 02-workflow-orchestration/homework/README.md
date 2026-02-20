# Module 2 Homework Solution: Workflow Orchestration with Kestra

This homework focuses on workflow orchestration using Kestra and processing NYC taxi data to GCP (Cloud Storage & BigQuery).

---

## 📋 Homework Summary - Answers

| Question | Answer |
|----------|--------|
| Q1: Yellow taxi Dec 2020 file size | **128.3 MiB** |
| Q2: Variable rendering (green, 2020-04) | **green_tripdata_2020-04.csv** |
| Q3: Yellow taxi 2020 total rows | **24,648,499** |
| Q4: Green taxi 2020 total rows | **1,734,051** |
| Q5: Yellow taxi March 2021 rows | **1,925,152** |
| Q6: Timezone configuration | **Add a `timezone` property set to `America/New_York` in the `Schedule` trigger configuration** |

---

## Prerequisites

Before starting the homework, ensure you have:

✅ GCP service account configured in Kestra 

✅ Flow 06 executed to store GCP configuration in KV store

✅ Flow 07 executed to create GCS bucket and BigQuery dataset

✅ Kestra running at http://localhost:8080

---

## Question 1. Understanding File Sizes

Within the execution for `Yellow` Taxi data for the year `2020` and month `12`: what is the uncompressed file size (i.e. the output file `yellow_tripdata_2020-12.csv` of the `extract` task)?


- ✅ 128.3 MiB
- 134.5 MiB
- 364.7 MiB
- 692.6 MiB

Converting to MiB (binary):
- 134,481,400 ÷ (1024 × 1024) = 128.24 MiB ≈ 128.3 MiB
---

## Question 2. Understanding Variable Rendering

What is the rendered value of the variable `file` when the inputs `taxi` is set to `green`, `year` is set to `2020`, and `month` is set to `04` during execution?

- `{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv` 
- ✅ `green_tripdata_2020-04.csv`
- `green_tripdata_04_2020.csv`
- `green_tripdata_2020.csv`

---

## Question 3. Counting Yellow Taxi Rows (2020)

How many rows are there for the `Yellow` Taxi data for all CSV files in the year 2020?

- 13,537.299
- ✅ 24,648,499
- 18,324,219
- 29,430,127

```bash
SELECT COUNT(*) 
FROM `de-zoomcamp-5467.zoomcamp.yellow_tripdata` 
WHERE filename IN (
  "yellow_tripdata_2020-01.csv",
  "yellow_tripdata_2020-02.csv",
  "yellow_tripdata_2020-03.csv",
  "yellow_tripdata_2020-04.csv",
  "yellow_tripdata_2020-05.csv",
  "yellow_tripdata_2020-06.csv",
  "yellow_tripdata_2020-07.csv",
  "yellow_tripdata_2020-08.csv",
  "yellow_tripdata_2020-09.csv",
  "yellow_tripdata_2020-10.csv",
  "yellow_tripdata_2020-11.csv",
  "yellow_tripdata_2020-12.csv"
)
```
---

## Question 4. Counting Green Taxi Rows (2020)

How many rows are there for the `Green` Taxi data for all CSV files in the year 2020?

- 5,327,301
- 936,199
- ✅ 1,734,051
- 1,342,034

```bash
SELECT COUNT(*) 
FROM `de-zoomcamp-5467.zoomcamp.green_tripdata` 
WHERE filename IN (
  "green_tripdata_2020-01.csv",
  "green_tripdata_2020-02.csv",
  "green_tripdata_2020-03.csv",
  "green_tripdata_2020-04.csv",
  "green_tripdata_2020-05.csv",
  "green_tripdata_2020-06.csv",
  "green_tripdata_2020-07.csv",
  "green_tripdata_2020-08.csv",
  "green_tripdata_2020-09.csv",
  "green_tripdata_2020-10.csv",
  "green_tripdata_2020-11.csv",
  "green_tripdata_2020-12.csv"
)
```
---

## Question 5. Counting Yellow Taxi Rows (March 2021)

How many rows are there for the `Yellow` Taxi data for the March 2021 CSV file?

- 1,428,092
- 706,911
- ✅ 1,925,152
- 2,561,031

### Steps to Solve

1. **Execute Flow 08:**
   - Inputs: `taxi=yellow`, `year=2021`, `month=03`
   - Note: You can type `2021` in the year field (allowCustomValue is enabled)

2. **Find the row count:**
   - After execution completes, go to the **Metrics** tab
   - Find the **`bq_yellow_merge`** task row
   - Look for **`num.dml.affected.rows`** metric

### Solution

From the **Metrics** tab, the **bq_yellow_merge** task shows:
- **num.dml.affected.rows: 1,925,152**

---

## Question 6. Timezone Configuration

How would you configure the timezone to New York in a Schedule trigger?

- Add a `timezone` property set to `EST` in the `Schedule` trigger configuration  
- ✅ Add a `timezone` property set to `America/New_York` in the `Schedule` trigger configuration
- Add a `timezone` property set to `UTC-5` in the `Schedule` trigger configuration
- Add a `location` property set to `New_York` in the `Schedule` trigger configuration  

**Reference:** [Kestra Schedule Trigger Documentation](https://kestra.io/plugins/core/triggers/io.kestra.plugin.core.trigger.schedule)

---
