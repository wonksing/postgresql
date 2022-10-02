# Replication using WAL

## Configuration
- publisher
```
# logical
wal_level=logical

# 적어도 서브스크립션 수만큼은 설정해야함
max_replication_slots=60

# 적어도 `max_replication_slots` + 물리적인 복제 수
max_wal_senders=
```

- subscriber
```
# The subscriber also requires the max_replication_slots be set to configure 
# how many replication origins can be tracked
# In this case it should be set to at least the number of subscriptions that 
# will be added to the subscriber.
max_replication_slots=

# max_logical_replication_workers must be set to at least the number of 
# subscriptions, again plus some reserve for the table synchronization
max_logical_replication_workers=

# max_logical_replication_workers + 1
# some extensions and parallel queries also take worker slots from max_worker_processes
max_worker_processes=
```

## Create replication user on source
- Create Replication user
```bash
# max connections = 40
createuser -Upostgres repuser -P -c 40 --replication;

# max connections = 100
createuser -Upostgres replica -P -c 100 --replication;
```

## Grant repuser on source
```sql
grant connect on database ${DATABASE_NAME} to repuser;
grant usage on schema public to repuser;
grant all privileges on all tables in schema public to repuser;
grant all privileges on database ${DATABASE_NAME} to repuser;
grant select, insert, update, delete on all tables in schema public to repuser;
```

## Create Publication(with user)
```sql
-- for all tables
create publication ${NAME_OF_PUB} for all tables;

-- drop publication
drop publication ${NAME_OF_PUB};

-- create publication with specified tables
CREATE PUBLICATION ${NAME_OF_PUB} FOR TABLE ${TABLE_NAME1}, ${TABLE_NAME2};
```

## Create Subscription(using superuser, postgres)
```sql
-- create subscription
create subscription ${NAME_OF_SUB} connection 'dbname=${DATABASE_NAME} host=${IP_ADDR} user=repuser password=${PASSWORD} port=5432' publication ${NAME_OF_PUB};

-- alter subscription, change connection info
alter subscription ${NAME_OF_SUB} connection 'dbname=${DATABASE_NAME} host=${IP_ADDR} user=repuser password=${PASSWORD} port=5432';
```

## Triggers
복제노드에서는 트리거를 끄는 것이 좋다. 
```sql
-- find triggers
select event_object_schema as table_schema,
       event_object_table as table_name,
       trigger_schema,
       trigger_name,
       string_agg(event_manipulation, ',') as event,
       action_timing as activation,
       action_condition as condition,
       action_statement as definition
from information_schema.triggers
group by 1,2,3,4,6,7,8
order by table_schema,
         table_name;

-- enable
ALTER TABLE ${TABLE_NAME} ENABLE TRIGGER ALL;

-- disable
ALTER TABLE ${TABLE_NAME} DISABLE TRIGGER ALL;
```