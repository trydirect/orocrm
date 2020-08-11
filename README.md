# OroCRM 4.1.4

## Deploy OroCRM with docker-compose

```
$ cd orocrm/v4_1_4/build && docker-compose up -d
```

## Install application and admin user with Installation Wizard by opening install.php in the browser or from CLI:

```
$ docker-compose exec orocrm bash -c "cd backend && php bin/console oro:install --env=prod --timeout=10000"
```

## Or user silent Installation

For silent installation, use -n (no interaction) and -q (silence the output messages) parameters, and set the required parameters value, like in the example below. Replace items in bold with the information specific to your deployment.

```
$ docker-compose exec orocrm bash -c "cd backend && php bin/console oro:install 
   --application-url=<URL that is configured as an entry point for Oro application>
   --env=prod
   --user-name=admin
   --user-email=admin@example.com
   --user-firstname=John
   --user-lastname=Doe
   --user-password=admin
   --sample-data=y
   --organization-name=AcmeInc
   --language=en
   --formatting-code=en
   --timeout=10000"
```

## Below application directories must be writable both by the web server and the command line user:

* var/sessions
* var/attachment
* var/cache
* var/import_export
* var/logs
* public/media
* public/uploads
* public/js

```
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/public/media"
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/public/uploads"
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/public/js"
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/var/sessions"
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/var/attachment"
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/var/cache"
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/var/import_export"
$ docker-compose exec orocrm bash -c "chmod 777 -R /var/www/backend/var/logs"
```

## Restart orocrm container in order to enable oro message consumer

```
$ docker-compose restart orocrm
```

## Quick deployment to cloud
##### Amazon AWS, Digital Ocean, Hetzner and others
[<img src="https://img.shields.io/badge/quick%20deploy-%40try.direct-brightgreen.svg">](https://try.direct/server/user/deploy/Im9yb2NybXw2fDYi.EAoFeA.415nFHXyG9VetN493hGPReKR5OE/)