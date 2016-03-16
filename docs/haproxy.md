# Understanding dockercloud/haproxy

All of our services are exposed to the outside world using the
`dockercloud/haproxy:1.0.1` image.  It must be provisioned with full API rights,
and probably 'privileged' as well.  It connects ports 80/443 to whatever backend
port your docker-cloud service exposes - you may have to enable a port for some
unusually configured docker services/images.  Do not expose the port on the
"node", but make sure it is known internally, dockercloud-haproxy will
automatically detect the internally exposed port and expose it, load balanced,
to the world.  On the internal service, it is important to specify the
`VIRTUAL_HOST` environment variable, and possible a `VIRTUAL_HOST_WEIGHT`, which
combine to route the incoming traffic to the appropriate service.  See the
(dockercloud/haproxy docs)[https://github.com/docker/dockercloud-haproxy] for
more info.
