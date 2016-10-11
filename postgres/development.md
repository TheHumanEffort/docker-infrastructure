# Docker Compose & PostgreSQL

See production.md for production use, this is a description on how to
manage a PostgreSQL instance in development using docker-compose.

# docker-compose.yml

```
app:
  build: .
  links:
    - postgres
postgres:
  image: postgres:9.5
  volumes:
    - ./:/application
  environment:
    - POSTGRES_USER=pguser
    - POSTGRES_PASSWORD=pgpass
    - PGUSER=pguser
    - PGPASSWORD=pgpass
    - PGHOST=localhost
```

By duplicately-defining the user and password, we make administrative
tasks easy.  The POSTGRES_* environment variables are for the postgres
docker image, and the PG* are for the built-in postgres tools.

# Common database tasks:

Dumping the database to a file:
```
export DB_NAME=<db-name> DB_FILE=db.dump
docker-compose run postgres pg_dump -Fc --no-acl --no-owner $DB_NAME > /application/$DB_FILE
```

Restoring a datasbase dump:
```
export DB_NAME=<db-name> DB_FILE=db.dump
docker-compose run postgres dropdb $DB_NAME
docker-compose run postgres createdb $DB_NAME
docker-compose run postgres pg_restore -Fc --no-acl --no-owner -d $DB_NAME /application/$DB_FILE
```

Create a new database:
```
export DB_NAME=app_dev
docker-compose run postgres createdb $DB_NAME
```

Doing arbitrary database shit to the DB:
```
docker-compose run postgres bash
```
