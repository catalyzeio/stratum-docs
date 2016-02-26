---
title: Local Testing Guide
---

# Local Testing

## How can I test locally before pushing to Stratum?

While we do not provide a local development environment with the full capabilities of the Stratum platform, you can use the following to test that your application builds and will run.

In order to run this you will need to install [boot2docker](http://boot2docker.io/), or already have a local docker host.


#### Setup BTD:

```
boot2docker up
$(boot2docker shellinit)
```

#### Grab the BTD ip
```
BTD_IP=$(echo ${DOCKER_HOST} | grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}')
```
#### Get your app

For demonstration purposes we just pull heroku's node-js sample. When you try it out, you should replace the github url below with a pointer to your code.

```
git clone https://github.com/heroku/node-js-sample
```


#### Get buildstep

```
git clone https://github.com/progrium/buildstep.git
```

#### Build your code into a container

Call it `myapp` or whatever you choose.

```
cd /tmp/buildstep
tar cC /tmp/node-js-sample . | ./buildstep myapp
```

#### Run it

```
docker run -p 8080:8080 -e PORT=8080 -e DATABASE_URL=mongodb://your.mongo.container.ip:12345 -d myapp /bin/bash -c "/start web"
```

Replace the ``your.mongo.container.ip:12345`` with the URL that we provided you.

#### Test it

```
curl http://$BTD_IP:8080
```

#### What if I need to use a custom buildpack?

Set the environment variable `BUILDPACK_URL` and you will be good to go. We've used a sample customer buildpack in the example below. Please adjust it to whatever specific buildpack you would like to test. As a note of caution, please do not use any and all buildpacks out there. Please exercise some caution as some buildpacks can be malicious. In general, if you don't fully understand all the things in the buildpack, please do not use it.

```
echo "export BUILDPACK_URL=https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt.git" > /tmp/node-js-sample/.env
cd /tmp/buildstep
tar cC /tmp/node-js-sample . | ./buildstep myapp-custom
```

#### Run it
```
docker run -p 9090:9090 -e PORT=9090 -e DATABASE_URL=mongodb://your.mongo.container.ip:12345 -d myapp-custom /bin/bash -c "/start web"
```

#### Test it
```
curl http://$BTD_IP:9090
```


### Tips:

- If you need to add environment variables for the build, create the file `.env` in your application directory with the required list of environment variables. The format should be as follows: `export MYVAR=value`
- To add multiple environment variables for the application at runtime, use `docker --env-file env.list` .... `env.list` uses the format `MYVAR=value`
- App logs can be found here: docker logs -f {container id}
- Want to watch the run logs continuously? `docker logs -f {container id}` Hit `Ctrl-C` to exit.
