# Drop database

## Limit connections
```sql
/* Method 1: update system catalog */
UPDATE pg_database SET datallowconn = 'f' WHERE datname = '${DATABASE_NAME}';
```

```sql
/* Method 2: use ALTER DATABASE. Superusers still can connect!
ALTER DATABASE mydb CONNECTION LIMIT 0; */

-- kill existing connections
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = '${DATABASE_NAME}';
```

```sql
-- For old versions of PostgreSQL (up to 9.1), change pid to procpid:
SELECT pg_terminate_backend(procpid)
FROM pg_stat_activity
WHERE datname = '${DATABASE_NAME}';
```

## Drop database
```sql
DROP DATABASE ${DATABASE_NAME};
```
