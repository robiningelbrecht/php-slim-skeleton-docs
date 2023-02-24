# Template engine

> Twig, The flexible, fast, and secure template engine for PHP

The template engine of choice for this project is [Twig](https://twig.symfony.com/) and 
can be used to render anything HTML related.

## Create a template

Start off with creating a new file in the `templates` directory

```twig showLineNumbers title="templates/users.html.twig"
<h1>Users</h1>
<ul>
    {% for user in users %}
        <li>{{ user.username|e }}</li>
    {% endfor %}
</ul>
```

## Render the template

Then render your template in for example a _RequestHandler_, by injecting the Twig environment:

```php showLineNumbers title="UserOverviewRequestHandler.php"
class UserOverviewRequestHandler
{
    public function __construct(
        private readonly Environment $twig,
    ) {
    }

    public function handle(
        ServerRequestInterface $request,
        ResponseInterface $response): ResponseInterface
    {
        $template = $this->twig->load('users.html.twig');
        $response->getBody()->write($template->render(/*...*/));

        return $response;
    }
}

```
