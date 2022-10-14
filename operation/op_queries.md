# Operation Queries

[postgresql documentation](https://www.postgresql.org/docs/12/monitoring-stats.html#PG-STAT-SUBSCRIPTION) 참고.
## 

```sql
select *
from pg_stat_subscription;
```

## list indexes
```
SELECT
    tablename,
    indexname,
    indexdef
FROM
    pg_indexes
WHERE
    schemaname = 'public'
ORDER BY
    tablename,
    indexname;
```

## create index
- unique index (null 허용 가능)
```
create unique index idx_TABLE_NAME_1 on TABLE_NAME (COLUMN1, COLUMN2);
```