---
title: Python + Postgres Guide
category: guide
---

# Getting started with Python, Django and PostgreSQL on The Platform

Welcome! We've written this guide to help you get started with Python on Datica. You can be up and running with a HIPAA-compliant instance running Django.

----------

If you're used to other Platform as a Service offerings (Heroku, Google App Engine), you'll notice some similarities to Datica's The Platform. Our intent in design was to speed up deployment while abstracting away some of the difficulties of HIPAA-compliant setup. If you're more familiar with deploying your code directly onto a server, there will be some new concepts here to learn. It's worth noting this is *not* Heroku: we'll try to call out divergence in code when possible, but you'll want to follow the guide closely even if a lot of this is already old hat to you.

This guide is supplemental to the Django tutorials but will emphasize the key parts to get your app running on Datica's The Platform. We have developed a sample application that you can also reference throughout the guide and as we walkthrough deploying an application. You can find the [Python Sample App on GitHub](https://github.com/catalyzeio/python-sample-app).

## Pre-requisites

***You have a Platform account with Datica***

If you don't, you can register for a free trial and try building this out on our Sandbox. This provides you with the easiest path to being ready for HIPAA compliance with one more simple deploy. Contact sales@datica.com to find out more.

***You have an existing Python project, with requirements using pip and setup tools***

If you don’t, we have a sample app that you can use as a starting point: https://github.com/catalyzeio/python-sample-app. Though, if you are new to Django, we recommending working through the tutorials in their [documentation pages](https://docs.djangoproject.com/en/1.8/).

***You’ve read and understand the documentation for Python 2.7, Django and Postgres.***

***You have Python, virtualenv, pip and setuptools installed***

You may want to install Python with brew on OS X: `brew install python`

If you don’t, pip/setuptools are installed with Python: http://docs.python-guide.org/en/latest/starting/install/osx/

## Quick Rundown
Once you have a Datica Environment and an application all it takes is a few steps.

```
# Checkout the application
$ git clone git@github.com:catalyzeio/python-sample-app.git
$ cd python-sample-app

# Initialize the repo with your Datica code Service
$ datica init
$ datica -E "<your_env_name>" git-remote add app01

# Push your code to Datica
$ git push datica master

# After the application has been deployed, run migrations
$ datica -E "<your_env_name>" console app01 "python manage.py migrate"

# Go checkout your live app!
```

## First Steps
Alright, lets backup a little bit. If you don't already have an environment, you can get setup with one through the Datica Dashboard. Check out the [Getting Started docs](/compliant-cloud/getting-started). The following sections will highlight several important parts about setting up your Django project in order to have a successful deployment on the Platform.

### Project Requirements
Datica's The Platform leverages buildpacks to bundle up your application and all of its dependencies. This bundle is built into a Docker container that will be shipped into production. The Python buildpack is employed when the build process detects the `requirements.txt` file in the root level of your project's repository. The requirements file declares the project dependencies your application requires in order to run. To make your life easier, definitely install the "django-toolbelt" first, it installs everything you need to get started with Django.

```
pip install django-toolbelt
pip freeze > requirements.txt

git add requirements.txt
git commit -m "Created a requirements.txt file to track project dependencies."
```

#### Specifying a Python Runtime
If your application requires a specific version of python you may specify it in the file `runtime.txt` (also added to the root of your application repository). The buildpack will read this file and attempt to provide your application with the desired runtime. The contents of this file include a single line specifying a python version in the format of `python-{{version_string}}` (At the time of writing) The currently supported runtimes include:

```
python-2.7.9
python-3.4.3
```

### Prep for the database connection
The database connection string is provided to your application through the `DATABASE_URL` environment variable. This is super easy using the `dj_database_url` module installed with the `django-toolbelt` package.

```
# settings.py
import dj_database_url

DATABASES = {
    'default': dj_database_url.config()
}
```

### Logging
If you send your application logs to standard out they will be collected and shipped automatically to the logging server configured for your environment. A sample logging config is provided below:

```
# settings.py
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler'
        },
    },
}
```

## Deploying Your App
Ok, so now you have an application that you can run locally, project dependencies defined in the `requirements.txt` file and a python runtime specified in the `runtime.txt` file. You are ready to deploy your application onto the Platform!

```
$ cd /path/to/your/repo
$ datica init
# Notice the git remote "datica" has been created
$ git push datica master
```

If you watch the terminal you will see the build output scroll by. Pay attention to this in order to see how your application is being built. You should see the application dependencies getting installed and any errors that may occur. You will know the build has completed when you see the following output:

```
$ git push datica master
Counting objects: 31, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (23/23), done.
Writing objects: 100% (31/31), 9.02 KiB | 0 bytes/s, done.
Total 31 (delta 2), reused 0 (delta 0)
remote:
remote: Prepping Build........................................................................
-----> Python app detected
-----> Stack changed, re-installing runtime
-----> Installing runtime (python-2.7.8)
-----> Installing dependencies with pip
       Downloading/unpacking Django==1.8.1 (from -r requirements.txt (line 1))
       Downloading/unpacking psycopg2==2.6 (from -r requirements.txt (line 2))
       Running setup.py (path:/tmp/pip_build_u24819/psycopg2/setup.py) egg_info for package psycopg2

       Installing collected packages: Django, psycopg2
...

-----> Discovering process types
       Procfile declares types -> web
remote: Finalizing Build (Note: This can take a few minutes to complete)...................................................
remote: Complete. Built Successfully!
```

Alright, after your build is successful your application is ready to be deployed. Follow up with the Datica support team to get the app launched. The first time the application is launched is a manual step, after the initial deployment subsequent Git pushes will automatically redeploy the application. You will receive additional info during the onboarding process that is not covered here. For an overview of the onboarding process check out the docs [here](/compliant-cloud/getting-started).

> **A note on build failures:**
Dealing with build failures in the buildpack system can be a little tricky, but with a little patience you'll quickly be able to spot and resolve issues as they arise. When an error occurs in the build process you will often encounter a lengthy stack trace. Glance through the stack trace and look for clues about what went wrong. Oftentimes a dependency fails to build because of an incompatibility with the runtime or the provider where the dependency is hosted is down. If your build process attempts to connect to the database your build may fail. During the build process your application will not have access to the network where the database is running. In rare cases you may need to dig inside the buildpack source to really understand what is happening behind the scenes. Here is a short list of the most common build issues:
>
	1. Missing project dependencies in `requirements.txt`
	2. Forgot to check into your repository a required file
	3. Broken dependencies

## Security & SSL

The [`ALLOWED_HOSTS`](https://docs.djangoproject.com/en/1.9/ref/settings/#allowed-hosts) settings indicates which hostnames Django is allowed to serve. You should list the domain(s) your application runs on, as well as the the hostname of your Datica proxy:

```python
ALLOWED_HOSTS = [
    'api.example.com',
    'pod0XXXX.catalyzeapps.com',
]
```

Datica automatically forces all requesets to SSL, but it's good practice to enforce SSL conformance at all layers, including the application layer. Django provides several settings which make it easy to force SSL.

However, because Datica proxies requests to Django over HTTP rather than HTTPS, all incoming requests to Django will _appear_ to be non-SSL (even though they originally _were_ SSL). To fix this, we can use the [`SECURE_PROXY_SSL_HEADER`](https://docs.djangoproject.com/en/1.9/ref/settings/#std:setting-SECURE_PROXY_SSL_HEADER) setting, which tells Django that when the `HTTP_X_FORWARDED_PROTO` header is equal to `"https"` (set by Datica's proxies) then Django should consider the request as SSL.

```python
SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

It is recommended you review Django's documentation on [Settings](https://docs.djangoproject.com/en/1.9/ref/settings/) and [Security](https://docs.djangoproject.com/en/1.9/topics/security/).

## Now What?
Now that your application has been deployed a good place to start is by checking out the application logs. You can sign onto the logging server by pointing your browser at the `/logging/` endpoint from your environment's Datica domain name (remember to include the trailing slash). You will be prompted to sign in, the credentials are the same as logging into the Datica Dashboard. Each user who has access to view the Environment on the Dashboard will also be able to access the logging server. You can add additional users to the environment via the CLI. Through the interface you can view and filter logs from the various sources throughout your environment be it the database, cache, or application. The logging server is built atop the ELK (Elasticsearch, Logstash, and Kibana) stack and incorporates many powerful features. Checkout out our guide on managing logs in your environment.

You can access your application through the Datica URL configured for your environment or you can create a record for your domain name with your DNS provider.

After the initial deployment we can checkout the app and notice if there are errors. We need to run database migrations before the application will run correctly. To do this we'll fire up the Datica Console:

```
$ datica -E "<your_env_name>" console app01 "python manage.py migrate"
Opening console to service 'a2cb4141-0fb2-4ed5-8e7a-bb59b918dbfc'
Waiting for the console to be ready... This might take a bit.
...................................................
Connecting...
Connection opened
Operations to perform:
  Synchronize unmigrated apps: messages, staticfiles
  Apply all migrations: contenttypes, hello, auth, sessions, admin
Synchronizing apps without migrations:
  Creating tables...
    Running deferred SQL...
  Installing custom SQL...
Running migrations:
  Rendering model states... DONE
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  ...
```

After the database migrations have completed, we can check back on the application which should operate as intended. Using the console you can access other services in your environment:

```
# Log into the Django shell
$ datica -E "<your_env_name>" console app01 "python manage.py shell"

# Log into the Postgres shell
$ datica -E "<your_env_name>" console db01
```

There are more console features on their way. If a command is not supported, open a ticket [here](https://product.datica.com/compliant-cloud) via the "Contact Support" button for your environment.

### Making updates to your application

Now you may have noticed that you would like to change something about your application. You are all setup to modify code commit it to your repository and push to the `datica master` remote and branch just like we did in the beginning. You'll see all of the build output scroll past but what makes these rebuilds different is that your application will now automatically start to redeploy after the build completes successfully. After the build completes it may take up to a couple minutes for the redeploy to complete. If your app logs on startup you'll be able to see precisely when the application came back online. Likewise you can use the CLI to edit, add or remove environment variables for your application. After making a change to environment variables through the CLI or dashboard, remember you will need to manually redeploy (command available in the CLI only) in order for the variable changes to take effect.


## Tips and Tricks

**Build a feature branch**
If you are working locally on a branch and want to deploy the branch to the platform you can push it to the Platform like:

```
git push datica feature-branch:master
```

**Initiate rebuild**
If, for any reason, you want to trigger a rebuild of your application a quick way to do so is push a new commit:
```
git commit --allow-empty -m "trigger rebuild"; git push datica master
```
