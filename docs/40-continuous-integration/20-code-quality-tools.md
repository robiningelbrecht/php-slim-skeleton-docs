# Code quality tools

A few other tools have been added to ensure code quality (to a certain degree).

## PHP Coding Standards Fixer

> The [PHP Coding Standards Fixer](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer) (PHP CS Fixer) tool fixes your code to follow standards; 
> whether you want to follow PHP coding standards as defined in the PSR-1, PSR-2, etc., 
> or other community driven ones like the Symfony one. You can also define your (team's) style through configuration.

You can run the fixer by executing

```bash
docker-compose run --rm php-cli composer lint:fix
```

The configuration is stored in `./.php-cs-fixer.dist.php`

## PHPStan

> [PHPStan](https://phpstan.org/) scans your whole codebase and looks for both obvious & tricky bugs. Even in those rarely 
> executed if statements that certainly aren't covered by tests. You can run it on your machine and in 
> CI to prevent those bugs ever reaching your customers in production.

You can run PHPStan by executing

```bash
docker-compose run --rm php-cli composer phpstan:run
```

The configuration is stored in `./phpstan.neon`

To generate a new baseline, run

```bash
docker-compose run --rm php-cli composer phpstan:generate-baseline
```