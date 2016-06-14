---
title: Local Testing Guide
category: manage
---

# Local Testing

## How can I test locally before pushing to Stratum?

While we do not provide a local development environment with the full capabilities of the Stratum platform, you can use the following to test that your application builds and will run.

In order to run this you will need to install [Docker Toolbox](https://www.docker.com/products/docker-toolbox). Alternatively, you may just install the [docker-machine](https://docs.docker.com/machine/install-machine/) directly.

#### Setup Docker Toolbox

Before running any docker commands, you need to first create a docker host (if you do not already have one created). List your existing machines with

```
$ docker-machine ls
```

The rest of this guide assumes your docker host is named `dev`. If this is not the case for your local setup, just replace `dev` with the name of your docker host. If you already have a host created, skip this next creation step and be sure to start up the host. If you need to create a host, we'll name it `dev`, run

```
$ docker-machine create -d virtualbox dev
```

Once created, the host will automatically be started up. If you need to start it manually, run

```
$ docker-machine start dev
```

#### Initialize our Shell

Now that the docker host is running, you'll need to know how to connect to it. Run

```
$ docker-machine env dev
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/catalyze/.docker/machine/machines/dev"
export DOCKER_MACHINE_NAME="dev"
# Run this command to configure your shell:
# eval "$(docker-machine env dev)"
```

to see connection details. Persist those environment variables by running

```
$ eval "$(docker-machine env dev)"
```

#### Grab the Docker IP

For ease of use later in the guide, export the docker host IP

```
$ export DOCKER_IP=$(docker-machine ip dev)
```

#### Make a note of your working directory

From here on out there will be a base path referenced in relation to the current working directory. Let's save that in an environment variable for easier access. Navigate to a directory where you would like to clone your code repository and the buildstep repository. We chose `/Users/catalyze/github` here.

```
$ cd /Users/catalyze/github
$ export BASE_PATH=$(pwd)
```

#### Get your app

For demonstration purposes we'll pull Heroku's node-js sample. When you try it out, you should replace the Github URL below with a pointer to your code.

```
$ git clone https://github.com/heroku/node-js-sample
```

#### Get buildstep

Buildstep is how the application will be built in a similar manner to the way Catalyze builds your application. Clone buildstep

```
$ git clone https://github.com/progrium/buildstep.git
```

#### Build your code into a container

Pick a name for your app, such as `myapp`. `myapp` will be the name of the docker image created by the buildstep process. This image name will be used in the `docker run` commands in later steps. Navigate to the cloned buildstep repository and build your code into a container

```
$ cd ${BASE_PATH}/buildstep
$ tar cC ${BASE_PATH}/node-js-sample . | ./buildstep myapp
```

#### Run it

Now run your container

```
$ docker run -p 8080:8080 -e PORT=8080 -e DATABASE_URL=mongodb://your.mongo.container.ip:27017 -d myapp /bin/bash -c "/start web"
```

Replace the `mongodb://your.mongo.container.ip:27017` with the URL for your local database eg.: `mongodb://localhost:27017`. Replace `myapp` with the docker image name you chose earlier.

> NOTE: If you are running docker-machine on Mac or Windows, docker is running inside of a local VM and not on your host machine. Referencing localhost from within a docker container references the VM and not the Mac/Windows host. This is important if you are running your database on your host machine and want to reference it from your running container. To do this, put your host's IP address in the connection string instead of localhost.

#### Test it

Your container should be up and running and accessible on port 8080 at `${DOCKER_IP}`.

```
$ curl http://${DOCKER_IP}:8080
```

#### Checking Logs

You can find the running docker container with

```
$ docker ps
```

Find the `CONTAINER ID` and insert it into the following command

```
$ docker logs CONTAINER_ID
```

You can follow the logs by adding a `-f`. Hit `Ctrl-C` to exit.

```
$ docker logs -f CONTAINER_ID
```

or exec into the container and navigate the file system

```
$ docker exec -it CONTAINER_ID bash
```

#### What if I need to use a custom buildpack?

Set the environment variable `BUILDPACK_URL` and you will be good to go. We've used a sample custom buildpack in the example below. Please adjust it to whatever specific buildpack you would like to test. As a note of caution, please do not use any and all buildpacks out there. Exercise some caution as some buildpacks can be malicious. In general, if you don't fully understand all the things in the buildpack, please do not use it.

```
$ echo "export BUILDPACK_URL=https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt.git" > ${BASE_PATH}/node-js-sample/.env
$ cd ${BASE_PATH}/buildstep
$ tar cC ${BASE_PATH}/node-js-sample . | ./buildstep myapp-custom
```

#### Run it

Much like the previous section, run the customized container

```
$ docker run -p 9090:9090 -e PORT=9090 -e DATABASE_URL=mongodb://your.mongo.container.ip:27017 -d myapp-custom /bin/bash -c "/start web"
```

#### Test it

And again, your customized application should be available on port 9090 at the ${DOCKER_IP} IP address.

```
$ curl http://${DOCKER_IP}:9090
```

### Tips

* If you need to add environment variables for the build, create the file `.env` in your application directory with the required list of environment variables. The format should be as follows: `export MYVAR=value`
* To add multiple environment variables for the application at runtime, use `docker --env-file env.list` .... `env.list` uses the format `MYVAR=value`
