# Test suite

## CommandHandlerTestCase

## EventListenerTestCase

## WebTestCase

Run test suite docker-compose run --rm php-cli vendor/bin/phpunit

```
"lint:fix": " ./vendor/bin/php-cs-fixer fix --config=.php-cs-fixer.dist.php",
"phpstan:run": " ./vendor/bin/phpstan analyse --memory-limit=1G",
"phpstan:generate-baseline": " ./vendor/bin/phpstan analyse --generate-baseline --memory-limit=1G",
```
