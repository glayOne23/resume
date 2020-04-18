# 1. Intro
- Pola arsitektur pada OOP
- tujuan : memisahkan antara tampilan, data, dan process (view, model, controller)
- Sifat : Begitu buka file mvcnya, halaman akan menampilkan controller default yang didalam controller default terdapat method default.
![](img_mvc/1.png?raw=true)

# 2. Persiapan
## Struktur folder dan file-file utama :
1. public --> menyimpan file-file yang bisa diakses oleh user
    1. index.php --> file utama yang akan diakses oleh user
    2. css, js, img
    3. .htaccess
2. app --> menyimpan folder dan file-file utama dari aplikasi mvc (nggk bisa diakses user)
    1. core
        1. App.php -->
        2. Controller.php --> 
    2. controllers
    3. views
    4. models
    5. init.php
    6. .htaccess
- Dari index.php bisa dipanggil aplikasi mvcnya melalui init.php
    ```php
    <?php

    require_once "../app/init.php";

    $app = new App;
    ```
- Guna init.php --> memanggil file yang dibutuhkan yang terdapat pada folder app. Teknik ini dinamakan bootstraping
- Bootstraping --> memanggil 1 file yang akan memanggil seluruh aplikasi mvcnya
- Isi init.php :
    ```php
    <?php

    require_once "core/App.php";
    require_once "core/Controller.php";
    ```
    - 2 file tersebut merupakan 2 class utama pembentuk mvcnya
- Cek apa sudah berjalan sesuai keinginan : 
1. di core/App.php 
    ```php
    class App {
        public function __construct(){
            echo "OK!!";
        }
    }
    ```
2. di core/Controller.php 
    ```php
    class Controller {
    
    }
    ```
# 3. Routing
- .htaccess --> untuk setting server
1.  .htaccess di app dir :
    - Guna : agar folder app tidak bisa diakses
        ```htaccess
        Options -Indexes
        ```
2.  .htaccess di public dir :
    - Guna : 
    1. untuk menangkap url setelah /domain/public/ yang nanti akan diolah oleh App.php
    2. Menghilangkan index.php untuk mempercantik url
        ```htaccess
        Options -Multiviews

        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php?url=$1 [L]
        ```
        - Liat kode diatas, url setelah /domain/public/ akan disimpan pada $_GET["url"] yang nanti akan diolah oleh App.php
3. Di App.php
    ```php
    <?php

    class App {
        public function __construct()
        {
            $url = $this->parseURL();
            var_dump($url);
        }

        public function parseURL()
        {
            if(isset($_GET["url"]))
            {   
                // menghapus / terakhir
                $url = rtrim($_GET["url"], '/');
                // membersihkan url dari karakter-karakter tidak diinginkan
                $url = filter_var($url, FILTER_SANITIZE_URL);
                // memasukkan karakter dalam aaray dengan batasan '/'
                $url = explode('/', $url);
                return $url;
            }
        }
    }
    ```
    - Sementara ini hanya untuk men dd url apakah sudah berhasil ditangkap atau belum
- Result :
    ``` html
    http://localhost/mvc-sandika/public/about/

    <!-- result -->
    array(1) { [0]=> string(5) "about" }
    ```

# 4. Controller
- Disini akan dibahas bagaimana array yang ditangkap pada url itu akan diproses
- Nangkepnya di prosesnya di App.php
    ```php
    <?php

    class App {

        // controller, method, dan parameter default yang dipanggil dari url
        protected $controller = 'Home';
        protected $method = 'index';
        protected $params = [];

        public function __construct()
        {
            $url = $this->parseURL();

            $url = $this->get_controller_from_url($url);
            $url = $this->get_method_controller_from_url($url);
            $this->get_params_from_url($url);

            // Jalankan controller dan method, serta kirim params jika ada
            // namanya callback function : call_user_func_array( [ class, method ], parameter_dalam_array );
            // guna : agar bisa ngirim parameter terserah berapa
            call_user_func_array( [ $this->controller, $this->method ], $this->params );
        }


        public function parseURL()
        {
            if(isset($_GET["url"]))
            {   
                // menghapus '/' terakhir
                $url = rtrim($_GET["url"], '/');
                // membersihkan url dari karakter-karakter tidak diinginkan
                $url = filter_var($url, FILTER_SANITIZE_URL);
                // memasukkan karakter dalam array dengan batasan '/'
                $url = explode('/', $url);
                return $url;
            }
        }


        // untuk mendapatkan controller dari url (dalam kasus ini karakter setelah /public/)
        public function get_controller_from_url($url) 
        {
            $expected_controller = ucfirst($url[0]);
            // jika controller ada, lakukan perintahnya
            if (file_exists('../app/controllers/' . $expected_controller . '.php') ) {
                $this->controller = $expected_controller;
                unset($url[0]);
            }
            
            // inisiasi controller , jika pada url controller tidak ada, maka akan menggunakan controller default
            require_once '../app/controllers/' . $this->controller . '.php';
            $this->controller = new $this->controller;

            return $url;
        }


        // untuk mendapatkan method dari url jika ada(dalam kasus ini karakter setelah /public/controller)
        public function get_method_controller_from_url($url)
        {
            if (isset($url[1])) 
            {
                if (method_exists($this->controller, $url[1]) ) 
                {
                    $this->method = $url[1];
                    unset($url[1]);
                }
            }
            return $url;
        }


        // untuk mendapatkan params dari url jika ada (dalam kasus ini karakter setelah /public/controller/method/)
        public function get_params_from_url($url)
        {
            if ( !empty($url) ) 
            {
                $this->params = array_values($url);
            }
        }
    }
    ```
- Buat Controller Home :
    ```php
    <?php

    class Home {

        public function index() 
        {
            echo '<h1>Halaman Home</h1>';
        }
    }
    ```
- Buat Controller About :
    ```php
    <?php

    class About {

        public function index($nama='Jajang', $profesi= 'Pemain Minecraft') 
        {
            echo "<h1>Halo saya $nama, Saya seorang $profesi </h1>";
        }
    }
    ```
- Sementara belum ada view jadi menampikannya langsung pada controller
- Result
    ```php
    http://localhost/mvc-sandika/public/

    Halaman Home
    ```
    ```php
    http://localhost/mvc-sandika/public/about/Andi/Pelaut

    Halo saya Andi, Saya seorang Pelaut
    ```
    ```php
    http://localhost/mvc-sandika/public/about/

    Halo saya Jajang, Saya seorang Pemain Minecraft
    ```

# 5. View
- Jadi semua controller akan inherit dari core/Controller. Di core/Controller, terdapat method view() untuk mengarahkan semua tampilan pada folder view.
- core/Controller tidak perlu di require_once pada setiap controller karena sudah di panggil pada file init.php
- Ingat ini : 
    ```php
    <?php

    require_once "core/App.php";
    require_once "core/Controller.php";
    ```
- Pada core/Controller :
    ```php
    <?php

    class Controller {
        public function view ($view, $data = [])
        {
            require_once '../app/views/' . $view . '.php';
        }
    }
    ```
- pada Home Controller :
    ```php
    <?php

    class Home extends Controller{

        public function index() 
        {
            $this->view('home/index');
        }
    }
    ```
- pada views/home/index.php :
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Home</title>
    </head>
    <body>
        <h1>Selamat Datang</h1>
    </body>
    </html>
    ```
- Result :
    ```php
    http://localhost/mvc-sandika/public/

    Selamat Datang
    ```
- Pada About Controller :
    ```php
    <?php

    class About extends Controller{

        public function index($nama='Jajang', $profesi= 'Pemain Minecraft') 
        {
            $data['nama'] = $nama;
            $data['profesi'] = $profesi;

            $this->view('about/index', $data);
        }

        public function page ()
        {
            $this->view('about/page');
        }
    }
    ```
- pada views/about/index.php :
    ```php
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Halaman About</title>
    </head>
    <body>
        <h1>Halo saya <?= $data['nama']; ?>, saya seorang <?= $data['profesi']; ?>  </h1>
    </body>
    </html>
    ```
# 5.1. Membuat Template
- Code yang selalu sama pada tiap view dapat dimasukkan pada view khusus agar nanti cukup controller memanggil view khusus tersebut. Gunanya untuk memperingkas tiap view.
- Steps : 
1. buat folder template pada views, buat file header.php dan footer.php 
- Pada template/header.php :
    ```php
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title><?= $data['judul']?></title>
    </head>
    <body>
    ```
- Pada template/footer.php :
    ```php
    </body>
    </html>
    ```
2. Hapus baris code yang sama pada tiap view yang sudah terdapat pada header.php dan footer.php
3. Panggil template view pada controller
- di Home Controller :
    ```php
    <?php

    class Home extends Controller{

        public function index() 
        {
            $data['judul'] = "Home";

            $this->view('template/header', $data);
            $this->view('home/index');
            $this->view('template/footer');
        }
    }
    ```
- di About Controller :
    ```php
    <?php

    class About extends Controller{

        public function index($nama='Jajang', $profesi= 'Pemain Minecraft') 
        {
            $data['nama'] = $nama;
            $data['profesi'] = $profesi;

            $data['judul'] = 'About Me';

            $this->view('template/header', $data);
            $this->view('about/index', $data);
            $this->view('template/footer');
        }

        public function page ()
        {
            $data['judul'] = "My Pages";

            $this->view('template/header', $data);
            $this->view('about/page');
            $this->view('template/footer');
        }

    }
    ```

# 6. Assets
- Menampilkan CSS, JS, dan Gambar
- Steps :
1. Copy file js dan css nya, paste pada folder css dan js pada folder public
2. buat file constants agar penulisan absolute path nya tidak berulang
- Buat file core/Constants.php isi dengan :
    ```php
    <?php
    // define(nama_constants, isi_constants);
    define('BASEURL', 'http://localhost/mvc-sandika/public');
    ```
- Beri tahu file init.php kalo file core/Constants.php akan sering dipanggil
    ```php
    <?php

    require_once "core/App.php";
    require_once "core/Controller.php";
    require_once "core/Constants.php";
    ```
3. beri path absolute (path dari url) yang mengarah pada file css dan jsnya
- Pada template/header.php tambahkan:
    ```php
    <link rel="stylesheet" href="<?= BASEURL?>/css/bootstrap.min.css">
    ```
- Pada template/footer.php tambahkan:
    ```php
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="<?=BASEURL?>/js/bootstrap.min.js"></script>
    ```

# 7. Model
- Model itu isinya luas, bisa berupa mengolah data, object domain, data mapper, services. Intinya business logic dari aplikasi yang akan kita buat.
- Steps :
1. di core/Controller tambahkan fungsi untuk memanggil model dari controller
    ```php
    public function model ($model)
    {
        require_once '../app/models/' . $model . '.php';

        // inisiasi model
        return new $model;
    }
    ```
2. Buat model baru : disini models/User.php : (di contoh ini hanya untuk memanggil $nama)
    ```php
    <?php

    class User {

        private $nama="Hammam";

        public function getUser()
        {
            return $this->nama;
        }
    }
    ```
3. Pada About Controller panggil Model dan fungsinya:
    ```php
    public function page ()
    {
        $data['judul'] = "My Pages";

        // ini modelnya
        $data['nama'] = $this->model('User')->getUser();

        $this->view('template/header', $data);
        $this->view('about/page', $data);
        $this->view('template/footer');
    }
    ```
4. di views/about/page.php
    ```php
    <h1>My Pages</h1>

    <?=$data['nama'];?>
    ```

# 7.1. Model and Database (Using PDO) Part 1
