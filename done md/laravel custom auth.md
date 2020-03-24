## Steps secara ringkas:
1. buat middleware, daftarin
2. buat model autheticatable
3. buat guard admin dan jajang dan provider
4. buat login get, post, logout
5. buat password harus pake Hash
6. redirectifautheticate diubah

- guard--> buat cek session / token dan simpan , digunain di middleware sama controller dalam kasus ini, bisa jga di view
- middleware--> buat penengah, dalamnya ada guard

- Disini yang akan dibuat custom login laravel dengan 2 role, admin dan jajang. 1 tempat login bisa dibuat authentikasi admin sekalian jajang.
## Steps :
1. bikin table admin dan jajang , isi coloumn kedua table sama  yaitu name dan password
2. di Model Admin dan Jajang ganti extends dengan Autheticatable dan beri use Notifiable;
    ```php
    <?php

    namespace App\Models;


    use Illuminate\Notifications\Notifiable;
    use Illuminate\Contracts\Auth\MustVerifyEmail;
    use Illuminate\Foundation\Auth\User as Authenticatable;

    use Illuminate\Database\Eloquent\Model;

    class Admin extends Authenticatable
    {
        use Notifiable;
    }
    ```
    - di Model Jajang sama kaya gtu tinggal ganti Admin jdi Jajang
2. Bikin data dummy pake tinker, ingat password harus di Hash : https://medium.com/hello-laravel/tinker-with-the-data-laravel-2d08e8250fc8
3. bikin route2nya isinya :
    ```php
    <?php

    Route::get('/', function () {
        return view('welcome');
    });

    Route::get('/dashboard', function () {
        return view('dashboard');
    })->middleware('admin');

    Route::get('/hiya', function () {
        return view('hiya');
    })->middleware('admin');


    Route::get('/jajang', function () {
        return view('jajang');
    })->middleware('jajang');

    Route::get('/login', 'LoginController@index')->middleware('guest');
    Route::post('/login', 'LoginController@post');
    Route::post('/logout', 'LoginController@logout');
    ```
    - yang ada `->middleware('nama_middleware');` itu urlnya dikasih middleware, yang berarti bisa diakses klo middleware memenuhi
4. bikin AdminMiddleware dan JajangMiddleware, daftarin di kernel
    ```php
    <?php

    namespace App\Http\Middleware;

    use Closure;
    use Auth;
    class AdminMiddleware
    {
        /**
         * Handle an incoming request.
         *
         * @param  \Illuminate\Http\Request  $request
         * @param  \Closure  $next
         * @return mixed
         */
        public function handle($request, Closure $next)
        {
            if (Auth::guard('admin')->check()) {
                // if successful, then redirect to their intended location
                return $next($request);
            } 
            abort(403);
        }
    }
    ```
    - JajangMiddleware sama aja isinya cma admin diganti jajang
    - Isi seperti itu , `Auth::guard('admin')->check()` itu buat ngecek sessionnya cocok apa nggk sesuai guard admin

6. bikin guard, gunanya buat bikins session, authetikasi,dll. bikin juga provider guardnya bwt ngasih tau di table mana guardnya berjalan
    ```php
    <?php

    return [

        'defaults' => [
            'guard' => 'web',
            'passwords' => 'users',
        ],


        'guards' => [
            'web' => [
                'driver' => 'session',
                'provider' => 'users',
            ],

            'api' => [
                'driver' => 'token',
                'provider' => 'users',
                'hash' => false,
            ],

            'admin' => [
                'driver' => 'session',
                'provider' => 'admin',
            ],
            'jajang' => [
                'driver' => 'session',
                'provider' => 'jajang',
            ],
        ],

        'providers' => [
            'users' => [
                'driver' => 'eloquent',
                'model' => App\User::class,
            ],

            'admin' => [
                'driver' => 'eloquent',
                'model' => App\Models\Admin::class,
            ],

            'jajang' => [
                'driver' => 'eloquent',
                'model' => App\Models\Jajang::class,
            ],
    
        ],

        'passwords' => [
            'users' => [
                'provider' => 'users',
                'table' => 'password_resets',
                'expire' => 60,
                'throttle' => 60,
            ],
        ],


        'password_timeout' => 10800,

    ];
    ```
    - Disitu liat :
        - di guards, tambahi admin sama jajang, disitu driver pake session dan providernya admin sama jajang
        - di providers, tambahi admin sama jajang, disitu drivernya Eloquest dan modelnya sesuaikan table
7. di LoginController :
    ```php
    <?php

    namespace App\Http\Controllers;

    use Illuminate\Http\Request;
    use Auth;
    // use Illuminate\Support\Facades\Auth;

    class LoginController extends Controller
    {
        /**
         * Display a listing of the resource.
         *
         * @return \Illuminate\Http\Response
         */
        public function index()
        {
            return view('login');
        }

        /**
         * Show the form for creating a new resource.
         *
         * @return \Illuminate\Http\Response
         */
        public function post(Request $request)
        {
            $errors = $request->validate([
                'name' => 'required',
                'password' => 'required'
            ]);
                // Attempt to log the user in
                // Passwordnya pake bcrypt
            if (Auth::guard('admin')->attempt(['name' => $request->name, 'password' => $request->password])) {
                // if successful, then redirect to their intended location
                return redirect()->intended('/dashboard');
            } else if (Auth::guard('jajang')->attempt(['name' => $request->name, 'password' => $request->password])) {
                return redirect()->intended('/jajang');
            }
            return redirect()->intended('/login');
        }

        public function logout()
        {
        if (Auth::guard('admin')->check()) {
            Auth::guard('admin')->logout();
        } elseif (Auth::guard('jajang')->check()) {
            Auth::guard('jajang')->logout();
        }
        return redirect('/login');
        }
    }
    ```
    - Nah di login ini session dibuat , jika sukses akan di redirect
8. ubah RedirectIfAuthenticated.php biar nggk bisa akses login jika udah login
    ```php
    <?php

    namespace App\Http\Middleware;

    use App\Providers\RouteServiceProvider;
    use Closure;
    use Illuminate\Support\Facades\Auth;

    class RedirectIfAuthenticated
    {
        /**
         * Handle an incoming request.
         *
         * @param  \Illuminate\Http\Request  $request
         * @param  \Closure  $next
         * @param  string|null  $guard
         * @return mixed
         */
        public function handle($request, Closure $next, $guard = null)
        {
            if (Auth::guard('admin')->check()) {

                return redirect('/dashboard');
        
            } else if (Auth::guard('jajang')->check()) {
        
                return redirect('/jajang');
                
            }
        
            return $next($request);
        }
    }

    ```
7. di login.blade.php :
    ```html
    <!DOCTYPE HTML>
    <!--
        Eventually by HTML5 UP
        html5up.net | @ajlkn
        Free for personal and commercial use under the CCA 3.0 license (html5up.net/license)
    -->
    <html>
        <head>
            <title>Eventually by HTML5 UP</title>
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
            <link rel="stylesheet" href="/eventually/assets/css/main.css" />
        </head>
        <body class="is-preload">

            <!-- Header -->
                <header id="header">
                    <h1>Eventually</h1>
                    <p>A simple template for telling the world when you'll launch<br />
                    your next big thing. Brought to you by <a href="http://html5up.net">HTML5 UP</a>.</p>
                </header>

            <!-- Signup Form -->
                <form id="signup-form" method="post" action="/login">
                    @csrf
                    <input type="text" name="name" id="name" placeholder="Username" /> 
                    <input type="password" name="password" id="password" placeholder="Password" />
                    <input type="submit" value="Sign Up" />
                </form>
                    @if ($errors->has('name'))
                        <p style="color:red;">{{$errors->first('name')}} </p>
                    @endif
                    @if ($errors->has('password'))
                        <p style="color:red;">{{$errors->first('password')}} </p>
                    @endif
                    @if ($errors->has('credential'))
                        <p style="color:red;">{{$errors->first('credential')}} </p>
                    @endif
                

            <!-- Footer -->
                <footer id="footer">
                    <ul class="icons">
                        <li><a href="#" class="icon brands fa-twitter"><span class="label">Twitter</span></a></li>
                        <li><a href="#" class="icon brands fa-instagram"><span class="label">Instagram</span></a></li>
                        <li><a href="#" class="icon brands fa-github"><span class="label">GitHub</span></a></li>
                        <li><a href="#" class="icon fa-envelope"><span class="label">Email</span></a></li>
                    </ul>
                    <ul class="copyright">
                        <li>&copy; Untitled.</li><li>Credits: <a href="http://html5up.net">HTML5 UP</a></li>
                    </ul>
                </footer>

            <!-- Scripts -->
                <script src="/eventually/assets/js/main.js"></script>

        </body>
    </html>
    ```

- Source :
    - https://medium.com/@alfinchandra4/catatan-laravel-manual-login-logout-multi-auth-fa9c8ca12e54
    - https://www.phpclasses.org/blog/post/918-How-to-use-Laravel-Multi-Auth-Guard-in-Laravel-58.html
    - https://medium.com/hello-laravel/tinker-with-the-data-laravel-2d08e8250fc8