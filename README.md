# Case-Study 8 Fresh-Segments

<img src='https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoft%20sql%20server&logoColor=white)'/>

![image](https://github.com/Shailesh-python/Case-Study-8-Fresh-Segments/blob/main/Case%20Study%208.png)

---

There are 2 data tables available to us in `fresh_segments` schema which we can use to run our SQL queries with:

1. `interest_metrics`
2. `interest_map`

### A.Data Exploration and Cleansing

## [Question #1](#case-study-questions)
> What is count of records in the fresh_segments.interest_metrics for each month_year value sorted in chronological order (earliest to latest) with the null values appearing first?

```sql
SELECT
  month_year,
  COUNT(*) AS record_count
FROM fresh_segments.interest_metrics
GROUP BY month_year
ORDER BY month_year;
```
![image](https://user-images.githubusercontent.com/81180156/192119545-0e992447-5fdd-4010-970a-1c038b4a9873.png)

## [Question #2](#case-study-questions)
> What do you think we should do with these null values in the fresh_segments.interest_metrics?

The null values appear in `_month`, `_year`, `month_year`, and `interest_id`. The corresponding values in `composition`, `index_value`, `ranking`, and `percentile_ranking` fields are not meaningful without the specific information on interest_id and dates.

Before dropping the values, it would be useful to find out the percentage of `null` values.

```SQL
SELECT 
  ROUND(100 * (SUM(CASE WHEN interest_id IS NULL THEN 1 END) * 1.0 /
    COUNT(*)),2) AS null_perc
FROM fresh_segments.interest_metrics
```
| null_perc      |
|----------------|
|   8.36         |

## [Question #3](#case-study-questions)
> How many interest_id values exist in the fresh_segments.interest_metrics table but not in the fresh_segments.interest_map table? What about the other way around?
```SQL
SELECT 
  COUNT(DISTINCT map.id) AS map_id_count,
  COUNT(DISTINCT metrics.interest_id) AS metrics_id_count,
  SUM(CASE WHEN map.id is NULL THEN 1 END) AS not_in_metric,
  SUM(CASE WHEN metrics.interest_id is NULL THEN 1 END) AS not_in_map
FROM fresh_segments.interest_map map
FULL OUTER JOIN fresh_segments.interest_metrics metrics
  ON metrics.interest_id = map.id;
```
![image](https://user-images.githubusercontent.com/81180156/192119709-19f6dd9b-19e6-4d9c-9fb7-8751e632631c.png)

## [Question #4](#case-study-questions)
> Summarise the id values in the fresh_segments.interest_map by its total record count in this table.
 
```sql
SELECT 
	COUNT(DISTINCT IM.id) AS total_record
FROM fresh_segments.interest_map IM
```

| total_records  |
|----------------|
|   1209         |

## [Question #5](#case-study-questions)
> What sort of table join should we perform for our analysis and why? Check your logic by checking the rows where interest_id = 21246 in your joined output and include all columns from fresh_segments.interest_metrics and all columns from fresh_segments.interest_map except from the id column.

```sql
SELECT 
	* 
FROM fresh_segments.interest_metrics ME
INNER JOIN fresh_segments.interest_map MP
ON ME.interest_id = MP.id
WHERE ME.interest_id = 21246
	AND ME.[month] IS NOT NULL
```
![image](https://user-images.githubusercontent.com/81180156/192159343-0280c215-da51-49a6-86d3-aa4ebd13d739.png)



GGG
