# Node for Docker

Node has a few quirks that can make it annoying to work with in
Docker.  This example shows how to get around these quirks simply and
easily.

## Usage

Use the supplied Makefile, Dockerfile and docker-compose.yml files in
your node projects to give you a reasonable environment for building
and developing a node project.

```
make build
```

Will build your first project.

## Development Mode

It is common practice in development to share the application folder
from the host to the docker VM.  When you do this, you start sharing
the node_modules directory from the local machine to the docker
environment.  This means that binaries and other links can have
problems with permissions or architectures.  To get around this, we
install the dependencies outside of `/application` the main place
where application code lives.  This way, you can share the whole
`/application` directory without getting your dependencies crossed.
By specifying a `--prefix` in the `npm install` commands, and
specifying `NODE_PATH`, we install and reference the packages in the
correct place.

## Production

In production, we can drop the `nodemon` line, though it doesn't
really cost overly much.

## Node versions

It is trivially easy to use any version of node that is convenient
changing the first line of the Dockerfile.
