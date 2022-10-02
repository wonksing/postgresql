# Dump 

## Dump with pg_dump
### dump schema
```bash
pg_dump -U${USER_NAME} -E UTF8 -F t --no-owner --no-tablespaces --no-publications --no-subscriptions --schema-only --verbose --file=schema.dump ${DATABASE_NAME}
```

### dump data
```bash
pg_dump -U${USER_NAME} --data-only --no-owner --no-tablespaces --no-publications --no-subscriptions --format=tar --file=data.dump --encoding=UTF8 --verbose --column-inserts -t ${TABLE_NAME1} -t ${TABLE_NAME2} -t ${TABLE_NAME3} -t ${SEQUENCE_NAME1} -t ${SEQUENCE_NAME2} -t ${SEQUENCE_NAME3} ${DATABASE_NAME}
```