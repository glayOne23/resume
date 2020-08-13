# Preparation :
1. composer create-project --prefer-dist laravel/lumen project-name
2. php -S localhost:8000 -t public
3. sudo apt-get install php7.3-pgsql
4. install psql and create database :
    - https://www.niagahoster.co.id/blog/cara-install-postgresql-di-ubuntu-18-04/ 
    -https://stackoverflow.com/questions/7695962/postgresql-password-authentication-failed-for-user-postgres
5. di .env setting database :
    ```
    DB_CONNECTION=pgsql
    DB_HOST=127.0.0.1
    DB_PORT=5432
    DB_DATABASE=portfolio
    DB_USERNAME=postgres
    DB_PASSWORD=postgres
    ```
6. install jwt for lumen : https://jwt-auth.readthedocs.io/en/develop/lumen-installation/
7. enable eloquent : https://medium.com/@petehouston/enable-eloquent-orm-in-laravel-lumen-micro-framework-7a4f2fbcaf5d
8. jwt quick start : https://medium.com/@ridhofebriansa/cara-menggunakan-jwt-json-web-token-pada-lumen-5-8-dan-postgresql-1018120fa735
9. create route and controller
    - Route :
        ```php
        <?php


        // $router->get('/', function () use ($router) {
        //     return $router->app->version();
        // });


        $router->group(['prefix' => 'api'], function ($router) {

            $router->post('login', 'AuthController@login');
            
            $router->group(['middleware' => 'auth:api'], function ($router) {
                $router->get('me', 'AuthController@me');
                $router->post('logout', 'AuthController@logout');
                $router->post('refresh', 'AuthController@refresh');
            });

        });

        ```
    - controller :
        ```php
        <?php

        namespace App\Http\Controllers;
        use \Illuminate\Http\Request;
        use Illuminate\Support\Facades\Auth;

        class AuthController extends Controller
        {

            
            public function __construct()
            {
                // $this->middleware('auth:api', ['except' => ['login']]);
            }
            

            public function login(Request $request)
            {
                $credentials = request(['name', 'password']);

                if (! $token = auth()->attempt($credentials)) {
                    return response()->json(['error' => 'Unauthorized'], 401);
                }

                return $this->respondWithToken($token);
            }


            public function me()
            {
                return response()->json(auth()->user());
            }

            
            public function logout()
            {
                auth()->logout();

                return response()->json(['message' => 'Successfully logged out']);
            }

            
            public function refresh()
            {
                return $this->respondWithToken(auth()->refresh());
            }

            
            protected function respondWithToken($token)
            {
                return response()->json([
                    'access_token' => $token,
                    'token_type' => 'bearer',
                    'expires_in' => auth()->factory()->getTTL() * 60
                ]);
            }
        }
        ```
10. enable cors : https://daengweb.id/membuat-aplikasi-ekspedisi-lumen-6-4-cors-handling-fetch-loggedin-user


