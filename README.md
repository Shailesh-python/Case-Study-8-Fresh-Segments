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
The null values appear in _month, _year, month_year, and interest_id. The corresponding values in composition, index_value, ranking, and percentile_ranking fields are not meaningful without the specific information on interest_id and dates.

Before dropping the values, it would be useful to find out the percentage of null values.

