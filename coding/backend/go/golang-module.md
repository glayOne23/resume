# Golang Module

## 1. Membuat Module
- Untuk mengambil library/dependency dari project lain
- Untuk membuat module baru, kita bisa menggunakan perintah : `go mod init nama-module`
- Go-Lang akan secara otomatis membuat file go.mod yang berisikan informasi nama module dan juga versi Go-Lang yang kita gunakan
## 2. Rilis Module
- Go-Lang terintegrasi baik dengan Git
- Untuk merilis module, kita hanya perlu membuat Tag di Git
- Alur rilis module:
    1. bikin akun github
    2. seusaikan nama module dengan url github
    3. bikin tag    
        - git tag v1.0.0
        - git push origin v1.0.0    
## 3. Menambah Dependency
- `go get nama-module`
- exp: `go get github.com/ProgrammerZamanNow/go-say-hello`
## 4. Upgrade Module
- Buat tag baru di Git
## 5. Upgrade Dependency
- Untuk upgrade dependency ke versi terbaru, kita bisa mengubah isi go.mod, lalu mengubah tag nya menjadi tag terbaru
- Untuk mendownload versi terbaru, gunakan perintah : go get
- Code utama:
    ```go
    module github.com/glayOne23/app-say-hello

    go 1.16

    require github.com/ProgrammerZamanNow/go-say-hello v1.5.0 // indirect
    ```
    ```go
    package main

    import (
        "fmt"
        "github.com/ProgrammerZamanNow/go-say-hello"
    )

    func main()  {
        fmt.Println(go_say_hello.SayHello())

    }
    ```    
- Code dependency
    ```go
    module github.com/ProgrammerZamanNow/go-say-hello

    go 1.15
    ```
    ```go
    package go_say_hello

    func SayHello() string {
        return "Hello World"
    }
    ```
## Major Upgrade
- Major upgrade biasanya terjadi dikarenakan ada perubahan pada isi kode program kita, sehingga membuatnya tidak backward compatible
- Sebaiknya hal ini sebisa mungkin dihindari
- Namun jika tidak bisa dihindari, strategy terbaik adalah merubah nama module
- Langkah-langkah:
1. `go get github.com/ProgrammerZamanNow/go-say-hello/v2`
2. Code:
- Code utama:
    ```go
    module github.com/glayOne23/app-say-hello

    go 1.16

    require github.com/ProgrammerZamanNow/go-say-hello/v2 v2.0.0 // indirect
    ```
    ```go
    package main

    import (
        "fmt"
        "github.com/ProgrammerZamanNow/go-say-hello/v2"
    )

    func main()  {
        fmt.Println(go_say_hello.SayHello("Anto"))

    }
    ```
- Code dependency:
    ```go
    module github.com/ProgrammerZamanNow/go-say-hello/v2

    go 1.15
    ```
    ```go
    package go_say_hello

    func SayHello(name string) string {
        return "Hello " + name
    }
    ```





    
