## Install PostgreSQL to Mac

```sh
$ brew install postgresql
$ postgres --version
$ postgres -D /usr/local/var/postgres # 起動
# export PGDATA=/usr/local/var/postgres
$ pg_ctl start
$ pg_ctl stop
```

## Create DB User

```sh
$ createuser -P vegeta
# Input password twice
$ createdb dragon_ball_db -O vegeta
$ psql -U vegeta dragon_ball_db
```
