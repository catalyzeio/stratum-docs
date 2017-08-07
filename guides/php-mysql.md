---
title: PHP + MySQL Guide
category: guide
---

# Deploying a PHP+MySQL Application on Compliant Cloud

## Introduction
This guide will cover the basics of deploying a PHP app built using the [Laravel](https://laravel.com) framework that stores data in a MySQL database. We have already created an example application using these steps [here](https://github.com/catalyzeio/php-example-app). Feel free to follow this guide or fork and clone the example application to create your own working copy to deploy to Compliant Cloud.


## Prerequisites
There are a few things you need before we can get started deploying your application.

### Tools Needed
We assume you have base knowledge of the following tools, and installed. If not, you can find information on each of the tools by visiting their link and following the directions there for installing them.

- [git](https://git-scm.com/)
- [Datica CLI](https://github.com/daticahealth/cli)

### Contract with Datica
You need a signed contract with [Datica](https://datica.com/), and already have an environment provisioned for use. If you need to register for Datica you can [start here](https://datica.com/compliant-cloud).

### Terms to know
Some basic terms that you should know:

- Environment ID: a GUID assigned to your environment by Datica. Eg: `9cdde031-5342-4a0d-949c-31253227bd12`
- Environment Label: a name YOU picked for your environment. Eg: `MyHealthApp-Production`


## Setting up your local php application using Laravel
Lets get your php application setup for deployment. For this example we are using the [Laravel](https://laravel.com) framework. To setup a new Laravel project, follow the directions on their documentation [here](https://laravel.com/docs/5.0). Otherwise the example application should be good to go right out of the box.

### Developing locally
To run the example application locally, you can use the Homestead vagrant image which is pretty easy to setup. You can find more information on [Homestead here](https://laravel.com/docs/5.0/homestead).

## Initialize your repository to your code service
We need to initialize your Datica code service to your Laravel application. To do this you need to use [git](https://git-scm.com/) and the [Compliant Cloud CLI](https://github.com/daticahealth/cli).

Using a command line, navigate to a working copy of your application, or fork the [example php application](https://github.com/catalyzeio/php-example-app), and run the following commands:

```
$ datica init
Username:
Password:
# will prompt you to pick an environment and code service you have access to
"datica" remote added.
```

The [Compliant Cloud CLI](https://github.com/daticahealth/cli) added a git remote to your local repo so you can now push code to your environment on Datica.

```
# git remote -v
datica    ssh://git@git.sandbox.catalyzeapps.com:2222/csb0155-app01.git (fetch)
datica    ssh://git@git.sandbox.catalyzeapps.com:2222/csb0155-app01.git (push)
origin  https://github.com/catalyzeio/php-example-app.git (fetch)
origin  https://github.com/catalyzeio/php-example-app.git (push)
#
```

Your remotes will be unique to your origin and enivornment on Datica.

## Deploying your code

So now that we have everything in order, lets deploy your application to Compliant Cloud.

*Note: Your application needs a [Procfile](https://github.com/catalyzeio/php-example-app/blob/master/Procfile) for deployment. If you are using the example application you do not need to worry about this as it is already done for you.*

Run the command below from within your working copy. This will push  our code up to Compliant Cloud and start the build process.

```
# git push datica master
... Lots of Build Output Here ...
remote: ---> XXXXXXX
remote: Finalizing Build (Note: This can take a few minutes to complete)....................................................
remote: Complete. Built Successfully!
#
```

## Connecting to MySQL
To get your laravel application talking to MySQL, you need to edit the `config/database.php` file. You can view the edited file [here](https://github.com/catalyzeio/php-example-app/blob/master/config/database.php). You just need to read in enviroment variables and set the config options.

At the top of database.php, add the following:

```
// Quick fix for parsing our database conneciton string to something Laravel can use in the config
if(getenv("DATABASE_URL")){
	$url = parse_url(getenv("DATABASE_URL"));
	$host = $url["host"];
	$username = $url["user"];
	$password = $url["pass"];
	$database = substr($url["path"], 1);
}else{
	$host = env('DB_HOST', 'localhost');
	$username = env('DB_USERNAME', 'forge');
	$password = env('DB_PASSWORD', '');
	$database = env('DB_DATABASE', 'forge');
}
```

Then find the MySQL config options further down in database.php and edit the host, database, username, and password variables to this:

```
'mysql' => [
	'driver'    => 'mysql',
	'host'      => $host,
	'database'  => $database,
	'username'  => $username,
	'password'  => $password,
	'charset'   => 'utf8',
	'collation' => 'utf8_unicode_ci',
	'prefix'    => '',
	'strict'    => false,
]
```

Now your database settings will be automatically picked up when deployed on Datica or read from the `.env` file when developing locally.

## Using environment variables
Using environment variables in PHP and Laravel is pretty straight forward. Just use the `getenv()` function any where you need to access an environment variable. You can find more documentation on the `getenv()` function [here](http://php.net/manual/en/function.getenv.php).

### Example
`$databaseUrl = getenv("DATABASE_URL");`

### Updating Environment Variables
Use the [Compliant Cloud CLI](https://github.com/daticahealth/cli) to update your environment variables.

The [Datica CLI](https://github.com/daticahealth/cli) makes it pretty straight forward for updating environment variables. Just change into the local directory of your project and use the following commands. For more information on using the [Datica CLI](https://github.com/daticahealth/cli), head over to the [documentation](/compliant-cloud/cli-reference#vars).

#### List all Variables
`datica -E "<your_env_name>" vars list <service_name>`

#### Adding
`datica -E "<your_env_name>" vars set <service_name> -v A=B`

#### Removing
`datica -E "<your_env_name>" vars unset <service_name> A`

## Creating schema for database
You can use the [Datica CLI](https://github.com/daticahealth/cli) to run migrations on the MySQL database easily. Just run the following commands below to populate MySQL with the proper tables for the example application. If you are creating your own application, you can find more information [here](https://laravel.com/docs/5.0/migrations) on migrations with Laravel.

### Example
First we need to find the label for the application service. The following will return a list of all services that belong to your environment:

`datica -E "<your_env_name>" status`

You should see some output like:

```
Overriding BaaS URL: https://api-staging.datica.com
Overriding Compliant Cloud URL: https://api-sbox05.catalyzeapps.com:7443
environment state: running
	app01 (size = c0, build status = finished, deploy status = running)
	db01 (size = c1, image = percona, status = running)
```

The first item in the list is the application service `app01`. We will target `app01` and send a command to it so we can run the migration in the example app.

`datica -E "<your_env_name>" console app01 'php artisan migrate --force'`

That should give you a similar output as:

```
Opening console to service 'ee1af29c-5555-5555-5555-8e4ec6a6666a'
Waiting for the console to be ready... This might take a bit.
.................................................Connecting...
Connection opened
Migration table created successfully.
Migrated: 2014_10_12_000000_create_users_table
Migrated: 2014_10_12_100000_create_password_resets_table
Migrated: 2015_05_05_174234_create_visitors_table
loproxy: stopped
Connection closed: Going away (1006))
Cleaning up
```

As you can see, the migrations have run, and the database is setup for the example application.


## Redeploying
After the initial deployment of the application on Datica, it's pretty easy to update your code. Just do another `git push`:

```
# git push datica master
... Lots of Build Output Here ...
remote: ---> XXXXXXX
remote: Finalizing Build (Note: This can take a few minutes to complete)....................................................
remote: Complete. Built Successfully!
#
```

## Add logging to your php application
Logging works easily right out of the box with Laravel. To Enable logging that works with Datica, you need to edit the `/config/app.php` config file.

Look for this in the app.php config file:

```
/*
|--------------------------------------------------------------------------
| Logging Configuration
|--------------------------------------------------------------------------
|
| Here you may configure the log settings for your application. Out of
| the box, Laravel uses the Monolog PHP logging library. This gives
| you a variety of powerful log handlers / formatters to utilize.
|
| Available Settings: "single", "daily", "syslog", "errorlog"
|
*/

'log' => 'daily',
```

You need to change the value from `daily` to `syslog` and you should be all set. Below is an example on how to send information to logging within your Laravel application.

### Example

```
Log::info('This is some useful information.');

Log::warning('Something could be going wrong.');

Log::error('Something is really going wrong.');
```

If you would like more information on logging and laravel you can go [here](https://laravel.com/docs/5.0/errors).

Additionally, for using php standalone with no framework, you can use the `syslog()` function. More information on that can be found [here](http://php.net/manual/en/function.syslog.php).

### Viewing Logs
Once your application is logging, you can view those logs using the dashboard. Just sign into the [Compliant Cloud Dashboard](https://product.datica.com/compliant-cloud), navigate to the environments dashboard, and click on "Monitoring" or "Logging" on any environment within your dashboard.
