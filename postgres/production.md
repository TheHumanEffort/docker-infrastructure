# Docker & PostgreSQL

Running Postgresql on docker is currently at "a good start".  It allows us to do
many cool things, like quick deployment of new projects and automatic backup. It
currently is not well-tooled for more complex operations like automatic
failover, replication, and more.  Tackling this will be more complicated, and
since we have not yet had any reliability issues with our postgres deployments
so far, we haven't spent a lot of time on this yet.

## Images

For now, we have used the default postgres:9.4 version (many ubuntu distros do
not easily ship with 9.5 clients, and so backup becomes more difficult with it).
We configure each postgres instance with a unique username/password, which is
reported to the client application via environment variables.

## Backup

We use our (docker-pg-to-s3)[https://github.com/TheHumanEffort/docker-pg-to-s3]
image.  Any linked postgres container is backed up automatically every hour to
S3.

## Manual backup

It is easy to backup a postgres instance.  Gain access to the postgres service
by linking to it or by executing into it directly.  For example, using docker
cloud:

```
docker-cloud container ps | grep postgres
docker-cloud container exec <postgres-hash> bash
```

Or using kubectl:
```
kubectl get pod | grep postgres
kubectl exec -it <postgres-pod> bash
```

Once you're in, you may need to install `curl` or `openssl` (using, for example
`apt-get install`).  Once in, use one of these scripts:

If you're on the host itself:
```
PGUSER=$POSTGRES_USER PGPASSWORD=$POSTGRES_PASSWORD pg_dump -Fc --no-acl --no-owner $DATABASE_NAME > pg.dump
```

If you're linked against it (assuming the name of the link is `postgres`)

```
PGHOST=postgres PGUSER=$POSTGRES_ENV_POSTGRES_USER PGPASSWORD=$POSTGRES_ENV_POSTGRES_PASSWORD pg_dump -Fc --no-acl --no-owner $DATABASE_NAME > pg.dump
```

Now you have a `pg.dump` file containing the database.  Using this handy cURL
script, you can upload that file to S3:

```
#!/bin/bash
file=$1
bucket=$AWS_BUCKET
resource="/${bucket}/${file}"
contentType="application/x-compressed-tar"
dateValue=`date -R`
stringToSign="PUT\n\n${contentType}\n${dateValue}\n${resource}"
s3Key=$AWS_ACCESS_KEY_ID
s3Secret=$AWS_SECRET_ACCESS_KEY
signature=`echo -en ${stringToSign} | openssl sha1 -hmac ${s3Secret} -binary | base64`
curl -X PUT -T "${file}" \
  -H "Host: ${bucket}.s3.amazonaws.com" \
  -H "Date: ${dateValue}" \
  -H "Content-Type: ${contentType}" \
  -H "Authorization: AWS ${s3Key}:${signature}" \
  https://${bucket}.s3.amazonaws.com/${file}
```

## Manual restore

Once you're in to the DB server (with cURL installed), download the S3-saved
backup by right-clicking the file in the AWS browser and clicking "Download", in
the popup, right click the link and copy the URL for it.  Use curl to download
it:

```
curl '<the-url-you-copied>' -o pg.dump
```

Then restore it like so (drop db if it currently exists - you don't want to
restore on top of an existing DB).

```
export PGUSER=$POSTGRES_USER PGPASSWORD=$POSTGRES_PASSWORD
dropdb <db_name> # you may need to disconnect clients for this to work!
createdb <db_name> # make sure this works!!!
pg_restore -Fc --no-acl --no-owner -d <db_name> pg.dump
```

In some cases, it will be necessary to restore the DB from the web
container.  If there isn't a DB, web processes may die, however.  If
this happens, you can set the "command" to execute forever doing
nothing, allowing you to `docker-cloud container exec` into the
container and get your stuff done:

```
bash -c "tail -f /dev/null"
```


## Automatic Backups

Automated backups are made using `pg_dumpall`, which spits out SQL, which is
gzipped and stored in S3.  To restore this, use `psql` instead of `pg_restore`:

```
export PGUSER=$POSTGRES_USER PGPASSWORD=$POSTGRES_PASSWORD
gunzip pg.dump.gz
psql < pg.dump
```
