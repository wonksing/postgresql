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

## column information
```sql
SELECT *
FROM information_schema.columns
WHERE table_schema = 'public'
AND table_name   = 's_set_log';

select c.relname, 
-- 	a.attrelid as "tableoid",
	a.attname  as "colname",
	a.attnum   as "columnoid",
    (SELECT col_description(a.attrelid, a.attnum)) AS COMMENT,
-- 	b.ordinal_position, 
	b.column_default, 
	b.is_nullable, 
	b.data_type, 
	b.character_maximum_length
from
    pg_catalog.pg_class c
    inner join pg_catalog.pg_attribute a on a.attrelid = c.oid
 	inner join information_schema.columns b on b.table_schema = 'public' and b.table_name = c.relname and b.column_name = a.attname
where
    c.relname = 's_set_log'
    and a.attnum > 0
    and a.attisdropped is false
    and pg_catalog.pg_table_is_visible(c.oid)
order by a.attrelid, a.attnum;

```