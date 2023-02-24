# Test suite

There's a fairly extensive test suite available in `/tests`. To run all the test, execute

```bash
docker-compose run --rm php-cli vendor/bin/phpunit
```

If you want to start adding your own tests, there are some helpers that can make your life easier.

## ContainerTestCase

The `ContainerTestCase` class will bootstrap the container with the `env.test` 
environment variables. This allows you to use the container in your tests:

```php showLineNumbers title="YourImportantClassTest.php"
class YourImportantClassTest extends ContainerTestCase
{
    private ClassName $yourImportantClass;

    public function testItDoesTheImportantThing(): void
    {
        $this->assertTrue($this->yourImportantClass->doTheImportantThing());
    }

    protected function setUp(): void
    {
        parent::setUp();
        $this->yourImportantClass = $this->getContainer()->get(YourImportantClass::class);
    }
}
```

## CommandHandlerTestCase

The `CommandHandlerTestCase` will automatically test if the commandHandler was registered in the CommandBus,
if not, the test will fail with

:::danger

CommandHandler "%s" not found in CommandBus. Did you tag it with an attribute?

:::

```php showLineNumbers title="YourImportantCommandHandlerTest.php"
class YourImportantCommandHandlerTest extends CommandHandlerTestCase
{
    private YourImportantCommandHandler $yourImportantCommandHandler;

    public function testHandle(): void
    {
        // Do some assertions.
        
        $this->yourImportantCommandHandler->handle(/*...*/);
    }

    protected function setUp(): void
    {
        parent::setUp();
        $this->yourImportantCommandHandler = new YourImportantCommandHandler();
    }

    protected function getCommandHandler(): CommandHandler
    {
        return $this->yourImportantCommandHandler;
    }
}
```

## ConsoleCommandTestCase

The `ConsoleCommandTestCase` will automatically test if the console command was registered in the application,
if not, the test will fail with

:::danger

ConsoleCommand "%s" not found in ConsoleCommandContainer. Did you tag it with an attribute?

:::

It also provides some basic functionality to test console output, input and questions.

```php showLineNumbers title="YourImportantConsoleCommandTest.php"
class YourImportantConsoleCommandTest extends ConsoleCommandTestCase
{
    private YourImportantConsoleCommand $yourImportantConsoleCommand;

    public function testExecute(): void
    {
        // Do some assertions.

        $command = $this->getCommandInApplication('app:your:command');

        $commandTester = new CommandTester($command);
        $commandTester->execute([
            'command' => $command->getName(),
        ]);
    }

    protected function setUp(): void
    {
        parent::setUp();
        $this->yourImportantConsoleCommand = new YourImportantConsoleCommand();
    }

    protected function getConsoleCommand(): Command
    {
        return $this->yourImportantConsoleCommand;
    }
}
```

## DatabaseTestCase

Apart from bootstrapping the container, the `DatabaseTestCase` will also bootstrap a test database
and connection. To speed up the test suite, queries are never committed and rolled back on `tearDown()`.

```php showLineNumbers title="YourImportantRepositoryTest.php"
class YourImportantRepositoryTest extends DatabaseTestCase
{
    private YourImportantRepository $yourImportantRepository;

    public function testYourImportantMethod(): void
    {
        $this->yourImportantRepository->add(/*...*/);
 
        $this->assertEquals(
            $expectedResult,
            $this->yourImportantRepository->find(/*...*/)
        );
    }

    protected function setUp(): void
    {
        parent::setUp();

        $this->yourImportantRepository = new YourImportantRepository($this->getConnection());
    }
```

## EventListenerTestCase

The `EventListenerTestCase` will automatically test if the event listener was registered in the EventBus,
if not, the test will fail with

:::danger

EventListener "%s" not found in EventBus. Did you tag it with an attribute?

:::

```php showLineNumbers title="YourImportantEventListenerTest.php"
class YourImportantEventListenerTest extends EventListenerTestCase
{
    private YourImportantEventListener $yourImportantEventListener;

    public function testSomethingHappened(): void
    {
        // Do some assertions.

        $this->yourImportantEventListener->notifyThat(new SomethingHappened(/*...*/));
    }

    protected function setUp(): void
    {
        parent::setUp();

        $this->yourImportantEventListener = new YourImportantEventListener(/*...*/);
    }

    protected function getEventListener(): EventListener
    {
        return $this->yourImportantEventListener;
    }
}
```

## WebTestCase

The `WebTestCase` can be used for full on integration tests. It will bootstrap your app and provide
you with the possibility to test requests and responses

```php
class YourImportantWebTest extends WebTestCase
{
 
    public function testYourImportantEndpoint(): void
    {
        // Add test data to database.
        
        $response = $this->getApp()->handle(
            $this->createRequest(
                'GET',
                '/important-endpoint',
            )
        );

        $this->assertEquals(200, $response->getStatusCode());
    }
}
```

## Snapshot testing

> Snapshot testing is a way to test without writing actual test cases. A typical snapshot 
> test case takes a snapshot, then compares it to a reference snapshot file stored alongside the test. 
> The test will fail if the two snapshots do not match: either the change is unexpected, 
> or the reference snapshot needs to be updated.

The existing tests uses snapshot testing extensively, and it 
relies on [spatie/phpunit-snapshot-assertions](https://github.com/spatie/phpunit-snapshot-assertions) to do so.

I highly recommend to start doing this as well, it will spare you a lot of time while writing tests.

To clean up your snapshots and regenerate them afterwards, you can run

```bash
docker-compose run --rm php-cli composer snapshots:cleanup
```
