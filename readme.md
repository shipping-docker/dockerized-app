# Dockerizing Your Application

This is an example repository of a "Dockerized" application, as covered on the [Dockerizing Your Application](https://shippingdocker.com/dockerized-app/) preview series from [shippingdocker.com](https://shippingdocker.com).

## Try It Out

```bash
# Get repo
git clone https://github.com/shipping-docker/dockerized-app.git
cd dockerized-app

# Build app/node images
./develop build

# Get laravel requirements
./develop composer install
./develop npm install

# Spin up Laravel (defaults to binding to port 80)
./develop up -d
# Alternatively, bind to port 8888 (or any other port)
APP_PORT=8888 ./develop up -d

# Run tests
./develop test
```

You may need to create a `.env` file with the following modified from the `.env.example` file (don't forget to also run `./develop art key:generate`):

```
APP_KEY=SOME-KEY-HERE

DB_HOST=mysql

CACHE_DRIVER=redis
SESSION_DRIVER=redis

REDIS_HOST=redis
```

## Features

1. Docker Compose configuration for dev vs ci environments
2. `develop` helper to run commands against your application, and to more easily use `docker-compose`
3. Dynamically decide to load xDebug module based on `APP_ENV`
4. Environment considerations for running within CI (e.g. Jenkins)
5. Docker builds for `app` container (Nginx+PHP+composer), `node` contaienr (npm, node, suitable for gulp)

## `develop` Helper

The `develop` command helper provides the following commands:

### Artisan

`art` command

Run `artisan` commands within an `app` container. 

```bash
./develop art [...]

# e.g.
./develop art make:controller -h
```

### Composer

`composer` command

Run `composer` command within an `app` container.

```bash
./develop composer [...]

# e.g.
./develop composer require predis/predis
```

### Test

`test` command

Run `phpunit` command within an `app` container.

```bash
./develop test [...]

# e.g.
./develop test
./develop test tests/unit/SpecificFileToTest.php
./develop test --filter 'TestNamespace\\TestCaseClass::testMethod'
```

Run `phpunit` command within an already-running `app` container. This is quicker as it uses `exec` to run a command inside a running container, instead of `run` to spin up a new container.

```bash
./develop t [...]

# e.g.
./develop t
./develop t tests/unit/SpecificFileToTest.php
./develop t --filter 'TestNamespace\\TestCaseClass::testMethod'
```

### NPM

`npm` command

Run `npm` commands within a `node` container.

```bash
./develop npm [...]

# e.g.
./develop npm install
./develop npm install --save gulp
```

### Gulp

`gulp` command

Run `gulp` commands within a `node` container.

```bash
./develop gulp [...]

# e.g.
./develop gulp
./develop gulp --production
```

### Docker Compose

`docker-compose` command

Any command not matching above falls back to running `docker-compose`.

```bash
./develop [...]

# e.g.
./develop ps
./develop up -d
./develop down
./develop exec app bash
./develop run --rm -w /var/www/html app bash
```







