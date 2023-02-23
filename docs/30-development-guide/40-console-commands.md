# Console commands

The console application uses the [symfony console](https://symfony.com/doc/current/console.html) component
to leverage CLI functionality. All commands are bootstrapped by the `bin/console` script.

There are 2 console commands added by default:

```
> app:amqp:consume  Start consuming a given AMQP queue
> app:cache:clear   Clear all caches
```

You can run them by executing

```bash
docker-compose run --rm php-cli bin/console app:cache:clear
docker-compose run --rm php-cli bin/console app:amqp:consume [NAME-OF-QUEUE]
```

## Creating a new command

To add a new console command, create a new class in `src/Console` and
add the `AsCommand` attribute to it:

```php showLineNumbers title="CreateUserConsoleCommand.php"
#[AsCommand(name: 'app:user:create')]
class CreateUserConsoleCommand extends Command
{
    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        // ...
        return Command::SUCCESS;
    }
}
```

## Configuring the command

You can optionally define a description, help message and the input options and arguments 
by overriding the `configure()` method:

```php showLineNumbers title="CreateUserConsoleCommand.php"
#[AsCommand(name: 'app:user:create')]
class CreateUserConsoleCommand extends Command
{
    // The command description shown when running "bin/console list"
    protected static $defaultDescription = 'Creates a new user.';

    protected function configure(): void
    {
        $this
            // The command help shown when running the command with the "--help" option
            ->setHelp('This command allows you to create a user...')
            ->addArgument(...)
            ->addOption(...):
    }
}
```

## Running the command

You can now run the new command by executing

```bash
docker-compose run --rm php-cli bin/console app:user:create
```

The full documentation for the Symfony console component 
is available on [https://symfony.com/doc/current/console.html](https://symfony.com/doc/current/console.html)
