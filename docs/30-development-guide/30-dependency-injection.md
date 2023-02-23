# DI Container

## PHP-DI

> The dependency injection container for humans

The full documentation for PHP-DI is available on [https://php-di.org/doc/](https://php-di.org/doc/).

:::info

#### Rules for using a container and dependency injection

- Never get an entry from the container directly (always use dependency injection)
- More generally, write code decoupled from the container
- Type-hint against interfaces, configure which implementation to use in the container's configuration

:::

## Compiler passes

Compiler passes give you an opportunity to manipulate other service definitions that have been registered
with the service container. It's a mechanism copied from Symfony:
[https://symfony.com/doc/current/service_container/compiler_passes.html](https://symfony.com/doc/current/service_container/compiler_passes.html)

### Registering a new compiler pass

- Create a new class that implements `CompilerPass` and implement the method `process()`
- Manipulate the service definitions you need to manipulate
```php showLineNumbers title="MyCoolCompilerPass.php"
class MyCoolCompilerPass implements CompilerPass
{
    public function process(ContainerBuilder $container): void
    {
        $definition = $container->findDefinition(SomeDefinition::class);
        foreach($things as $thing){
            $definition->method('register', \DI\autowire($thing));
        }
        $container->addDefinitions(
            [SomeDefinition::class => $definition],
        );
    }
}
```
- Add the compiler pass in `config/compiler-passes.php`

```php showLineNumbers title="config/compiler-passes.php"
return [
    new MyCoolCompilerPass(),
];
```