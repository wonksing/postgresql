# Postgresql 12 Installation

- Install RPM repository
```bash
sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

- Install client
```bash
sudo yum install postgresql12
```

- Install server
```bash
yum install postgresql12-server postgresql12-devel postgresql12-libs postgresql12-contrib
```

- Add Postgres User
```bash
sudo useradd postgres
sudo passwd postgres
sudo chown postgres:postgres -R /data/postgresql
sudo chmod 755 /data
```

- Switch to postgres user
```bash
sudo su postgres
```

- Set Enviornment Variables
```
LD_LIBRARY_PATH=/usr/pgsql-12/bin
export LD_LIBRARY_PATH
PATH=/usr/pgsql-12/bin:$PATH
export PATH
PGDATA=/data/postgresql/12/data
export PGDATA
```

- Initialize Database
```bash
initdb -D /data/postgresql/12/data -U postgres --encoding=UTF8 --locale=ko_KR.UTF-8 --lc-collate=C --lc-ctype=ko_KR.UTF-8
```

- Start Database
```
pg_ctl -D /data/postgresql/12/data -l /data/postgresql/12/logs/postgres.log start
```

- Stop Database
```
pg_ctl -D /data/postgresql/12/data stop
```

- Create user
```bash
createuser -Upostgres -P -s -e ${USER_NAME}
```

- Create database
```bash
createdb -U${USER_NAME} -O${USER_NAME} ${DATABASE_NAME}
```

- Create Replication user
```bash
# max connections = 40
createuser -Upostgres repuser -P -c 40 --replication;

# max connections = 100
createuser -Upostgres replica -P -c 100 --replication;
```

- psql, Login to database with user
```bash
psql -U${USER_NAME} ${DATABASE_NAME}
```

- Install Extensions
```sql
CREATE EXTENSION pgcrypto;
CREATE EXTENSION pg_trgm;
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```