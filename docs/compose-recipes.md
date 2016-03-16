# docker-compose Recipes

Docker-compose automates the process of launching, linking and managing
containers.  It allows you to boot up an entire stack with one command, and it
stores the all-important description of how things tie together in a simple YAML
file.  That said, it has some usability shortcomings, which we can overcome
by understanding them.

### start/up/scale/rm/stop

Containers take the following path:

None -> Running <-> Stopped -> Deleted

`docker-compose start` takes `stopped` containers and puts them in the `running`
state.  It _does not_ create containers (none -> running).  It runs the
containers in the background.  If you want to start up a stack in the
background, what you want is `docker-compose up -d`

`docker-compose up` takes any stack, and gets it into the `running` state as
best it can, and runs it in the foreground, tailing log output to the terminal.
You can also use it to start it up in the background using `docker-compose up
-d`

`docker-compose stop` takes the stack and stops all of the containers.

`docker-compose scale <service>=<scale>` takes a service and does what is
necessary to get the # of copies requested (`<scale>`) of the given service, and
runs them in the background.

`docker-compose rm` removes stopped containers

`docker-compose logs` prints out logging information from the stack

Any of the above commands can be supplied a service name in order to narrow the
scope of what you intend (i.e. `docker-compose up web` will only start up `web`
and services that it depends on).
