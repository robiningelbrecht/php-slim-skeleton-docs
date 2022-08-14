---
slug: /docker-setup
---

# Docker setup

The local development environment is [Docker](https://www.docker.com/) based and
contains 5 containers. To manage the containers, [Docker Compose](https://docs.docker.com/compose/) is used.

## docker-compose.yml

The `docker-compose.yml` file can easily be updated to fit your needs.

```yaml showLineNumbers title="docker-compose.yml"
version: '3.8'

services:
  # Database container used for read and write model.
  mysql:
    container_name: mysql
    image: mysql:8.0
    command: --sort_buffer_size=512K
    volumes:
      - ./docker/mysql/mysql.databases.sql:/docker-entrypoint-initdb.d/01-databases.sql
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: root

  # RabbitMQ container, used to support AMQP.
  rabbitmq:
    container_name: rabbitmq
    image: rabbitmq:3.8-management
    ports:
      - 5672:5672
      - 15672:15672
    environment:
      RABBITMQ_DEFAULT_USER: rabbitmq
      RABBITMQ_DEFAULT_PASS: rabbitmq
      RABBITMQ_DEFAULT_VHOST: app

  # PHP cli container, mainly used for development and running the test suite.
  php-cli:
    build: ./docker/php-cli
    container_name: php-cli
    volumes:
      - ./:/var/www/
    working_dir: /var/www

  # PHP-fpm container, used to access the app through web requests.
  php-fpm:
    build: ./docker/php-fpm
    container_name: php-fpm
    volumes:
      - ./:/var/www/
    working_dir: /var/www
    depends_on:
      - mysql

  # Nginx container, used to configure the webserver.
  nginx:
    image: nginx:stable-alpine
    container_name: nginx
    ports:
      - '8080:80'
    volumes:
      - ./public/:/var/www/public/
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php-fpm
```

## Extra Docker config

Some containers need extra configuration. These config files are stored in 
`./docker/[container-name]`.

:::info

Important to keep in mind is to configure the `./docker/mysql/mysql.databases.sql` file
to make sure the necessary databases are created during the initial build of the containers.

:::