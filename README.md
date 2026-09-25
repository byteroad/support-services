# Support Services

Support services for the OGC API Infrastructure of DGT. This composition installs the following services:

* Matomo: for logs
* GeoHealthCheck: for monitoring
* Apache: web server
* Static sites
  
## Quick Setup

You will need `docker` and `docker-compose` installed in your system, in order to run this infrastructure. 

## Start pygeoapi

Type:

```
docker compose up
```

Or, if you want to run it in the background

```
docker compose up -d
```

## Environment Variables

This compositions read secrets from an environment file on this folder: ```.env```.

Create this file with the following format, replacing variables by reasonable values.

```
GHC_MINIMAL_RUN_FREQUENCY_MINS=2
GHC_RUNNER_IN_WEBAPP=True
GHC_NOTIFICATIONS=True
GHC_NOTIFICATIONS_VERBOSITY=True
GHC_ADMIN_EMAIL='info@foo.com'
```

For Matomo we need also:

```
MYSQL_PASSWORD=matomo
MYSQL_DATABASE=matomo
MYSQL_USER=matomo
MARIADB_ROOT_PASSWORD=matomo
MATOMO_DATABASE_ADAPTER=mysql
MATOMO_DATABASE_TABLES_PREFIX=matomo_
MATOMO_DATABASE_USERNAME=matomo
MATOMO_DATABASE_PASSWORD=matomo
MATOMO_DATABASE_DBNAME=matomo
MARIADB_AUTO_UPGRADE=1
MARIADB_INITDB_SKIP_TZINFO=1
USER_MATOMO=user
PASSWORD_MATOMO=matomo
```

## Setup Matomo

Go to [http:localhost:8081](http:localhost:8081) and follow the wizard. Confirm everything until you initialized the DB. 

In the *Superuser* section please use 

```
username: user
password: matomo
```

(Otherwise you have to change this manually in ./matomo/refresh_logs.sh command)

In the *Set up a Website* section please use `http://localhost` as *Website URL*. 

Confirm all the next steps until you arrive to the end of the wizard (login screen).

The stats from the logs are refreshed every minute. So wait a minute before logging in.

When the containers are running, run this one-time configuration command:

```
docker compose exec matomo bash -c 'echo "proxy_uri_header = 1" >> /var/www/html/config/config.ini.php'
```
## Generate Diagram

```bash
docker run --rm -it --name dcv -v $(pwd):/input pmsipilot/docker-compose-viz render --force --no-volumes -m image docker-compose.yml
```

## License

This project is released under an [MIT License](./LICENSE)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
