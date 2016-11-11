Easy Salt testing with Docker
===========

This repo provides a Salt setup with one master and one (or multiple) minions to be able to test and work with states, pillars and all other salt functionality. By using docker it's easy to reproduce and test you code.

Image includes both salt-master, salt-minion, salt-api and also salt-cloud to be able to test and troubleshoot all things salt. `SALT_USE` environment variable is used to determent if container should be running as master or minion.

Salt master is auto accepting all minions.

## Get it running

### Salt master/minon with docker run

Run one container with a master/minion setup.

```
docker run -it --name=saltmaster -h master -p 4505 -p 4506 \
   -p 8080 -p 8081 -e SALT_NAME=master -e SALT_USE=master \
   -v `pwd`/srv/salt:/srv/salt:rw thikade/salt
```

By jumping in with `docker exec -i -t saltmaster bash` your able to test/troubleshoot. Now your ready to write you states and test them out.

### Salt cluster with docker compose

Using docker [machine](https://github.com/docker/machine) and [composer](https://github.com/docker/compose) to get a multi-minion setup. Copy and configure `docker-compose.yml.example` to `docker-compose.yml` and run the following

```
brew install docker-machine docker-compose
docker-machine create --driver virtualbox salt
eval "$(docker-machine env salt)"
docker-compose up
```

By jumping in with `docker exec -it saltmaster bash` your able to test/troubleshoot. Now your ready to write you states and test them out.

## Environment variables

Env variables are used to set config on startup, you can set the following envs

 - `SALT_USE`  - master/minion, defaults to master
 - `SALT_NAME` - minion name, defaults to to master
 - `SALT_GRAINS` - set minion grains as json, defaults to none
 - `LOG_LEVEL` - log level, defaults to info
 - `OPTIONS`   - other options passed into salt process, defaults to none

## Volumes

Following paths can be mounted from the container. `/srv/salt` is needed to run your local states.

 - `/etc/salt` - Master/Minion config
 - `/var/cache/salt` - job data cache
 - `/var/logs/salt` - logs
 - `/srv/salt` - states, pillar reactors

## Build

### Use the pre built image
The pre built image can be downloaded using docker directly. After that you do not need to use this command again, you will have the image on your local computer.

```
docker pull thikade/salt
```

### Build the docker image by yourself
If you prefer you can easily build the docker image by yourself. After this the image is ready for use on your machine and can be used for multiple starts.

```
git clone git@github.com:thikade/salt-docker.git
cd salt-docker
docker build -t thikade/salt .
```
