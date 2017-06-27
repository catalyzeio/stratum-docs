---
title: Migrating to Compliant Cloud from Heroku
category: guide
---

This guide covers the migration of an application that was developed for Heroku to [Compliant Cloud](https://datica.com/compliant-cloud).

## Prerequisites

This guide assumes that you already have an application set up and working - for examples, this will use the application described in the [Node+Mongo guide](/compliant-cloud/articles/guides/node-mongo).

The other prerequisite is that you've already talked with our Sales team and worked with Datica to get an environment provisioned and ready for you, including all of the databases and caches that your application needs. To get that process started, send us a message at [sales@datica.com](mailto:sales@datica.com).

## Terminology, Similarities, and Differences

Many concepts translate between Compliant Cloud and Heroku. Some are a little different, and some have a different name.

* Your code is built using [buildpacks](/compliant-cloud/articles/buildpacks), the same way as on Heroku.

* To deploy on Compliant Cloud, you push using [git](https://git-scm.com/).

* Heroku **Application** = Compliant Cloud **Environment**

  On Heroku, you have one repository with one or more Procfile targets. On Compliant Cloud, you have one or more **Code Services**, each instance of which is equivalent to a **Dyno** on Heroku. What this means is that working with multiple codebases that use different buildpacks is easier, and only one Code Service needs to be updated at a time.

* **Workers**

  Compliant Cloud supports workers the same way Heroku does - they're invoked out of a Procfile target of your choosing. Because there can be more than one code service, however, you choose which code service to launch the worker process out of.

* Heroku **Addon** = Compliant Cloud **Service**

  On Compliant Cloud, your databases and caches are dedicated instances called **database services** and **cache services**, running in their own containers within your environment, in the configuration of your choice (either highly-available or single-instance).

* Heroku **Config Variables** = Compliant Cloud **Environment Variables**

  Same concept, different name. Environment Variables can vary between code services.

* Heroku **Organization Account** = Compliant Cloud **Organization**

  On Compliant Cloud, each environment belongs to an Organization. When Datica provisions an environment for a new customer, we also create an Organization for you, and assign your new environment to that Organization.

## 1. Gain Access to your Environment

When your first environment is provisioned, you'll receive two emails from us. The first will be from a member of our support team letting you know that your environment is ready, along with some basic information about your new environment and a link to our [Getting Started](/compliant-cloud/getting-started) guides.

The second will be an invitation to join your newly-created organization. Follow the instructions in that email, and register a new account if you don't already have one.

After you've joined your organization, navigate to the [Compliant Cloud dashboard](https://product.datica.com/compliant-cloud). You should see your environment listed there.

![environment summary](../images/migrate_envsummary.png)

Take note of its name - that's "My Shiny New Environment" in this case. Also take note of the name of your code service - that's "code-1" above.

## 2. Install the Datica CLI

The CLI can be found on Github: [https://github.com/daticahealth/cli](https://github.com/daticahealth/cli)

Follow the instructions on the readme to install it. You'll know it's installed when you can open a terminal and run `datica --version`.

## 3. Initialize Your Local Repo

To link up your local repo to your Compliant Cloud environment, open a terminal to its root directory and run the `datica init` command (it will sign you in) or `datica -E <your_env_name> git-remote add <service_name>` command.

```
$ cd my-repo

# If you have not signed in with the cli yet
$ datica init
Username or Email: demo-user@datica.com
Password:
# Will prompt you to pick an environment and code service you have access to
"datica" remote added.

# If you have signed in already
$ datica -E "My Shiny New Environment" git-remote add code-1
"datica" remote added.
```

The username and password here are the same credentials that you used to sign in to the Compliant Cloud dashboard.

This command does two things:

1. Signs your cli into the Datica Compliant Cloud platform
2. Adds the git remote `datica`

## 4. Set Variables

Checking variables on Heroku:

```
$ heroku config
=== my-heroku-app Config Vars
MONGODB_URI:  mongodb://username123:password456@ds012345.mlab.com:23624/username123
```

Checking variables on Compliant Cloud:

```
$ datica -E "<your_env_name>" vars list <service_name>
DATABASE_URL=mongodb://mongodb-12345.internal:27017/catalyze
MONGO01_URL=mongodb://mongodb-12345.internal:27017/catalyze
```

As you can see, the connection string for the database is provided in a similar way, but the variable names don't match. You have two choices, here - either change your code to use a different name, or add a new variable to your code service to match its name from Heroku. To do the latter:

```
$ datica -E "<your_env_name>" vars set <service_name> -v MONGODB_URI=mongodb://mongodb-12345.internal:27017/catalyze
Set. For these environment variables to take effect, you will need to redeploy your service with "datica redeploy"
$ datica -E "<your_env_name>" vars list <service_name>
DATABASE_URL=mongodb://mongodb-12345.internal:27017/catalyze
MONGO01_URL=mongodb://mongodb-12345.internal:27017/catalyze
MONGODB_URI=mongodb://mongodb-12345.internal:27017/catalyze
```

Note that warning. Environment variables are only picked up when a service is deployed or redeployed - any changes you make here require a fresh container to take effect.

## 5. Add an SSH Public Key

Compliant Cloud uses public key auth to authenticate git pushes. To add a public key to your Datica account (you only need to do this once):

```
$ datica -E "<your_env_name>" keys add my-ssh-key ~/.ssh/my-ssh-key.pub
```

For more details on SSH key usage and debugging, take a look at our [SSH Keys Guide](/compliant-cloud/articles/ssh-keys).

## 6. Add a Site (and Cert)

So that Compliant Cloud knows what hostnames to accept traffic on and what services to route that traffic to, we need to set up a Site.

If you're migrating, you'd probably like to see your code working before cutting over fully to Compliant Cloud. You can use a different domain or subdomain for now (`staging.example.com`, for example) - another site can always be added later.

### Adding an SSL Cert

To add an SSL Cert you already have on hand, use the `certs create` command in the CLI:

```
$ datica -E "<your_env_name>" certs create my-ssl-cert ./my-cert.crt ./my-cert.key
```

If you don't have a signed cert for the subdomain you intend to use, you can use a self-signed cert for testing.

For more help on adding and debugging certs, take a look at our [Self-Service SSL Guide](/compliant-cloud/articles/guides/self-service-SSL).

### Adding a Site

Use the `sites create` command in the CLI:

```
$ datica -E "<your_env_name>" sites create staging.example.com code-1 my-ssl-cert
Created 'staging.example.com'
```

### Update Your Domain's DNS

You will need to add a CNAME record from the hostname you used above to the Datica public hostname for your environment. To find that hostname, use the `sites list` command:

```
$ datica -E "<your_env_name>" sites list
  NAME                          CERT                          UPSTREAM SERVICE
  pod0012345.catalyzeapps.com   pod0012345.catalyzeapps.com
  staging.example.com         my-ssl-cert                   code-1
```

The public hostname is the `*.catalyzeapps.com` URL - `pod0012345.catalyzeapps.com` in this case.

For more information on setting your DNS correctly, take a look at our [Setting Up DNS Guide](/compliant-cloud/articles/setting-up-dns).

### Restart Your Environment's Service Proxy

Each environment has a special service called the Service Proxy. The Service Proxy is responsible for routing traffic from the outside world into your environment. In order for it to pick up the new site you just added, the service proxy needs to be reloaded, and redeploying it is the way to do that.

```
$ datica -E "<your_env_name>" redeploy service_proxy
```

## 7. Push Code

Just like on Heroku, push to datica master:

```
$ git push datica master
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1012 bytes | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
remote:
remote: Prepping Build.................
remote: [buildstep] Building application
-----> Node.js app detected
...
... (truncating output for this guide)
...
remote: [buildstep] Build finished
remote: Finalizing Build.............
remote: Redeploying Service....
remote: Complete. Built Successfully!
To ssh://git@git.pod00.catalyzeapps.com:2222/pod0012345-code-123456789.git
 * [new branch]      master -> master
```

## 8. Check out your running app

Navigate to the hostname you configured above, with https (https://staging.example.com/ for this guide).

Note that it may take a few minutes for your application to show up - several factors can cause delay in this (DNS propagation, AWS load balancers taking effect, or slow-starting applications are the most common causes).

If you think you've waited too long and your application still hasn't started, check your logs. Your logs can be accessed via the `logs` command in your CLI, or at `https://your-env's-public-hostname/logging/` - there is also a link to view logs in the Compliant Cloud dashboard.

## Further Help

A topic this guide does not cover is database schema setup and data migration - for more on this, take a look at our [Database Import Guide](/compliant-cloud/articles/cli-database-import).

If your application is still having issues, or you have questions about any part of this process, please [contact Datica Support](/compliant-cloud/articles/contact).
