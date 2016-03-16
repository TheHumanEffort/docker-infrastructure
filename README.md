# The Human Effort's Docker Infrastructure Repo

What's going on?  We're using Docker Cloud to manage all of our deployments, and
Docker Engine for our local development environments.  This makes it easier to
standardize the environments within which we run our code.

## Important Concepts

Docker documentation has a lot to it, and doesn't do a good job highlighting the
important things that are going on.  The things that I have found super
important conceptually to understanding Docker and how it relates to development
and deployment are in docs/concepts.md.

## Prerequisites

If you're on __MacOS__:

* Install (Docker Engine)[https://docs.docker.com/engine/installation/mac]
* Install (Dinghy)[https://github.com/codekitchen/dinghy] Installation
  instructions are on that page, but if you ignored them, this is the tl/dr:

```
# First install dinghy:
brew tap codekitchen/dinghy  && brew install dinghy

# Then initialize it:
dinghy create --prodiver virtualbox

# Then make sure your dinghy is set up to configure your shell:
echo '$(dinghy shellinit)' >> ~/.bash_profile
```

If you're on something else:

* (Docker Engine)[https://docs.docker.com/engine/installation/]
* (Docker Compose)[https://docs.docker.com/compose/install/] (on most platforms this comes with docker engine)

## Handy docker formulae that will get you through:


### I'm getting weird errors!

If you see:

```
ERROR: Couldn't connect to Docker daemon - you might need to run `docker-machine start default`. 
```

Then you should do this:

```
dinghy up # this may restart your dinghy, that's OK
$(dinghy shellinit)
```


### OMFG, I'm out of space:

Cleanup exited-but-not-removed containers:

```
docker rm -v $(docker ps --filter status=exited -q 2>/dev/null) 2>/dev/null
```

Remove unused images:

```
docker rmi $(docker images --filter dangling=true -q 2>/dev/null) 2>/dev/null
```
