# Docker
- Part 1:
    - Slide : https://docs.google.com/presentation/d/1LoCIoqR68t-y7P7eOs_TVoooZy4mq-tc2cwInQAtfy0/edit?usp=sharing
    - Source Code : https://github.com/ProgrammerZamanNow/belajar-docker-dasar
- Part 2:
    - Slide : https://docs.google.com/presentation/d/1bW0-88g_s54-X_rBLaZ-N2EhW3HngAZSN6UInyBlIn8/edit?usp=sharing
    - Source Code : https://github.com/ProgrammerZamanNow/belajar-docker-dockerfile
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
- Why? Pada kenyataannya, yang digunakan di laptop kita cuma docker client. Sedangkan docker server diinstall di server production.
    - docker client --> cuma terminal untuk memberi perintah ke docker server
    - docker server --> guna : melakukan management container. Secara lebih detail, dia akan mengatur container, image, dan juga terkoneksi ke registry container.

## 5. Container Registry
- Adalah tempat yang digunakan untuk menyimpan image docker sebelum kita deploy ke tiap server dockernya.
- Contoh Container Registry :
    1. Docker Hub : https://hub.docker.com/
    2. Google Container Registry : https://cloud.google.com/container-registry
    3. AWS Elastic Container Registry : https://aws.amazon.com/ecr/

## 6. Images
- Hasil dari distribution filenya/package/bundle dari aplikasi kita yang akan kita deploy ke registry.
- Nggak kayak installer, ini kaya aplikasi yang udah jadi yang cuma tinggal di run aja tanpa install.
- Di docker hub, udah banyak image2 yang kita bisa pakai dengan gratis tanpa perlu kita membuat imagenya lagi. Seperti web server(apache), daatabase(mysql, postgre).
## 7. Container
- adalah hasil instansiasi dari image. Dan didalam docker kita bisa running beberapa container dari image yang sama.

## 8. Mengambil Image dari Registry
```js
// melihat daftar image yang ada
$ docker images
// download image mongo dengan versi 4.1, jika versi tidak disebutkan maka otomatis akan mendonload latest 
$ docker pull mongo:4.1
```
## 9. Menghapus Docker Image
- Untuk menghapus image, kita juga harus menghapus container yang menggunakan image tersebut.
```js
$ sudo docker image rm mongo:4.1
```
## 10. Membuat Container
```js
// melihat container yang sedang jalan
$ sudo docker container ls
// melihat semua container yang ada
$ sudo docker container ls --all
// membuat container dari image yang sudah didownload
// --name digunakan untuk memberi nama pada container, disini diberi nama mongoserver1
$ sudo docker container create --name mongoserver1  mongo:4.1
```
## 11. Menjalankan Container
```js
// menjalankan container
$ sudo docker container start mongoserver1
```
- Setelah container dibuat, ketika menjalankan `sudo docker container ls`, maka akan terdapat port untuk mongoserver1 yaitu port 27017. Tapi level akses port ini hanya sebatas pada container ini saja, agar dapat diakses dari luar container dibutuhkan tambahan step yag akan dijelaskan nanti (perlu diekxpose).

## 12. Menghapus Container
```js
// menghentikan container
$ sudo docker stop mongoserver1
// menghentikan 2 container sekaligus
$ sudo docker stop mongoserver1 mongoserver2
// menghapus container (ini juga dapat dihapus sekaligus)
$ sudo docker rm mongoserver1
```
## 13. Container Log
```js
// melihat log di container
$ sudo docker logs mongoserver1
// melihat log secara realtime
$ sudo docker logs -f mongoserver1
```
## 14. Container Exec
- Guna: mengeksekusi program yang ada di containernya
```js
$ sudo docker container exec -i -t mongoserver1 /bin/bash
// -i -> argumen interaktif, menjaga input tetap aktif
// -t -> argumen untuk alokasi pseudo-TTY (terminal akses)
```
## 15. Membuka Port untuk Container (Port Forwarding)
- Guna agar dapat diakses dari luar container
- Harus dilakukan ketika membuat containernya, jika sudah dibuat, maka tidak bisa dibuka
```js
// 8080 -> port jika ingin diakses dari luar
// 27017 -> port didalam container
// -p -> untuk melakukan port forwarding
// --publish == -p
$ sudo docker container create --name ngix1 -p 8080:80  nginx:latest
```
## 16. Container Environment Variable
- Adalah konfigurasi global yang berubah-ubah sesuai peletakkan program (mirip .env di Laravel  atau .settings di Django)
- Guna: bisa mengubah-ubah konfigurasi aplikasi, tanpa harus mengubah kode aplikasinya lagi
- Contoh: username dan password mysql, seperti .env di laravel
```js
$ sudo docker container  create --name mongoserver1 -p 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=anto --env MONGO_INITDB_ROOT_PASSWORD=kewer mongo:latest
```
## 17. Container Stats
- Saat menjalankan beberapa container, di sistem Host, penggunaan resource seperti CPU dan Memory hanya terlihat digunakan oleh Docker saja
- Kadang kita ingin melihat detail dari penggunaan resource untuk tiap container nya
- Untungnya docker memiliki kemampuan untuk melihat penggunaan resource dari tiap container yang sedang berjalan
```js
$ sudo docker container stats
```
## 18. Container Resources Limit
- Saat membuat container, secara default dia akan menggunakan semua CPU dan Memory yang diberikan ke Docker (Mac dan Windows), dan akan menggunakan semua CPU dan Memory yang tersedia di sistem Host (Linux)
- Jika terjadi kesalahan, misal container terlalu banyak memakan CPU dan Memory, maka bisa berdampak terhadap performa container lain, atau bahkan ke sistem host
- Oleh karena itu, ada baiknya ketika kita membuat container, kita memberikan resource limit terhadap container nya
    ### 18.1. Memory
    - Saat membuat container, kita bisa menentukan jumlah memory yang bisa digunakan oleh container ini, dengan menggunakan perintah --memory diikuti dengan angka memory yang diperbolehkan untuk digunakan
    - Kita bisa menambahkan ukuran dalam bentu b (bytes), k (kilo bytes), m (mega bytes), atau g (giga bytes), misal 100m artinya 100 mega bytes
    ### 18.2. CPU
    - Selain mengatur Memory, kita juga bisa menentukan berapa jumlah CPU yang bisa digunakan oleh container dengan parameter --cpus
    - Jika misal kita set dengan nilai 1.5, artinya container bisa menggunakan satu dan setengah CPU core
    ```js    
    $ sudo docker container create --name smallnginx -p 8081:80 --memory 100m --cpus 0.5 nginx:latest
    // memorynya limit 100 mb
    // cpunya limit 1/2 cpu
    ```
## 19. Bind Mounts
- Bind Mounts merupakan kemampuan melakukan mounting (sharing) file atau folder yang terdapat di sistem host ke container yang terdapat di docker
- Guna: mengirim konfigurasi dari luar container, atau misal menyimpan data yang dibuat di aplikasi di dalam container ke dalam folder di sistem host
- Untuk melakukan mounting, kita bisa menggunakan parameter --mount ketika membuat container
- Isi dari parameter --mount memiliki aturan tersendiri
    |Parameter  |Keterangan                        
    |-----------|----------------------------------
    |type       |Tipe mount, bind atau volume
    |source     |Lokasi file atau folder di sistem host
    |destination|Lokasi file atau folder di container
    readonly    |Jika ada, maka file atau folder hanya bisa dibaca di container, tidak bisa ditulis

```js
$ sudo docker container create --name mongodata --mount "type=bind, source=/home/btiums/projects/django/docker/mongo-data,destination=/data/db" -p 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=anto --env MONGO_INITDB_ROOT_PASSWORD=kewer mongo:latest
```
## 20. Docker Volume
- Fitur Bind Mounts sudah ada sejak Docker versi awal, di versi terbaru direkomendasikan menggunakan Docker Volume
- Docker Volume mirip dengan Bind Mounts, bedanya adalah terdapat management Volume, dimana kita bisa membuat Volume, melihat daftar Volume, dan menghapus Volume
- Volume sendiri bisa dianggap storage yang digunakan untuk menyimpan data, bedanya dengan Bind Mounts, pada bind mounts, data disimpan pada sistem host, sedangkan pada volume, data di manage oleh Docker
- Saat kita membuat container, dimanakah data di dalam container itu disimpan? secara default semua data container disimpan di dalam volume
- Jika kita coba melihat docker volume, kita akan lihat bahwa ada banyak volume yang sudah terbuat, walaupun kita belum pernah membuatnya sama sekali
```js
// melihat semua volume
$ docker volume ls
// membuat volume
$ docker volume create mongo-volume
// menghapus volume
$ docker volume rm mongo-volume
```
## 21. Container Volume
- Volume yang sudah kita buat, bisa kita gunakan di container
- Keuntungan menggunakan volume adalah, jika container kita hapus, data akan tetap aman di volume
- Cara menggunakan volume di container sama dengan menggunakan bind mount, kita bisa menggunakan parameter --mount, namun dengan menggunakan type volume dan source nama volume

```js
// membuat volume
$ sudo docker volume create mongodata
// membuat container yang datanya ditaruh di volume yang kita buat
$ $ sudo docker container create --name mongo1 --mount "type=volume, source=mongodata,destination=/data/db" -p 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=anto --env MONGO_INITDB_ROOT_PASSWORD=kewer mongo:latest
```
## 22. Backup Volume
## 23. Restore Volume
## 24. Docker Network



---
## Mungkin Akan Tidak Terpakai:
## 14. Membuat Image dengan Dockerfile (Cara Membuat Image Sendiri)
- Untuk membuat image, kita harus belajar membuat Dockerfile.
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
        <!-- /app adalah folder didalam image -->
        COPY main.go /app/main.go

        <!-- memberitahu image cara run aplikasinya -->
        <!-- aslinya go run /app/main.go -->
        CMD ["go", "run", "/app/main.go"]
        ```
    3. `docker build --tag app-golang:1.0 .`
- Run image yang sudah dibuat :
    ```js
    $ sudo docker images
    $ sudo docker container create --name app1 -p 8080:8080  app-golang:1.0
    $ sudo docker container start app1
    ``` 
    - buka : http://localhost:8080/    
## 15. Mengupload Image ke Registry
1. masuk https://hub.docker.com/ -> Create a Repository
2. Isi name dan visibility
3. lihat sub menu `Tags` untuk command membuat image baru dari lokal (agar terdapat reponamenya)
4. `sudo docker login`
5. push sesuai dengan Docker Command
## 17. Integrasi Container dengan Network (Integrasi Antar Beberapa Container)
- Pada dasarnya, antar container itu terisolasi sendiri-sendiri, agar dapat terhubung, maka kita harus membuat container-container tersebut dalam Network yang sama.
- Cara: buat Network lalu masukkan container-container dalam Network yang sama
    ```js
    // buat Network 
    $ sudo docker network create nama_network
    // cek network yang ada 
    $ sudo docker network ls
    // menambahkan container-container pada network
    $ sudo docker network connect nama_network nama_container_1
    $ sudo docker network connect nama_network nama_container_2
    $ dst    
    // menghubungkan antar container
    // misal disini menghubungkan Java dengan Mongo dan Redis mongo dan redist host yang biasanya localhost diganti dengan nama container mongo dan redis
    // mongoserver1 -> nama container mongo
    // redis1 -> nama container redis
    $ sudo docker container create --name java_docker -p 8080:8080 -e Name=Javanya -e MONGO_HOST=mongoserver1 -e MONGO_PORT=27017 -e REDIS_HOST=redis1 -e REDIS_PORT=6379 java-docker:1.0
    ```
## 18. Docker Compose
- Secara otomatis membuat container, network, menghubungkan antar container, konfigurasi container, dll
- dalam format yml
- Install docker compose : https://docs.docker.com/compose/install/
- Langkah-langkah:
    1. buat `docker-compose.yml`, isi dengan:
        ```yml
        # versi docker compose yang dipakai
        version: "3.7"

        # container-container yang ingin dibuat
        services:
            # attribute (cuma nama, bebas mau dinamain apa)
            mongo: 
                # nama container (namabebas juga)
                container_name: mongo
                # image di registry
                image: mongo:4-xenial
                # port yang diakses dari luar dan port di dalam
                ports: 
                    - 27017:27017
                # set network
                networks:
                    - java_network
            redis:
                container_name: redis
                image: redis:5
                ports:
                    - 6379:6379
                networks:
                    - java_network            
            java-docker:
                container_name: java-docker
                image: java-docker:1.0
                ports:
                    - 8080:8080    
                # karena java-docker bergantung pada redis dan mongo, maka java-docker akan terinstall setelah redis dan mongo
                depends_on:
                    - redis
                    - mongo
                # global environment untuk java-docker
                environment:
                    - NAME=Docker
                    - MONGO_HOST=mongo
                    - MONGO_PORT=27017
                    - REDIS_HOST=redis
                    - REDIS_PORT=6379
                networks:
                    - java_network
        networks:
            # nama attribute (bebas)
            java_network:
                name: java_network
        ```
    2. Membuat dan menjalankan docker compose:
    ```js
    // up untuk membuat dan menjalakan docker compose
    // flag -d untuk menjalakan di background 
    $ sudo-docker-compose up -d
    // menghentikan dan menghapus docker compose
    $ sudo docker-compose down 
    // menjalakan tanpa membuat
    $ sudo-docker-compose start
    // menghentikan tanpa menghapus
    $ sudo-docker-compose stop
    ```
## 19. Manage Data di Docker
- Guna menyimpan data di luar container: Contoh kasus data database mongodb, jika container dihapus, maka database ikut terhapus, untuk mengatasinya adalah dengan mengatur datanya.
- Ada beberapa macam:
    1. Volumes
    2. Bind Mounts    
### 19.1. Volumes
- is the preferred mechanism for persisting data generated by and used by Docker containers.
- *Catatan*: 
    - Cek dulu di dokumentasi image, di direktori mana data tersebut disimpan.
    - misal: di mongodb data disimpan di direktori /data/db.
- Steps: studi kasus mongo db:
    ```js
    // buat volume
    $ sudo docker volume create mongo_data
    // mongo_data -> nama volume yang sudah dibuat
    // /data/db -> direktori penyimpan data di image mongo
    $ sudo docker create --name=mongoserver1 -v mongo_data:/data/db -p 27017:27017 mongo:4-xenial
    ```
- Maka data tersimpan di volume tersebut dan tidak akan terhapus ketika container dihapus. 
- Di docker, volume dapat di backup, restore, dan migrate data volume: 
https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes
### 19.2. Bind Mounts
- Menaruh data pada direktory lokal komputer. Kalau volume itu disimpan di docker dan tidak bisa dilihat isinya secara lokal, kalau bind mounts, data ada pada lokal dan dapat dilihat isinya dalam format sesuai docker.
- Steps:
    ```js
    // menaruh data ke lokal
    // /home/btiums/Documents/Coba -> path tempat menyimpan datanya
    $ sudo docker create --name=mongoserver1 -v /home/btiums/Documents/CobaMongo:/data/db -p 27017:27017 mongo:4-xenial
    ```
- Rekomendasi menggunakan volumes. Karena untuk backup, restore, dan migrate harus dilakukan secara manual. Tidak ada feature bawaan.
## 21. Kitematic (Hanya Untuk Windows dan Mac)
- Adalah aplikasi UI Dekstop untuk mempermudah membuat container di Docker.
## 22. Membersihkan Sampah Sisa Docker yang Dibuat
```js
// images
// prune untuk menghapus image yang tidak dipakai container
$ sudo docker image prune
// prune untuk menghapus container yang tidak stop
$ sudo docker container prune
// prune untuk menghapus volume yang tidak dipakai 
$ sudo docker volume prune
// prune untuk menghapus network yang tidak dipakai 
$ sudo docker network prune
// menghapus image, container, dan network yang tidak terpakai tanpa menghapus volume
$ sudo docker system prune
// menghapus image, container, volume, dan network yang tidak terpakai
$ sudo docker system prune -a --volumes
```




