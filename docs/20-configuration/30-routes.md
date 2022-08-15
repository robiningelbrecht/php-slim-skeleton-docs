# Routes

`./config/routes.php` is the file where you can register routes. The *Slim* documentation contains
examples on how to create and configure routes:
[https://www.slimframework.com/docs/v4/objects/routing.html](https://www.slimframework.com/docs/v4/objects/routing.html)


```php showLineNumbers title="config/routes.php"
return function (App $app) {
    // Set default route strategy.
    $routeCollector = $app->getRouteCollector();
    $routeCollector->setDefaultInvocationStrategy(new RequestResponseArgs());

    // The routes registered in your app.
    $app
        ->get('/your-route-slug', YourRequestHandler::class.':handle')
        ->setName('index');
};

```