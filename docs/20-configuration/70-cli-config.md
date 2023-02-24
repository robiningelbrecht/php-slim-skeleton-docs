# Cli config

`./config/cli-config.php` is required for [Doctrine migrations](/development-guide/migrations) 
Cli to work. This file does not need any editing, it just has to be there 🥹...

```php showLineNumbers title="config/cli-config.php"
/** @var \DI\Container $container */
$container = ContainerFactory::create();

return DependencyFactory::fromEntityManager(
    new ConfigurationArray($container->get(Settings::class)->get('doctrine.migrations')),
    new ExistingEntityManager($container->get(EntityManager::class))
);
```