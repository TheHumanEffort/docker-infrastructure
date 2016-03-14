# The Human Effort's Docker Infrastructure Repo

What's going on?  We're using Docker Cloud to manage all of our

## Important Concepts

Docker documentation has a lot to it, and doesn't do a good job highlighting the
important things that are going on.  The things that I have found super
important conceptually to understanding Docker and how it relates to development
and deployment are in docs/concepts.md

## Prerequisites

If you're on mac, install {Dinghy}[https://github.com/codekitchen/dinghy].
Dinghy extends docker to have much faster filesystem sharing between docker and
the computer you're working on.  No matter what, install:

* {Docker Engine}[https://docs.docker.com/engine/installation/]
* {Docker Compose}[https://docs.docker.com/compose/install/] (on most platforms this comes with docker engine)



## Handy docker formulae that will get you through:


### OMFG, I'm out of space:

Cleanup exited-but-not-removed containers:

```
docker rm -v $(docker ps --filter status=exited -q 2>/dev/null) 2>/dev/null
```

Remove unused images:

```
docker rmi $(docker images --filter dangling=true -q 2>/dev/null) 2>/dev/null
```
