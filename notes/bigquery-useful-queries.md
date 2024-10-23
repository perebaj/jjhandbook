# Useful queries for lazy minds

Percentile calculation
```sql
SELECT
  DISTINCT thing_to_group,
  PERCENTILE_CONT(data_to_calculate_percentile, 0.5) OVER (PARTITION BY thing_to_group) p50,
  PERCENTILE_CONT(data_to_calculate_percentile, 0.8) OVER (PARTITION BY thing_to_group) p80,
  PERCENTILE_CONT(data_to_calculate_percentile, 0.9) OVER (PARTITION BY thing_to_group) p90,
  PERCENTILE_CONT(data_to_calculate_percentile, 0.95) OVER (PARTITION BY thing_to_group) p95
FROM
  `dataset`
```
