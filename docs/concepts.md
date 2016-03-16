# Docker Concepts

The fundamentals of Docker are laid out pretty well on the Docker marketing
page: https://www.docker.com/what-docker.  The basics are:

 - __services__ are composed of zero or more __containers__ and provide
   functionality (say, a postgres database, an API server, or a continuous
   integration server)

 - __containers__ are a running instance of an __image__, they act like a
   micro-computer that is designed to be lean, efficient, and not have much
   extra  Typically, __containers__ have just one long-running process, and
   close when that process closes.

 - __images__ are a concrete representation of the hard drive of a computer at
   some point in time, they are composed of one or more __layers__.  Any given
   layer has one parent, and can have many direct children.  An __image__ is
   actually just a reference to a specific layer, the top layer in a stack going
   back to the root ancestor.  You can reference and tag images, and build new
   images on top of old ones.  Images can be manually constructed using the
   `docker commit` command, or can be built automatically using __Dockerfiles__,
   which internally use that mechanism.  Dockerfiles define an image as a series
   of shell commands and operations that should be performed in order to arrive
   at the __image__.

 - __layers__ act like "git for the filesystem", each layer having a few
   statemnts like "add this file here, remove this file there, change that file
   there".

## Practical applications & rationale

Docker allows for convenient reference to dependencies.  For example, we have
clients running various versions of Ruby.  Instead of having a nightmarish local
environment involving dozens of rubies managed by RVM, rbenv or some other state
manager (not to mention a memory-hogging collection of MySQL, PostgreSQL,
elasticsearch, redis... servers), we explicitly define the dependencies of the
application on a per-application basis using Dockerfiles and docker-compose
files. At any point, we can boot up the client's entire collection of services
and dependencies using one command that is the same for every client:

```
docker-compose up app
```

The app could be a Node app, Rails app, whatever.  This is mainly designed for
web applications, but can easily be made to work for non-web applications, in
which case it might look more like:

```
docker-compose run app
```
