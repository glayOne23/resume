# Design Pattern

## 1) Singleton
```java
package designpattern.singleton

public class DatabaseHelper {
    
    private static Connection connection;

    public static Connection getConnection () {
        
        // jika class belum diinstansiasi, buat object baru
        if (connection == null) {
            connection = new Connection ("localhost", "root", "");
        }

        // jika class sudah diinstansiasi, pakai object itu
        return connection;  

    }

}
```
```java
package designpattern.singleton

public class OrderDetailService () {

    public void save (Int orderId, String product) {
        DatabaseHelper.getConnection().sql("INSERT INTO ORDER_DETAILS ...");
    }
}
```
---
## 2) Builder
- Adalah salah satu pattern yang memisahkan cara pembuatan object dari class objectnya.
- Sifat :
    1. Build complex objects step by step, it can return different objects based on the given data
    2. Focuses on building complex objects __step by step__ and returns them
    3. Has Fuctionality to decide which objects should be returned
- Sebelum mendalami builder pattern, harus memahami method chaining : biasanya terletak pada setter :
    ```php
    public function setEmail($email)
    {
        $this->email = $email;
        return $this;
    }
    ```
- dengan adanya `$this` pada setter, kita bisa chaining beberapa method sekaligus
    ```php
    $obj->setFoo ('foo')->setBar('bar')->setBaz('baz')->setFarble('farble');
    ```
- Contoh masalah :
    1. di Customer.php
        ```php
        <?php

        class Customer 
        {

            private $id;

            private $firstName;

            private $lastName;

            private $email;

            public function __construct($id, $firstName, $lastName, $email){

                $this->id = $id;
                $this->firstName = $firstName;
                $this->lastName = $lastName;
                $this->email = $email;

            }

            
            public function getId()
            {
                return $this->id;
            }

            
            public function setId($id)
            {
                $this->id = $id;

                return $this;
            }

            
            public function getFirstName()
            {
                return $this->firstName;
            }

            
            public function setFirstName($firstName)
            {
                $this->firstName = $firstName;

                return $this;
            }

            
            public function getLastName()
            {
                return $this->lastName;
            }

            
            public function setLastName($lastName)
            {
                $this->lastName = $lastName;

                return $this;
            }

            
            public function getEmail()
            {
                return $this->email;
            }

            
            public function setEmail($email)
            {
                $this->email = $email;

                return $this;
            }
            
        }
        ```
    2. di App.php
        ```php
        <?php

        require_once("Customer.php");

        $c = new Customer(1, "hammam", "islami", "hammam@test.test");
        ```
    - Kode tersebut akan bermasalah jika, property perlu ditambahkan (misal ditambah nomor telefon) dan dimasukkan pada argument __constructor Costumer, maka kode `new Customer(1, "hammam", "islami", "hammam@test.test");` akan error, untuk itu solusinya bisa membuat class Builder :
- Builder Pattern example :
    1. Customer.php -class objectnya- : 
        ```php
        // isi sama kaya Customer.php diatas
        ```
    2. CustomerBuilder.php -class buildernya- : isi dengan property Customer, setter Customer, dan build() untuk memasukkan data pada __constructor Customer.
        ```php
        <?php

        class CustomerBuilder 
        {

            // 1) properties
            private $id;

            private $firstName;

            private $lastName;

            private $email;

            private $phone;

            // 2) setters
            public function setId($id)
            {
                $this->id = $id;
                return $this;
            }

            public function setFirstName($firstName)
            {
                $this->firstName = $firstName;
                return $this;
            }

            public function setLastName($lastName)
            {
                $this->lastName = $lastName;
                return $this;
            }

            public function setEmail($email)
            {
                $this->email = $email;
                return $this;
            }

            public function setPhone($phone)
            {
                $this->phone = $phone;
                return $this;
            }

            // 3) build
            public function build() : Customer {
                
                return new Customer(
                    $this->id,
                    $this->firstName,
                    $this->lastName,
                    $this->email,
                    $this->phone
                );
            }
        }
        ```
    3. App.php -running-
        ```php
        require_once("Customer.php");
        require_once("CustomerBuilder.php");

        // 1) look at this
        $c = (new CustomerBuilder())
            ->setFirstName("hammam")
            ->setLastName("islami")
            ->setPhone("081126553223")
            ->build();
        ```
    - dengan cara ini, jika ada penambahan argument pada constructor kode lama tidak akan bermasalah 

---
## 3) Factory
- Digunakan ketika kita memiliki standar tertentu dalam instansiasi objek.
- Kasus 1 :
    1. di Employee.php :
        ```php
        <?php

        class Employee {
            
            private $name;

            private $title;

            private $salary;

            public function __construct($name, $title, $salary){
                $this->name = $name;
                $this->title = $title;
                $this->salary = $salary;
            }

            public function getName()
            {
                return $this->name;
            }

            public function getTitle()
            {
                return $this->title;
            }

            public function getSalary()
            {
                return $this->salary;
            }

        }
        ```
    2. di App,php :
        ```php
        <?php

        require_once("Employee.php");

        $manager1 = new Employee("Hammam", "Manager", 50);

        $manager2 = new Employee("Anto", "Manager", 50);

        $staff1 = new Employee("Kewer", "Staff", 10);

        $staff2 = new Employee("Jajang", "Staff", 10);

        ```
    - Pada kasus diatas, ada standar tertentu dimana title yang sama akan selalu memiliki salary yang sama
- Solusi : buat class Factory
    1. di Employee.php tidak berubah
    2. buat EmployeeFactory :
        ```php
        <?php

        class EmployeeFactory 
        {

            // masukkan standar
            static function createManager($name) : Employee {
                return new Employee($name, "Manager", 50);
            }

            static function createStaff($name) : Employee {
                return new Employee($name, "Staff", 10);
            }

        }
        ```
    3. di App.php :
        ```php
        <?php

        require_once("Employee.php");
        require_once("EmployeeFactory.php");

        // tinggal memanggil static method dari class Factory
        $manager1 = EmployeeFactory::createManager("Hammam");

        $manager2 = EmployeeFactory::createManager("Jajang");

        echo $manager1->getName();
        ```
- Kasus 2 :
    1. di Animal.php :
        ```php
        <?php

        interface Animal 
        {
            function speak();
        }


        class Tiger implements Animal 
        {
            function speak()
            {
                echo "Roar!";
            }
        }

        class Cat implements Animal 
        {
            function speak()
            {
                echo "Meow";
            }
        }

        class Dog implements Animal 
        {
            function speak()
            {
                echo "Gog gog";
            }
        }
        ```
    2. di App.php :
        ```php
        require_once("Animal.php");
        require_once("AnimalFactory");

        $tiger = new Tiger();

        $cat = new Cat();

        $tiger->speak();
        ```
    - Sebenarnya nggk masalah, tapi kalo misal kedepannya class implementasi dari Animal ini berubah (misal class Tiger deprecated, dan diubah menjadi class Tiger2), karena semua orang bikin implementasinya langsung, kaya `$tiger = new Tiger();` akan jadi problem kalo implementasinya sudah banyak karena harus ngubah semua yang akses class Tiger.
- Solusi : pake Factory : dengan Factory kita juga bisa tidak mengekspos Class Implementasi Animalnya
    1. di Animal.php sama
    2. buat AnimalFactory :
        ```php
        <?php

        class AnimalFactory 
        {
            public static function create ($type) : Animal 
            {
                if ($type = "tiger"){
                    return new Tiger();
                }

                if ($type = "cat"){
                    return new Cat();
                }

                if ($type = "dog"){
                    return new Dog();
                }
            }
        }
        ```
    3. di App.php :
        ```php
        require_once("Animal.php");
        require_once("AnimalFactory.php");

        $tiger = AnimalFactory->create("tiger");

        echo $tiger::speak();
        ```
    - Dengan begitu, misal class Tiger deprecated, dan diganti class Tiger2, maka cukup diubah di sini aja menjadi Tiger2 :
        ```php
        if ($type = "tiger"){
            return new Tiger2();
        }
        ```

---
## 4) Abstract Factory
- Digunakan ketika kita memiliki standar tertentu dalam instansiasi objek dan cakupannya lebih luas (antar class yang berbeda jenis).
- Contoh Kasus :
    1. Game.php 
        ```php
        <?php

        class Game 
        {

            private $level;

            private $arena;

            // disini Level dan Arena adalah interface
            // Kenapa interface? karena implementasi level dan arena ada banyak, bisa easy, medium, dan hard
            public function __construct(Level $level, Arena $arena)
            {
                $this->level = $level;
                $this->arena = $arena;
            }

            function start()
            {
                $this->level->start();
                $this->arena->start();
            }

        }
        ```
    2. Level.php
        ```php
        <?php

        interface Level 
        {
            function start();
        }


        class LevelEasy implements Level
        {
            function start()
            {
                echo "Level Easy";
            }
        }

        class LevelMedium implements Level
        {
            function start()
            {
                echo "Level Medium";
            }
        }

        class LevelHard implements Level
        {
            function start()
            {
                echo "Level Hard";
            }
        }
        ```
    3. Arena.php
        ```php
        <?php

        interface Arena 
        {
            function start();
        }


        class ArenaEasy implements Arena
        {
            function start()
            {
                echo "Arena Easy";
            }
        }

        class ArenaMedium implements Arena
        {
            function start()
            {
                echo "Arena Medium";
            }
        }

        class ArenaHard implements Arena
        {
            function start()
            {
                echo "Arena Hard";
            }
        }
        ```
    4. App.php
        ```php
        <?php

        require_once("Game.php");
        require_once("Level.php");
        require_once("Arena.php");

        //disini guna interface tadi, jadi class-class implementasi dari Level dan Arena bisa dimasukkan dalam game
        $gameEasy = new Game(new LevelEasy(), new ArenaEasy());
        $gameEasy = new Game(new LevelMedium(), new ArenaMedium());
        $gameEasy->start();
        ```
    - Tapi disini ada standar, yaitu Level yang sama harus dibarengi Arena yang sama,misal Level Easy maka Arena juga harus Easy, gimana caranya? kalo Factory  memberi standar dalam class yang sejenis, maka AbstractFactory memberi standar untuk antar class yang berbeda jenis.
- Solusi : bikin Factory untuk class yang mencakupnya (Game.php) :
    1. GameFactory.php : di GameFactory.php langsung dibuat paketan, level dan arena otomatis memiliki tingkat kesulitan game yang sama
        ```php
        <?php

        interface GameFactory
        {
            function createLevel(): Level;
            function createArena(): Arena;
        }

        class GameFactoryEasy implements GameFactory
        {
            function createLevel(): Level
            {
                return new LevelEasy();
            }

            function createArena(): Arena
            {
                return new ArenaEasy();
            }
        }

        class GameFactoryMedium implements GameFactory
        {
            function createLevel(): Level
            {
                return new LevelMedium();
            }

            function createArena(): Arena
            {
                return new ArenaMedium();
            }
        }

        class GameFactoryHard implements GameFactory
        {
            function createLevel(): Level
            {
                return new LevelHard();
            }

            function createArena(): Arena
            {
                return new ArenaHard();
            }
        }
        ```
    2. Ubah Game.php :
        ```php
        <?php


        class Game 
        {

            private $level;

            private $arena;

            // implementasi GameFactory
            public function __construct(GameFactory $gameFactory)
            {
                $this->level = $gameFactory->createLevel();
                $this->arena = $gameFactory->createArena();
            }

            function start()
            {
                $this->level->start();
                $this->arena->start();
            }

        }
        ```
    3. di App.php
        ```php
        <?php

        require_once("Game.php");
        require_once("GameFactory.php");
        require_once("Level.php");
        require_once("Arena.php");

        // manggil langsung class GameFactory-nya
        $gameEasy = new Game(new GameFactoryHard);
        $gameEasy->start();
        ```
---
## 5) Prototype
- Mirip clone, object yang sudah dibuat di clone
- Contoh kasus :
    1. di Store.php
        ```php
        <?php

        class Store 
        {

            private $name;
            private $city;
            private $country;
            private $category;


            public function __construct ($name, $city, $country, $category)
            {
                $this->name = $name;
                $this->city = $city;
                $this->country = $country;
                $this->category = $category;
            }


            public function getName()
            {
                return $this->name;
            }

            public function setName($name)
            {
                $this->name = $name;
                return $this;
            }

            public function getCity()
            {
                return $this->city;
            }

            public function setCity($city)
            {
                $this->city = $city;
                return $this;
            }

            public function getCountry()
            {
                return $this->country;
            }

            public function setCountry($country)
            {
                $this->country = $country;
                return $this;
            }

            public function getCategory()
            {
                return $this->category;
            }

            public function setCategory($category)
            {
                $this->category = $category;
                return $this;
            }
        }
        ```
    2. di App.php
        ```php
        <?php

        require_once("Store.php");

        $store1 = new Store("TokoA", "Jakarta", "Indonesia", "Gadget");

        $store2 = new Store("TokoB", "Jakarta", "Indonesia", "Gadget");

        $store3 = new Store("TokoC", "Bandung", "Indonesia", "Gadget");

        $store4 = new Store("TokoD", "Bandung", "Indonesia", "Fashion");
        ```
    - Lihat pada App.php, terdapat kesamaan pada argumennya seperti Jakarta, Indonesia, Gadget. Daripada ngetik banyak-banyak lagi, bagaimana kalau di clone dan attribute yang ingin diganti tinggal diganti aja? 
- Solusi : Prototype :
    1. di Store.php buat fungsi clone :
        ```php
        <?php

        class Store 
        {

            private $name;
            private $city;
            private $country;
            private $category;


            public function __construct ($name, $city, $country, $category)
            {
                $this->name = $name;
                $this->city = $city;
                $this->country = $country;
                $this->category = $category;
            }


            public function getName()
            {
                return $this->name;
            }

            public function setName($name)
            {
                $this->name = $name;
                return $this;
            }

            public function getCity()
            {
                return $this->city;
            }

            public function setCity($city)
            {
                $this->city = $city;
                return $this;
            }

            public function getCountry()
            {
                return $this->country;
            }

            public function setCountry($country)
            {
                $this->country = $country;
                return $this;
            }

            public function getCategory()
            {
                return $this->category;
            }

            public function setCategory($category)
            {
                $this->category = $category;
                return $this;
            }

            //1) Jika bahasa pemrograman tidak memiliki fitur object clonning
            public function cloneManual(): Store 
            {
                return new Store(
                    $this->name,
                    $this->city,
                    $this->country,
                    $this->category,
                );
            }

            //2) Jika bahasa pemrograman memiliki fitur object clonning
            public function clone(): Store 
            {
                return clone $this;
            }
        }
        ```
    2. di App.php
        ```php
        <?php

        require_once("Store.php");


        $store1 = new Store("TokoA", "Jakarta", "Indonesia", "Gadget");

        $store2 = $store1->clone();
        $store2->setName("TokoB");

        $store3 = $store1->clone();
        $store3->setName("TokoC");
        $store3->setCity("Bandung");

        $store4 = $store3->clone();
        $store4->setName("TokoD");
        $store4->setCategory("Fashion");
        ```

---
## 6) Object Pool
- Lihat kembali ke bab 1 singleton, disitu kita cuma bisa bikin satu koneksi aja. Prakteknya, penggunaan satu koneksi saja akan membuat koneksi database terasa lamban karena tiap request harus mengantri. Tapi kalo tiap request menggunakan satu koneksi, maka akan boros resource. Solusinya adalah dengan membatasi jumlah koneksi yang bisa digunakan.
- Solusi : Disini akan dibatasi koneksi berjumlah 100, jika koneksi sudah selesai dipakai, maka koneksi harus dikembalikan dalam list.
1. di DatabasePool.java
    ```java
    package designpattern.singleton

    public class DatabasePool {

        private static List<Connection> pool = new ArrayList<>();

        // 1) membuat 100 koneksi dan diletakkan pada list
        static {
            for (int i=0; i<100; i++) {
                Connection connection = new Connection ("localhost", "root", "");
            }
            pool.add(connection);
        }

        // 2) pake koneksinya, setiap dipake, hilang 1 dari list
        public static Connection getConnection(){
            if(pool.isEmpty()){
                throw new RunTimeException("Koneksinya habis");
            }
            else{
                Connection connection = pool.remove(0);
                return connection;
            }
        }

        // 3) jika sudah selesai terpakai, masukkan lagi koneksi dalam list
        public static void close(Connection connection){
            pool.add(connection);
        }
        
    }
    ```
    ```java
    package designpattern.singleton

    public class OrderDetailService () {

        public void save (Int orderId, String product) {
            // memakai koneksi
            Connection connection = DatabasePool.getConnection();
            connection.sql("INSERT INTO ORDER_DETAILS ...");
            // jika sudah harus dikembalikan
            DatabasePool.close();
        }
    }
    ```

---
## 7) Adapter
- Gimana cara menampilkan beberapa jenis produk yang berbeda dalam 1 catalog/ 1 cara
- Adapter : men-generalkan class yang berbeda2 dalam satu adapter
- Kasus : Ketika ketemu class2 yang beda jenis , tapi pengen rubah dalam bentuk standar, dimana untuk ngubahnya itu nggak gampang maka bikin adapter aja
- Step : 
    1. bikin general adapternya dulu `interface`
    2. bikin detail implementasi adapter `class`
- Contoh Kasus : display Catalog : terdapat 2 class Book dan Screencast ingin ditampilkan keduanya dalam bentuk catalog misal : [title] by [author]
    1. di Book.java
        ```java
        package Adapter;

        public class Book {
        
            private String title;
            
            private String author;

            public Book(String title, String author) {
                this.title = title;
                this.author = author;
            }

            public String getTitle(){ return title;}

            public void setTitle(String title){ this.title = title;}

            public String getAuthor(){ return author;}

            public void setAuthor(String author){ this.author = author;}

        }
        ```
    2. di Screencast.java
        ```java
        package Adapter;

        public class Screencast {
        
            private String title;
            
            private String author;

            private Long duration;

            public Screencast(String title, String author, Long duration) {
                this.title = title;
                this.author = author;
                this.duration = duration;
            }

            public String getTitle(){ return title;}

            public void setTitle(String title){ this.title = title;}

            public String getAuthor(){ return author;}

            public void setAuthor(String author){ this.author = author;}

            public Long getDuration(){ return duration;}

            public void setDuration(Long duration){ this.duration = duration;}
            
        }
        ```
    3. di App.java
        ```java
        package Adapter;

        import java.util.ArrayList;
        import java.util.List;

        public class App {

            public static void main(String[] args) {
                List<Object> list = new ArrayList<>();

                list.add(new Book("Pemrograman Java", "Jajang"));
                list.add(new Book("Pemrograman PHP", "Anto"));
                list.add(new Book("Pemrograman Python", "Kewer"));

                list.add(new Screencast("Laravel", "Hilman", 100L));
                list.add(new Screencast("Django", "Andi", 100L));
                list.add(new Screencast("Node JS", "Budi", 100L));
                
                
                // Catalog title by author
                // Kelemahan setiap ada class baru maka harus ada if lagi (harus ngecek manual)
                list.forEach(item ->{
                    if(item instanceof Book){
                        Book book = (Book) item;    
                        System.out.println(book.getTitle() + " by " + book.getAuthor());
                    }
                    else if(item instanceof Screencast){
                        Screencast screencast = (Screencast) item;
                        System.out.println(screencast.getTitle() + " by " + screencast.getAuthor());
                    }
                });

            }
            
        }

        ```
    - Kelemahannya disini adalah setiap ada class baru yang ingin ditampilkan dalam bentuk catalog, harus tambah if lagi gimana solusinya ? Adapter :
- Solusi : Adapter :
    1. bikin general adapter : CatalogAdapter.java :
        ```java
        package Adapter;

        public interface CatalogAdapter {
            
            String getCatalogTitle();

        }
        ```
    2. Implementasi general adapter : BookCatalogAdapter.java :
        ```java
        package Adapter;

        public class BookCatalogAdapter implements CatalogAdapter{
            
            // nyimpen class-nya disini
            private Book book;

            public BookCatalogAdapter (Book book) {
                this.book = book;
            }

            @Override
            public String getCatalogTitle() {
                return book.getTitle() + " by " + book.getAuthor();
            }
        }
        ```
    3. Implementasi general adapter : ScreencastCatalogAdapter.java :
        ```java
        package Adapter;

        public class ScreencastCatalogAdapter implements CatalogAdapter {
            
            // nyimpen class-nya disini
            private Screencast screencast;

            public ScreencastCatalogAdapter (Screencast screencast) {
                this.screencast = screencast;
            }

            @Override
            public String getCatalogTitle() {
                return screencast.getTitle() + " by " + screencast.getAuthor();
            }
        }
        ```
    4. cara manggil : App.java
        ```php
        package Adapter;

        import java.util.ArrayList;
        import java.util.List;

        public class App {

            public static void main(String[] args) {
                List<CatalogAdapter> list = new ArrayList<>();

                list.add(new BookCatalogAdapter(new Book("Pemrograman Java", "Jajang")));
                list.add(new BookCatalogAdapter(new Book("Pemrograman PHP", "Anto")));
                list.add(new BookCatalogAdapter(new Book("Pemrograman Python", "Kewer")));

                list.add(new ScreencastCatalogAdapter(new Screencast("Laravel", "Hilman", 100L)));
                list.add(new ScreencastCatalogAdapter(new Screencast("Django", "Andi", 100L)));
                list.add(new ScreencastCatalogAdapter(new Screencast("Node JS", "Budi", 100L)));
                
                
                // Catalog title by author
                // Kelemahan setiap ada class baru maka harus ada if lagi (harus ngecek manual)
                list.forEach(item ->{            
                    System.out.println(item.getCatalogTitle());
                });

            }
        }
        ```

---
## 7) Repository
- Dulu namanya Data Access Object
- Kasus : misal database ada tabel produk, biasanya di java kita membuat representasi class dari tabel tersebut



