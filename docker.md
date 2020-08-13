# Docker

## 1. Pengenalan Docker
![](img_docker/1.png?raw=true)

## 2. Virtual Machine v.s Container
![](img_docker/2.png?raw=true)

## 3. Instalasi
- https://docs.docker.com/engine/install/ubuntu/#installation-methods
- In mint : https://computingforgeeks.com/install-docker-and-docker-compose-on-linux-mint-19/

## 4. Arsitektur Docker
![](img_docker/3.png?raw=true)
- Ketika install docker, kita sebenarnya menginstall 2 aplikasi, docker client dan docker server/DOCKER_HOST.
- Why? Pada kenyataannya, yang digunakan di laptop kita cuma docker client. Sedangkan docker client diinstall di docker server.
    - docker client --> cuma terminal untuk memberi perintah ke docker server
    - docker server --> guna : melakukan management container. Secara lebih detail, dia akan mengatur container, image, dan juga terkoneksi ke registry container.

## 5. Container Registry
- Adalah tempat yang digunakan untuk menyimpan image docker sebelum kita deploy ke tiap server dockernya.
- Kenapa nggak langsung ke server dockernya?
- Contoh Container Registry :
    1. Docker Hub : https://hub.docker.com/
    2. Google Container Registry : https://cloud.google.com/container-registry
    3. AWS Elastic Container Registry : https://aws.amazon.com/ecr/

## 6. Images
- Hasil dari distribution filenya/package/bundle dari aplikasi kita yang akan kita deploy ke registry.
- Nggak kayak installer, ini kaya aplikasi yang udah jadi yang cuma tinggal di run aja tanpa install.
- Di docker hub, udah banyak image2 yang kita bisa pakai dengan gratis tanpa perlu kita membuat imagenya lagi. Seperti web server(apache), daatabase(mysql, postgre).
## 7. Container
- adalah image yang kita running/ hasil instansiasi dari image. Dan didalam docker kita bisa running beberapa container dari image yang sama.

## 8. Mengambil Image dari Registry
```js
// melihat daftar image yang ada
$ docker images
// download image mongo dengan versi 4.1, jika versi tidak disebutkan maka otomatis akan mendonload latest 
$ docker pull mongo:4.1
```
## 9. Membuat Container
```js
// melihat container yang sedang jalan
$ sudo docker container ls
// melihat semua container yang ada
$ sudo docker container ls --all
// membuat container dari image yang sudah didownload
// --name digunakan untuk memberi nama pada container, disini diberi nama mongoserver1
$ sudo docker container create --name mongoserver1  mongo:4.1
```
## 10. Menjalankan Container
```js
// menjalankan container
$ sudo docker container start mongoserver1
```
- Setelah container dibuat, ketika menjalankan `sudo docker container ls`, maka akan terdapat port untuk mongoserver1 yaitu port 27017. Tapi level akses port ini hanya sebatas pada container ini saja, agar dapat diakses dari luar dibutuhkan tambahan step yag akan dijelaskan nanti.

## 11. Menghapus Container
```js
// menghentikan container
$ sudo docker stop mongoserver1
// menghapus container
$ sudo docker rm mongoserver1
```
## 12. Membuka Port untuk Container
```js
// 8080 -> port jika ingin diakses dari luar
// 27017 -> port didalam container
$ sudo docker container create --name mongoserver1 -p 8080:27017  mongo:4.1
```
## 13. Menghapus Docker Image
- Untuk menghapus image, kita juga harus menghapus container yang menggunakan image tersebut.
```js
$ sudo docker image rm mongo:4.1
```
## 14. Membuat Image dengan Dockerfile
- Misal ketika ingin membuat aplikasi dengan golang, dibanding menginstall go lalu build imagenya, mending pake image go yang sudah ada di registry docker hub.
- Steps :
    1. buat main.go :
        ```go
        package main

        import (
            "fmt"
            "net/http"
        )

        func main() {
            http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
                fmt.Fprintf(w, "Hello World!")
            })

            http.ListenAndServe(":8080", nil)
        }
        ```
    2. buat Dockerfile :
        ```docker
        <!-- menggunakan golang image dari docker hub -->
        FROM golang:1.11.4

        <!-- copy semua file aplikasi kita ke image yang ingin dibuat -->
        COPY main.go /app/main.go

        <!-- memberitahu image cara run aplikasinya -->
        CMD ["go", "run", "/app/main.go"]
        ```
    3. `docker build --tag app-golang:1.0 .`
- Run image yang sudah dibuat :
    ```
    $ sudo docker images
    $ sudo docker container create --name app1 -p 8080:8080  app-golang:1.0
    $ sudo docker container start app1
    ``` 
    - buka : http://localhost:8080/

    
