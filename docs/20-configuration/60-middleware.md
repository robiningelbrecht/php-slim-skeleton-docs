# Middleware

*Slim Framework* uses middleware to run code before and after your Slim application to manipulate 
the Request and Response objects as you see fit. This can be used to authenticate requests before your app runs
or protect your app from cross-site request forgery. `./config/middleware.php` is the place to do this.

Check out [https://www.slimframework.com/docs/v4/concepts/middleware.html](https://www.slimframework.com/docs/v4/concepts/middleware.html) 
for detailed information.


```php showLineNumbers title="config/middelware.php"
return function (App $app) {
    $callableResolver = $app->getCallableResolver();
    $responseFactory = $app->getResponseFactory();

    $serverRequestCreator = ServerRequestCreatorFactory::create();
    $request = $serverRequestCreator->createServerRequestFromGlobals();

    $settings = $app->getContainer()->get(Settings::class);

    $errorHandler = new HttpErrorHandler($callableResolver, $responseFactory);
    $shutdownHandler = new ShutdownHandler($request, $errorHandler, $settings->get('slim.displayErrorDetails'));
    register_shutdown_function($shutdownHandler);

    // Add Error Handling Middleware
    $errorMiddleware = $app->addErrorMiddleware(
        $settings->get('slim.displayErrorDetails'),
        $settings->get('slim.logErrors'),
        $settings->get('slim.logErrorDetails'),
    );
    $errorMiddleware->setDefaultErrorHandler($errorHandler);
};
```