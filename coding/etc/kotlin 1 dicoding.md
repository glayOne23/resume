```java
fun main(){
    // print tanpa ganti baris
    print("hai hammam")  
    // print ganti baris
    println("Hello word")
}
```
---
# 3 Data types :
- Syntax : `var identifier: Type = initialization`
- var and val :
    - var --> variable mutable
    - val --> varable immutable
- exp : `var isi: String = "Hai"`
## 1. Char :
- ketika kita pengen menggunakan teks, kita bisa make 2; `Char` dan `String`
- `Char` hanya bisa 1 character dan dikutik satu '': var satu_Huruf = `A`
- Bisa diincrement
## 2 String :
- Pada dasarnya string itu array jadi kita bisa lihat karakter ke beberapanya :
    ``` java
    fun main(){
        val test ="Kotlin"
        val firstChar = test[0]

        println($firstChar)
    }
    ```
    - output : `K`
## 3. Array :
- `val mixArray = arrayOf(1, 3, 5, 7 , "Dicoding" , true)`
- jika pengen 1 jenis type data saja : `val intArray = intArrayOf(1, 3, 5, 7)`
- Array juga bisa didefinisikan dengan Array(), yg membutuhkan 2 argumen yaitu size dan fungsi lambda :
    ``` java
    val intArray = Array(4, { i -> i * i }) // [0, 1, 4, 9]
    ```
## 4. Function :
- syntax :
    ```java
    fun functionName(param1: Type1, param2: Type2, ...): ReturnType {
        return result
    }
    ```
- example :
    ```java
    fun setUser(name: String, age: Inte): String{
        return ("Your name is $name and your age is $age")
    }
    ```
- jika cuma 1 expression kita bisa menggunakan expression body dan nggk usah ngasih Return type:
    ``` java
    fun setUser(name: String, age: Int) = "Your name is $name, and you $age years old"
    ```
- Jika nggk pengen `return` kita bisa menggunakan ReturnType `Unit` :
    ``` java
    fun printUser(name: String): Unit {
        print("Your name is $name")
    }
    ```
    - `Unit` nya nggk ditulis jga bisa
## 5. If
``` java
val openHours = 7
val now = 7
val office: String
office = if (now > 7) {
    "Office already open"
} else if (now == openHours){
    "Wait a minute, office will be open"
} else {
    "Office is closed"
}
 
print(office)
```
- tidak ada ternary operator di kotlin, karena sdh digantikan if statement
## 6. Boolean
## 7. Number
- Int (32 Bit), Long (64 Bit), Short (16 Bit), Byte (8 Bit), Double (64 Bit) (mirip float)
- untuk mengetahui nilai max atau minimal menggunakan property MAX_VALUE dan MIN_VALUE:
    ``` java
    fun main() {
        val maxInt = Int.MAX_VALUE
        val minInt = Int.MIN_VALUE
    
        println(maxInt)
        println(minInt)
    
        /*
        output :
                2147483647
                -2147483648
        */
    }
    ```
- Jika kita memasukan nilai melebihi nilai maksimal yang dapat disimpan, maka akan terjadi overflow. Nilai yang akan dikembalikan adalah nilai minimal yang dapat disimpan.
- untuk konversi kita nggk bisa lngnsg koversi, tapi pake toInt() / toLong(), dst
    ``` java
    fun main() {
        val stringNumber = "23"
        val intNumber = 3
    
        print(intNumber + stringNumber.toInt())
        /*
        output: 26
        */
    }
    ```
## 8. Nullable Type
1. Mengatasi null, kotlin mampu membedakan mana yg boleh null mana yang nggk :
    - `val text: String = null // compile time error`
2. biar boleh tambahin `?`:
    - `val text: String? = null // ready to go`
    - tapi jika gini, pas dicari length akkan error: 
        ``` java
        val text: String? = null
        val textLength = text.length // compile time error
        ```
3. Terus ngatasiinya ?pake if :
    ```java
    val text: String? = null    
    if (text != null){
        val textLength = text.length // ready to go
    }
    ```
## 9. Save Calls and Elvis Operator
1. save calls operator (?.)
    - dengan menulis ini di setelah (.) , kompiler akan melewatkan proses jika objek tersebut bernilai null :
        ```java
        val text: String? = null
        text?.length
        ```
2. elvis operator(?:)
    - memungkinkan kita untuk menetapkan default value atau nilai dasar jika objek bernilai null:
        ``` java
        val text: String? = null
        val textLength = text?.length ?: 0
        ```
## 10. String Template
- Sebuah fitur yang memungkinkan kita untuk menyisipkan sebuah variabel ke dalam sebuah String tanpa concatenation (penggabungan objek String menggunakan +)
- Untuk menggunakan string template, kita hanya perlu menambahkan karakter $ sebelum nama variabel yang akan disisipkan 
- exp :
    ```java
    val name = "Kotlin"
    print("My name is $name")
    ```
- Kita juga bisa menyisipkan sebuah expression ke dalam sebuah string template. Caranya, sisipkan expression ke dalam curly braces yang diikuti karakter $ :
    ``` java
    fun main() {
        val hour = 7
        print("Office ${if (hour > 7) "already close" else "is open"}")
    }
    ```
---
# 4 Control Flow
- adalah cara kita mengontrol alur dari sebuah program berdasarkan kondisi saat program tersebut berjalan.
- Control Flow yang akan dipelajari :
    - Enumeration
    - When Expression
    - Expression & Statement
    - While and Do While
    - Range and For Loop
    - Break and Continue Labels
## 1. Enumeration
- skip sek
## 2. When
- when akan mencocokan semua argumen yang berada di setiap branch secara berurutan sampai salah satu kondisi terpenuhi
    ``` java
    fun main() {
        val value = 20
    
        when(value){
            6 -> println("value is 6")
            7 -> println("value is 7")
            8 -> println("value is 8")
            else -> println("value cannot be reached")
        }
    }
    
    /*
    output: value cannot be reached
    */
    ```
- ika kita memiliki dua atau lebih baris kode yang akan kita jalankan di setiap branch, kita bisa memindahkannya ke dalam curly braces seperti berikut:
    ```java
    fun main() {
        val value = 7
        val stringOfValue = when (value) {
            6 -> {
                println("Six")
                "value is 6"
            }
            7 -> {
                println("Seven")
                "value is 7"
            }
            8 -> {
                println("Eight")
                "value is 8"
            }
            else -> {
                println("undefined")
                "value cannot be reached"
            }
        }
    
        println(stringOfValue)
    }
    
    /*
    output : 
        seven
        value is 7
    */
    ```
- when expression juga bisa kita gunakan untuk memeriksa nilai yang terdapat pada sebuah Range atau Collection :
    ```java
    fun main() {
        val value =  27
        val ranges = 10..50
    
        when(value){
            in ranges -> println("value is in the range")
            !in ranges -> println("value is outside the range")
            else -> println("value undefined")
        }
    }
    
    /*
    output : value is in the range
    */
    ```
- when juga memungkinkan kita untuk memeriksa instance dengan tipe tertentu dari sebuah objek menggunakan is atau !is :
    ```java
    fun main() {
        val anyType : Any = 100L
        when(anyType){
            is Long -> println("the value has a Long type")
            is String -> println("the value has a String type")
            else -> println("undefined")
        }
    }
    
    /*
    output : the value has a Long type
    */
    ```
## 3. while, do..while 
```java
fun main() {
    var counter = 1
    while (counter <= 7){
        println("Hello, World!")
        counter++
    }
}
```
```java
fun main() {
    var counter = 1
    do {
        println("Hello, World!")
        counter++
    } while (counter <= 7)
}
````
## 4. for loop
- for dapat digunakan pada Ranges, Collections, Arrays dan apapun yang menyediakan iterator.
- standar for in range:
    ```java
    fun main() {
        val ranges = 1..5
        for (i in ranges){
            println("value is $i!")
        }
    }
    ```
- standar 2 for in range:
    ```java
    fun main() {
        val ranges = 1.rangeTo(5)
        for (i in ranges){
            println("value is $i!")
        }
    }
    ```
- for in range dengan step buat lompat2 :
    ```java
    fun main() {
        val ranges = 1.rangeTo(10) step 3
        for (i in ranges ){
            println("value is $i!")
        }
    }
    
    /*
    output :
        value is 1!
        value is 4!
        value is 7!
        value is 10!
    */
    ```
- for in range dengan step buat lompat2 + index :
    ```java
    fun main() {
        val ranges = 1.rangeTo(10) step 3
        for ((index, value) in ranges.withIndex()) {
            println("value $value with index $index")
        }
    }
    /*
    output :
        value 1 with index 0
        value 4 with index 1
        value 7 with index 2
        value 10 with index 3
    */
    ```
- foreach :
    ```java
    fun main() {
        val ranges = 1.rangeTo(10) step 3
        ranges.forEach { value ->
            println("value is $value!")
        }
    }
    ```
- foreach + index :
    ```java
    fun main() {
    
        val ranges = 1.rangeTo(10) step 3
        ranges.forEachIndexed { index, value ->
            println("value $value with index $index")
        }
    }
    ```
- jika cma indexnya aja tanpa value, ganti argument ke 2 dengan `_` :
    ```java
    fun main() {
        val ranges = 1.rangeTo(10) step 3
        ranges.forEachIndexed { index, _ ->
            println("index $index")
        }
    }
    ```
## 5. range :
- Kita dapat menentukan nilai awal dan nilai akhir pada Range. Range direpresentasikan dengan operator `..` atau dengan fungsi `rangeTo()` dan `downTo()` :
- standar : 
    ```java
    val rangeInt = 1..10
    ```
- nyari tau step pake `.step` :
    ```java
    fun main() {
        val rangeInt = 1..10
        print(rangeInt.step)
    }
    ```
- step :
    ```java
    fun main() {
        val rangeInt = 1..10 step 2
        rangeInt.forEach {
            print("$it ")
        }
    }
    ```
- `rangeTo()` :
    ```java
    val rangeInt = 1.rangeTo(10)
    ```
- `downTo()` :
    ```java
    val downInt = 10.downTo(1)
    ```
- Range pada char :
    ```java
    val rangeChar = 'A'.rangeTo('F')
    ```
## 6. break dan continue
- jika ada `null` :
    ```java
    fun main() {
        val listOfInt = listOf(1, 2, 3, null, 5, null, 7)
    
        for (i in listOfInt) {
            if (i == null) continue
            print(i)
        }
    }
    /*
    output: 12357
    */
    ```
- Break dan Continue Label
    - Pada Kotlin, sebuah expression dapat ditandai dengan sebuah label. Label pada Kotlin memiliki sebuah identifier yang diikuti dengan tanda `@`. Contoh dari sebuah label adalah `foo@ ` atau `bar@`.
        ```java
        fun main() {
            loop@ for (i in 1..10) {
                println("Outside Loop")
        
                for (j in 1..10) {
                    println("Inside Loop")
                    if ( j > 2) break@loop
                }
            }
        }
        
        /*
        output :
            Outside Loop
            Inside Loop
            Inside Loop
            Inside Loop
        */
        ```
    - Pada kode diatas, break yang sudah ditandai dengan label akan dilompati ke titik awal proses perulangan yang sudah ditandai dengan label. break akan menghentikan proses perulangan terluar dari dalam proses perulangan, di mana break tersebut dipanggil.
---
# 5 Data Classes
- Kotlin mengenalkan konsep data class yang merupakan sebuah kelas sederhana yang bisa berperan sebagai data container. Data class adalah sebuah kelas yang tidak memiliki logika apapun dan juga tidak memiliki fungsionalitas lain selain menangani data.
- Kenapa disebut dengan kelas sederhana? Data class mampu menyediakan beberapa fungsionalitas yang biasanya kita butuhkan untuk mengelola data hanya dengan sebuah keyword `data`.
    ```kotlin
    data class User(val name : String, val age : Int)
    ```
- Hanya dengan satu baris kode di atas, kompiler akan secara otomatis menghasilkan `constructor`, `toString()`, `equals()`, `hashCode()`, `copy()` dan juga fungsi `componentN()`.
- Beberapa hal yang perlu diperhatikan dalam membuat sebuah data class adalah:
    - Konstruktor utama pada kelas tersebut harus memiliki setidaknya satu parameter;
    - Semua konstruktor utama perlu dideklarasikan sebagai val atau var;
    - Modifier dari sebuah data class tidak bisa abstract, open, sealed, atau inner.
## Perbedaan Data Class dengan Class biasa
- `toString()` :
    - di class biasa :
        ```java
        class User(val name : String, val age : Int){
        
            override fun toString(): String {
                return "User(name=$name, age=$age)"
            }
        }
        ```
    - di data class :
        ```kotlin
        data class DataUser(val name : String, val age : Int)
        ```
    - jika di run di fun main():
        ```java
        val user = User("nrohmen", 17)
        val dataUser = DataUser("nrohmen", 17)
        
        println(user)
        println(dataUser)
        ```
    - Hasil :
        ```java
        User(name=nrohmen, age=17)
        DataUser(name=nrohmen, age=17)
        ```
- `equals()` :
    - jika di run :
        ```java
        fun main(){
            val dataUser = DataUser("nrohmen", 17)
            val dataUser2 = DataUser("nrohmen", 17)
            val dataUser3 = DataUser("dimas", 24)
        
            println(dataUser.equals(dataUser2))
            println(dataUser.equals(dataUser3))

            val user = User("nrohmen", 17)
            val user2 = User("nrohmen", 17)
            val user3 = User("dimas", 24)
        
            println(user.equals(user2))
            println(user.equals(user3))
        
        }
        ```
    - di class boilerplatenya:
        ```java
        class User(val name : String, val age : Int){
        
            override fun equals(other: Any?): Boolean {
                if (this === other) return true
                if (javaClass != other?.javaClass) return false
        
                other as User
        
                if (name != other.name) return false
                if (age != other.age) return false
        
                return true
            }
            
        }
        ```
    - di data class nggk usah panjang2 cukup kaya yg contoh 1 aja
## Menyalin dan Memodifikasi Data Class
- `copy()` :
    ```java
    fun main(){
        val dataUser = DataUser("nrohmen", 17)
        // mengcopy dataUser
        val dataUser4 = dataUser.copy()
        // mengcopy dataUser dan mengganti value dari object
        val dataUser5 = dataUser.copy(age = 18)
    }
    ```
## Destructuring Declarations
- adalah proses memetakan objek menjadi sebuah variabel
- Dengan fungsi `componentN()` yang ada pada data class, kita bisa menguraikan sebuah objek menjadi beberapa properti yang dimilikinya. Sebagai contoh, kita ingin menguraikan objek dataUser:
    ```java
    fun main(){
        val dataUser = DataUser("nrohmen", 17)
    
        val name = dataUser.component1()
        val age = dataUser.component2()
    
        println("My name is $name, I am $age years old")
    }
    ```
- cara lain (langsung):
    ```java
    fun main(){
        val dataUser = DataUser("nrohmen", 17)
        val (name, age) = dataUser
    
        println("My name is $name, I am $age years old")
    }
    ```
- data class bertujuan untuk mengurangi jumlah kode boilerplate yang Anda tuliskan
- kita juga bisa membuat fungsi di dalam data class:
    ```java
    data class DataUser(val name : String, val age : Int){
        fun intro(){
            println("My name is $name, I am $age years old")
        }
    }

    fun main(){
        val dataUser = DataUser("nrohmen", 23)
        dataUser.intro()
    }
    ```
---
# 5.1 Collection
- Collections merupakan sebuah objek yang bisa menyimpan kumpulan objek lain termasuk data class.
- Besifat `immutable`
- Di dalam collections terdapat beberapa objek turunan, di antaranya adalah List, Set, dan Map.
## 1, List
- List bisa berisi bermacam - macam tipe data seperti Int, String, Boolean atau yang lainnya.
- syntax :
    ```java
    // type data dideklarasikan
    val numberList : List<Int> = listOf(1, 2, 3, 4, 5)
    // type data tidak dideklarasikan
    val numberList = listOf(1, 2, 3, 4, 5)
    val anyList = listOf('a', "Kotlin", 3, true)
    ```
- arena setiap objek pada Kotlin merupakan turunan dari kelas `Any`, maka variabel anyList tersebut akan memiliki tipe data `List<Any>`. 
- Untuk membuat List mutable, gunakan fungsi `mutableListOf`:
    ```java
    val anyList = mutableListOf('a', "Kotlin", 3, true, User())

    // maka data bisa ditambah, dihapus, atau diganti
    anyList.add('d') // menambah item di akhir list
    anyList.add(1, 'love') // menambah item pada indeks ke-1
    anyList[3] = false // mengubah nilai item pada indeks ke-3
    anyList.remove(User()) // menghapus item User()
    ```
## 2. Set
- Set merupakan sebuah collection yang hanya dapat menyimpan nilai yang unik.
    ```java
    val integerSet = setOf(1, 2, 4, 2, 1, 5)

    fun main(){
        println(integerSet)
    
        // Output: [1, 2, 4, 5]
        //Secara otomatis fungsi setOf akan membuang angka yang sama.
    }
    ```
    ```java
    val setA = setOf(1, 2, 4, 2, 1, 5)
    val setB = setOf(1, 2, 4, 5)
    println(setA == setB)
    
    // Output: true
    ```
- fungsi `union()` dan `intersect()` pada set :
    ```java
    val setA = setOf(1, 2, 4, 2, 1, 5)
    val setC = setOf(1, 5, 7)
    val union = setA.union(setC)
    val intersect = setA.intersect(setC)
    
    println(union)
    println(intersect)
    
    // union: [1, 2, 4, 5, 7]
    // intersect: [1, 5]
    ```
- Pada Set kita bisa menambah dan menghapus item namun tak bisa mengubah nilai seperti pada List.
## 3. Map
-  Map, sebuah collection yang dapat menyimpan data dengan format key-value.
    ```java
    val capital = mapOf(
        "Jakarta" to "Indonesia",
        "London" to "England",
        "New Delhi" to "India"
    )
    ```
- Akses value : dengan manggil keynya :
    ```java
    // cara 1
    println(capital["Jakarta"])
    // Output: Indonesia

    // cara 2
    println(capital.getValue("Jakarta")) 
    // Output: Indonesia
    ```
    - Perbedaan cara 1 dan cara 2:
        - [ ] -> konsol akan menampilkan hasil null saat key yang dicari tidak ada di dalam Map
        - getValue() -> konsol akan memberikan sebuah Exception.
- menampilkan semua key : `keys()` :
    ```java
    val mapKeys = capital.keys
    // mapKeys: [Jakarta, London, New Delhi]
    ```
- menampilkan semua value : values() :
    ```java
    val mapValues = capital.values
    // mapValues: [Indonesia, England, India]
    ```
- Untuk menambahkan key-value ke dalam map, kita perlu memastikan bahwa map yang digunakan adalah mutable. Mari kita ubah map capital yang sudah kita buat sebelumnya menjadi mutable :
    ```java
    val mutableCapital = capital.toMutableMap()

    // sekarang bisa ditambah
    fun main(){
        mutableCapital.put("Amsterdam", "Netherlands")
        mutableCapital.put("Berlin", "Germany")
    }
    ```
- Namun perlu diperhatikan bahwa menggunakan mutable collection itu tidak disarankan. Apabila terdapat sebuah mutable collection yang diubah oleh lebih dari 1 proses, hasil nya akan sulit untuk diprediksi.
## 4. Collections Operations
- beberapa fungsi operasi yang bisa kita gunakan untuk mengakses data di dalamnya.
### 1. filter() dan filterNot()
- menghasilkan list baru dari seleksi berdasarkan kondisi yang kita berikan
    ```java
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    val evenList = numberList.filter { it % 2 == 0 }
    
    // evenList: [2, 4, 6, 8, 10]
    ```
### 2. map()
- membuat collection baru sesuai perubahan yang akan kita lakukan dari collection sebelumnya
    ```java
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    val multipliedBy5 = numberList.map { it * 5 }
    
    // multipliedBy5: [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]
    ```
### 3. count()
- menghitung jumlah item yang ada di dalam collection
    ```java
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    print(numberList.count())
    
    // Output: 10
    ```
- count() dengan syarat :
    ```java
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    print(numberList.count { it % 3 == 0 })
    
    // Output: 3
    ```
### 4. find(), firstOrNull(), lastOrNull()
- Fungsi find() ini memiliki cara kerja yang sama dengan fungsi firstOrNull(). Artinya, jika di dalam collection tidak ditemukan data yang sesuai, maka fungsi akan mengembalikan nilai null. Tidak seperti fungsi filter() atau map() yang akan melakukan iterasi terhadap seluruh item, fungsi find() dan firstOrNull() ini akan langsung mengembalikan nilai ketika kondisi terpenuhi. Kemudian jika Anda ingin mencari item terakhir, gunakan fungsi lastOrNull().
    ```java
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    val firstOddNumber = numberList.find { it % 2 == 1 }
    
    // firstOddNumber: 1
    ```
### 5. first() dan last()
- mirip dgn lastOrNull() dan firstOrNull(), tapi bedanya jika syarat tidak terpenuhi maka akan menghasilkan exception
    ```java
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    val moreThan10 = numberList.first { it > 10 }
    print(moreThan10)
    
    // Output: Exception in thread "main" java.util.NoSuchElementException: Collection contains no element matching the predicate.

    ```
### 6. sum()
```java
val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
val total = numberList.sum()

// total: 55
```
### 7. sorted()
- mengurutkan item yang ada di dalam collection. Secara default fungsi sorted() ini akan mengurutkan data secara ascending.
    ```java
    val kotlinChar = listOf('k', 'o', 't', 'l', 'i', 'n')
    val ascendingSort = kotlinChar.sorted()
    println(ascendingSort)
    
    // ascendingSort: [i, k, l, n, o, t]
    ```
- agar descending pake sortedDescending()
    ```java
    val kotlinChar = listOf('k', 'o', 't', 'l', 'i', 'n')
    val descendingSort = kotlinChar.sortedDescending()
    println(descendingSort)
    
    // descendingSort: [t, o, n, l, k, i]

    ```
---
# 5.2. Sequence
- Tiga (3) jenis collection yang sudah kita pelajari sebelumnya merupakan jenis collection yang menjalankan eager evaluation.
- Eager evolution --> eksekusi coding per block
- Lazy evolution --> eksekusi coding per item
- Untuk menerapkan lazy  atau vertical evaluation maka kita perlu mengubah list menjadi Sequence. Caranya sangat sederhana, yaitu dengan memanggil fungsi asSequence().
- exp :
    ```java
    fun main() {
        // eager evolution
        val list1 = (1..1000000).toList()
        list1.filter { it % 5 == 0 }.map { it * 2 }.forEach { println(it) }

        // lazy evolution
        val list2 = (1..1000000).toList()
        list2.asSequence().filter { it % 5 == 0 }.map { it + it }.forEach { println(it) }
    }
    ```
    - Pada coding diatas, eager evolution akan mengeksekusi per block (fungsi), mulai dari filter() 1-1000000 ,jika selesai dilanjutkan map(), lalu baru di print
    - pada lazy evolution akan engeksekusi per item, mulai dari 1-1000000, misal angka 1, apakah dia lolos filter() jika iya lanjut mapping sampai di print.
- generateSequence() :
    ```java
    fun main() {
        val sequenceNumber = generateSequence(1) { it + 1 }
        sequenceNumber.take(5).forEach { print("$it ") }
    }
    // Output: 1 2 3 4 5
    ```
- generateSequence() memiliki 2 parameter, param1 item pertama dalam collection, param2 itu iterasinya, dia bakal looping trs, untuk batasnya pake fungsi take()
---
# 6. Functional Programming
## 1. Anatomy of Function
- read module
## 2. Named and Default Argument
- named argument :
    ```java
    fun main() {
        val fullName = getFullName(middle = " is " , first = "Kotlin", last = "Awesome")
        print(fullName)
    }
    
    fun getFullName(first: String, middle: String, last: String): String {
        return "$first $middle $last"
    }
    ```
- default argument :
    ```java
    fun main() {
        val fullName = getFullName(first = "Dicoding")
        print(fullName)
    }
    
    fun getFullName(first: String = "Kotlin", middle: String = " is ", last: String = "Awesome"): String {
        return "$first $middle $last"
    }
    
    /*
        output : Dicoding is Awesome
    */
    ```
## 3. Vararg Argument
- Dengan  vararg, sebuah fungsi dapat memiliki jumlah parameter berdasarkan jumlah argumen yang kita masukan ketika fungsi tersebut dipanggil. 
    ```java
    fun main() {
        val number = sumNumbers(10, 20, 30, 40)
        print(number)
    }
    
    fun sumNumbers(vararg number: Int): Int {
        return number.sum()
    }
    
    /*
    output : 100
    */
    ```
- Ketika sebuah parameter ditentukan dengan vararg, pada dasarnya semua argumen yang dilampirkan, ditampung di  dalam sebuah Array `<out T>`.
- Oleh karena itu, kita juga bisa menerapkan Generic untuk tipe parameter ketika parameter tersebut ditentukan dengan vararg.
    ```java
    fun <T> asList(vararg input: T): List<T> {
        val result = ArrayList<T>()
        for (item in input)
            result.add(item)
        return result
    }
    ```
- Karena pada dasarnya adalah sebuah Array, maka kita bisa memanggil fungsi atau properti yang tersedia pada kelas Array dari dalam fungsi tersebut :
    ```java
    fun main() {
        val number = sumNumbers(10, 20, 30, 40, 50)
        print(number)
    }
    
    fun getNumberSize(vararg number: Int): Int {
        return number.size
    }
    
    /*
    output : 5
    */
    ```
### Aturan pada varag :
1. tidak diizinkan untuk memiliki 2 (dua) parameter bertanda vararg.
    ```java
    fun sumNumbers(vararg number: Int, vararg number2: Int)

    // Kotlin: Multiple vararg-parameters are prohibited
    ```
2. jika kita ingin menambahkan parameter baru tanpa kata kunci vararg, parameter yang ditandai dengan vararg harus berada pada posisi pertama :
- Lalu bagaimana cara melampirkan argumen untuk parameter name di atas? Kita bisa memanfaatkan named argument untuk menandai,  
    ```java
    fun main() {
        sets(10, 10, name = "Kotlin")
    }
    
    fun sets(vararg number: Int, name: String): Int {
        ...
    }
    ```
### Perbedaan Vararg vs Array<T>
- Array<T> : Ketika fungsi di atas dipanggil, fungsi tersebut membutuhkan argumen berupa nilai yang sudah berbentuk Array
```java
fun main() {
    val number = arrayOf(10, 20, 30, 40)
    jumlah(number)
}
 
fun jumlah(number: Array<Int>) {
    return number.sum()
}
```
- Varag : Kita bisa memasukkan argumen satu persatu.contoh kaya disebelum2nya
- Lalu apakah bisa kita memasukkan nilai yang sudah berbentuk Array sebagai argumen untuk parameter yang ditandai dengan vararg? Tentu bisa! Dengan memanfaatkan `spread operator (*)` seperti berikut:
```kotlin
fun main() {
    val number = intArrayOf(10, 20, 30, 40)
    sets(10, 20, 20, *number , 10)
}
 
fun sets(vararg number: Int): Int {
    ...
}
```
## 4. Extensions
- Kotlin memungkinkan kita untuk menambahkan sebuah fungsi baru pada sebuah kelas tanpa harus mewarisi kelas tersebut. Misal kita ingin menambahkan fungsi baru untuk kelas Int, maka kita akan menuliskannya seperti berikut:
    ```java
    class NewInt : Int(){
        fun printInt(){
            println("value $this")
        }
    }
    ```
- Ketika dijalankan, kode di atas akan gagal dikompilasi, kenapa? Karena kelas Int bersifat final, sehingga tidak memungkinkan untuk mewarisi kelas tersebut. Untuk itu, kita bisa melakukannya dengan deklarasi khusus yang disebut dengan Extensions. 
- Kotlin mendukung 2 (dua) extension yang dapat digunakan, yaitu Extension Functions dan Extension Properties. Jika extension functions digunakan untuk menambahkan fungsi baru, extension properties tentunya digunakan untuk menambahkan sebuah properti baru.
### 1. Extension Functions
- Sintax : `receiver type.nama fungsi` :
    ```java
    fun Int.printInt() {
        print("value $this")
    }
    ```
- Kelas Int pada kode di atas digunakan sebagai receiver type, sedangkan kata kunci this adalah receiver type yang bertindak sebagai objeknya. Nilai dari objek tersebut bisa digunakan di dalam extension yang sudah dibuat. 
- Untuk memanggil extension functions :
    ```java
    fun main() {
        10.printInt()
    }
    
    fun Int.printInt() {
        print("value $this")
    }
    
    /*
    output : value 10
    */
    ```
- Kita juga bisa menetapkan jika extension functions tersebut dapat mengembalikan nilai :
    ```java
    fun main() {
        println(10.plusThree())
    }
    
    fun Int.plusThree(): Int {
        return this + 3
    }
    
    /*
    output : 13
    */
    ```
### 2. Extension Properties
- syntax :
    ```java
    val Int.slice: Int
        get() = this / 2
    ```
- Eksekusi :
    ```java
    fun main() {
        println(10.slice)
    }
    
    val Int.slice: Int
        get() = this / 2
    
    /*
    output : 5
    */
    ```
- extension tidak benar-benar mengubah sebuah kelas dengan menambahkan sebuah fungsi atau properti baru. Ini karena extension memiliki hubungan langsung dengan kelas yang ingin diperluas fungsionalitasnya. Sehingga extension properties hanya bisa dideklarasikan dengan cara menyediakan getter atau setter secara eksplisit.
## 5. Nullable Receiver
- Kita bisa juga mendeklarasikan sebuah extension dengan nullable receiver type. Alhasil, extension tersebut bisa dipanggil pada objek yang bahkan nilainya null.
    ```java
    val Int?.slice: Int
        get() = if (this == null) 0 else this / 2

    ```
- Selain menggunakan if expression, kita juga bisa menggunakan elvis operator:
    ```java
    val Int?.slice: Int
        get() = this ?: 0 / 2
    ```
- Eksekusi :
    ```java
    fun main() {
        val value: Int? = null
    
        println(value.slice)
    }
    
    val Int?.slice: Int
        get() = this ?: 0 / 2
    
    /*
    output : 0
    */
    ```
- Lalu kapan kita membutuhkannya? Tentunya jika kita mempunyai sebuah objek yang bernilai null. Saat kita tidak menetapkannya dengan nullable receiver type, maka kita perlu memeriksa apakah objek tersebut bernilai null atau tidak? Bisa juga dengan menggunakan operator safe call setiap kali extension tersebut dipanggil :
    ```java
    fun main() {
        val value: Int? = null
        val value1: Int? = null
    
        println(value?.slice)
        println(value1?.slice)
    }
    
    val Int.slice: Int
        get() = this / 2
    
    /*
    output : null
                null
    
    */
    ```
- bandingkan jika menggunakan nullable receiver :
    ```java
    fun main() {
        val value: Int? = null
        val value1: Int? = null
    
        println(value.slice)
        println(value1.slice)
    }
    
    val Int?.slice: Int
        get() = this ?: 0 / 2
    
    /*
    output : 0
                0
    
    */
    ```
## 6.  Lambda
- Lambda expression, biasa disebut dengan anonymous function. Bisa disebut sebagai anonymous karena lambda tidak memiliki sebuah nama seperti halnya sebuah fungsi pada umumnya
- lambda juga dapat memiliki daftar parameter, body dan return type. 
- Karakteristik lamda :
    -  kita tidak perlu mendeklarasi tipe spesifik untuk nilai kembaliannya. Tipe tersebut akan ditentukan oleh kompiler secara otomatis.
    - lambda tidak membutuhkan kata kunci fun dan visibility modifier saat dideklarasikan, karena lambda bersifat anonymous.
    - Parameter yang akan ditetapkan berada di dalam kurung kurawal {}.
    - kata kunci return tidak diperlukan lagi karena kompiler akan secara otomatis mengembalikan nilai dari dalam body.
    - Lambda expression dapat digunakan sebagai argumen untuk sebuah parameter dan dapat disimpan ke dalam sebuah variabel.
- exp :
    ```java
    fun main() {
        message()
    }
    
    val message = { println("Hello From Lambda") }
    
    /*
    output : Hello From Lambda
    */
    ```
- Jika kita ingin menambahkan sebuah parameter pada fungsi lambda :
    ```java
    fun main() {
        printMessage("Hello From Lambda")
    }
    
    val printMessage = { message: String -> println(message) }
    
    /*
    output : Hello From Lambda
    */
    ```
- Trs cara agar bisa mengembalikan nilai (return type) :
    ```java
    fun main() {
        val length = messageLength("Hello From lambda")
        println("Message length $length")
    }
    
    val messageLength = { message: String -> message.length }
    
    /*
    output : Message length 17
    */
    ```
- Bisa kita perhatikan, kita tidak membutuhkan tipe kembalian dan kata kunci return untuk mengembalikan sebuah nilai. Pada dasarnya, kompiler akan mengembalikan nilai berdasarkan expression atau statement di baris terakhir di dalam body.

## 7. High-Order Function
- adalah sebuah fungsi yang menggunakan fungsi lainnya sebagai parameter, menjadikan return type, ataupun keduanya. 
- Ketika kita mendeklarasikan sebuah higher-order function, maka kita perlu menentukan tipe deklarasi dari fungsi yang menjadi parameter.(penjelasan lanjut akan dibahas di function type)
    ```java
    fun main() {
        printResult(10 ,sum)
    }
    
    //look at this, sum() jadi parameter di fungsi printResult()
    fun printResult(value: Int, sum: (Int) -> Int) {
        val result = sum(value)
        println(result)
    }
    
    var sum: (Int) -> Int = { value -> value + value }
    
    /*
    output : 20
    ```
- Yang perlu diperhatikan adalah, jika argumen terakhir dari fungsi merupakan sebuah lambda expression, maka lambda expression tersebut ditempatkan di luar parenthesis
    ```java
    fun main() {
        //look at this ini yang dimaksud ditaru diluar
        printResult(10){ value ->
            value + value
        }
    }
    
    fun printResult(value: Int, sum: (Int) -> Int) {
        val result = sum(value)
        println(result)
    }
    
    /*
    output : 20
    */
    ```
## 8. Lambda with Receiver
- 
## 9. Function Type
- Ketika kita mendeklarasikan sebuah higher-order function, maka kita perlu menentukan tipe deklarasi dari fungsi yang menjadi parameter.
- Syntax :
    ```java
    (Int, Int) -> String
    ```
- Setiap function type memiliki tanda kurung . Di dalamnya terdapat sebuah parameter dan jumlah tipe yang menandakan jumlah parameter dari fungsi tersebut. Pada contoh di atas, fungsi tersebut memiliki 2 (dua) parameter dengan tipe Int dan memiliki tipe kembalian String. Ketika kita tidak ingin fungsi tersebut mengembalikan nilai, kita bisa menggunakan Unit. Berbeda dengan fungsi pada umumnya, jika menggunakan tipe kembalian Unit, kita wajib menuliskannya.
- Kita bisa menyingkat function type menggunakan `typealias` :
    ```java
    typealias Arithmetic = (Int, Int) -> Int
    
    val sum: Arithmetic = { valueA, valueB -> valueA + valueB }
    val multiply: Arithmetic = { valueA, valueB -> valueA * valueB }
    ``` 
- Untuk membuat instance dari sebuah function type, terdapat beberapa cara. Salah satunya dengan lambda expression yang sudah kita bahas pada modul sebelumnya.kaya contoh diatas.
- untuk menggunakan instance-nya, kita bisa memanfaatkan operator invoke() :
    ```java
    val sumResult = sum.invoke(10, 10)
    val multiplyResult = multiply.invoke(20, 20)
    ```
- Atau kita bisa menuliskannya secara langsung dengan menghilangkan operator invoke():
    ```java
    val sumResult = sum(10, 10)
    val multiplyResult = multiply(20, 20)
    ```
- Kita juga bisa menandai function type sebagai nullable dengan menempatkannya di dalam tanda kurung dan diakhiri dengan safe call :
    ```java
    typealias Arithmetic = ((Int, Int) -> Int)?
    val sum: Arithmetic = { valueA, valueB -> valueA + valueB }
    ```
- Contoh penggunaan function type yang ditandai sebagai nullable:
    ```java
    sum?.invoke(10, 20)
    ```
## 10. Library Helper
- 
## 11. Scope Function with Lambda Receiver
- 
## 12. Scope Function with Lambda Argument
-
## 13. Member References
-
## 14. Function inside Function
- 
## 15. Advance Collection Function
-
## 16. Recursion
-
---
# 7. Object Oriented Programming
- Sebagai contoh mobil, dalam OOP blueprint adalah class, part-part mobil adalah properties, kemampuan mobil adalah behaviour, dan hasil realisasi blueprint adalah object
## 1. Object
- Objek merupakan hasil realisasi dari sebuah blueprint atau class yang tentunya memiliki fungsi dan juga properti sama seperti blueprint-nya. Artinya, dengan membuat objek kita dapat mengakses fungsi dan properti yang terdapat pada kelas tersebut.
- di kotlin, semua adalah object, bahkan data type primitif sepert string, int, boolean adalah object
## 2. Class
- Class merupakan sebuah blueprint. Di dalam kelas ini kita mendefinisikan sesuatu yang merupakan attribute ataupun behaviour
- Contoh class :
    ```java
    class Animal(val name: String,
                val weight: Double,
                val age: Int,
                val isMammal: Boolean
    ) {
    
        fun eat(){
            println("$name makan!")
        }
    
        fun sleep() {
            println("$name tidur!")
        }
    }
    
    fun main() {
        val dicodingCat = Animal("Dicoding Miaw", 4.2, 2,true)
        println("Nama: ${dicodingCat.name}, Berat: ${dicodingCat.weight}, Umur: ${dicodingCat.age}, mamalia: ${dicodingCat.isMammal}" )
        dicodingCat.eat()
        dicodingCat.sleep()
    }
    ```
## 3. Properties
- Property Accessor
    - Secara standar ketika properti pada kelas dibuat `mutable`, maka Kotlin akan menghasilkan fungsi `getter` dan `setter` pada properti tersebut. Jika properti pada sebuah kelas dibuat `read-only`, Kotlin hanya akan menghasilkan fungsi `getter` pada properti tersebut. Namun sebenarnya Anda bisa membuat fungsi getter dan setter secara manual jika pada kasus tertentu Anda perlu untuk override fungsi tersebut.
    ```java
    class Animal{
        var name: String = "Dicoding Miaw"
    }
    
    fun main(){
        val dicodingCat = Animal()
        println("Nama: ${dicodingCat.name}" )
        dicodingCat.name = "Goose"
        println("Nama: ${dicodingCat.name}")
    }
    
    /*
    output:
    Nama: Dicoding Miaw
    Nama: Goose
    */
    ```
- Pada kode  ${dicodingCat.name} sebenarnya terjadi proses pemanggilan fungsi getter pada properti name. Namun kita tidak melakukan override pada fungsi getter  sehingga fungsi tersebut hanya mengembalikan nilai name saja. Begitu juga pada kode dicodingCat.name = "Goose" pada kode tersebut terjadi pemanggilan fungsi setter pada properti name. 
- Tetapi jika kita melakukan override pada fungsi getter dan juga setter, maka kita dapat menambahkan kode lain pada fungsi getter sesuai dengan kebutuhan :
    ```java
    class Animal{
        var name: String = "Dicoding Miaw"
            get(){
                println("Fungsi Getter terpanggil")
                return field
            }
            set(value){
                println("Fungsi Setter terpanggil")
                field = value
            }
    }
    
    fun main(){
        val dicodingCat = Animal()
        println("Nama: ${dicodingCat.name}" )
        dicodingCat.name = "Goose"
        println("Nama: ${dicodingCat.name}")
    }
    
    /*
    output:
    Fungsi Getter terpanggil
    Nama: Dicoding Miaw
    Fungsi Setter terpanggil
    Fungsi Getter terpanggil
    Nama: Goose
    */
    ```
- Pada fungsi get(), kita perlu mengembalikan nilai sesuai tipe data dari propertinya atau kita dapat mengembalikan nilai dari properti itu sendiri dengan menggunakan keyword field. Sedangkan untuk fungsi set() kita memerlukan sebuah parameter. Ini merupakan sebuah nilai baru yang nantinya akan dijadikan nilai properti. Pada kode di atas parameter tersebut ditetapkan dengan nama value
## 4. Constructor
- Ketika suatu objek dibuat, semua properti pada kelas tersebut harus memiliki nilai. Kita dapat langsung menginisialisasi pada properti tertentu atau menginisialisasinya melalui constructor (konstruktor). Konstruktor merupakan fungsi spesial yang digunakan untuk menginisialisasi properti yang terdapat pada kelas tersebut. 
- Terdapat 3 (tiga) tipe konstruktor pada Kotlin, yaitu primary constructor, secondary constructor dan default constructor.
## 4.1. Primary Constructor 
- Jika kita akan membuat suatu objek dari sebuah kelas dan kelas tersebut memiliki primary constructor di dalamnya, maka kita diharuskan mengirim nilai sesuai properti yang dibutuhkan. 
- Penulisan primary constructor mirip seperti parameter pada fungsi. Properti cukup dituliskan pada header class diawali dengan var atau val.
    ```java
    class Animal(var name: String, var weight: Double, var age: Int = 0, var isMammal: Boolean = true){

    }

    fun main(){
        val dicodingCat = Animal("Dicoding Miaw", 4.2)
        println("Nama: ${dicodingCat.name}, Berat: ${dicodingCat.weight}, Umur: ${dicodingCat.age}, mamalia: ${dicodingCat.isMammal}" )
    }
    
    /*
    output:
        Nama: Dicoding Miaw, Berat: 4.2, Umur: 0, mamalia: true
    */
    ``` 
- disitu jga bisa diberi default value seperti diatas
- Kotlin menyediakan `blok init` yang memungkinkan kita untuk menuliskan properti didalam body class, tujuannya membantu dalam memvalidasi sebuah nilai properti sebelum diinisialisasi :
    ```java
    class Animal(name: String, weight: Double, age: Int, isMammal: Boolean) {
        val name: String
        val weight: Double
        val age: Int
        val isMammal: Boolean
    
        init {
            this.weight = if(weight < 0) 0.1 else weight
            this.age = if(age < 0) 0  else age
            this.name = name
            this.isMammal = isMammal
        }
    }
    ```
## 4.2. Secondary Cnstructor
- digunakan ketika kita ingin menginisialisasi sebuah kelas dengan cara yang lain. 
- contoh, kita bisa menambahkan secondary constructor pada kelas Animal:
    ```java
    class Animal(name: String, weight: Double, age: Int) {
        val name: String
        val weight: Double
        val age: Int
        var isMammal: Boolean
    
        init {
            this.weight = if(weight < 0) 0.1 else weight
            this.age = if(age < 0) 0  else age
            this.name = name
            this.isMammal = false
        }
    
        constructor(name: String, weight: Double, age: Int, isMammal: Boolean) : this(name, weight, age) {
            this.isMammal = isMammal
        }
    }
    
    fun main() {
        val dicodingCat = Animal("Dicoding Miaw", 2.5, 2, true)
        println("Nama: ${dicodingCat.name}, Berat: ${dicodingCat.weight}, Umur: ${dicodingCat.age}, mamalia: ${dicodingCat.isMammal}")
    
        val dicodingBird = Animal("Dicoding tweet", 0.5, 1)
        println("Nama: ${dicodingBird.name}, Berat: ${dicodingBird.weight}, Umur: ${dicodingBird.age}, mamalia: ${dicodingBird.isMammal}")
    }
    
    /*
    output:
        Nama: Dicoding Miaw, Berat: 2.5, Umur: 2, mamalia: true
        Nama: Dicoding tweet, Berat: 0.5, Umur: 1, mamalia: false
    */
    ```
- Dengan begitu, objek Animal dapat diinisialisasi dengan secondary constructor ketika nilai name, weight, age dan isMammal tersedia. Tetapi jika nilai isMammal tidak tersedia,  primary constructor lah yang akan digunakan dan nilai isMammal dapat diinisialisasi pada blok init dengan nilai default.
## 4.3. Default Constructor
- Kotlin secara otomatis membuat sebuah default constructor pada kelas jika kita tidak membuat sebuah konstruktor secara manual
    ```java
    class Animal{
        val name: String = "Dicoding Miaw"
        val weight: Double = 4.2
        val age: Int = 2
        val isMammal: Boolean = true
    }
    
    fun main(){
        val dicodingCat = Animal()
        println("Nama: ${dicodingCat.name}, Berat: ${dicodingCat.weight}, Umur: ${dicodingCat.age}, mamalia: ${dicodingCat.isMammal}" )
    }
    
    /*
    output:
        Nama: Dicoding Miaw, Berat: 4.2, Umur: 2, mamalia: true
    */
    ```
- Ketika kita membuat sebuah objek, default konstruktor akan dipanggil. Konstruktot tersebut akan menginisialisasi properti yang terdapat pada kelas dengan nilai default.
## 5. Inheritances
- cara extends di kotlin pake `:` 
    ```java
    class ChildClass: ParentClass {
    
    }
    ```
- Untuk membuat sebuah super atau parent class kita akan membutuhkan open class. Kelas pada Kotlin secara default bersifat final, oleh karena itu kita harus mengubahnya menjadi open class sebelum melakukan extends kelas tersebut
    ```java
    open class Animal(val name: String, val weight: Double, val age: Int, val isCarnivore: Boolean){
    
        open fun eat(){
            println("$name sedang makan!")
        }
    
        open fun sleep(){
            println("$name sedang tidur!")
        }
    }
    ```
- extends :
    ```java
    class Cat(pName: String, pWeight: Double, pAge: Int, pIsCarnivore: Boolean, val furColor: String, val numberOfFeet: Int)
        : Animal(pName, pWeight, pAge, pIsCarnivore) {
    
        fun playWithHuman() {
            println("$name bermain bersama Manusia !")
        }
    
        override fun eat(){
            println("$name sedang memakan ikan !")
        }
    
        override fun sleep() {
            println("$name sedang tidur di bantal !")
        }
    }
    ```
- run :
    ```java
    fun main(){
        val dicodingCat = Cat("Dicoding Miaw", 3.2, 2, true, "Brown", 4)
    
        dicodingCat.playWithHuman()
        dicodingCat.eat()
        dicodingCat.sleep()
    }
    
    /*
    output:
        Dicoding Miaw bermain bersama Manusia !
        Dicoding Miaw sedang memakan ikan !
        Dicoding Miaw sedang tidur di bantal !
    */
    ```
## 6. Abstract Class
- abstract merupakan gambaran umum dari sebuah kelas. Ia tidak dapat direalisasikan dalam sebuah objek. Pada modul sebelumnya kita sudah mempunyai kelas Animal. Secara harfiah hewan merupakan sebuah sifat. Kita tidak tahu bagaimana objek hewan tersebut. Kita tahu bentuk kucing, ikan dan ular seperti apa tetapi tidak untuk hewan. Maka dari itu konsep abstract class perlu diterapkan agar kelas Animal tidak dapat direalisasikan dalam bentuk objek namun tetap dapat menurunkan sifatnya kepada child class-nya.
    ```java
    abstract class Animal(var name: String, var weight: Double, var age: Int, var isCarnivore: Boolean){
    
        fun eat(){
            println("$name sedang makan !")
        }
    
        fun sleep(){
            println("$name sedang tidur !")
        }
    }
    ```
- Dengan begitu kelas Animal tidak dapat kita inisialisasikan menjadi sebuah objek. 
## 7. Visibility Modifiers (Hak Akses)
- Macam-macam  hak akses :
1. Public : Anggota yang diberi modifier ini dapat diakses dari manapun.
2. Private : Hak akses yang cakupannya paling terbatas. Anggota yang menerapkannya hanya dapat diakses pada scope yang sama.
3. Protected : Hak akses yang cakupannya terbatas pada hirarki kelas. Anggota hanya dapat diakses pada kelas turunannya atau kelas itu sendiri.
4. Internal : Hak akses yang cakupannya terbatas pada satu modul. Anggota yang menggunakannya tidak dapat diakses diluar dari modulnya tersebut.
- Semua modifier tersebut bisa digunakan untuk kelas, objek, konstruktor, fungsi, beserta properti yang ada di dalamnya. Kecuali modifier protected yang hanya bisa digunakan untuk anggota di dalam sebuah kelas dan interface. Protected tidak bisa digunakan pada package member seperti kelas, objek, dan yang lainnya. 
- `Untuk mengakses properties yang bukan cakupannya, harus membuat getter dan setter dalam class pemilik`
## 8. Import dan Packages
- Seluruh konten pada Kotlin, seperti kelas dan fungsi, dibungkus dalam sebuah package. Package tersebut digunakan untuk mengelompokkan kelas, fungsi dan variabel yang mempunyai kemiripan fungsionalitas. 
```java
import kotlin.math.*
import kotlin.random.Random
 
fun main(){
    println(PI)
    println(cos(120.0))
    println(sqrt(9.0))
    println(Random(0).nextInt(1, 10))
}
 
 
/*
Output:
    3.141592653589793
    0.8141809705265618
    3.0
    5
*/
```
## 8.1. Membuat Package Baru
- Idealnya sebuah package pada Kotlin dituliskan dengan awalan nama domain perusahaan yang dibalik. Contoh, com.dicoding. Kemudian diikuti dengan nama package yang akan digunakan.
- How to make new package:
1. src ->klik kanan -> new -> package -> beri nama misal com.dicoding.oop.utils
2. buat file MyMath.kt dalam dir utils, otomatis akan ada package `com.dicoding.oop.utils`
- How to run :
1. bikin di MyMath.kt :
    ```java
    package com.dicoding.oop.utils
    
    fun sayHello() = println("Hello From package utils")
    
    const val PI = 3.1415926535  // package level variable
    
    fun pow(number: Double, power: Double) : Double {
        var result = 1.0
        var counter = power
        while (counter > 0) {
            result *= number
            counter--
        }
        return result
    }
    
    fun factorial(number: Double) : Double {
        var result = 1.0
        var counter = 1.0
        while (counter <= number) {
            result *= counter
            counter++
        }
    
        return result
    }
    
    fun areaOfCircle(radius: Double) : Double {
        return PI * 2 * radius
    }
    ```
2. Panggil :
    ```java
    package com.dicoding.oop
    
    import com.dicoding.oop.utils.*
    
    fun main() {
        sayHello()
        println(factorial(4.0))
        println(pow(3.0, 2.0))
        println(PI)
        println(areaOfCircle(13.0))
    }
    
    /*
    output:
        Hello From package com.dicoding.oop.utils
        24.0
        9.0
        3.1415926535
        81.681408991
    */
    ```
## 9. Exception Handling
- Exception adalah event (kejadian) yang dapat mengacaukan jalannya suatu program. Pada Kotlin semua exception bersifat Unchecked, yang artinya exception terjadi karena kesalahan pada kode kita. 
- Contoh Exception yang sering terjadi :
1. ArithmaticException --> pembagian dengan 0
2. NullPointerException -> null
3. NumberFormatException -> convert format yang salah
- Menangani Exception dengantry-catch, try-catch-finally, multiple catch
## 9.1 try-catch
```java
fun main() {
    val someNullValue: String? = null
    lateinit var someMustNotNullValue: String
 
    try {
        someMustNotNullValue = someNullValue!!
        println(someMustNotNullValue)
    } catch (e: Exception) {
        someMustNotNullValue = "Nilai String Null"
        println(someMustNotNullValue)
    }
}
 
/*
output:
    Nilai String Null
*/
```
## 9.2 try-catch-finally
- finally akan dieksekusi apapun yanf terjadi
    ```java
    fun main() {
        val someNullValue: String? = null
        lateinit var someMustNotNullValue: String
    
        try {
            someMustNotNullValue = someNullValue!!
        } catch (e: Exception) {
            someMustNotNullValue = "Nilai String Null"
        } finally {
            println(someMustNotNullValue)
        }
    }
    
    /*
    output:
        Nilai String Null
    */
    ```
## 9.3 Multiple catch
```java
import kotlin.NumberFormatException
 
fun main() {
    val someStringValue: String? = null
    var someIntValue: Int = 0
 
    try {
        someIntValue = someStringValue!!.toInt()
    } catch (e: NullPointerException) {
        someIntValue = 0
    } catch (e: NumberFormatException) {
        someIntValue = -1
    } finally {
        when(someIntValue){
            0 -> println("Catch block NullPointerException terpanggil !")
            -1 -> println("Catch block NumberFormatException terpanggil !")
            else -> println(someIntValue)
        }
    }
}
 
/*
output:
    Catch block NullPointerException terpanggil!
*/
```
## 10. Overloading
- Fungsi dengan nama yang sama tapi memiliki jumlah parameter yang berbeda
    ```java
    class Animal(private var name: String) {
        fun eat() {
            println("$name makan!")
        }
    
        fun eat(typeFood: String) {
            println("$name memakan $typeFood!")
        }
    
        fun eat(typeFood: String, quantity: Double) {
            println("$name memakan $typeFood sebanyak $quantity grams!")
        }
    
        fun sleep() {
            println("$name tidur!")
        }
    }
    ```
    ```java
    fun main() {
        val dicodingCat = Animal("Dicoding Miaw")
    
        dicodingCat.eat()
        dicodingCat.eat("Ikan Tuna")
        dicodingCat.eat("Ikan Tuna", 450.0)
    }
    ```
## 11. Extension Properties
- Extension properties pada Kotlin sama halnya seperti melakukannya pada Extension function. Kita dapat menambahkan sebuah properti tanpa harus membuat sebuah kelas yang mewarisi kelas tersebut. Tetapi perlu diingat bahwa properti yang kita buat bukan benar - benar berada pada kelas. Sebabnya, Extension properties dilakukan di luar kelas. Dengan demikian, Extension properties hanya bisa didefinisikan dengan cara menyediakan getter dan/atau setter secara eksplisit
    ```java
    class Animal(var name: String, var weight: Double, var age: Int, var isMammal: Boolean)
    
    val Animal.getAnimalInfo : String
        get() =  "Nama: ${this.name}, Berat: ${this.weight}, Umur: ${this.age} Mamalia: ${this.isMammal}"
    ```
    ```java
    fun main() {
        val dicodingCat = Animal("Dicoding Miaw", 5.0, 2, true)
        println(dicodingCat.getAnimalInfo)
    }
    ```
## 12. Interfaces
- Seperti boilerplate kosongan yang harus di override oleh class anakannya
- Pada umumnya penamaan sebuah interface dituliskan dengan awalan huruf I kapital
- Sama seperti fungsi, kita juga diharuskan melakukan override properti. Overriding properti bisa dilakukan pada sebuah konstruktor kelas
```java
interface IFly {
    fun fly()
    val numberOfWings: Int
}
```
```java
class Bird(override val numberOfWings: Int) : IFly {
 
    override fun fly() {
        if(numberOfWings > 0) println("Flying with $numberOfWings wings")
        else println("I'm Flying without wings")
    }
}
```
## 13. Property Delegation
- Pengelolaan properti kelas baik itu memberikan atau merubah sebuah nilai dapat didelegasikan kepada kelas lain. Dengan ini kita dapat meminimalisir boilerplate dalam penulisan getter dan setter (jika properties menggunakan var) pada setiap kelas yang kita buat.
-  menerapkan getter dan setter pada setiap properti kelasnya, maka kita perlu menuliskan getter dan setter tersebut pada seluruh kelas. Hal tersebut dapat mengurangi efisiensi dalam menuliskan kode karena terlalu banyak kode yang harus kita tulis secara berulang. Solusinya, kita perlu membuat sebuah kelas yang memang bertugas untuk mengatur atau mengelola fungsi getter dan setter untuk sebuah properti kelas. Teknik tersebut pada Kotlin dinamakan Delegate.
- How ?
1. Buat delegation class nya dulu
    ```java
    import kotlin.reflect.KProperty;
    
    
    class DelegateName {
        private var value: String = "Default"
    
        operator fun getValue(classRef: Any?, property: KProperty<*>) : String {
            println("Fungsi ini sama seperti getter untuk properti ${property.name} pada class $classRef")
            return value
        }
    
        operator fun setValue(classRef: Any?, property: KProperty<*>, newValue: String){
            println("Fungsi ini sama seperti setter untuk properti ${property.name} pada class $classRef")
            println("Nilai ${property.name} dari: $value akan berubah menjadi $newValue")
            value = newValue
        }
    }
    ```
2. Class yang proertynya mw didelegasiin diberi keyword `by` diikuti class delegasinya:
    ```java
    class Animal {
        var name: String by DelegateName()
    }
    
    class Person {
        var name: String by DelegateName()
    }
    
    class Hero {
        var name: String by DelegateName()
    }

    ```
3. run
    ```java
    fun main() {
        val animal = Animal()
        animal.name = "Dicoding Miaw"
        println("Nama Hewan: ${animal.name}")
    
        val person = Person()
        person.name = "Dimas"
        println("Nama Orang: ${person.name}")
    
        val hero = Hero()
        hero.name = "Gatotkaca"
        println("Nama Pahlawan: ${hero.name}")
    }
    
    /*
    output:
        Fungsi ini sama seperti setter untuk properti name pada class Animal@17f052a3
        Nilai name dari: Default akan berubah menjadi Dicoding Miaw
        Fungsi ini sama seperti getter untuk properti name pada class Animal@17f052a3
        Nama Hewan: Dicoding Miaw
        Fungsi ini sama seperti setter untuk properti name pada class Person@2e0fa5d3
        Nilai name dari: Default akan berubah menjadi Dimas
        Fungsi ini sama seperti getter untuk properti name pada class Person@2e0fa5d3
        Nama Orang: Dimas
        Fungsi ini sama seperti setter untuk properti name pada class Hero@5010be6
        Nilai name dari: Default akan berubah menjadi Gatotkaca
        Fungsi ini sama seperti getter untuk properti name pada class Hero@5010be6
        Nama Pahlawan: Gatotkaca
    */
    ```
- Jika pengen generic, ganti string dengan Any biar semua data type bisa masuk
---
# 8. Generics `<>`
- Kotlin termasuk dalam bahasa pemrograman statically typed. Ketika menambahkan variabel baru, maka secara otomatis tipe dari variabel tersebut dapat dikenali pada saat kompilasi. 
- Contoh : jika dalam list isinya sama, type data yang disimpen bisa nggk ditulis
```java
val contributor = listOf<String>("jasoet", "alfian","nrohmen","dimas","widy")
// bisa ditulis
val contributor = listOf("alfian","nrohmen","dimans","widy")
```
- Berbeda klo list kosongan, type data yang disimpan harus dideklarasikan :
```java
val contributor = listOf<String>()
```
- Secara umum generic merupakan konsep yang digunakan untuk menentukan tipe data yang akan kita gunakan. Pendeklarasiannya ditandai dengan tipe parameter. Kita juga bisa mengganti tipe parameter menjadi tipe yang lebih spesifik dengan menentukan instance dari tipe tersebut.
## 1. Mendeklarasikan Kelas Generic
- Interface Awal
    ```java
    interface List<T>{
        operator fun get(index: Int) : T
    }
    ```
- Class implementasinya 
    ```java
    class LongList : List<Long>{
        override fun get(index: Int): Long {
            /* .. */
        }
    }
 
    class ArrayList<T> : List<T>{
        override fun get(index: Int): T {
            /* .. */
        }
    }
    ```
- Manggil
    ```java
    fun main() {
        val longArrayList = ArrayList<Long>()
        val longList = Longlist()
        val firstLong = longArrayList.get(0)
    }
    ```
- Bedakan Class Implementasinya, ArrayList butuh parameter ketika bikin instancenya karena ArrayList pake <T> , sedangkan LongList nggk karana type parameternya dah disebutin yaitu <Long>
## 2. Mendeklarasikan Fungsi Generic
- Syntax dasar :
    ```java
    fun <T> run(): T {
        /*...*/
    }
    ```
- Generic pada sebuah fungsi dibutuhkan ketika kita membuat sebuah fungsi yang berhubungan dengan List. Misalnya, list yang dapat digunakan untuk berbagai tipe dan tidak terpaku pada tipe tertentu. 
- Tipe argumen dari parameter fungsi generic ditentukan ketika fungsi tersebut dipanggil.
- Contoh penerapan fungsi generic bisa kita lihat pada deklarasi fungsi slice yang merupakan extensions function dari kelas List :
    ```java
    public fun <T> List<T>.slice(indices: Iterable<Int>): List<T> {
        /*...*/
    }
    ```
- Tipe parameter pada fungsi slice() di atas digunakan sebagai receiver dan return type. Ketika fungsi tersebut dipanggil dari sebuah List dengan tipe tertentu, kita bisa menentukan tipe argumennya secara spesifik seperti berikut:
    ```java
    fun main() {
        val numbers = (1..100).toList()
        print(numbers.slice<Int>(1..10))
    }
    
    /*
    output : [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
    */
    ```
- Seperti yang telah disebutkan sebelumnya, jika semua nilai yang berada di dalamnya memiliki tipe yang sama, kita bisa menyederhanakan. Caranya, hapus tipe parameter tersebut :
    ```java
    fun main() {
        val numbers = (1..100).toList()
        print(numbers.slice(1..10))
    }
    
    /*
    output : [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
    */
    ```
## 3. Constraint Type Parameter
- Kita bisa membatasi tipe apa saja yang dapat digunakan sebagai parameter. 
- Caranya? menambahkan tanda titik dua (:) setelah tipe parameter yang kemudian diikuti oleh tipe yang akan dijadikan batasan:
    ```java
    class ListNumber<T : Number> : List<T>{
        override fun get(index: Int): T {
            /* .. */
        }
    }
    ```
    - dengan ini, hanya angka yang boleh ada pada type argument
    ```java
    fun main() {
        val numbers = ListNumber<Long>()
        val numbers2 = ListNumber<Int>()
        val numbers3 = ListNumber<String>() // error : Type argument is not within its bounds
    }
    
    class ListNumber<T : Number> : List<T>{
        override fun get(index: Int): T {
            /* .. */
        }
    }
    ```
- Contoh lain :
    ```java
    fun <T : Number> List<T>.sumNumber() : T {
        /* .. */
    }
    ```
- Fungsi di atas merupakan extensions function dari kelas List yang mempunyai tipe parameter. Sama seperti deklarasi generic pada sebuah fungsi, tipe parameter T pada fungsi tersebut juga akan digunakan sebagai receiver dan return type :
    ```java
    fun main() {
        val numbers = listOf(1, 2, 3, 4, 5)
        numbers.sumNumber()
        val names = listOf("dicoding", "academy")
        names.sumNumber() // error : inferred type String is not a subtype of Number
    }
    
    fun <T : Number> List<T>.sumNumber() : T {
        /* .. */
    }
    ```
## 4. Variance
- adalah konsep yang menggambarkan bagaimana sebuah tipe yang memiliki subtipe yang sama tetapi tipe argumen yang berbeda saling berkaitan satu sama lain.
- Skip sek
---
# 9. Concurency
- Concurrency adalah beberapa proses yang terjadi secara bersamaan dalam suatu sistem.
- Contoh dunia nyata kaya halu arus lintas yang ramai
- Di dunia komputer, concurrency umumnya terkait dengan banyaknya core atau inti dari prosesor (CPU). 
- Dengan concurrency, kita bisa memanfaatkan kinerja prosesor dengan lebih optimal. Concurrency memungkinkan sebuah sistem untuk bisa dikendalikan dengan mudah. Sebagai contoh, suatu fungsi bisa dijalankan, dihentikan, ataupun dipengaruhi oleh fungsi lain yang jalan secara bersamaan. 
## 1. Concurrency vs Parallelism
- Concurrency terjadi apabila terdapat 2 (dua) atau lebih proses yang tumpang tindih dalam satu waktu. Ini bisa terjadi jika ada 2 (dua) atau lebih thread yang sedang aktif. Dan jika thread tersebut dijalankan oleh komputer yang hanya memiliki 1 (satu) core, semua thread tidak akan dijalankan secara paralel. 
- Concurrency memungkinkan sebuah komputer yang hanya memiliki 1 (satu) core tampak seakan mengerjakan banyak tugas sekaligus. Padahal sebenarnya tugas-tugas tersebut dilakukan secara bergantian.
-  Parallelism terjadi ketika 2 (dua) proses dijalankan pada titik waktu yang sama persis. Parallelism bisa dilakukan jika terdapat 2 (dua) atau lebih thread dan komputer juga memiliki 2 (dua) core atau lebih. Sehingga setiap core dapat menjalankan perintah dari masing-masing thread secara bersamaan. 
- Concurency :
    ![](img_kotlin_dicoding/concurency.png?raw=true)
- Parallelism :
    ![](img_kotlin_dicoding/paralellism.png?raw=true)
# 9.1. Process, Thread, I/O-Bound
- Saat kita mulai menjalankan sebuah aplikasi, sistem operasi akan membuat sebuah proses, dan melampirkan sebuah thread padanya, dan setelah itu mulai menjalankan thread tersebut. Semua aktivitas tersebut akan bergantung pada perangkat yang digunakan, terutama perangkat perangkat input dan output (I/O).
- Untuk bisa memahami dan juga menerapkan concurrency dengan benar, developer perlu mempelajari beberapa konsep dasar seperti `Process`, `Thread` dan `I/O bound`:
## 1. Process
- Sebuah proses (process) merupakan bagian dari aplikasi yang sedang dijalankan. 
- Aplikasi adalah serangkaian proses yang saling bekerja sama. Untuk memfasilitasi komunikasi antar proses, sebagian besar sistem operasi mendukung sumber daya Inter Process Communication (IPC)
- Kita pasti mengenal dengan sebuah konsep yang bernama multitasking atau melakukan banyak tugas secara bersamaan. Saat multitasking, sebenarnya sistem operasi hanya beralih di antara berbagai proses dengan sangat cepat untuk memberikan kesan bahwa proses ini sedang dijalankan secara bersamaan. Sebaliknya, multiprocessing adalah metode untuk menggunakan lebih dari satu CPU dalam menjalankan tugas.
## 2. Thread
- Thread biasa dikenal dengan proses yang ringan. Membuat thread baru membutuhkan lebih sedikit sumber daya daripada membuat proses baru.
- Perbedaan utama antara proses dan thread adalah bahwa thread biasanya merupakan komponen dari suatu proses. Selain itu, thread biasanya memungkinkan untuk berbagi sumber daya seperti memori dan data. Dimana 2 (dua) hal tersebut jarang dilakukan oleh sebuah proses.
- Ada 2 thread yang sering terdegar yaitu :
1. main thread --> thread utama yang mengeksekusi fungsi utama dari sebuah aplikasi, dan life cycle dari sebuah proses akan terikat padanya.
2. UI thread --> berfungsi untuk memperbarui user interface (antarmuka) dan juga merespon aksi yang diberikan pada aplikasi.
## 3. I/O Bound
- Bottlenecks atau kemacetan adalah suatu hal yang penting untuk ditangani demi mengoptimalkan kinerja aplikasi.Perangkat input dan output biasanya sering mempengaruhi sebuah aplikasi mengalami bottlenecks. Sebagai contoh, memori yang terbatas, kecepatan prosesor, dsb.
- Cara mengatasinya ?
    - I/O-bound merupakan sebuah algoritma yang bergantung pada perangkat input atau output. Waktu untuk mengeksekusi sebuah I/O-bound tergantung pada kecepatan perangkat yang digunakan. Sebagai contoh, suatu algoritma untuk membaca dan menulis sebuah dokumen. Ini adalah operasi I/O yang akan tergantung pada kecepatan di mana berkas tersebut dapat diakses. Berkas yang disimpan pada SSD akan lebih cepat diakses dibandingkan berkas yang disimpan pada HDD.
    - Algoritma I/O-bound akan selalu menunggu sesuatu yang lain. Penantian terus-menerus ini memungkinkan perangkat yang hanya memiliki satu core untuk menggunakan prosesor demi tugas-tugas bermanfaat lainnya sambil menunggu. Jadi algoritma concurrent yang terikat dengan I/O akan melakukan hal yang sama, terlepas dari eksekusi yang terjadi -apakah paralel atau dalam satu core?
- Jadi terapkan concurency jika aplikasi yang Anda kembangkan punya ketergantungan dengan perangkat I/O.
# 9.2. Permasalahan pada Concurency
- Ada beberapa permasalahan yang wajib bisa kita tangani pada concurrency, yaitu deadlocks, livelocks, starvation dan juga race conditions.
## 1. Deadlocks
- kondisi di mana dua proses atau lebih saling menunggu proses yang lain untuk melepaskan resource yang sedang digunakan.
- Deadlocks mengakibatkan proses-proses yang sedang berjalan, tak kunjung selesai. Kasus ini merupakan umum terjadi ketika banyak proses yang saling berbagi sumber daya. 
- Ibarat mobil yang saling tunggu agar bisa jalan di jalan yang cma bsa dilewati 1 mboil, itulah deadlock
    ![](img_kotlin_dicoding/deadlock.png?raw=true)
## 2. Livelock
- Mirip deadlock, livelocks juga merupakan kondisi di mana sebuah proses tidak bisa melanjutkan tugasnya. Namun yang membedakannya adalah bahwa selama livelocks terjadi, keadaan dari aplikasi tetap bisa berubah. Walaupun perubahan keadaan tersebut menyebabkan proses berjalan dengan tidak semestinya.
- Contoh : ketika orang jalan trs tembukan, bakal saling kiri-kanan ampe bsa jalan
    ![](img_kotlin_dicoding/livelock.gif?raw=true)
## 3. Starvation
- Starvation merupakan sebuah kondisi yang biasanya terjadi setelah deadlock. Kondisi deadlock sering kali menyebabkan sebuah proses kekurangan sumber daya sehingga mengalami starvation atau kelaparan. Pada kondisi seperti ini, thread tak dapat akses reguler ke sumber daya bersama dan membuat proses terhenti.
- Selain deadlock, hal lain yang bisa mengakibatkan starvation adalah ketika terjadi kesalahan sistem. Akibatnya, ada ketimpangan dalam pembagian sumber daya. Sebagai contoh, ketika satu proses bisa mendapat sumber daya, namun tidak dengan proses lain. Biasanya, kesalahan seperti itu disebabkan oleh algoritma penjadwalan (scheduling algorithm) yang kurang tepat atau bisa juga karena resource leak atau kebocoran sumber daya.
- Salah satu solusi untuk starvation adalah dengan menerapkan algoritma penjadwalan dengan antrian prioritas (process priority) dan juga menerapkan teknik aging atau penuaan. Aging merupakan sebuah teknik yang secara bertahap meningkatkan prioritas sebuah proses yang menunggu dalam sistem pada waktu yang cukup lama.
## 4. Race Conditions
- Race condition merupakan masalah umum pada concurrency, yaitu kondisi di mana terdapat banyak thread yang mengakses data yang dibagikan bersama dan mencoba mengubahnya secara bersamaan. 
- Contoh sederhana dari race condition bisa kita lihat pada aplikasi perbelanjaan online. Katakanlah Anda menemukan barang yang ingin Anda beli di sebuah toko online. Pada halaman deskripsi, tampil informasi bahwa stok barang hanya sisa 1 (satu). Lalu Anda langsung mengambil keputusan dengan menekan tombol beli. Pada saat yang sama, ada pengguna lain yang ternyata lebih dahulu membelinya. Kondisi seperti inilah yang mengakibatkan sebuah state atau keadaan, berubah tanpa Anda sadari. Jika sistem tidak dirancang sedemikian rupa, maka masalah tak terduga, bisa mengemuka.
- Ketika race condition terjadi, maka sistem bisa saja melakukan proses yang tidak semestinya. Pada saat itu bug akan muncul. Race condition dikenal sebagai masalah yang sulit untuk direproduksi dan di-debug, karena hasil akhirnya tidak deterministik dan tergantung pada waktu relatif dari thread yang menghalangi. Masalah yang muncul pada production bisa jadi tidak ditemukan pada saat debug. Oleh karena itu, lebih baik menghindari race condition dengan cara berhati-hati saat merencanakan sebuah sistem. Ini lebih baik daripada harus berusaha memperbaikinya setelah itu. Lebih repot. 
# 9.3. Coroutines
- Coroutines adalah cara baru untuk menulis kode asynchronous dan non-blocking.
- Coroutines berjalan di atas sebuah thread.
- Sering disebut threads yang ringan, Coroutines mendefinisikan eksekusi dari sekumpulan instruksi untuk dieksekusi oleh prosesor. Selain itu, coroutines juga memiliki life cycle yang sama dengan thread.
- Coroutines dijalankan di dalam threads. Satu thread dapat memiliki banyak coroutine di dalamnya. Namun, seperti yang sudah disebutkan, hanya satu instruksi yang dapat dijalankan dalam thread pada waktu tertentu. Artinya, jika Anda memiliki sepuluh coroutines di thread yang sama, hanya satu dari sepuluh coroutines tersebut yang akan berjalan pada titik waktu tertentu.
## 1. Memulai Coroutines
- coroutines bukanlah bagian dari bahasa Kotlin. Coroutines hanyalah library lain yang disediakan oleh JetBrains. 
- Untuk menambahkan,tambahkan dependecies ini pada build.gradle.kts
    ```java
    dependencies {
        implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.2.2")
    }
    ```
- Contoh awal :
    ```java
    import kotlinx.coroutines.*
    
    //runBLocking{} --> memulai coroutine utama
    //launch{} -->  menjalankan coroutine baru
    
    fun main() = runBlocking{
        launch {
            delay(1000L)
            println("Coroutines!")
        }
        println("Hello,")
        delay(2000L)
    }
    /*
    output:
    Hello,
    Coroutines!
    */
    ```
    - Hello, akan muncul lebih dahulu dibanding Coroutines! dengan rentang 1 detik, kenapa?
    1. Fungsi `delay(1000L)` di dalam `launch` digunakan untuk menunda kode berikutnya selama 1 detik. `delay` adalah fungsi yang spesial pada coroutines. Ia merupakan sebuah suspending function yang tidak akan memblokir sebuah thread. Selama proses penundaan tersebut, main thread akan terus berjalan sehingga fungsi println("Hello,") akan langsung dijalankan. Setelah 1 detik, baru fungsi println("Coroutines!") akan dijalankan.
    2. `delay(2000L)` digunakan untuk menunda selama 2 detik sebelum JVM berakhir. Tanpa kode ini, JVM akan langsung berhenti ketika kode terakhir dijalankan, sehingga kode di dalam launch tidak akan pernah dijalankan.
## 2. Coroutines Builder
- `launch` dan `runBlocking` merupakan coroutines builder, yaitu sebuah fungsi yang mengambil suspending lambda dan membuat coroutine untuk menjalankannya.
- Macam2 coroutines builder :
1. `launch`
    - digunakan untuk memulai sebuah coroutines yang tidak akan mengembalikan sebuah hasil. launch akan menghasilkan Job yang bisa kita gunakan untuk membatalkan eksekusi.
2. `runBlocking`
    - Fungsi ini dibuat untuk menjembatani blocking code menjadi kode yang dapat ditangguhkan. runBlocking akan memblokir sebuah thread yang sedang berjalan hingga eksekusi coroutine selesai. Seperti contoh sebelumnya, kita bisa menggunakannya pada fungsi main() dan bisa juga untuk penerapan unit test. 
3. `async`
    - Kebalikan dari launch, fungsi ini digunakan untuk memulai sebuah coroutine yang akan mengembalikan sebuah hasil. Ketika menggunakannya, Anda harus berhati-hati karena ia akan menangkap setiap exception yang terjadi di dalam coroutine. Jadi async akan mengembalikan `Deferred` yang berisi hasil atau exception. Ketika yang dikembalikan adalah exception, maka Anda harus siap untuk menanganinya.
- Contoh penerapan async :
    ```java
    suspend fun getCapital(): Int {
        delay(1000L)
        return 50000
    }
    
    suspend fun getIncome(): Int {
        delay(1000L)
        return 75000
    }
    ```
    - Coba jalankan :
        ```java
        import kotlinx.coroutines.*
        
        fun main() = runBlocking {
            val capital = getCapital()
            val income = getIncome()
            println("Your profit is ${income - capital}")
        }
        ```
    - Kode tersebut akan bejalan secara sequential which means `getIncome()` dieksekusi duluan setelah selesai `getCapital()` akan dieksekusi pada println (butuh total 2 detik)
    - Nah jika pake `async` kode tersebut dapat dijalankan secara bersamaan :
        ```java
        import kotlinx.coroutines.*
    
        fun main() = runBlocking {
            //memanggil getCapital() dan getIncome() dalam async
            val capital = async { getCapital() }
            val income = async { getIncome() }
            //untuk mengakses hasilnya perlu menggunakan fungsi await() 
            println("Your profit is ${income.await() - capital.await()}")
        }
        ```
    - Eksekusi :
        ```java
        import kotlinx.coroutines.*
        import kotlin.system.measureTimeMillis
        
        fun main() = runBlocking {
            val timeOne = measureTimeMillis {
                val capital = getCapital()
                val income = getIncome()
                println("Your profit is ${income - capital}")
            }
        
            val timeTwo = measureTimeMillis {
                val capital = async { getCapital() }
                val income = async { getIncome() }
                println("Your profit is ${income.await() - capital.await()}")
            }
        
            println("Completed in $timeOne ms vs $timeTwo ms")
        
        }
        ```
    - Hasil :
        ```java
        Your profit is 25000
        Your profit is 25000
        Completed in 2013 ms vs 1025 ms
        ```
## 3. Job and Deferred
- Secara umum, fungsi asynchronous pada coroutines terbagi menjadi 2 (dua) jenis :
 1. fungsi yang mengembalikan hasil
    - contoh :  fungsi untuk mengambil informasi dari web service yang menghasilkan respon berupa JSON atau yang lainnya
 2. fungsi yang tidak mengembalikan hasil
    - contoh : biasanya digunakan untuk mengirimkan analitik, menuliskan log, atau tugas sejenis lainnya.
- Kita bisa mengakses fungsi yang sudah dijalankan. Misalnya, ketika kita ingin membatalkan tugasnya atau memberikan instruksi tambahan ketika fungsi tersebut telah mencapai kondisi tertentu. Untuk bisa melakukannya, Anda perlu memahami tentang `Job` dan `Deferred` pada coroutines.
## 3.1. Job
- adalah sebuah hasil dari perintah asynchronous yang dijalankan. Objek dari job akan merepresentasikan coroutine yang sebenarnya.
- Sebuah Job memiliki  3 properti yang nantinya bisa dipetakan ke dalam setiap state atau keadaan :
1. isActive --> properti yang menunjukkan ketika sebuah job sedang aktif.
2. isCompleted --> properti yang menunjukkan ketika sebuah job telah selesai.
3. isCanceled --> properti yang menunjukkan ketika sebuah job telah dibatalkan.
- Pada dasarnya, job akan segera dijalankan setelah ia dibuat. Namun kita juga bisa membuat sebuah job tanpa menjalankannya. Job memiliki beberapa siklus hidup mulai dari pertama kali ia dibuat hingga akhirnya selesai. Kira-kira seperti inilah siklus dari sebuah job jika digambarkan dalam sebuah diagram:
    ![](img_kotlin_dicoding/siklusjob.png?raw=true)
    1. New --> Keadaan di mana sebuah job telah diinisialisasi namun belum pernah dijalankan.
    2. Active --> job sedang dijalankan. job yang sedang ditangguhkan (suspended job) juga termasuk ke dalam job yang aktif.
    3.Completed --> Ketika job sudah tidak berjalan lagi. Ini berlaku untuk job yang berakhir secara normal, dibatalkan, ataupun karena suatu pengecualian.
    4. Cancelling --> kondisi ketika fungsi cancel() dipanggil pada job yang sedang aktif dan memerlukan waktu untuk pembatalan tersebut selesai.
    5. Cancelled --> Keadaan yang dimiliki oleh sebuah job yang sudah berhasil dibatalkan. Perlu diketahui bahwa job yang dibatalkan juga dapat dianggap sebagai Completed job.
### 1. Membuat Job Baru
- Job dapat diinisialisasikan menggunakan fungsi launch() maupun Job() seperti berikut:
    ```java
    //menggunakan launch():
    fun main() = runBlocking {
        val job = launch {
            // Do background task here
        }
    }
    
    //menggunakan Job():
    fun main() = runBlocking {
        val job = Job()
    }
    ```
- Setelah diinisialisasikan, job akan memiliki state New dan akan langsung dijalankan. Jika ingin membuat sebuah job tanpa langsung menjalankannya, bisa memanfaatkan `CoroutineStart.LAZY` :
    ```java
    fun main() = runBlocking {
        val job = launch(start = CoroutineStart.LAZY) {
            TODO("Not implemented yet!")
        }
    }
    ```
- Setelah membuat sebuah job, kini kita bisa mulai menjalankan job tersebut. Kita bisa menggunakan fungsi `start()` seperti berikut:
    ```java
    fun main() = runBlocking {
        val job = launch(start = CoroutineStart.LAZY) {
            delay(1000L)
            println("Start new job!")
        }
    
        job.start()
        println("Other task")
    }
    ```
    ```java
    //output :
    Other task
    Start new job!
    ```
- Atau bisa menjalankan menggunakan fungsi `join()` :
    ```java
    fun main() = runBlocking {
        val job = launch(start = CoroutineStart.LAZY) {
            delay(1000L)
            println("Start new job!")
        }
    
        job.join()
        println("Other task")
    }

    ```
    ```java
    Start new job!
    Other task
    ```
- Pebedaan menggunkan `start()` dan `join()` : `start()` akan memulai job tanpa harus menunggu job tersebut selesai tapi eksekusi tetap jalan, sedangkan `join()` akan menunda eksekusi sampai job selesai.
### 2. Membatalkan Job
- Anda bisa melakukannya dengan memanggil fungsi cancel() seperti berikut:
    ```java
    fun main() = runBlocking {
        val job = launch {
            delay(5000)
            println("Start new job!")
        }
    
        delay(2000)
        job.cancel()
        println("Cancelling job...")
        if (job.isCancelled){
            println("Job is cancelled")
        }
    }
    ```
    - Saat fungsi cancel() dipanggil, job akan memasuki state Cancelling sampai pembatalan tersebut berhasil. Kemudian setelah pembatalan berhasil, job akan memiliki state Cancelled dan Completed.
- Perlu diketahui bahwa jika cancel() dipanggil dalam job baru yang belum dijalankan, job tersebut tidak akan melalui state Cancelling, melainkan akan langsung memasuki state Cancelled.
- Kita juga bisa menambahkan parameter terhadap fungsi cancel(), yaitu parameter `cause` yang bisa digunakan untuk memberitahu kenapa sebuah job dibatalkan.
    ```java
    @InternalCoroutinesApi
    fun main() = runBlocking {
        val job = launch {
            delay(5000)
            println("Start new job!")
        }
    
        delay(2000)
        job.cancel(cause = CancellationException("time is up!"))
        println("Cancelling job...")
        if (job.isCancelled){
            println("Job is cancelled because ${job.getCancellationException().message}")
        }
    }
    ```
- `CancellationException` akan mengirimkan nilainya sebagai pengecualian dari job tersebut. Kita pun bisa mengakses nilai tersebut dengan fungsi getCancellationException. Karena `getCancellationException` masih tahap eksperimen, Anda perlu menambahkan anotasi `@InternalCoroutinesApi`.
## 3.2. Deferred
- Sebelumnya, fungsi async akan mengembalikan nilai deferred yang berupa hasil atau exception. Deferred adalah nilai tangguhan yang dihasilkan dari proses coroutines. Nilai ini nantinya bisa kita kelola sesuai dengan kebutuhan.  
- Deferred dapat kita ciptakan secara manual. Meskipun begitu, dalam praktiknya, jarang kita membuat deferred secara manual. Biasanya kita hanya bekerja dengan deferred yang dihasilkan oleh async.
- Deferred juga memiliki life cycle yang sama dengan job. Perbedaanya hanyalah pada tipe hasil yang diberikan. Selain memberikan hasil ketika proses komputasi sukses, ia juga bisa memberikan hasil saat proses tersebut gagal. Hasil dari deferred tersedia ketika mencapai state `completed` dan dapat diakses dengan fungsi `await`. Deferred akan mengirimkan pengecualian jika ia telah gagal. Kita bisa mengakses nilai pengecualian tersebut dengan fungsi `getCompletionExceptionOrNull`.
- Pada dasarnya, nilai deferred juga merupakan sebuah job. Ia diciptakan dan dimulai pada saat coroutines mencapai state active. Bagaimanapun, fungsi async juga memiliki opsional parameter seperti CoroutineStart.LAZY untuk memulainya. Dengan begitu, deferred juga bisa diaktifkan saat fungsi start, join, atau await dipanggil.
- Contoh seperti pada modul sebelumnya :
    ```java
    import kotlinx.coroutines.*
    
    fun main() = runBlocking {
        val capital = async { getCapital() }
        val income = async { getIncome() }
        println("Your profit is ${income.await() - capital.await()}")
    }
    ```
    - capital dan income adalah contoh dari nilai deferred yang untuk mengaksesnya kita membutuhkan fungsi await. 
## 4. Coroutine Dispatcher
- Seperti yang sudah kita ketahui, coroutines berjalan di atas sebuah thread. Tentunya kita harus mengetahui thread mana yang akan digunakan untuk menjalankan dan melanjutkan sebuah coroutine. Untuk menentukannya kita membutuhkan sebuah base class bernama `CoroutineDispatcher`. Di dalam kelas tersebut kita akan menemukan beberapa objek yang nantinya bisa digunakan untuk menentukan thread yang berfungsi menjalankan coroutines.
- Intinya `CoroutineDispatcher` tu buat menentukan thread mana yang mw digunakan untuk menjalankan coroutines
## 4.1  Beberapa objek yang nantinya bisa digunakan untuk menentukan thread yang berfungsi menjalankan coroutines :
### 1. Dispatcher.Default
- Merupakan dispatcher dasar yang digunakan oleh semua standard builders seperti launch, async, dll jika tidak ada dispatcher lain yang ditentukan. 
- `Dispatcher.Default` menggunakan kumpulan thread yang ada pada JVM. Pada dasarnya, jumlah maksimal thread yang digunakan adalah sama dengan jumlah core dari CPU.
    ```java
    //untuk menggunakannya, cukup menggunakan coroutines builder tanpa harus menuliskan dispatcher secara spesifik:
    launch {
        // TODO: Implement suspending lambda here
    }
    //bisa juga menuliskannya secara eksplisit:
    launch(Dispatcher.Default){
        // TODO: Implement suspending lambda here
    }
    ```
### 2. Dispatcher.IO
- Sebuah dispatcher yang dapat digunakan untuk membongkar pemblokiran operasi I/O. Ia akan menggunakan kumpulan thread yang dibuat berdasarkan permintaan. 
    ```java
    launch(Dispatcher.IO){
        // TODO: Implement algorithm here
    }
    ```
### 3. Dispatcher.Unconfined
- Dispatcher ini akan menjalankan coroutines pada thread yang sedang berjalan sampai mencapai titik penangguhan. Setelah penangguhan, coroutines akan dilanjutkan pada thread dimana komputasi penangguhan yang dipanggil.
- Sebagai contoh, ketika fungsi a memanggil fungsi b, yang dijalankan dengan dispatcher dalam thread tertentu, fungsi a akan dilanjutkan dalam thread yang sama dengan fungsi b dijalankan. Perhatikan kode berikut:
    ```java
    import kotlinx.coroutines.*
    
    fun main() = runBlocking<Unit> {
        launch(Dispatchers.Unconfined) {
            println("Starting in ${Thread.currentThread().name}")
            delay(1000)
            println("Resuming in ${Thread.currentThread().name}")
        }.start()
    }
    ```
    ```java
    //hasil :
    Starting in main
    Resuming in kotlinx.coroutines.DefaultExecutor
    ```
    - Artinya, coroutine telah dimulai dari `main thread`, kemudian tertunda oleh fungsi delay selama 1 detik. Setelah itu, coroutine dilanjutkan kembali pada thread `DefaultExecutor`.
## 4.2. Bersamaan dengan objek-objek tersebut, ada beberapa builder yang dapat digunakan untuk menentukan thread yang dibutuhkan:
### 1. Single Thread Context
- Dispatcher ini menjamin bahwa setiap saat coroutine akan dijalankan pada thread yang Anda tentukan.
- Untuk menerapkannya, Anda bisa memanfaatkan `newSingleThreadContext()` :
    ```java
    import kotlinx.coroutines.*
    
    fun main() = runBlocking<Unit> {
        val dispatcher = newSingleThreadContext("myThread")
        launch(dispatcher) {
            println("Starting in ${Thread.currentThread().name}")
            delay(1000)
            println("Resuming in ${Thread.currentThread().name}")
        }.start()
    }
    ```
    ```java
    Starting in myThread
    Resuming in myThread
    ```
    - Walaupun sudah menjalankan fungsi delay, coroutine akan tetap berjalan pada myThread.
### 2. Thread Pool
- Sebuah dispatcher yang memiliki kumpulan thread. Ia akan memulai dan melanjutkan coroutine di salah satu thread yang tersedia pada kumpulan tersebut. Runtime akan menentukan thread mana yang tersedia dan juga menentukan bagaimana proses distribusi bebannya.
- Anda bisa menerapkan thread pool dengan fungsi newFixedThreadPoolContext() seperti berikut:
    ```java
    import kotlinx.coroutines.*
    
    fun main() = runBlocking<Unit> {
        val dispatcher = newFixedThreadPoolContext(3, "myPool")
    
        launch(dispatcher) {
            println("Starting in ${Thread.currentThread().name}")
            delay(1000)
            println("Resuming in ${Thread.currentThread().name}")
        }.start()
    }
    ```
    ```java
    Starting in myPool-1
    Resuming in myPool-2
    ```
    - da kode di atas, kita telah menetapkan thread myPool sebanyak 3 thread. Runtime akan secara otomatis menentukan pada thread mana coroutine akan dijalankan dan dilanjutkan.
## 5. Channel
- sebuah program dapat memiliki banyak thread dan dalam beberapa thread bisa terdapat jutaan coroutines. Lalu, bagaimana jika ada 2 (dua) coroutines yang saling ingin berinteraksi satu sama lain? Channels adalah jawabnya.
- Beberapa masalah yang muncul pada concurrency seperti deadlock, race conditions, dan lainnya, sering kali dipicu oleh satu hal, apa itu? Rupanya problem pembagian memori atau sumber daya antar thread. Untuk mengatasinya, banyak programming language seperti Go, Dart, dan juga Kotlin telah menyediakan channels.
- Channels adalah nilai deferred yang menyediakan cara mudah untuk mentransfer nilai tunggal antara coroutine. Pada dasarnya, channels sangat mirip dengan `BlockingQueue`. Namun, alih-alih memblokir sebuah thread, channels menangguhkan sebuah coroutine yang jauh lebih ringan.
- Contoh :
    ```java
    import kotlinx.coroutines.*
    import kotlinx.coroutines.channels.Channel
    
    fun main() = runBlocking(CoroutineName("main")) {
        val channel = Channel<Int>()
        launch(CoroutineName("v1coroutine")){
            println("Sending from ${Thread.currentThread().name}")
            for (x in 1..5) channel.send(x * x)
        }
    
        repeat(5) { println(channel.receive()) }
        println("received in ${Thread.currentThread().name}")
    }
    ```
    ```java
    Sending from main @v1coroutine#2
    1
    4
    9
    16
    25
    received in main @main#1
    ```
-  Pada coroutine v1coroutine, channels telah mengirimkan nilai dari hasil komputasi dengan menggunakan fungsi `send`. Setelah itu, di coroutine lain (main) channel menerima nilai dengan menggunakan fungsi `receive`.
- Kesimpulannya, channels memungkinkan komunikasi yang aman antar kode concurrent. Ia membuat kode concurrent dapat berkomunikasi dengan mengirim dan menerima pesan tanpa harus peduli di thread mana coroutine berjalan. 


 


