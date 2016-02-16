# Background Processing and Workers in Stratum

Background processing includes any operations performed outside of the main application. These operations are typically long running tasks that can be more resource intensive. You don't want these operations to detract from the performance of your application and have your customers' experience damaged, so the logical choice is to separate them.

## Stratum Background Processing

Catalyze has designed a feature to help you easily deploy background "worker" processes that exist within your application's code base. Workers are long running processes that will run along side of your application. Workers are ***not*** tasks designed to be run once (rake tasks, database migrations, etc). A couple common frameworks for background processing include [Sidekiq (Ruby)](http://sidekiq.org/) and [Celery (Python)](http://www.celeryproject.org/). These frameworks are convenient because they allow you to develop and maintain your background processing along with your application code in one repository. These frameworks typically operate asynchronously by passing messages via a broker or queue (RabbitMQ and Redis are popular options).

## Stratum Worker Containers
Worker containers are clones of an application container. They are built and run with the same Git repository that an application container uses. They have access to the same resources as the parent application.

Right now, worker containers mirror the resource sizing of their parent applications.

## How do I deploy a worker?

There are just two things you need to do in order to launch a worker process:

### Add a Target to Your Procfile

In your application's Procfile, you can have multiple targets defined. If you look at the contents of your Procfile you will see a line like `web: ...` with the command to start up your web application. After adding a worker target your Procfile might look like this:

```
web: bundle exec unicorn -p $PORT -c ./config/unicorn.rb
worker: bundle exec sidekiq
```

After modifying the Procfile, commit and push the new changes Catalyze.

### Launch The Worker with the CLI

Now that the Procfile changes are deployed we are ready to launch the worker process with the Stratum CLI. The worker command takes one argument, the name of the Procfile start you want to run as a worker process.

Make sure that you're using the right environment association for the service that will be launching the worker!

`catalyze associate MyProdEnv-app02 app02`

Using the Procfile declared above the command would be:

 `catalyze -E MyProdEnv-app02 worker worker`.

 If you had named the worker target more descriptively, say "notification-sender" the CLI command would be `catalyze -E MyProdEnv-app02 worker notification-sender`
