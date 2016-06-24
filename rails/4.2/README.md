## Rails docker infrastructure

Using Docker, we can have more consistentcy between our development
and production environments.  We can declaratively state resource
dependencies and inter-relationships easily.

Using Docker-compose in development allows us to quickly get up and
running with all the thigns we need.  To get started, copy everything
in this directory into your Rails app.

### Development:

Build the base image for your app, this caches a "starting point", so
you don't have to deal with super-slow rebuilding of the development
image every time you add a dependency.

```
make prepare
docker-compose build web
```

Now to start the rails app:

```
docker-compose up -d postgres
docker-compose run web rake db:create
docker-compose run web rake db:seed
docker-cmopose up web
```

It will start a postgres instance, and link the rails app against it.
If you have test data in, say, a postgres dump file in your app
directory, you can do:

```
docker-compose run postgres bash
export PGHOST=postgres PGUSER=$POSTGRES_USER PGPASSWORD=$POSTGRES_PASSWORD
createdb <dbname>
pgrestore -Fc --no-owner --no-acl -d <dbname> <dump_file.dump>
```

### Details:

The database.docker.yml will get copied over during production
deployment, you may have to copy it for development deploymen

