# Middleware

*Slim Framework* uses middleware to run code before and after your Slim application to manipulate 
the Request and Response objects as you see fit. This can be used to authenticate requests before your app runs
or protect your app from cross-site request forgery. `./config/middleware.php` is the place to do this.

Check out [https://www.slimframework.com/docs/v4/concepts/middleware.html](https://www.slimframework.com/docs/v4/concepts/middleware.html) 
for detailed information.


```php showLineNumbers title="config/middelware.php"
return function (App $app) {
    /** @var Settings $settings */
    $settings = $app->getContainer()->get(Settings::class);

    $errorMiddleware = $app->addErrorMiddleware(
        $settings->get('slim.displayErrorDetails'),
        $settings->get('slim.logErrors'),
        $settings->get('slim.logErrorDetails'),
    );

    /** @var \Slim\Handlers\ErrorHandler $errorHandler */
    $errorHandler = $errorMiddleware->getDefaultErrorHandler();
    if (!$settings->get('slim.whoops.enabled')) {
        $errorHandler->registerErrorRenderer('text/html', DefaultHtmlErrorRenderer::class);
        $errorHandler->setDefaultErrorRenderer('text/html', DefaultHtmlErrorRenderer::class);

        return;
    }

    $errorHandler->registerErrorRenderer('text/html', WhoopsHtmlErrorRenderer::class);
    $errorHandler->registerErrorRenderer('application/json', WhoopsJsonErrorRenderer::class);
    $errorHandler->setDefaultErrorRenderer('text/html', WhoopsHtmlErrorRenderer::class);
};
```