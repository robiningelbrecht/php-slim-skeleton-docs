# Environment files

There are three `.env` files included. They all have the same parameters but their values can differ.

``` 
# Possible values "dev", "test" or "production"
# When set to "production", the DI will be compiled and cached.
ENVIRONMENT=dev

# Error logging
DISPLAY_ERROR_DETAILS=1
LOG_ERRORS=0
LOG_ERROR_DETAILS=0

# Database credentials
DATABASE_DRIVER=pdo_mysql
DATABASE_HOST=mysql
DATABASE_PORT=3306
DATABASE_NAME=the-coolest-pokemon
DATABASE_USER=root
DATABASE_PASSWORD=root

# RabbitMQ credentials
RABBITMQ_HOST=rabbitmq
RABBITMQ_PORT=5672
RABBITMQ_USER=rabbitmq
RABBITMQ_PASS=rabbitmq
RABBITMQ_VHOST=app
```

## .env

The main environment file used for your environment. This file is included in `.gitignore` 
as it can contain sensitive data.

## .env.test

This environment file is used for running the test suite on your local machine.

## .env.github-actions

This environment file is used while running the CI workflow in GitHub actions.