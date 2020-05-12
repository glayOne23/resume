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
    7. config/config.php --> konstanta-konstanta konfigurasi
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
2. Buat model baru : disini models/UserModel.php : (di contoh ini hanya untuk memanggil $nama)
    ```php
    <?php

    class UserModel {

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
        $data['nama'] = $this->model('UserModel')->getUser();

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

# 8.1. Model and Database Menggunakan PDO [Menampilkan Semua Data]
- Sebelumnya, bikin folder baru untuk mengatur semua konfigurasi : app/config/config.php, isinya konstanta-konstanta konfigurasi global
- Karena semua konstanta-konstanta konfigurasi ada disitu, konstanta pada Constants.php dapat dipindah ke app/config/config.php, file core/Constants.php dihapus aja, dan ubah file init.phpnya :
    ```php
    <?php

    require_once "core/App.php";
    require_once "core/Controller.php";
    require_once "config/config.php";
    ```
- di app/config/config.php
    ```php
    <?php

    define('BASEURL', 'http://localhost/mvc-sandika/public');

    // DB
    define('DB_HOST', '127.0.0.1');
    define('DB_USER', 'root');
    define('DB_PWD', '');
    define('DB_NAME', 'mvc_sandika');
    ```
- Steps menyambungkan DB ke MVC : misal buat DB bernama mvc_sandika, disana terdapat tabel achievements yang berisi prestasi-prestasi user yang akan ditampilkan pada halaman Achievement
1. buat core/Database.php yang berisi setting database :
    ```php
    <?php

    class Database {

        // mengambil dari kontanta di config/config.php
        private $host = DB_HOST;
        private $user = DB_USER;
        private $pwd = DB_PWD;
        private $db_name = DB_NAME;

        // $dbh stands for database handler (for connecting to db)
        private $dbh;
        // stmt stands for statement (for saving query)
        private $stmt;

        public function __construct()
        {
            // data source name
            $dsn = "mysql:host=$this->host;dbname=$this->db_name";

            // untuk optimasi
            $option = [
                PDO::ATTR_PERSISTENT =>true,
                PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION
            ];

            try {
                $this->dbh = new PDO ($dsn, $this->user, $this->pwd, $option);
            } catch (PDOException $e) {
                die($e->getMessage());
            }
        }


        // buat function generic untuk query (bisa dipake untuk query apapun)
        public function query($query){
            $this->stmt = $this->dbh->prepare($query);
        }

        // untuk binding data (misal ada where nya, maka untuk menghidari sql injection bersihin dulu querynya)
        public function bind($param, $value, $type = null){
            if (is_null($type)){
                switch(true){
                    case is_int($value) :
                        $type = PDO::PARAM_INT;
                        break;  
                    case is_bool($value) :
                        $type = PDO::PARAM_BOOL;
                        break;
                    case is_null($value) :
                        $type = PDO::PARAM_NULL;
                        break;    
                    default :
                        $type = PDO::PARAM_STR;
                }
            }

            $this->stmt->bindValue($param, $value, $type);
        }

        // eksekusi querynya
        public function execute(){
            $this->stmt->execute();
        }

        // untuk menampilkan semua hasil
        public function all(){
            $this->execute();
            return $this->stmt->fetchAll(PDO::FETCH_ASSOC);
        }

        // untuk menampilkan 1 hasil
        public function single(){
            $this->execute();
            return $this->stmt->fetch(PDO::FETCH_ASSOC);
        }
    }
    ```
2. daftarkan Database.php pada file init.php :
    ```php
    <?php

    require_once "core/App.php";
    require_once "core/Controller.php";
    require_once "core/Database.php";

    require_once "config/config.php";
    ```
3. menggunakan Database.php untuk men-query-kan semua data achievements pada model AchievementModel.php
    ```php
    <?php

    class AchievementModel {

        // table related to this model
        private $table = "achievements";
        
        private $db;

        // begitu manggil model ini, maka akan langsung instansiasi class Database untuk konek ke db 
        public function __construct()
        {
            $this->db = new Database;
        }

        public function getAll()
        {
            $this->db->query("SELECT * FROM $this->table");
            return $this->db->all();
        }
    }
    ```
4. masukkan ke template. bikin controller, dan view untuk menampilkan Achievement :
5. di template/header.php , tambahkan :
    ```php
    <li class="nav-item">
    <a class="nav-link" href="<?=BASEURL?>/achievement/index">Achievement</a>
    </li>
    ```
6. buat controller Achievement.php :
    ```php
    <?php

    class Achievement extends Controller{

        public function index() 
        {
            $data['judul'] = "Achievement";
            // menampilkan semua achievement
            $data['achievements'] = $this->model('AchievementModel')->getAll();

            $this->view('template/header', $data);
            $this->view('achievement/index', $data);
            $this->view('template/footer');
        }
    }
    ```
7. buat view achievement/index.php :
    ```html
    <ul class="list-group">
    <?php foreach ($data['achievements'] as $achievement) :?>
    <li class="list-group-item"><h3><?= $achievement['judul']?></h3></li>
    <li class="list-group-item"><?= $achievement['caption']?> </li>
    <?endforeach?>
    </ul>
    ```
# 8.2. Model and Database Menggunakan PDO [Menampilkan 1 Data]
- Steps : Idenya adalah membuat badge detail pada tiap achievement yang muncul, ketika badge diklik, maka akan mempilkan detail achievementnya :
1. ubah view achievement/index.php :
    ```html
    <div class="row">
        <div class="col-6">
            <ul class="list-group">
            <?php foreach ($data['achievements'] as $achievement) :?>
            <li class="list-group-item d-flex justify-content-between align-items-center" >
            <?= $achievement['judul']?>
            <a href="<?= BASEURL?>/achievement/detail/<?= $achievement['id']?>" class="badge badge-primary" >detail</a>
            </li>
            <?endforeach?>
            </ul>
        </div>
    </div>
    ```
2. di AchievementModel, buat method untuk menampilkan detail achievement :
    ```php
        public function getAchievementById($id)
        {
            // :id untuk bind data
            $this->db->query("SELECT * FROM $this->table WHERE id=:id");
            $this->db->bind('id', $id);
            return $this->db->single();
        }
    ```
3. di controller Achievement, panggil method  tersebut :
    ```php
    public function detail($id) 
    {
        $data['judul'] = "Achievement";
        $data['achievement'] = $this->model('AchievementModel')->getAchievementById($id);

        $this->view('template/header', $data);
        $this->view('achievement/detail', $data);
        $this->view('template/footer');
    }
    ```
4. buat view achievement/detail :
    ```html
    <div class="row">
    <div class="col-sm-6">
        <div class="card">
        <div class="card-body">
            <h5 class="card-title"><?= $data['achievement']['judul']?></h5>
            <p class="card-text"><?= $data['achievement']['caption']?></p>
            <a href="<?= BASEURL?>/achievement/" class="btn btn-primary">Kembali</a>
        </div>
        </div>
    </div>
    </div>
    ```
# 8.3. Model and Database Menggunakan PDO [Insert Data]
- Steps :
1. di view achievement/index.php : tambahkan button dan modal untuk insert data :
    ```html
    ---
    
    <div class="mt-3 mb-3">
    <!-- Button trigger modal -->
    <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#achievementModal">
    Add Achievement
    </button>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="achievementModal" tabindex="-1" role="dialog" aria-labelledby="achievementModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title" id="achievementModalLabel">Add Achievement</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
            </button>
        </div>
            <form action="<?= BASEURL; ?>/achievement/insert" method="post">
                <div class="modal-body">
                    <div class="form-group">                    
                        <input type="text" class="form-control" id="judul" name="judul" placeholder="Judul">
                    </div>
                    <div class="form-group">                    
                        <input type="text" class="form-control" id="judul" name="caption" placeholder="Caption">
                    </div>
                </div>
                <div class="modal-footer">                
                    <button type="submit" class="btn btn-primary">Save</button>
                </div>
            </form>
        </div>
    </div>
    </div>

    ---
    ``` 
2. di contoller Achievement :buat method untuk tambahkan data achievement dan masuk ke halaman /achievement jika data berhasil masuk:
    ```php
    public function insert()
    {
        if($this->model('AchievementModel')->addData($_POST) > 0) {
            header('Location:' . BASEURL . '/achievement' );
            exit;
        }
    }
    ```
3. di model AchievementModel : buat method untuk tambah data achievement
    ```php
    public function addData($data)
    {
        $this->db->query("INSERT INTO `achievements` (`id`, `judul`, `caption`) VALUES (NULL, :judul, :caption);" );
        $this->db->bind('judul', $data['judul']);
        $this->db->bind('caption', $data['caption']);
        $this->db->execute();       
        
        // untuk menghitung data agar bisa redirect
        return $this->db->rowCount();
    }
    ```
4. di core/Database, buat fungsi rowCount untuk menhitung data :
    ```php
    // menhitung ada berapa baris pda statement
    public function rowCount()
    {
        return $this->stmt->rowCount();
    }
    ```
# 8.4. Model and Database Menggunakan PDO [Flash Message]
- Menggunakan session
- Steps :
1. di core buat Flasher.php : untuk mengelola flash message :
    ```php
    <?php

    class Flasher {

        // set session untuk flash
        public static function setFlash($message, $action, $type)
        {
            $_SESSION['flash'] =[
                'message' => $message,
                'action' => $action,
                'type' => $type
            ];
        }

        // jika session flash ada maka tampilkan flash
        public static function flash() 
        {
            if (isset($_SESSION['flash'])) {
                echo '<div class="alert alert-' . $_SESSION['flash']['type'] . ' alert-dismissible fade show" role="alert">
                        Achievement data<strong> ' . $_SESSION['flash']['message'] . '</strong> ' . $_SESSION['flash']['action'] . ' 
                        <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>';
                unset($_SESSION['flash']);
            }
        }

    }
    ```
2. require class Flasher pada init.php :
    ```php
    <?php

    require_once "core/App.php";
    require_once "core/Controller.php";
    require_once "core/Database.php";
    require_once "core/Flasher.php";

    require_once "config/config.php";
    ```
3. di public/index.php aktifkan sessionnya :
    ```php
    <?php

    if(!session_id()) session_start();

    require_once "../app/init.php";

    $app = new App;

    ```
4. letakkan class Flash pada achievement/index untuk menampilkan flash di sana :
    ```php
    <div class="row" >
    <div class="col-6">
        <?php Flasher::flash();?>
    </div>
    </div>
    ```
5. Pada controller Achievement/insert : beri flash jika berhasil atau gagal :
    ```php
    public function insert()
    {
        if($this->model('AchievementModel')->addData($_POST) > 0) {
            Flasher::setFlash("successfully", "added", "success");
            header('Location:' . BASEURL . '/achievement' );
            exit;
        }else{
            Flasher::setFlash("failed", "to add", "danger");
            header('Location:' . BASEURL . '/achievement' );
            exit;
        }
    }
    ```
# 8.5. Model and Database Menggunakan PDO [Delete]
- Steps :
1. di view achievement/index.php : tambahankan button delete
    ```php
    <!-- index achievements -->
    <div class="row">
        <div class="col-6">
            <ul class="list-group">
            <?php foreach ($data['achievements'] as $achievement) :?>
            <li class="list-group-item" >
            <?= $achievement['judul']?>
            <a href="<?=BASEURL?>/achievement/delete/<?=$achievement['id']?>" class="badge badge-danger float-right ml-1" onclick="return confirm ('Are you want do delete?')" >delete</a>
            <a href="<?= BASEURL?>/achievement/detail/<?= $achievement['id']?>" class="badge badge-primary float-right" >detail</a>
            </li>
            <?endforeach?>
            </ul>
        </div>
    </div>
    ```
2. di Achievement.php : buat delete method yang menerima $id dari url
    ```php
        public function delete($id)
        {
            if($this->model('AchievementModel')->deleteData($id) > 0) {
                Flasher::setFlash("successfully", "deleted", "success");
                header('Location:' . BASEURL . '/achievement' );
                exit;
            }else{
                Flasher::setFlash("failed", "to delete", "danger");
                header('Location:' . BASEURL . '/achievement' );
                exit;
            }
        }
    ```
3. di model AchievementModel.php :
    ```php
    public function deleteData($id)
    {
        $this->db->query("DELETE FROM `achievements` WHERE id = :id;" );
        $this->db->bind('id', $id);
        $this->db->execute();       
        
        return $this->db->rowCount();
    }
    ```
# 8.6. Model and Database Menggunakan PDO [Update + AJAX]
1. buat tombol update
2. buat js baru bernama script.js pada /public/js dan insert dalam view footer :
    ```html
    </div>
    </body>
    <script src="https://code.jquery.com/jquery-3.3.1.js" integrity="sha256-2Kok7MbOyxpgUVvAk/HJ2jigOSYS2auK4Pfzbm7uH60=" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="<?=BASEURL?>/js/bootstrap.min.js"></script>
    <script src="<?=BASEURL?>/js/script.js"></script>
    </html>
    ```
3. di achievement/index :
    ```html
    <div class="row" >
    <div class="col-6">
        <?php Flasher::flash();?>
    </div>
    </div>
    <div class="mt-3 mb-3">
    <!-- Button trigger modal -->
    <!-- 3) tambahkan id addDataButton untuk jquery (kata Add Achievement biar naggak keganti Update Achievement) -->
    <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#achievementModal" id="addDataButton">
    Add Achievement
    </button>
    </div>

    <!-- Modal Insert Achievement-->
    <div class="modal fade" id="achievementModal" tabindex="-1" role="dialog" aria-labelledby="achievementModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title" id="achievementModalLabel">Add Achievement</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
            </button>
        </div>
        <form action="<?= BASEURL; ?>/achievement/insert" method="post">
            <!-- 6 tambahkan hidden input type untuk update data -->
            <input type="hidden" name="id" id="id">
            <div class="modal-body">
                <div class="form-group">                    
                    <input type="text" class="form-control" id="judul" name="judul" placeholder="Judul">
                </div>
                <div class="form-group">                    
                    <input type="text" class="form-control" id="caption" name="caption" placeholder="Caption">
                </div>
            </div>
            <div class="modal-footer">                
                <button type="submit" class="btn btn-primary">Save</button>
            </div>
        </form>
        </div>
    </div>
    </div>

    <!-- index achievements -->
    <div class="row">
        <div class="col-6">
            <ul class="list-group">
            <?php foreach ($data['achievements'] as $achievement) :?>
            <li class="list-group-item" >
            <?= $achievement['judul']?>
            <a href="<?=BASEURL?>/achievement/delete/<?=$achievement['id']?>" class="badge badge-danger float-right ml-1" onclick="return confirm ('Are you want do delete?')" >delete</a>
            <!-- 1) href nggak perlu ada isinya soalnya ntar pake ajax -->
            <!-- 2) class showUpdateModal buat nandain jquery kalo link ini diclick maka akan menampilkan modal untuk update -->
            <!-- 4) data-id adalah attribute buatan agar nanti bisa ditangkap jquery -->
            <a href="" class="badge badge-success float-right ml-1 showUpdateModal" data-toggle="modal" data-target="#achievementModal" data-id= "<?=$achievement['id'];?>" > update</a>
            <a href="<?= BASEURL?>/achievement/detail/<?= $achievement['id']?>" class="badge badge-primary float-right" >detail</a>
            </li>
            <?endforeach?>
            </ul>
        </div>
    </div>

    ```
4. di script.js :
    ```js
    // 1) ketika dokumen sudah siap, jalankan fungsi dibawah ini
    $(function () {

        $('#addDataButton').on('click', function(){
            // 3) Biar nggak berubah menjadi Update Achievement Modal Labelnya
            $('#achievementModalLabel').html('Add Achievement');
            // 5.5) Biar datanya kosong ketika klik Add Achievement
            $('#judul').val('');
            $('#caption').val('');
        });

        // 2) kenapa yang ditangkap class bukan id? karena tombol updatenya kan banyak, nggak bisa pake id
        $('.showUpdateModal').on('click', function(){
            $('#achievementModalLabel').html('Update Achievement');

            // 4) menangkap data-id update yang berisi id achievement
            const id = $(this).data('id');

            // 6.1) ubah action menuju method updateAchievement
            $('.modal-content form').attr('action', 'http://localhost/mvc-sandika/public/achievement/update');
            
            $.ajax({
                // 5.1) mengambil data ke url ini untuk mendapatkan data achievement sesuai id yang kita kirimkan
                url : 'http://localhost/mvc-sandika/public/achievement/getDataForUpdate',
                // 5.2) mengirim id dari const id ke url dengan nama id
                data : {id:id},
                // 5.3) data dikirimkan dengan method post, agar ntar bis datangkep pake $_POST
                method : 'post',
                dataType : 'json',
                // 5.4) jika permintaan ajax di url berhasil, jika ada data yang dikembalikan, maka akan ditangkap pada variabel data
                success : function(data){
                    // 5.5) menampilkan data achievement yang akan diubah pada modal
                    $('#judul').val(data.judul);
                    $('#caption').val(data.caption);
                    // 6.2) ubah value input type hidden untuk id menjadi id sesuai dengan id achievement
                    $('#id').val(data.id);
                }

            });

        });

    });
    ```
5. di controller Achievement :
    ```php
    public function getDataForUpdate()
    {
        echo json_encode($this->model('AchievementModel')->getAchievementById($_POST['id']));
    }

    public function update()
    {
        if($this->model('AchievementModel')->updateData($_POST) > 0) {
            Flasher::setFlash("successfully", "updated", "success");
            header('Location:' . BASEURL . '/achievement' );
            exit;
        }else{
            Flasher::setFlash("failed", "to update", "danger");
            header('Location:' . BASEURL . '/achievement' );
            exit;
        }
    }
    ```
6. di model AchievementModel :
    ```php
    public function updateData($data)
    {
        $this->db->query("UPDATE `achievements` SET `judul` = :judul, `caption` = :caption WHERE `id` = :id;" );
        $this->db->bind('id', $data['id']);
        $this->db->bind('judul', $data['judul']);
        $this->db->bind('caption', $data['caption']);
        $this->db->execute();       
        
        return $this->db->rowCount();
    }
    ```
# 9. Searching
1. di achievement/index :
    ```html
    <!-- Searching -->
    <div class="row">
    <div class="col-6">  
        <form action="<?= BASEURL?>/achievement/search" method="post">
        <div class="input-group mb-3">
            <input type="text" class="form-control" name="keyword" placeholder="Search Achievement" aria-label="Search Achievement" aria-describedby="basic-addon2">
            <div class="input-group-append">
            <button class="btn btn-primary" type="submit">Search</button>
            </div>
        </div>
        </form>
    </div>
    </div>
    ```
2. di controller achievement :
    ```php
    public function search()
    {
        $data['judul'] = "Achievement";
        $data['achievements'] = $this->model('AchievementModel')->searchData();

        $this->view('template/header', $data);
        $this->view('achievement/index', $data);
        $this->view('template/footer');              
    }
    ```
3. di model AchievementModel :
    ```php
    public function searchData()
    {
        $keyword = $_POST['keyword'];
        $query = "SELECT * FROM `achievements` WHERE `judul` LIKE :keyword";
        $this->db->query($query);
        $this->db->bind('keyword', "%$keyword%");
        return $this->db->all();
    }
    ```





