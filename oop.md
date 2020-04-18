# 1,2,3. Class, Object, Property, dan Method
```php
<?php 

// ini class
class Produk {
    // ini property
    public $judul = "judul", 
           $penulis = "penulis",
           $penerbit = "penerbit",
           $harga = 0;

        
        // getter// ini method
    public function getLabel() {
        return "$this->penulis, $this->penerbit";
    }

}

// ini instansiasi class menjadi object
$produk3 = new Produk();
// isi object
$produk3->judul = "Naruto";
$produk3->penulis = "Masashi Kishimoto";
$produk3->penerbit = "Shonen Jump";
$produk3->harga = 30000;

$produk4 = new Produk();
$produk4->judul = "Uncharted";
$produk4->penulis = "Neil Druckmann";
$produk4->penerbit = "Sony Computer";
$produk4->harga = 250000;

echo "Komik : " . $produk3->getLabel();
echo "<br>";
echo "Game : " . $produk4->getLabel();
```

# 4. Constructor
```php
<?php 

class Produk {
    public $judul, 
           $penulis,
           $penerbit,
           $harga;

    //constructor, method pertama yang dijalankan ketika class diinstansiasi
    public function __construct( $judul = "judul", $penulis = "penulis",$penerbit = "penerbit", $harga = 0 ) {
        $this->judul = $judul;
        $this->penulis = $penulis;
        $this->penerbit = $penerbit;
        $this->harga = $harga;
    }

    // getter
    public function getLabel() {
        return "$this->penulis, $this->penerbit";
    }

}

$produk1 = new Produk("Naruto", "Masashi Kishimoto", "Shonen Jump", 30000);
$produk2 = new Produk("Uncharted", "Neil Druckmann", "Sony Computer", 250000);
$produk3 = new Produk("Dragon Ball");

echo "Komik : " . $produk1->getLabel();
echo "<br>";
echo "Game : " . $produk2->getLabel();
echo "<br>";
var_dump($produk3);
```

# 5. Object Type
```php
<?php 

class Produk {
    public $judul, 
           $penulis,
           $penerbit,
           $harga;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0 ) {
        $this->judul = $judul;
        $this->penulis = $penulis;
        $this->penerbit = $penerbit;
        $this->harga = $harga;
    }

    // getter
    public function getLabel() {
        return "$this->penulis, $this->penerbit";
    }

}


class CetakInfoProduk {
    // Produk $produk adalah object type
    // Agar argumen yang bisa diterima harus instance dari class Produk
    public function cetak( Produk $produk ) {
        $str = "{$produk->judul} | {$produk->getLabel()} (Rp. {$produk->harga})";
        return $str;
    }
}



$produk1 = new Produk("Naruto", "Masashi Kishimoto", "Shonen Jump", 30000);
$produk2 = new Produk("Uncharted", "Neil Druckmann", "Sony Computer", 250000);


echo "Komik : " . $produk1->getLabel();
echo "<br>";
echo "Game : " . $produk2->getLabel();
echo "<br>";

$infoProduk1 = new CetakInfoProduk();
echo $infoProduk1->cetak($produk1);
```

# 6,7,8. Inheritance dan Overriding
```php
<?php 

class Produk {
    public $judul, 
           $penulis,
           $penerbit,
           $harga;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0 ) {
        $this->judul = $judul;
        $this->penulis = $penulis;
        $this->penerbit = $penerbit;
        $this->harga = $harga;
    }

    // getter
    public function getLabel() {
        return "$this->penulis, $this->penerbit";
    }

    // getter
    public function getInfoProduk() {
        $str = "{$this->judul} | {$this->getLabel()} (Rp. {$this->harga})";

        return $str;
    }

}


// inheritance
class Komik extends Produk {
    public $jmlHalaman;

    // overriding 
    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $jmlHalaman = 0 ) {

        // manggil method ___construct() milik parent untuk di overriding, liat parent::method() berada pada method yang namanya sama dengan method parentnya
        parent::__construct( $judul, $penulis, $penerbit, $harga );

        $this->jmlHalaman = $jmlHalaman;
    }

        
        // getter// overriding
    public function getInfoProduk() {

        // manggil method ___getInfoProduk() milik parent untuk di overriding, liat parent::method() berada pada method yang namanya sama dengan method parentnya
        $str = "Komik : " . parent::getInfoProduk() . " - {$this->jmlHalaman} Halaman.";
        return $str;
    }
}


// inheritance
class Game extends Produk {
    public $waktuMain;

    // overriding 
    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $waktuMain = 0 ) {
        // manggil method ___construct() milik parent untuk di overriding, liat parent::method() berada pada method yang namanya sama dengan method parentnya
        parent::__construct( $judul, $penulis, $penerbit, $harga );

        $this->waktuMain = $waktuMain;
    }

        
        // getter// overriding
    public function getInfoProduk() {
        // manggil method ___getInfoProduk() milik parent untuk di overriding, liat parent::method() berada pada method yang namanya sama dengan method parentnya
        $str = "Game : " . parent::getInfoProduk() . " ~ {$this->waktuMain} Jam.";
        return $str;
    }
}


class CetakInfoProduk {
    public function cetak( Produk $produk ) {
        $str = "{$produk->judul} | {$produk->getLabel()} (Rp. {$produk->harga})";
        return $str;
    }
}


$produk1 = new Komik("Naruto", "Masashi Kishimoto", "Shonen Jump", 30000, 100);
$produk2 = new Game("Uncharted", "Neil Druckmann", "Sony Computer", 250000, 50);

echo $produk1->getInfoProduk();
echo "<br>";
echo $produk2->getInfoProduk();
```

# 9,10. Visibility / Access Modifier,  Setter, dan Getter
- Mengatur hak akses property dan method
- ada 3 : 
    - public -> akses bebas
    - protected -> hanya dalam sebuah kelas / turunannya
    - private -> hanya dalam kelas tertentu
```php
<?php 

class Produk {
    private $judul, 
           $penulis,
           $penerbit,
           $harga,
           $diskon = 0;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0 ) {
        $this->judul = $judul;
        $this->penulis = $penulis;
        $this->penerbit = $penerbit;
        $this->harga = $harga;
    }

    // setter
    public function setJudul( $judul ) {
        $this->judul = $judul;
    }

    // getter
    public function getJudul() {
        return $this->judul;
    }

    // setter
    public function setPenulis( $penulis ) {
        $this->penulis = $penulis;
    }

    // getter
    public function getPenulis() {
        return $this->penulis;
    }

    // setter
    public function setPenerbit( $penerbit ) {
        $this->penerbit = $penerbit;
    }

    // getter
    public function getPenerbit() {
        return $this->penerbit;
    }

    // setter
    public function setDiskon( $diskon ) {
        $this->diskon = $diskon;
    }

    // getter
    public function getDiskon() {
        return $this->diskon;
    }

    // setter
    public function setHarga( $harga ) {
        $this->harga = $harga;
    }

    // getter
    public function getHarga() {
        return $this->harga - ( $this->harga * $this->diskon / 100 );
    }

    // getter
    public function getLabel() {
        return "$this->penulis, $this->penerbit";
    }

    // getter
    public function getInfoProduk() {
        $str = "{$this->judul} | {$this->getLabel()} (Rp. {$this->harga})";

        return $str;
    }

}


class Komik extends Produk {
    public $jmlHalaman;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $jmlHalaman = 0 ) {

        parent::__construct( $judul, $penulis, $penerbit, $harga );

        $this->jmlHalaman = $jmlHalaman;
    }

    // getter
    public function getInfoProduk() {
        $str = "Komik : " . parent::getInfoProduk() . " - {$this->jmlHalaman} Halaman.";
        return $str;
    }
}


class Game extends Produk {
    public $waktuMain;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $waktuMain = 0 ) {
        parent::__construct( $judul, $penulis, $penerbit, $harga );

        $this->waktuMain = $waktuMain;
    }

    // getter
    public function getInfoProduk() {
        $str = "Game : " . parent::getInfoProduk() . " ~ {$this->waktuMain} Jam.";
        return $str;
    }
}


class CetakInfoProduk {
    public function cetak( Produk $produk ) {
        $str = "{$produk->judul} | {$produk->getLabel()} (Rp. {$produk->harga})";
        return $str;
    }
}


$produk1 = new Komik("Naruto", "Masashi Kishimoto", "Shonen Jump", 30000, 100);
$produk2 = new Game("Uncharted", "Neil Druckmann", "Sony Computer", 250000, 50);

echo $produk1->getInfoProduk();
echo "<br>";
echo $produk2->getInfoProduk();
echo "<hr>";

$produk2->setDiskon(50);
echo $produk2->getHarga();
echo "<hr>";

$produk1->setPenulis("Sandhika Galih");
echo $produk1->getPenulis();
```

# 11. Static
- Guna : 
1. member yang terikat dengan class, bukan object
2. nilai static akan mengikuti perubahan terakhir, ketika dipanggil
3. membuat kode menjadi 'procedural'
4. biasanya digunakan untuk membuat fungsi bantuan / helper
5. atau digunakan di class utility pada framework

```php
<?php 

class ContohStatic {
    // static property 
    public static $angka = 1;

    // static method
    public static function halo() {
        // untuk static property nandainnya pake self , bukan $this
        return "Halo " . self::$angka++ . " kali.";
    }
    // manggil static dari non-static
    public static function halo2() {
        // untuk static property nandainnya pake self , bukan $this
        return "Halo " . self::$angka++ . " kali.";
    }
}

// manggil static property
echo ContohStatic::$angka;
echo "<br>";

// manggil static method
echo ContohStatic::halo();
echo "<hr>";

// nilai static akan mengikuti perubahan terakhir
$obj1 = new ContohStatic();
echo $obj1->halo2();
```

# 12. Constant
- Adalah sebuah untuk menyimpan nilai. Mirip variabel bedanya constant tidak bisa diubah nilainya
- Ada 2 sintax :
    1. define () --> untuk global (tidak bisa disimpan dalam class)
    2. const --> bisa disimpan dalam class
    ```php
    // cara 1
    define('NAMA', 'Sandhika Galih');

    // manggil cara 1 
    echo NAMA;

    // cara 2
    const UMUR = 32;
    //manggil cara 2
    echo UMUR;

    //cara 2 dalam class
    class Coba {
        const NAMA = 'Sandhika';
    }
    // manggil cara 2 dalam kelas
    echo Coba::NAMA;
    ```
- Beberapa magic constant :
    1. `__LINE__` --> isinya keterangan baris berapa constant ini berada
    2. `__FILE__` --> isinya keterangan file mana constant ini berada
    3. `__DIR__` --> isinya keterangan dir mana constant ini berada
    4. `__FUNCTION__` --> isinya keterangan function mana constant ini berada
    5. `__CLASS__` --> isinya keterangan mana mana constant ini berada
    6. `__TRAIT__` --> isinya keterangan trait mana constant ini berada
    7. `__METHOD__` --> isinya keterangan method mana constant ini berada
    8. `__NAMESPACE__` --> isinya keterangan namespace mana constant ini berada

    ```php
    echo __FILE__;


    function coba() {
        return __FUNCTION__;
    }

    echo coba();

    class Coba {
        public $kelas = __CLASS__;
    }

    $obj = new Coba;
    echo $obj->kelas;
    ```

# 13,14. Abstract
- Guna : Untuk membuaah class yang nggk bakal di instansiasi, cuma turunannya aja yang bakal di instansiasi
- Syarat :
    1. Memiliki minimal 1 method abstrak
    2. Digunakan dalam ‘pewarisan’ / inheritance untuk ‘memaksakan’ implementasi method abstrak yang sama untuk semua kelas turunannya
    3. Kelas abstrak boleh memiliki property / method reguler
    4. Kelas abstrak boleh memiliki property / static method
![](img_oop/1.png?raw=true)

    ```php
    <?php 

    // ini class abstaract
    abstract class Produk {
        private $judul, 
            $penulis,
            $penerbit,
            $harga,
            $diskon = 0;

        public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0 ) {
            $this->judul = $judul;
            $this->penulis = $penulis;
            $this->penerbit = $penerbit;
            $this->harga = $harga;
        }

        public function setJudul( $judul ) {
            $this->judul = $judul;
        }

        public function getJudul() {
            return $this->judul;
        }

        public function setPenulis( $penulis ) {
            $this->penulis = $penulis;
        }

        public function getPenulis() {
            return $this->penulis;
        }

        public function setPenerbit( $penerbit ) {
            $this->penerbit = $penerbit;
        }

        public function getPenerbit() {
            return $this->penerbit;
        }

        public function setDiskon( $diskon ) {
            $this->diskon = $diskon;
        }

        public function getDiskon() {
            return $this->diskon;
        }

        public function setHarga( $harga ) {
            $this->harga = $harga;
        }

        public function getHarga() {
            return $this->harga - ( $this->harga * $this->diskon / 100 );
        }

        public function getLabel() {
            return "$this->penulis, $this->penerbit";
        }

        public function getInfo() {
            $str = "{$this->judul} | {$this->getLabel()} (Rp. {$this->harga})";

            return $str;
        }
        
        // method abstract nya
        abstract public function getInfoProduk();


    }


    class Komik extends Produk {
        public $jmlHalaman;

        public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $jmlHalaman = 0 ) {

            parent::__construct( $judul, $penulis, $penerbit, $harga );

            $this->jmlHalaman = $jmlHalaman;
        }

        public function getInfoProduk() {
            $str = "Komik : " . $this->getInfo() . " - {$this->jmlHalaman} Halaman.";
            return $str;
        }
    }


    class Game extends Produk {
        public $waktuMain;

        public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $waktuMain = 0 ) {
            parent::__construct( $judul, $penulis, $penerbit, $harga );

            $this->waktuMain = $waktuMain;
        }

        public function getInfoProduk() {
            $str = "Game : " . $this->getInfo() . " ~ {$this->waktuMain} Jam.";
            return $str;
        }
    }


    class CetakInfoProduk {
        public $daftarProduk = array();

        public function tambahProduk( Produk $produk ) {
            $this->daftarProduk[] = $produk;
        }

        public function cetak() {
            $str = "DAFTAR PRODUK : <br>";

            foreach( $this->daftarProduk as $p ) {
                $str .= "- {$p->getInfoProduk()} <br>";
            }

            return $str;
        }
    }


    $produk1 = new Komik("Naruto", "Masashi Kishimoto", "Shonen Jump", 30000, 100);
    $produk2 = new Game("Uncharted", "Neil Druckmann", "Sony Computer", 250000, 50);

    $cetakProduk = new CetakInfoProduk();
    $cetakProduk->tambahProduk( $produk1 );
    $cetakProduk->tambahProduk( $produk2 );
    echo $cetakProduk->cetak();
    ```

# 15. Interface
- Ciri : 
    1. Kelas Abstrak yang sama sekali tidak memiliki implementasi
    2. Murni merupakan template untuk kelas turunannya
    3. Tidak boleh memiliki property, hanya deklarasi method saja
    4. Semua method harus dideklarasikan dengan visibility public
    5. Boleh mendeklarasikan __construct()
    6. Satu kelas boleh mengimplementasikan banyak interface
![](img_oop/2.png?raw=true)

# 16. Autoloading
- adalah cara praktis untuk memanggil class-class tanpa menggunakan require
- menggunakan `spl_autoload_register();`
- Contoh kasus : kita akan membuat file index.php sebagai file utama, folder App untuk menyimpan app-app yang dipanggil melalui file init.php, dan didalam folder App tedapat folder Produk untuk menyimpan class-class Produk
### 1. di index.php
```php
<?php 

.. manggil file init.php
require_once 'App/init.php';

// instansiasi class-class produk
$produk1 = new Komik("Naruto", "Masashi Kishimoto", "Shonen Jump", 30000, 100);
$produk2 = new Game("Uncharted", "Neil Druckmann", "Sony Computer", 250000, 50);

$cetakProduk = new CetakInfoProduk();
$cetakProduk->tambahProduk( $produk1 );
$cetakProduk->tambahProduk( $produk2 );
echo $cetakProduk->cetak();
``` 
### 2. di App/init.php
```php
<?php 

// daripada manggil class-class yang ada pada folder produk satu-satu, kita bisa menggunakan autoload
// require_once 'Produk/InfoProduk.php';
// require_once 'Produk/Produk.php';
// require_once 'Produk/Komik.php';
// require_once 'Produk/Game.php';
// require_once 'Produk/CetakInfoProduk.php';


// ini autoload
// spl_autoload_register(function($class));
// $class akan mendeteksi nama class-class yang dipanggil
// contoh pada index.php: $produk1 = new Komik("Naruto", "Masashi Kishimoto", "Shonen Jump", 30000, 100);, maka akan memanggil class Komik beserta class-class parent dan class yag ada pada class Komik, disini yang kan dipanggil adalah class Komik, Produk dan InfoProduk
spl_autoload_register(function( $class ){
    require_once __DIR__ . '/Produk/' . $class . '.php';
});
```
### 3.1. di App/Produk.php
```php
<?php 

Abstract class Produk {
    protected $judul, 
        $penulis,
        $penerbit,
        $harga,
        $diskon = 0;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0 ) {
        $this->judul = $judul;
        $this->penulis = $penulis;
        $this->penerbit = $penerbit;
        $this->harga = $harga;
    }

    public function setJudul( $judul ) {
        $this->judul = $judul;
    }

    public function getJudul() {
        return $this->judul;
    }

    public function setPenulis( $penulis ) {
        $this->penulis = $penulis;
    }

    public function getPenulis() {
        return $this->penulis;
    }

    public function setPenerbit( $penerbit ) {
        $this->penerbit = $penerbit;
    }

    public function getPenerbit() {
        return $this->penerbit;
    }

    public function setDiskon( $diskon ) {
        $this->diskon = $diskon;
    }

    public function getDiskon() {
        return $this->diskon;
    }

    public function setHarga( $harga ) {
        $this->harga = $harga;
    }

    public function getHarga() {
        return $this->harga - ( $this->harga * $this->diskon / 100 );
    }

    public function getLabel() {
        return "$this->penulis, $this->penerbit";
    }

    abstract public function getInfo();

}
```
### 3.2. di App/InfoProduk.php
```php
<?php 

interface InfoProduk {
    public function getInfoProduk();
}
```
### 3.3 di App/Komik.php
```php
<?php 

class Komik extends Produk implements InfoProduk {
    public $jmlHalaman;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $jmlHalaman = 0 ) {

        parent::__construct( $judul, $penulis, $penerbit, $harga );

        $this->jmlHalaman = $jmlHalaman;
    }

    public function getInfo() {
        $str = "{$this->judul} | {$this->getLabel()} (Rp. {$this->harga})";

        return $str;
    }

    public function getInfoProduk() {
        $str = "Komik : " . $this->getInfo() . " - {$this->jmlHalaman} Halaman.";
        return $str;
    }
}
```
### 3.4 di App/Game.php
```php
<?php 

class Game extends Produk implements InfoProduk {
    public $waktuMain;

    public function __construct( $judul = "judul", $penulis = "penulis", $penerbit = "penerbit", $harga = 0, $waktuMain = 0 ) {
        parent::__construct( $judul, $penulis, $penerbit, $harga );

        $this->waktuMain = $waktuMain;
    }

    public function getInfo() {
        $str = "{$this->judul} | {$this->getLabel()} (Rp. {$this->harga})";

        return $str;
    }

    public function getInfoProduk() {
        $str = "Game : " . $this->getInfo() . " ~ {$this->waktuMain} Jam.";
        return $str;
    }
}
```
### 3.4 di App/CetakInfoProduk.php
```php
<?php 

class CetakInfoProduk {
    public $daftarProduk = array();

    public function tambahProduk( Produk $produk ) {
        $this->daftarProduk[] = $produk;
    }

    public function cetak() {
        $str = "DAFTAR PRODUK : <br>";

        foreach( $this->daftarProduk as $p ) {
            $str .= "- {$p->getInfoProduk()} <br>";
        }

        return $str;
    }
}
```

# 16. Namespace
- adalah sebuah cara untuk mengelompokkan program ke dalam sebuah package tersendiri
- Studi kasus : di App tambahan folder bernama Service, tambahan file User.php pada folder Service dan folder Produk
### 1.1. di App/Produk/User.php
```php
<?php namespace App\Produk;

class User {
    public function __construct() {
        echo "ini adalah class " . __CLASS__;
    }
}
```
### 1.2. di App/Service/User.php
```php
<?php namespace App\Service;

class User {
    public function __construct() {
        echo "ini adalah class " . __CLASS__;
    }
}
```
### 2. Beri Namespace pada tiap file pada folder Produk dan folder Service
### 2.1. pada file-file Produk
```php
<?php 
// pake backslash '\' bukan '/'
namespace App\Produk;
---
```
### 2.2. pada file-file Produk
```php
<?php 
// pake backslash '\' bukan '/'
namespace App\Service;
---
```
### 3. di App/init/php
```php
<?php 

// untuk manggil file-file pada folder Produk
spl_autoload_register(function( $class ){
    // perlu di explode karena file ketika diberi namespace maka spl_autoload_register() akan mengambil bukan hanya nama classnya tapi beserta namespacenya
    // karena \ dianggap escape character maka \\ nya 2 kali
    $class = explode('\\', $class);
    $class = end($class);
    require_once __DIR__ . '/Produk/' . $class . '.php';
});

// untuk manggil file-file pada folder Produk
spl_autoload_register(function( $class ){
    $class = explode('\\', $class);
    $class = end($class);
    require_once __DIR__ . '/Service/' . $class . '.php';
});
```
### 4. di index.php
```php
<?php 

require_once 'App/init.php';

// cara menggunakan namespace memakai use
// require filenya sudah di autoload pada init.php
// as untuk alias nama
use App\Service\User as ServiceUser;
use App\Produk\User as ProdukUser;

new ServiceUser();
echo "<br>";
new ProdukUser();
```