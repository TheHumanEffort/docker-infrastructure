== Bad Docker and Docker Compose Error Messages

===== And what to do about them:

```
$ docker-compose up -d web
api_db_1 is up-to-date
Starting api_web_1
ERROR: Cannot link to a non running container: /api_db_1 AS /api_web_1/api_db_1
```

This means that you have a stopped container that you're trying to
start up that references a dead/gone container.  Delete the container
you're trying to start: `docker-compose rm -f web`, and start it up
again: `docker-compose up -d web`.
