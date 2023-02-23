# File structure

The project has a fairly straight forward file structure. The most important directories are :
- `config`: contains all Slim configuration files (see Configuration).
- `migrations`: contains all [database migrations](/docs/30-development-guide/80-migrations.md).
- `src`: the root directory for your code. Composer with autoload this directory as the `App` namespace.
- `tests`: the root directory for your tests. Composer with autoload this directory as the `App\Tests` namespace.
- `templates`: the root directory for all your [twig templates](/docs/30-development-guide/90-templating.md).

## Detailed overview
```
├── bin
│   └── console
├── config
├── docker
│   ├── mysql
│   ├── nginx
│   ├── php-cli
│   └── php-fpm
├── migrations
├── public
│   └── index.php
├── src
│   ├── Console
│   ├── Controller
│   ├── Domain
│   │   ├── ReadModel
│   │   └── WriteModel
│   └── Infrastructure
├── templates
└── tests
```