
PG Rails databses diff tool
===========================

How to find diff between databases in case when you have to revert production to last snapshot?

```
--------------------
|   Last snapshot  |   ? diff ?
-----------------------------------
|   Broken         |   Lost data  |
-----------------------------------
```

Tool looking for a diff rely on ActiveRecord's `created_at` and `updated_at` columns.

## Prerequisite

 * Ruby (see `.ruby-version`)
 * gem `bundler`
 * (optional) docker


## Setup

Adjust `.env` if needed. Snapshot may depend on `POSTGRES_USER`.


## Run

```
./pgdiff > report.txt
```


## Docker

use `docker-compose` or `docker stack deploy` with provided `docker/stack.yml`

For example:

```
(unless initialized) sudo docker swarm init
sudo env $(cat .env | grep ^[A-Z] | xargs) docker stack deploy --compose-file ./docker/stack.yaml pgdiff
```


### Import db

```
docker/db-import pgdiff_pg_last /path/to/last.sql
docker/db-import pgdiff_pg_lost /path/to/lost.sql
```

### Stop stack

```
sudo docker stack rm pgdiff
```

### Purge volumes


```
sudo docker volume ls | grep ' pgdiff_' | awk '{ print $2 }' | xargs sudo docker volume rm
```
