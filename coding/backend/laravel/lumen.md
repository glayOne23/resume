## 1 Installation
- composer create-project --prefer-dist laravel/lumen blog
- run in browser : php -S localhost:8000 -t public
- generate key : because there is no tinker here, so you must make key using str_random:
    ``` php
    $router->get('/key', function(){
    return str_random(32);
    });
    ```
    - copy the result on APP KEY in .env
## 2 Grouping URL
``` php
$router->group(['prefix' => 'admin'], function() use($router) {
    $router->get('/foo', function(){
        return "method get";
    });
});
```
- to call : 127.0.0.1/admin/foo
## 3 Middleware
1. Make Middleware, write into it : 
    ```php
    class AgeMiddleware
    {   
        public function handle($request, Closure $next)
        {
            if ($request->age < 17) {
                return redirect('/fail');
            }
            return $next($request);
        }
    }
    ```
2. Register the middleware in bootstrap\app:
- If u want make global middleware, uncomment `$app->middleware`
- if u want some route, uncomment `$app->routeMiddleware` and register new middleware :
    ```php
    $app->routeMiddleware([
        'auth' => App\Http\Middleware\Authenticate::class,
        'age' => App\Http\Middleware\AgeMiddleware::class,
    ]);
    ```
3. in route :
    ```php
    $router->group(['prefix' => 'admin', 'middleware' => 'age'], function() use($router) {

        $router->get('/age', function(){
            return "Old enough";
        });

    });

    $router->get('/fail', function(){
        return "Age doesnt enough";
    });
    ```
4. test it : `http://127.0.0.1:8000/admin/age?age=22`
## 4 Middleware on Controller
1. route : `$router->get('/rahasia', 'RahasiaController@index');`
2. in Rahasia Controller:
    ```php
    //implement to all function in this controller
    public function __construct()
    {
        $this->middleware('age');
    }
    //implement only for some function in this controller
    public function __construct()
    {
        $this->middleware('age',['only' => ['index']] );
    }
    //implement except for some function in this controller
    public function __construct()
    {
        $this->middleware('age',['except' => ['index']] );
    }
    ```
