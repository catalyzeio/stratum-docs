---
title: Worker Deploy
---

# How do I deploy a worker container?

There are just two things you need to do in order to launch a worker process:

## Add a Target to Your Procfile

In your application's Procfile, you can have multiple targets defined. If you look at the contents of your Procfile you will see a line like `web: ...` with the command to start up your web application. After adding a worker target your Procfile might look like this:

```
web: bundle exec unicorn -p $PORT -c ./config/unicorn.rb
worker: bundle exec sidekiq
```

After modifying the Procfile, commit and push the new changes Catalyze.

## Launch The Worker with the CLI

Now that the Procfile changes are deployed we are ready to launch the worker process with the Stratum CLI. The worker command takes one argument, the name of the Procfile start you want to run as a worker process.

Make sure that you're using the right environment association for the service that will be launching the worker!

`catalyze associate MyProdEnv-app02 app02`

Using the Procfile declared above the command would be:

`catalyze -E MyProdEnv-app02 worker worker`.

If you had named the worker target more descriptively, say "notification-sender" the CLI command would be:

`catalyze -E MyProdEnv-app02 worker notification-sender`
