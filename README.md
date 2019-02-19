# Docker Swarm with correct `REMOTE_ADDR`

Example repository to show how to get an HTTP server running inside a Docker Swarm environment and still be able to
read the correct `REMOTE_ADDR`.

This repository does not include an backend service to read the request and show the remote address.

## Motivation

[moby/moby/issues/25526](https://github.com/moby/moby/issues/25526)

## Requirements

Port `80` open (or just change the `docker-compose.yml` file)

## Deploying

```bash
$ docker stack deploy -c docker-compose.yml swarm-example
```

Now if you inspect the service logs, you will see that the remote address is not an internal docker overlay network,
but rather, your actual IP address, and that address will be sent to any backend service that reads the
`X-Forwarded-Host`.

```bash
$ curl http://node-address/
$ docker service logs swarm-example_server
...
swarm-example_server.0.uzze1mupb1n9@node1    | 123.45.67.89 - - [19/Feb/2019:10:09:07 +0000] "GET / HTTP/1.1" 502 388
...
```
