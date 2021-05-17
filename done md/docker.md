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
- Setelah container dibuat, ketika menjalankan `sudo docker container ls`, maka akan terdapat port untuk mongoserver1 yaitu port 27017. Tapi level akses port ini hanya sebatas pada container ini saja, agar dapat diakses dari luar container dibutuhkan tambahan step yag akan dijelaskan nanti (perlu diekxpose).

## 11. Menghapus Container
```js
// menghentikan container
$ sudo docker stop mongoserver1
// menghentikan 2 container sekaligus
$ sudo docker stop mongoserver1 mongoserver2
// menghapus container (ini juga dapat dihapus sekaligus)
$ sudo docker rm mongoserver1
```
## 12. Membuka Port untuk Container
- Guna agar dapat diakses dari luar container
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
## 16. Environment Variable di Docker
- Adalah konfigurasi global yang berubah-ubah sesuai peletakkan program (mirip .env di Laravel  atau .settings di Django)
- Ketika membuat container, buat environment variablenya: 
    ```js    
    // ada di flag -e nama_variablenya=isi_variable
    $ sudo docker container create --name app1 -p 8080:8080 -e NAME=Namanya app-golang:1.0    
    // jika lebih dari 1 variable
    $ sudo docker container create --name app1 -p 8080:8080 -e NAME=Namanya -e APP_VERSION=1.0 app-golang:1.0    
    // melihat isi container 
    $ sudo docker container inspect app1
    ``` 
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
## 20. Docker Exec (Menjalakan Command di Container yang Sedang Berjalan)
```js
// -ti -> singkatan dari 2 flag -t dan -i
// -t untuk tty, dan -i untuk interactive
// redis1 -> nama container
// karena container redis pake debian, maka masuk terminalnya menggunakan /bin/bash. Jika menggunakan windows atau linux lain, maka perlu disesuaikan.
// /bin/bash -> masuk ke bash dalam container
$ sudo docker exec -ti redis1 /bin/bash 
```
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




