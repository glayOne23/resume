# Go-lang untuk Pemula

## 2. Program Hello Word
- Hello world!:
	```go
	package main

	import "fmt"

	func main() {
		fmt.Println("Hello World!")
	}
	```
- untuk compile: 
	1. `go build <nama_file.go>`
	2. Akan terbentuk file `<nama_file>`. Jalankan dengan mengetik `./<nama_file>`
- Untuk menjalankan pada mode development tanpa compile: `go run <nama_file.go>`

## 3. Type Data Number
- ada 2 jenis:
	1. integer -> int8, int16, int32, int64, uint8, uint16, uint32, uint64
	2. floating point -> float32, float64, complex64, complex128
- Alias:
	- byte -> uint8
	- rune -> int32
	- int -> minimal int32 (ngikuti sistem informasinya berapa byte)
	- uint -> minimal uint32 (ngikuti sistem informasinya berapa byte)

## 4. Type Data Boolean
- true/false

## 5. Type Data String
- Code:
	```go
	package main

	import "fmt"

	func main() {
		fmt.Println("Anto Kewer")
		fmt.Println(len("Anto Kewer"))
		fmt.Println("Anto Kewer"[2])
	}
	```
- Hasil:
	```go
	Anto Kewer
	10
	116  // ini ada bite t
	```

## 6. Type Data Variable
- Code:
	```go
	package main

	import "fmt"

	func main() {
		// cara 1
		var name string
		name = "Anto"
		name = "Susanto"
		fmt.Println(name)	

		// cara 2
		var age = 3
		fmt.Println(age)	

		// cara 3
		wni := true
		fmt.Println(wni)

		// cara 4
		var (
			firstName = "Eko"
			lastName = "Budi"
		)
		fmt.Println(firstName, lastName)
	}

	```
- Hasil:
	```go
	Susanto
	3
	true
	Eko Budi
	```

## 7. Type Data Constant
```go
	const token = "fsjk67HG832hfls*&shfksfa8(YHUD87sfs"
```

## 8. Konversi Tipe Data
- Code:
	```go
	package main

	import "fmt"

	func main() {
		name := "Anto"

		fmt.Println(string(name[0]))
	}
	```
- Hasil: 
	```go
	A
	``` 

## 9. Type Declarations
- Untuk membuat alias terhadap type data yang sudah ada, dengan tujuan agar lebih mudah dimengerti
- Code:
	```go
	package main

	import "fmt"

	func main() {
		type Married bool

		var marriedStatus Married = true
		fmt.Println(marriedStatus)
	}
	```

## 13. Type Data Array
- Ukuran array tidak bisa berubah
- Code:
	```go
	package main

	import "fmt"

	func main() {
		// cara 1 
		// maksimal hanya bisa diisi 3 index dan dengan type data string
		var names [3]string
		names[0] = "Anto"
		names[1] = "Kewer"
		fmt.Println(names[0], names[1])

		// cara 2
		var names2 = [1]string{
			"Jajang",
		}
		fmt.Println(names2[0])
	}
	```

## 14. Type Data Slice
- Adalah potongan dari data Array
- Mirip array, tapi ukuran array bisa berubah
- Slice adalah data yang mengakses sebagian atau seluruh data di Array
- memiliki 3 data: pointer, length, dan capacity
	- pointer -> penunjuk data pertama di array pada slice
	- length -> panjang slice
	- capacity -> kapasitas dari slice, dimana length tidak boleh > capacity
- Jika pada array atau pada slice nilainya diubah, maka yang lain ikut berubah
- Code 1:
	```go
	package main

	import "fmt"

	func main() {
		// jika tidak tahu length array, bisa pakai [...]
		var months = [...]string{
			"Januari",
			"Februari",
			"Maret",
			"April",
			"Mei",
			"Juni",
			"Juli",
			"Agustus",
			"September",
			"Oktober",
			"November",
			"Desember",
		}

		// slice
		var slice1 = months[4:7] // ambil dari index 4 sampai 7 
		fmt.Println(slice1) // tampilkan slice
		fmt.Println(len(slice1)) // lihat length slice
		fmt.Println(cap(slice1)) //kapasitas slice (dari awal pengambilan sampai akhir array, bukan akhir pengambilan)
	}

	```
- Hasil 1:
	```go
	[Mei Juni Juli]
	3
	8
	```
- Code 2:
	```go
	package main

	import "fmt"

	func main() {
		// jika tidak tahu length array, bisa pakai [...]
		var months = [...]string{
			"Januari",
			"Februari",
			"Maret",
			"April",
			"Mei",
			"Juni",
			"Juli",
			"Agustus",
			"September",
			"Oktober",
			"November",
			"Desember",
		}
		fmt.Println("===Array awal===")
		fmt.Println(months)
		// slice		

		// append
		// append menambah data baru pada posisi terakhir slice.
		// Jika kapasitas sudah penuh, maka akan membuat array baru

		// kapasitas sudah penuh
		fmt.Println("===Array jika append dengan kapasitas slice sudah penuh===")
		slice1 := months[4:]
		slice2 := append(slice1, "Bulan Baru")
		// months tidak berubah
		fmt.Println(months) 
		fmt.Println(slice1) 
		fmt.Println(slice2)

		// kapasitas belum penuh
		fmt.Println("===Array jika append dengan kapasitas slice belum penuh===")
		slice3 := months[4:7]
		slice4 := append(slice3, "Bulan Baru 2")
		// bulan Agustus pada array akan berubah menjadi "Bulan Baru 2"
		fmt.Println(months) 
		fmt.Println(slice3) 
		fmt.Println(slice4)
	}

	```
- Hasil:
	```go
	===Array awal=== 
	[Januari Februari Maret April Mei Juni Juli Agustus September Oktober November Desember]

	===Array jika append dengan kapasitas slice sudah penuh===
	[Januari Februari Maret April Mei Juni Juli Agustus September Oktober November Desember]
	[Mei Juni Juli Agustus September Oktober November Desember]
	[Mei Juni Juli Agustus September Oktober November Desember Bulan Baru]
	
	===Array jika append dengan kapasitas slice belum penuh===
	[Januari Februari Maret April Mei Juni Juli Bulan Baru 2 September Oktober November Desember]
	[Mei Juni Juli]
	[Mei Juni Juli Bulan Baru 2]
	```
- Membuat slice dari awal, Men-copy slice, dan kemiripan array dengan slice:
	```go
	package main

	import "fmt"

	func main() {
		// membuat slice cara 1
		name := make([]string, 2, 5)
		name[0] = "Anto"
		name[1] = "Kewer"
		fmt.Println(name)

		// copy slice
		name2 := make([]string, len(name), cap(name))
		copy(name2, name)
		fmt.Println(name2)

		// Syntax pembuatan array dan slice itu mirip. jangan sampai ketuker
		// array
		address1 := [...]string{"Klodran", "Karanganyar"}
		fmt.Println(address1)
		// slice
		address2 := []string{"Pasar Kliwon", "Surakarta"}
		fmt.Println(address2)
	}

	```
- Hasil:
	```go
	[Anto Kewer]
	[Anto Kewer]
	[Klodran Karanganyar]
	[Pasar Kliwon Surakarta]
	```
## 15. Type Data Map
- Pasangan key-value
- Code:
	```go
	package main

	import "fmt"

	func main() {
		// cara 1 
		person := map[string]string{
			"name" : "Anto",
			"address" : "Solo",
		}
		fmt.Println(person)
		fmt.Println(person["name"])

		// cara 2
		var book map[string]string = make(map[string]string)
		book["title"] = "Judulnya"
		book["author"] = "Pengarangnya"
		book["salah"] = "Ups"
		fmt.Println(book)
		
		// hapus isi
		delete(book, "salah")
		fmt.Println(book)	
	}
	```
- Hasil:
	```go
	map[address:Solo name:Anto]
	Anto
	map[author:Pengarangnya salah:Ups title:Judulnya]
	map[author:Pengarangnya title:Judulnya]
	```

## 16. If statement
```go
package main

import "fmt"

func main() {
	name := "Anto"
	if name == "Anto"{
		fmt.Println("Betul")
	}else{
		fmt.Println("Salah")
	}

	//shorthand if
	if length :=len(name); length == 4{
		fmt.Println("betul")
	}else{
		fmt.Println("salah")
	}
}
```

## 17. Switch Expression
```go
package main

import "fmt"

func main() {
	name := "Anto"

	// syntax
	switch name {
	case "Anto":
		fmt.Println("Hallo Anto")
	case "Jajang":
		fmt.Println("Hallo Jajang")
	default:
		fmt.Println("Boleh kenalan nggak?")
	}

	// short statement
	switch length := len(name); length == 4 {
	case true:
		fmt.Println("Betul")
	case false:
		fmt.Println("Salah")
	}

	// kita bisa membuat switch tanpa varaiable expresi

	length := len(name)
	switch {
	case length > 10:
		fmt.Println("Nama terlalu panjang")
	case length > 5:
		fmt.Println("Nama sedang")
	case length > 3:
		fmt.Println("Nama pendek")
	default:
		fmt.Println("Nama tidak terdefinisi")
	}
}
```
## 18. For Loops
- Di golang cuma ada 1 looping; yaitu for.
```go
package main

import "fmt"

func main() {
	counter := 1

	// 1
	for counter <= 5{
		fmt.Println(counter)
		counter++
	}
	fmt.Println("=====================")

	// 2
	for i:=1; i <= 4; i++{
		fmt.Println("heu")
	}
	fmt.Println("=====================")

	// 3. Iterate slice/array
	slice := []string{"Anto", "Kewer"}
	for i:=0; i <len(slice); i++{
		fmt.Println(slice[i])
	}
	fmt.Println("=====================")

	// 4. Iterate slice/array (for range)
	for index, value := range slice{
		fmt.Println(index, value)
	}
	fmt.Println("=====================")
	// 4.1. Iterate slice/array (for range tanpa index)
	for _, value := range slice{
		fmt.Println(value)
	}
	fmt.Println("=====================")

	// 5. iterate map
	person := map[string]string{
		"name" : "Andi",
		"address" : "Surakarta",
	}

	for _, value := range person{
		fmt.Println(value)
	}
}
```

## 19. Break dan Continue
- Break -> menghentikan semua perulangan
- Continue -> Lanjut ke perulangan berikutnya

## 20, 21, 22, 23,24, 25. Function Dasar
- Code:
	```go
	package main

	import (
		"fmt"
		"strconv"
	)

	// 1. tanpa return
	func sayHai(name string){
		fmt.Println("Hello "+name)
	}

	// 2. dengan return
	// perlu type data pengembalian
	func sayHai_2(name string) string{
		return "Hello "+name
	}

	// 3. multiple return value
	func sayHai_3(name1 string, name2 string) (string, string){
		return name1, name2
	}

	// 4. mengabaikan beberapa multiple return value
	func sayHai_4(firstName string, lastName string) (string, string){
		return firstName, lastName
	}

	// 5. named return value
	func sayHai_5()(firstName string, lastName string){
		firstName="Anto"
		lastName="Kewer"
		return
	}

	// 6. variadic function (bisa menerima varags(variable arguments))
	// bisa menerima lebih dari 1 arguments
	func nilaiRaport(name string, nilai ...int) (string, int) {
		total := 0
		for _, a := range nilai{
			total += a
		}
		
		return name, total/len(nilai)
	}

	func main() {
		sayHai("Anto")

		fmt.Println(sayHai_2("Anto"))

		name1, name2 := sayHai_3("Anto", "Jajang")
		fmt.Println("Hello", name1, name2 )

		firstName, _ := sayHai_4("Andi", "Muktiono")
		fmt.Println(firstName)

		firstName1, _ := sayHai_5()
		fmt.Println(firstName1)

		name, nilai := nilaiRaport("Anto", 100, 90, 89, 90)
		fmt.Println("Nilai raport " + name + " adalah " + strconv.Itoa(nilai))	

		// 6.1 slice dimasukkan pada variadic function
		nilaiRaportBudi := []int{12,34,45,77,90}
		// slice jika dimasukkan pada varags, harus dipecah dengan namaSlice...
		name2, nilai2 := nilaiRaport("Budi", nilaiRaportBudi...)
		fmt.Println("Nilai raport " + name2 + " adalah " + strconv.Itoa(nilai2))
	}
	```
- Hasil:
	```go
	Hello Anto
	Hello Anto
	Hello Anto Jajang
	Andi
	Anto
	Nilai raport Anto adalah 92
	Nilai raport Budi adalah 51
	```

## 26, 27, 28. Function Value, Function as Parameter, Anonymous Function
- Function adalah first class citizen
- Function juga merupakan type data, dan bisa disimpan sebagai variable
- Code:
	```go
	package main

	import (
		"fmt"	
	)

	// 1
	func getName(name string)string{
		return name
	} 

	// 2. Function as Parameter
	func sayHelloWithFilter(name string, filter func(string)string){
		fmt.Println("Hello"+filter(name))
	}

	// 3. Function as Parameter dengan Type Declaration (biar nggak kepanjangan nulisnya)
	type Filter func(string)string
	func sayHelloWithFilter2(name string, filter Filter){
		fmt.Println("Hello "+filter(name))
	}

	func filter(name string)string{
		if name == "Asu"{
			return "..."
		} else{
			return name
		}
	}

	// 4. Anonymous Function
	type Blacklist func(string)bool

	func registerUser(name string, blacklist Blacklist){
		if blacklist(name){
			fmt.Println("You are blocked", name)
		}else{
			fmt.Println("Welcome", name)
		}
	}

	func main() {
		nama := getName
		fmt.Println(nama("Budi"))

		sayHelloWithFilter("Asu", filter)

		sayHelloWithFilter2("Anto", filter)

		// anonymous function
		blacklist1 := func(name string) bool {
			return name == "Anto"
		}
		registerUser("Anto", blacklist1)
		registerUser("Budi", blacklist1)
	}
	```
- Hasil:
	```go
	Budi
	Hello...
	Hello Anto
	You are blocked Anto
	Welcome Budi
	```
## 29. Recursive Function
- Fungsi yang memanggil fungsinya sendiri
- Code:
	```go
	package main

	import (
		"fmt"	
	)

	func factorial(angka int) int {	
		if angka == 1{
			return 1
		}else{	
			return angka * factorial(angka-1)
		}
	}

	func main() {
		fmt.Println(factorial(3))
	}
	```

## 30. Closure
- Kemampuan fungsi untuk berinteraksi dengan data pada scope yang sama

## 31. Defer, Panic, and Recover
1. Defer -> Fungsi yang bisa kita jadwalkan untuk dieksekusi setelah sebuah function lain selesai dieksekusi.
- Defer akan tetap dieksekusi meskipun function sebelumnya error
2. Panic -> Function yang digunakan untuk menghentikan program
- Panic func biasanya dipanggil jika terjadi error. Tetapi Defer tetep berjalan
3. Recover -> function yang digunakan untuk menangkap data panic.
- Dengan recover proses panic akan berhenti, sehingga program akan tetap berjalan.
- Code dibawah error akan terhenti, untuk recover agar tetap dapat catch, taruh pada defer

## 33. Struct
- Struct -> Sebuah template untuk menggabungkan beberapa type data dalam 1 kesatuan
- Data di struct disimpan dalam field
- Sederhananya, struct adalah kumpulan field
- Struct adalah template data, jadi tidak bisa digunakan langsung.
- jadi kita perlu membuat data/object dari struct yang sudah ada terlebih dahulu
- Code:
	```go
	package main

	import (
		"fmt"	
	)

	type Customer struct {
		Name, Address string
		Age			  int
	}

	func main() {
		// buat cara 1
		var eko Customer
		eko.Name = "Eko Kurniawan"
		eko.Address = "Solo"
		eko.Age = 25

		// buat cara 2
		anto := Customer{
			Name: "Anto",
			Address: "Jakarta",
			Age: 23,
		}

		// buat cara 3
		budi := Customer{"Budi", "Bali", 20}

		fmt.Println(anto)
		fmt.Println(budi)
	}
	```
- Hasil
	```go
	{Anto Jakarta 23}
	{Budi Bali 20}
	```

## 34. Struct Method
- Struct adalah type data seperti type data lainnya, dia bisa digunakan sebagai parameter pada function
- Namun jika kita ingin menambahkan method ke dalam struct, sehingga seolah-olah sebuah struct memiliki function
- Code:
	```go
	package main

	import (
		"fmt"	
	)

	type Customer struct {
		Name, Address string
		Age			  int
	}

	// fungsi yang ada struct
	func (customer Customer) sayHello()  {
		fmt.Println("Hello", customer.Name)
	}

	func main() {
		// buat cara 1
		var eko Customer
		eko.Name = "Eko Kurniawan"
		eko.Address = "Solo"
		eko.Age = 25

		// cara manggilnya kayak si struct punya method
		eko.sayHello()
	}

	```
- Hasil:
	```go
	Hello Eko Kurniawan
	```

## 35. Interface
- Interface adalah type data abstract. dia tidak memiliki implementasi langsung.
- Sebuah interface berisi deklarasi method-method
- Biasa digunakan sebagai kontrak
- Setiap data yang sesuai dengan kontrak interface, secara otomatis dianggap sebagai interface tersebut
- Sehingga tidak perlu mengimplementasikan interface secara manual
- Hal ini berbeda dengan bahasa pemrograman lain yang ketika membuat interface, kita harus menyebut secara eksplisit akan menggunakan interface yang mana.
- Code:
	```go
	package main

	import (
		"fmt"	
	)

	// ini interface
	type HasName interface {
		getName() string
		getAge() int
	}

	// ini function dengan kontrak interface hasName
	// kalau mau pakai function ini, harus mengimplementasikan interface hasName
	func sayHello(hasName HasName){
		fmt.Println("Hello", hasName.getName())
	}

	// struct

	// struct ini ingin mengimplementasikan interface HasName. Harus punya getName dan getAge pada struct-nya
	type Person struct {
		Name string
		Age int
	}

	// struct function 1
	func (person Person) getName() string{
		return person.Name
	}
	// struct function 2
	func (person Person) getAge() int{
		return person.Age
	}

	func main() {

		anto := Person {
			Name: "Anto",
			Age: 23,
		}

		sayHello(anto)
	}
	```
- Hasil:
	```go
	Hello Anto
	```

## 36. Interface Kosong
- Go-lang bukan bahasa pemrograman OOP
- interface kosong -> interface yang tidak memiliki deklarasi method satupun, hal ini membuat secara otomatis semua tipe data akan menjadi implementasinya.
- Interface kosong biasa digunakan kalau semua type data bisa diterima pada parameter tersebut.
- Code:
	```go
	package main

	import (
		"fmt"	
	)

	// disini type data returnnya bisa macam-macam
	func Ups(i int) interface{} {
		if i == 1{
			return 1
		} else if i == 2 {
			return true
		} else{
			return "Pog"
		}
	}

	func main() {

		random := Ups(2)

		var random2 interface{} = Ups(59559)

		fmt.Println(random)
		fmt.Println(random2)
	}
	```
- Hasil
	```go
	true
	Pog
	```

## 37. Nil
- Biasanya di dalam bahasa pemrograman lain, object yang belum diinisialisasi maka secara otomatis nilainya adalah null atau nil
- Berbeda dengan Go-Lang, di Go-Lang saat kita buat variable dengan tipe data tertentu, maka secara otomatis akan dibuatkan default value nya
- Namun di Go-Lang ada data nil, yaitu data kosong
- Nil sendiri hanya bisa digunakan di beberapa tipe data, seperti interface, function, map, slice, pointer dan channel
- Code:
	```go
	package main

	import (
		"fmt"	
	)

	func newMap(name string) map[string]string{
		if name == ""{
			return nil
		} else {
			return map[string]string{
				"name":name,
			}
		}	
	}

	func main() {

		kosong := newMap("")
		fmt.Println(kosong)
	}
	```
- Hasil:
	```go
	map[]
	```

## 38. Error Interface
- Go-Lang memiliki interface yang digunakan sebagai kontrak untuk membuat error, nama interface nya adalah error
- Untuk membuat error, kita tidak perlu manual.
- Go-Lang sudah menyediakan library untuk membuat helper secara mudah, yang terdapat di package errors (Package akan kita bahas secara detail di materi tersendiri)
- Code:
	```go
	package main

	import (
		"fmt"
		"errors"	
	)

	func pembagian(nilai int, pembagi int) (int, error) {
		if pembagi == 0{
			return 0, errors.New("Pembagi tidak boleh 0")
		} else{
			return nilai/pembagi, nil
		}
	}

	func main() {
		hasil, err := pembagian(2,0)

		if err == nil {
			fmt.Println(hasil)
		}else{
			fmt.Println(err)
		}

	}
	```

## 39. Type Assertions
- Mengubah type data menjadi type data yang kita inginkan
- Sering digunakan ketika bertemu dengan interface kosong
	```go
	package main

	import (
		"fmt"

	)

	func random() interface{} {
		return "Ups"
	}

	func main() {
		var result interface{} = random()
		// disini type assertionnya
		var resultString string = result.(string)
		fmt.Println(resultString)
	}
	```
- Saat salah menggunakan type assertions, maka bisa berakibat terjadi panic di aplikasi kita 
- Jika panic dan tidak ter-recover, maka otomatis program kita akan mati 
- Agar lebih aman, sebaiknya kita menggunakan switch expression untuk melakukan type assertions
	```go
	package main

	import (
		"fmt"

	)

	func random() interface{} {
		return 3
	}

	func main() {
		var result interface{} = random()
		// disini type assertionnya
		switch value := result.(type) {
		case int:
			fmt.Println("Value", value, "is integer")
		case string:
			fmt.Println("Value", value, "is string")
		default:
			fmt.Println("Unkown type")		
		}
	}
	```
## 40. Pointer
![](go/1.png?raw=true)
#### Pass by Value
- Secara default di Go-Lang semua variable itu di passing by value, bukan by reference
- Artinya, jika kita mengirim sebuah variable ke dalam function, method atau variable lain, sebenarnya yang dikirim adalah duplikasi value nya
#### Pointer
- Pointer adalah kemampuan membuat reference ke lokasi data di memory yang sama, tanpa menduplikasi data yang sudah ada
- Sederhananya, dengan kemampuan pointer, kita bisa membuat pass by reference
#### Operator &
- Untuk membuat sebuah variable dengan nilai pointer ke variable yang lain, kita bisa menggunakan operator & diikuti dengan nama variable nya
- Code:
	```go
	package main

	import (
		"fmt"

	)

	type Names struct {
		firstName, lastName string
	}

	func main() {
		// pas by value (dicopy paste)
		name1 := Names{"Muhammad Hammam", "Islami"}
		name2 := name1

		name2.firstName = "Anto"
		
		fmt.Println(name1)
		fmt.Println(name2)

		// pas by reference
		var name3 Names = Names{"Nurgiyatna", "Nurgiyatna"}
		// pakai & untuk menadakan bahwa dia pointer menuju name3
		// tanda * Memberi tahu kalau variable yang akan dideklarasikan adalah pointer
		var name4 *Names = &name3

		name4.firstName = "Anto"
		
		fmt.Println(name3)
		fmt.Println(name4)	
	}
	```
- Hasil:
	```go
	{Muhammad Hammam Islami}
	// pas by value
	{Anto Islami}
	{Anto Nurgiyatna}
	// pas by reference
	// ada tanda & yang menandakan dia pointer
	&{Anto Nurgiyatna}
	```
#### Operator *
- Ada behaviour unik ketika me-assign ulang variable pointer.
- Saat kita me-assign ulang variable pointer, sebenarnya kita membuat data baru di dalam memory. Sehingga data yang berubah hanya data yang kita assign
- Code:
	```go
	package main

	import (
		"fmt"

	)

	type Names struct {
		firstName, lastName string
	}

	func main() {	
		name1 := Names{"Andi", "Raharjo"}	
		var name2 *Names = &name1
		fmt.Println(name1)
		fmt.Println(name2)	
		// lihat disini
		name2 = &Names{"Anto", "Kewer"}		
		fmt.Println(name2)	
	}
	```
- Hasil:
	```go
	{Andi Raharjo}
	&{Andi Raharjo}
	// data pointer berubah
	&{Anto Kewer}
	```
- Jika kita ingin mengubah seluruh variable yang mengacu ke data tersebut, kita bisa menggunakan operator *
- Code:
	```go
	package main

	import (
		"fmt"

	)

	type Names struct {
		firstName, lastName string
	}

	func main() {	
		name1 := Names{"Andi", "Raharjo"}	
		var name2 *Names = &name1
		// lihat disini
		*name2 = Names{"Anto", "Kewer"}		
		fmt.Println(name1)	
		fmt.Println(name2)	
	}


	```
- Hasil:
	```go
	{Anto Kewer}
	&{Anto Kewer}
	```
#### Function New
- Sebelumnya untuk membuat pointer dengan menggunakan operator &
- Go-Lang juga memiliki function new yang bisa digunakan untuk membuat pointer
- Namun function new hanya mengembalikan pointer ke data kosong, artinya tidak ada data awal
	```go
	type Names struct {
		firstName, lastName string
	}

	func main() {	
		name1 := new(Names)
		name1.firstName = "Andi"
		fmt.Println(name1)	
	}
	```
## 41. Pointer di Function
- Saat kita membuat parameter di function, secara default adalah pass by value, artinya data akan di copy lalu dikirim ke function tersebut
- Oleh karena itu, jika kita mengubah data di dalam function, data yang aslinya tidak akan pernah berubah. 
- Hal ini membuat variable menjadi aman, karena tidak akan bisa diubah
- Namun kadang kita ingin membuat function yang bisa mengubah data asli parameter tersebut
- Untuk melakukan ini, kita juga bisa menggunakan pointer di function
- Untuk menjadikan sebuah parameter sebagai pointer, kita bisa menggunakan operator * di parameternya
- Code:
	```go
	package main

	import (
		"fmt"

	)

	type Names struct {
		firstName, lastName string
	}

	// menandakan parameter harus pointer
	func ChangeLastNameToEmpty(name *Names){
		name.lastName = ""
	}

	func main() {	
		name1 := Names{firstName: "Anto", lastName: "Kewer",}
		// tambahkan & yang menandakan dia pointer
		ChangeLastNameToEmpty(&name1)
		fmt.Println(name1)	
	}
	```
- Hasil:
	```go
	{Anto }
	```

## 42. Pointer di Method
- Walaupun method akan menempel di struct, tapi sebenarnya data struct yang diakses di dalam method adalah pass by value
- Sangat direkomendasikan menggunakan pointer di method, sehingga tidak boros memory karena harus selalu diduplikasi ketika memanggil method
- Code:
	```go
	package main

	import (
		"fmt"

	)

	type Names struct {
		firstName, lastName string
	}

	// menandakan parameter harus pointer
	func (name *Names) ChangeLastNameToEmpty(){
		name.lastName = ""
	}

	func main() {	
		name1 := Names{firstName: "Anto", lastName: "Kewer",}
		// lihat disini
		name1.ChangeLastNameToEmpty()
		fmt.Println(name1)	
	}
	```
- Hasil:
	```go
	{Anto }
	```

## 43. GOPATH
- cek go environment: `go env`

## 43. Package dan Import
#### Package
- Package adalah tempat yang bisa digunakan untuk mengorganisir kode program yang kita buat di Go-Lang
- Dengan menggunakan package, kita bisa merapikan kode program yang kita buat
- Package sendiri sebenarnya hanya direktori folder di sistem operasi kita
- Di folder golang-dasar/helpers, buat file sayHello.go
	```go
	// namai package
	package helpers

	// harus Upper Case agar function dapat diimport
	func SayHello(name string) string{
		return "Hello " + name
	}
	```
#### Import
```go
// untuk menandai dia main, beri nama package dengan main
package main

import (
	"fmt"
	"golang-dasar/helpers"
)

func main() {		
	fmt.Println(helpers.SayHello("Anto"))
}

```










