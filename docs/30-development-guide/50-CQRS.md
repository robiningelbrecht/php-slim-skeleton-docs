# CQRS

> CQRS stands for Command Query Responsibility Segregation It states that every method 
> should either be a command that performs an action, or a query that returns data to the caller, but not both.

## Commands & CommandHandlers

The skeleton allows you to use commands and command handlers to perform actions. These 2 always come in pairs,
when creating a new command in the write model, a corresponding command handler has to be created as well.

### Creating a new command

It's considered best practice to create a new namespace for every new command you create. 
This allows you to "bundle" each action (Command + CommandHandler).

```php showLineNumbers title="CreateUser.php"
namespace App\Domain\WriteModel\User\CreateUser;

class CreateUser extends DomainCommand
{
 
}
```

### Creating the corresponding command handler

Then create a corresponding command handler in the same namespace. Make sure that you
- Add the `AsCommandHandler` attribute to it
- The class name is `NameOfTheEvent` + `CommandHandler`.

```php showLineNumbers title="CreateUserCommandHandler.php"
namespace App\Domain\WriteModel\User\CreateUser;

#[AsCommandHandler]
class CreateUserCommandHandler implements CommandHandler
{
    public function __construct(
    ) {
    }

    public function handle(DomainCommand $command): void
    {
        assert($command instanceof CreateUser);

        // Do stuff.
    }
}
```

## Using the CommandBus

The CommandBus allows you to dispatch (=sync) commands. 
Just inject it into your service and call the `dispatch()` method. It will then magically 🧙‍
call the `handle()` method on the corresponding command handler.

```php
class SomeService
{
    public function __construct(
    ) {
         private readonly CommandBus $commandBus,
    }

    public function someMethod(): void
    {
        $this->commandBus->dispatch(new CreateUser(/*...*/));
    }
}
```