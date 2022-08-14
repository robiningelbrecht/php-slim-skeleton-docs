# Container

`./config/container.php` contains the DI container. Most times your classes will be auto-wired, 
but when [PHP-DI](/docs/30-development-guide/30-dependency-injection.md) is unable to do this, 
you'll need to add the definition of the class in this file. It already contains following default definitions:

```php showLineNumbers title="config/container.php"
return [
    // Clock.
    Clock::class => DI\factory([SystemClock::class, 'fromSystemTimezone']),
    // Twig Environment.
    FilesystemLoader::class => DI\create(FilesystemLoader::class)->constructor($appRoot.'/templates'),
    TwigEnvironment::class => DI\create(TwigEnvironment::class)->constructor(DI\get(FilesystemLoader::class)),
    // Doctrine Dbal.
    Connection::class => function (Settings $settings): Connection {
        return DriverManager::getConnection($settings->get('doctrine.connection'));
    },
    // Doctrine EntityManager.
    EntityManager::class => function (Settings $settings): EntityManager {
        $config = ORMSetup::createAttributeMetadataConfiguration(
            $settings->get('doctrine.metadata_dirs'),
            $settings->get('doctrine.dev_mode'),
        );

        return EntityManager::create($settings->get('doctrine.connection'), $config);
    },
    EntityManagerInterface::class => DI\get(EntityManager::class),
    // Console command application.
    Application::class => function (ConsoleCommandContainer $consoleCommandContainer) {
        $application = new Application();
        foreach ($consoleCommandContainer->getCommands() as $command) {
            $application->add($command);
        }

        return $application;
    },
    // Environment.
    Environment::class => function () {
        return Environment::from($_ENV['ENVIRONMENT']);
    },
    // Settings.
    Settings::class => DI\factory([Settings::class, 'load']),
    // AMQP.
    AMQPStreamConnectionFactory::class => function (Settings $settings) {
        $rabbitMqConfig = $settings->get('amqp.rabbitmq');

        return new AMQPStreamConnectionFactory(
            $rabbitMqConfig['host'],
            $rabbitMqConfig['port'],
            $rabbitMqConfig['username'],
            $rabbitMqConfig['password'],
            $rabbitMqConfig['vhost']
        );
    },
];
```