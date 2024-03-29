# GitHub actions

There's a GitHub workflow to run the test suite and code quality checks. It's configured to run 
for every pull request. It also pushes code coverage to [codecov.io](https://app.codecov.io/gh/robiningelbrecht/php-slim-skeleton):

```yaml
name: CI
on:
  pull_request:
  workflow_dispatch:
jobs:
  test-suite:
    name: PHPStan, PHPcs & Testsuite
    runs-on: ubuntu-latest
    services:
      mysql:
        image: mysql:8.0
        env:
          MYSQL_DATABASE: test-suite
          MYSQL_ROOT_PASSWORD: root
        ports:
          - 3306:3306

    steps:
      # https://github.com/marketplace/actions/setup-php-action
      - name: Setup PHP 8.1 with Xdebug 3.x
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.1'
          coverage: xdebug

      # https://github.com/marketplace/actions/checkout
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install dependencies
        run: composer install --prefer-dist

      - name: Copy .env file
        run: cp .env.github-actions .env && cp .env.github-actions .env.test

      - name: Wait for services to boot
        run: sleep 10

      - name: Run PHPStan
        run: vendor/bin/phpstan analyse

      - name: Run PHPcs fixer dry-run
        run: vendor/bin/php-cs-fixer fix --dry-run --stop-on-violation --config=.php-cs-fixer.dist.php

      - name: Run test suite
        run: vendor/bin/phpunit --testsuite unit --fail-on-incomplete  --log-junit junit.xml --coverage-clover clover.xml

      # https://github.com/marketplace/actions/codecov
      - name: Send test coverage to codecov.io
        uses: codecov/codecov-action@v3
        with:
          files: clover.xml
          fail_ci_if_error: true # optional (default = false)
          verbose: true # optional (default = false)

```