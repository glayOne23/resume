for latest info : 
- https://medium.com/employbl/build-authentication-into-your-laravel-api-with-json-web-tokens-jwt-cd223ace8d1a
- https://jwt-auth.readthedocs.io/en/docs/laravel-installation/ --> this one

## Intro laravel
- Idenya adalah membuat semacam platform tutorial 
- route-routenya :
    1. /api/tutorial --> liat semua daftar tutorial
    2. /api/tutorial/1 --> isi tutorial dengan id 1   
    3. /api/auth/signup
    4. /api/auth/signin
---
## Setting up API project
1. Setting akses db (.env:db, username, password)
2. Model User.php pindah ke file Models dan ganti namespacenya menjadi App\Models;
3. delete create_password_reset_table
4. di create_user_table :
    ``` php
    $table->increments('id');
    $table->string('username')->unique();
    $table->string('email')->unique();
    $table->string('password');
    $table->timestamp();
    ```
5. di Models User.php : yang fillable :
    ``` php 
    protected $fillable = ['username', 'email', 'password'];
    ```    
6. `php artisan migrate`
---
## Membuat Metode signup
1. Menentukan route API : agar dia otomatis berada pada prefix API:
    1. Buat semua berada pada middleware API :
        ``` php
        Route::grup(['middleware' => ['api']], function(){
            Route::post('auth/signup', 'AuthController@signup');
        });
        ```
    2. buat AuthController : `php artisan make:controller AuthController`, 
        1. isi dengan :
            ``` php
            //di method signup, die('hard') dulu buat cek routenya udah  bener apa blm juga boleh
            public function signup(Request request){
                //validasi datanya nggk boleh kosong dan harus unik tiap username dan emailnya
                $this->validate($request, [
                    'username' => 'required|unique:users',
                    'email' => 'required|unique:users',
                    'password' => 'required',
                ]);
                //kalo dah sukses, bisa masukin datanya ke db
                return User::create([
                    'username' => $request->json('username'),
                    'email' => $request->json('email'),
                    //password dibungkus dalam method bcript
                    'password' => bcript($request->json('password'))
                ]);
            }
            ```
        2. use App\Models\User;        
- Cara signupnya pake postman :
    1. di `/api/auth/signup` key and valuennya di bagian Header:
        key | value
        -|-    
        Content-Type | application/json
        Accept | application/json      
    2. di Body : pake raw isi dengan :
        ``` json
        {
            "username" : "Hammam",
            "password" : "123",
            "email" : "hammam@test.test"
        }
        ```
---
## Konsep JWT (Jason Web Token)
- perbedaan klo login dengan web biasa dibanding api, salah satu cara token yaitu jwt :
    - php --> nyimpen di session
    - jwt --> pake token(setelah user login pertama kali, user akan diberi token, token ini yang harus dimasukkan setiap aksi)
- di sini, kita akan menggunakan jwt dari : `tymondesigns/jwt-auth`
- cara install :
    1. composer require tymon/jwt-auth
---
## Setting awal JWT Tymon :
1. di config/app.php :
    1. di array providers tambah dengan :
    - `Tymon\JWTAuth\Providers\JWTAuthServiceProvider::class,` --> untuk laravel versi lama
    - `Tymon\JWTAuth\Providers\LaravelServiceProvider::class` --> untuk versi baru
    2. di array alias tambah dengan : `'JWTAuth'=>Tymon\JWTAuth\Facades\JWTAuth::class,`
2. jalankan command : untuk nambahin jwt.php di /config :<br/>
    - `php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"` --> untuk laravel versi lama
    - `php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"` --> untuk laravel versi baru
3. generate key :<br/>
- `php artisan jwt:generate` --> versi lama
- `php artisan jwt:secret` --> versi baru
    - copy key yang keluar, tambahkan di .env : ` JWT_SECRET= keynya`
4. setting jwt.php :
    - key pada JWT_SECRET bisa juga ditaruh disini pada `'secret'`
    - pada `user` ganti namespace model user menjadi 'App\Models\User'
5. Pada `auth.php` di bawah `'provider'` `'user'` `'model'` ganti dengan : `App\Models\User::class`    
---
## Sign In dengan Token
1. buat route untuk signin di api.php: `Route::post('auth/signin', 'AuthController@signin');`
2. di authController :
    1. panggil JWT :
        ``` php 
        use JWTAuth;
        use Tymon\JWTAuth\Exceptions\JWTExceptions;
        ```
    2. tambahkan fungsi untuk signin :
        ``` php
        public function signin(Request request){
            //validasi
            $this->validate($request, [
            'username' => 'required',
            'password' => 'required',
            ]);

            //setting untuk JWT
            // grab credentials from the request
            $credentials = $request->only('username', 'password');

            try {
                 // attempt to verify the credentials and create a token for the user
                if (! $token = JWTAuth::attempt($credentials)) {
                    return response()->json(['error' => 'invalid_credentials'], 401);
                }
             } catch (JWTException $e) {
                // something went wrong whilst attempting to encode the token
                return response()->json(['error' => 'could_not_create_token'], 500);
            }

             // all good so return the token
             // cara pertama cuma ngeluarin token   
            // return response()->json(compact('token'));
             //syntax compact('token') sama dengan 'token' => $token

             // cara kedua  ngeluarin user id dan token   
            return response()->json([
                'user_id_' => $request->user()->id,  
                'token'    => $token
            ]);
             
        }
        ```
- Cara signin di postman :
    1. di `/api/auth/signin` menggunakan metode POST
    2. di Body : pake raw isi dengan : buat signin
        ``` json
        {
            "username" : "Hammam",
            "password" : "123",
        }
        ```
    - hasil akan keluar token dan id usernya
---
## Middleware JWT
- Misal setelah login kan diberi token, nah gimana caranya dari token yang didapat kita bisa atur apa aja yang boleh dilihat/ diambil
- example untuk liat id, username, email, created_at, dan updated_at
- Cara membuat middlewarenya :
    1. Tambahin di bagian App/Http/Kernel.php di array routeMiddlewarenya :
        ``` php 
        'jwt.auth' => \Tymon\JWTAuth\Middleware\GetUserFromToken::class,
        'jwt.refresh' => \Tymon\JWTAuth\Middleware\RefreshToken::class,
        ```
    2. di api.php di middleware api kasih middleware lagi jika ingin menuju halaman yang ingin didefinisikan di dalamnya :
        ``` php
        Route::grup(['middleware' => ['api']], function(){
            Route::post('auth/signup', 'AuthController@signup');
            Route::post('auth/signin', 'AuthController@signin');

            Route::grup(['middleware' => ['jwt.auth']], function(){
                Route::get('/profile', 'UserController@show);
            });        
        });

        ```
    3. buat UserController dan isi dengan :
        ``` php
        public function show(Request $request){
            return $request->user(); 
        }
        ``` 
- Cara akses halamannya :
    1. sign in, copy tokennya
    2. menuju api/profile pake method 'GET' di bagian Header:
        key | value
        -|-    
        Authorization | Bearer tokennya
    3. hasilnya sintax json berisi id,username, email, created_at dan updated_at
---
## Membuat Tutorial
1. buat tabel tutorial : `php artisan make:migration create_tutorials_table --create=tutorials`
2. isi dengan :
    ``` php
    $table->increments('id');
    $table->string('title');
    $table->string('slug')->unique();
    //slug itu judul di url gtu kaya (api/tutorial/nama-hewan)
    $table->text('body');
    $table->integer('user_id')->unsigned(); 
    $table->timestamp();

    $table->foreign('user_id')->references(id)->on('users');
    ```
    - php artisan:migrate
3. buat route untuk halaman membuat tutorial :
    ``` php
    Route::grup(['middleware' => ['api']], function(){
        Route::post('auth/signup', 'AuthController@signup');
        Route::post('auth/signin', 'AuthController@signin');

        //yang didalem sini harus pya token untuk aksesnya    
        Route::grup(['middleware' => ['jwt.auth']], function(){
            Route::get('/profile', 'UserController@show');
            Route::post('/tutorial', 'TutorialController@store');            
        });        
    });

    ```
4. Buat Model untuk Tutorial, isi dengan :
    1. ubah namespace `App\Models;`
    2. isi fillable dan relation antara Tutorial dan User:
        ``` php
        protected $fillable = [
            'title', 'slug', 'body'
            //slug nggk usah karena dia otomatis ntar
        ];
        public function user(){
            return $this->belongsTo('App\Models\User');
        } 
        ```
5. di Model User :
    ``` php
    public function tutorials(){
            return $this->hasMany('App\Models\Tutorial');
    }
    ```            
6. Isi TutorialController dengan :
    1. tambahin Model Tutorial : `use App\Models\Tutorial;`
    2. fungsi create() :
        ```php
        public function create(Request $request){
            //validasi
            $this->validate($request, [
                'title' => 'required',
                'body' => 'required',
            ]);
            //kalo dah sukses, bisa masukin datanya ke db
            return $request->user()->tutorials()->create([
                'title' => $request->json('title'),
                //slug diambil dari title yang dibungkus dengan method str_slug() agar ramah di url
                'slug' => str_slug($request->json('title')),
                'body' => $request->json('body'),
            ]);
        }
        ```






    


