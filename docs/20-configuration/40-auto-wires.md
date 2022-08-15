# Auto wires

*PHP-DI* will take all the definitions it can find and compile them. That means that definitions like
autowired classes that are not listed in the configuration cannot be compiled since *PHP-DI* doesn't know about them: 
[https://php-di.org/doc/performances.html#optimizing-for-compilation](https://php-di.org/doc/performances.html#optimizing-for-compilation)

`./config/auto-wires.php` allows you to list the auto wired files the container does not know about,
These will then be used to compile the container, which in turn will give the app a performance boost.

```php showLineNumbers title="config/auto-wires.php"
return [
    SomeClass::class => autowire(),
    AnotherClass::class => autowire(),
];
```