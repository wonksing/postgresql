# Restore

## Restore with pg_restore

### Restore schema
```bash
pg_restore -U${USER_NAME} -v -W -F t -d ${DATABASE_NAME} schema.dump
```

### Restore data
```bash
# restore on a node
pg_restore -U${USER_NAME} -v -W -F t -d ${DATABASE_NAME} data.dump

# on replica node
pg_restore -U${USER_NAME} -v --role=replica -W -F t -d ${DATABASE_NAME} data.dump
```