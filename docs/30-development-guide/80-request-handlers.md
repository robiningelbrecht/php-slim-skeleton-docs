# Request handlers

Request handlers can be added in any namespace, but I advise you 
to add them in the `App/Controller` namespace.

```php showLineNumbers title="UserOverviewRequestHandler.php"
namespace App\Controller;

class UserOverviewRequestHandler
{
    public function __construct(
        private readonly UserOverviewRepository $userOverviewRepository,
    ) {
    }

    public function handle(
        ServerRequestInterface $request,
        ResponseInterface $response): ResponseInterface
    {
        $users = $this->userOverviewRepository->findonyBy(/*...*/);
        $response->getBody()->write(/*...*/);

        return $response;
    }
}
```

After adding your request handler you'll need to register it as a route as well.
Head over to `config/routes.php` and add it over there:

```php showLineNumbers title="config/routes.php"
return function (App $app) {
    // Set default route strategy.
    $routeCollector = $app->getRouteCollector();
    $routeCollector->setDefaultInvocationStrategy(new RequestResponseArgs());
    
    $app->get('/user/overview', UserOverviewRequestHandler::class.':handle');
};
```