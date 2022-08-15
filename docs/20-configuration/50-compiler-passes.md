# Compiler passes

Compiler passes give you an opportunity to manipulate other service definitions that have been registered 
with the service container. It's a mechanism copied from Symfony:
[https://symfony.com/doc/current/service_container/compiler_passes.html](https://symfony.com/doc/current/service_container/compiler_passes.html)

The existing compiler passes are mainly used to auto discover classes tagged with a class attribute.

`./config/compiler-passes.php` contains al passes that will be processed by the container builder. Check out
[DI Container](/docs/30-development-guide/30-dependency-injection.md) for a more detailed explanation about
compiler passes.

```php showLineNumbers title="config/compiler-passes.php"
return [
    // Compiler pass to auto discover console commands.
    new ConsoleCommandCompilerPass(),
    // Compiler pass to auto discover command handlers.
    new CommandHandlerCompilerPass(),
    // Compiler pass to auto discover event listeners.
    new EventListenerCompilerPass(),
    // Compiler pass to auto discover AMQP queues.
    new QueueCompilerPass(),
];
```