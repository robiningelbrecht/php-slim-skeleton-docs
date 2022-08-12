---
sidebar_position: 10
slug: /
---

# Get started

![img Slim](/img/banner-header.webp)

![img CI](https://github.com/robiningelbrecht/slim-skeleton-ddd-amqp/actions/workflows/ci.yml/badge.svg)


## Why?

## What you'll need

## Installation

:::caution

Be sure to choose the correct installation profile from the start as it is harder to change it afterwards.

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
composer create-project robiningelbrecht/slim-skeleton-ddd-amqp [app-name] --no-install --ignore-platform-reqs --stability=dev
# Build docker containers
docker-composer up -d --build
# Install dependencies
docker-compose run --rm php-cli composer install
# Initialize example
docker-compose run --rm php-cli composer example:init
# Start consuming the voting example queue
docker-compose run --rm php-cli bin/console amqp:consume add-vote-command-queue
``` 

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
# Remove example related code
docker-compose run --rm php-cli composer example:remove
``` 

  </TabItem>
</Tabs>


