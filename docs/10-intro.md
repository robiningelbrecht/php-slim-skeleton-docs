---
slug: /
---

# Get started

![img Slim](/img/banner-header.webp)

[![img CI](https://github.com/robiningelbrecht/slim-skeleton-ddd-amqp/actions/workflows/ci.yml/badge.svg)](https://github.com/robiningelbrecht/slim-skeleton-ddd-amqp/actions/workflows/ci.yml)
[![img codevoc.io](https://codecov.io/gh/robiningelbrecht/php-slim-skeleton/branch/master/graph/badge.svg?token=hgnlFWvWvw)](https://codecov.io/gh/robiningelbrecht/php-slim-skeleton)
[![img License](https://img.shields.io/github/license/robiningelbrecht/slim-skeleton-ddd-amqp?color=428f7e&logo=open%20source%20initiative&logoColor=white)](https://github.com/robiningelbrecht/slim-skeleton-ddd-amqp/blob/master/LICENSE)
[![img PHPStan](https://img.shields.io/badge/PHPStan-level%208-succes.svg?logo=php&logoColor=white&color=31C652)](https://phpstan.org/)
[![img PHP](https://img.shields.io/packagist/php-v/robiningelbrecht/slim-skeleton-ddd-amqp/dev-master?color=%23777bb3&logo=php&logoColor=white)](https://php.net/)

## Why?

I was in need of a no-nonsense, intuitive and easy-to-use skeleton to set up new projects.

## What you'll need

You'll need to have [Docker](https://docs.docker.com/get-docker/) and
[Docker Compose](https://docs.docker.com/compose/) installed to be able to develop and run 
this skeleton on your local development machine. See [Docker setup](/docs/15-docker.md) for more details.

## Installation

:::caution

Be sure to choose the correct installation profile from the start as it can be tedious to change it afterwards.

:::

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="default" label="Default" default>

The **default installation** profile has a complete working example including

* CommandHandlers
* EventListeners
* Domain classes
* Repositories
* ...

They are there to showcase the possibilities.

```bash
composer create-project robiningelbrecht/slim-skeleton-ddd-amqp:dev-master-with-examples [app-name] --no-install --ignore-platform-reqs --stability=dev
# Build docker containers
docker-composer up -d --build
# Install dependencies
docker-compose run --rm php-cli composer install
# Initialize example
docker-compose run --rm php-cli composer example:init
# Start consuming the voting example queue
docker-compose run --rm php-cli bin/console amqp:consume add-vote-command-queue
``` 

Now you should be able to navigate to `http://localhost:8080` where you can start exploring the example application:

![img Slim](/img/example-app-vote.webp)
*Pick your favorite Pokémon*
![img Slim](/img/example-app-results.webp)
*Result page of all "votes"*

  </TabItem>
  <TabItem value="minimal" label="Minimal">

The **minimal installation** profile has no examples. 
You should be using this profile if you know what's up and want to start a clean slate.

```bash
composer create-project robiningelbrecht/slim-skeleton-ddd-amqp [app-name] --no-install --ignore-platform-reqs --stability=dev
# Build docker containers
docker-composer up -d --build
# Install dependencies
docker-compose run --rm php-cli composer install
```

Now you should be set to start developing and coding!

  </TabItem>
</Tabs>


