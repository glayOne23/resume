# Redis

## Slides
- https://docs.google.com/presentation/d/10Ux3S0xJmrqlzffOmi63nQp3qntgfjqr6oI16XY1rtE/edit

## 1. Pengenalan Redis
- Redis -> Remote Dictionary Server -> key-value database system based on memory
- Disimpan di memory(ram) bukan hard disk. Jadi ketika redis sistemnya mati, datanya hilang
### 1.1. Apa itu In-Memory Database?
- Redis menyimpan datanya di memory, misal kapasitas ram 8 giga, ya maksimal segitu, itu pun belum dipotong penggunaan ram untuk sistem
- Kita bisa meminta redis menyimpan datanya secara regular permanent di disk
- Data disk hanya dijadikan `backup` ketika redis berjalan ulang, selama redis berjalan, redis hanya akan melakukan manipulasi data ke memory

## 2. Kapan butuh redis?
1. Ketika database utama lambat 
    - misal join pada postgres banyak 
    - toleransi response 2 detik
2. Ketika aplikasi lain lambat
    - misal request ke aplikasi third partynya banyak
3. Ketika ada proses berat di aplikasi
4. Membuat delayed job
    - Contoh: scalling proses yang asyncronus. Misal php kan defaultnya tidak bisa spawn thread, sehingga tidak bisa async, bisa pakai redis. Job taruh di redis, lalu akan ada scheduler untuk ambil job dari redis

## 3. Menginstall Redis
- Pakai di docker: docker run -d -p 6379:6379 --name personal_dev_redis --network personal_dev redis
### 3.1. Redis Server and Redis CLI
- Redis CLI digunakan untuk memberikan pemerintah ke Redis Server menggunakan command line
- Steps:
    1. docker container exec -it personal_dev_redis /bin/sh
    2. redis-cli -h localhost

## 4. Configuration File
- Jika tidak pakai, maka redis akan menggunakan settingan default. Tapi, ya pasti ada settingan default buat batasi si redis
- Buat:
    - Isi config dan docker compose ada di dir
    - `docker-compose -f docker-compose.yml up -d`

## 5. Database
- Redis memiliki konsep database seperti pada relational database mysql atau postgre
- Di redis kita bisa membuat database dan menggunakan database nya
- Namun sedikit berbeda, jika di relational database kita bisa membuat database dengan menggunakan nama database, di redis kita hanya bisa menggunakan angka sebagai database
- Secara default database di redis adalah 0 (nol)
- Kita bisa menggunakan database sejumlah maksimal sesuai dengan konfigurasi yang kita gunakan di file konfigurasi
    - Defaultnya 16 db:
        ```py
        # Set the number of databases. The default database is DB 0, you can select
        # a different one on a per-connection basis using SELECT <dbid> where
        # dbid is a number between 0 and 'databases'-1
        databases 16
        ```
- Pindah database : `select <no-databasenya>`

## 6. String
- Redis sebenarnya mendukung struktur data yang banyak, seperti String, List, Set, dan lain-lain
- Namun yang paling sering digunakan adalah struktur data String
```py
select 0
set nama "Muhammad Hammam"
get nama
exists nama
append nama " Islami"
# cari key
keys nam*
keys *a
keys *
# get beberapa
mget nama umur
# set beberapa
mset  nama "Muhammad Hammam Islami" umur "22"
```

## 7. Expiration
- Buat expiration data
```py
# key nama akan hilang dalam 60 detik
expire nama 60
setex nama2 120 "Anto Kewer"
# lihat sisa waktu
ttl nama2
```

## 8. Increment dan Decrement
- Operasi Increment & Decrement sekilas sangat mudah dilakukan, hanya tinggal mengupdate data yang di redis dengan data baru (data lama ditambah 1)
- Namun jika operasi dilakukan secara paralel dan dalam waktu yang sangat cepat, hal ini bisa memungkinkan `race condition`
- Untungnya redis memiliki operasi untuk melakukan increment dan decrement
    - Cara Manual:
        ```
        value = GET key
        value = value + 1
        SET key value
        ```
        - Ini bahaya jika data berubah sangat cepat
```py
incr counter
get counter # 1
incr counter # 2
get counter # 2
decr counter # 1
get counter # 1
incrby counter 10 # 11
decrby counter 5 # 6
```

## 9. Flush
- Hapus seluruh data di redis
```py
# hapus semua key yang ada di database tersebut
flushdb
# hapus semua key pada semua database
flushall
```

## 10. Pipeline
- Redis mendukung operasi bulk via pipeline, dimana kita bisa mengirim beberapa perintah sekaligus dalam satu request
- Namun perlu diketahui, server redis tidak akan membalas tiap perintah yang dikirim via pipeline
- Gunanya pengen naruh data yang banyak. Misal dari mysql sebanyak 1 juta data ke redis
- Cara: `cat sets.txt | redis-cli -h localhost --pipe`

## 11. Transaction
- Kayak db transaction di database
```py
multi # awali dengan ini untuk menandakan transaction
set nama "Anto Kewer"
set umur 23
exec # untuk mengeksekusinya

multi 
set anto 2000000
set kewer 1000000
discard # jika anto dan kewer ingin dibatalkan
```

## 12. Monitor
- Guna: untuk memonitor semua request yang masuk ke redis server
- Command:` monitor`

## 13. Server Information
- Melihat informasi jumlah memory yang sudah terpakai, konfigurasi, dll
```py
info
info keyspace
info memory

# melihat semua konfigurasi yang ada di file redis
# config <command>
config get databases

# slowlog <command>
slowlog get # melihat query yang lambat
slowlog get 10
```

## 14. Client Connection
- Redis menyimpan semua informasi client di server
- Hal ini memudahkan kita untuk melihat daftar client, dan juga mengecek jika ada anomali, seperti terlalu banyak koneksi client ke redis
- Misal cek berapa koneksi yang aktif, guna bisa ngasih tahu kalau ada client yang lupa close koneksi
```py
client list # list client
client id # check id client kita
# client kill <ip:port>
client kill 127.0.0.1:6379
```

## 15. Security
- Secara default, ketika kita menyalakan redis server, redis server akan mendengarkan request dari semua network interface. Ini sangat berbahaya, karena bisa jadi redis terekspos secara public
- Namun, redis punya second layer untuk pengecekan koneksi, yaitu mode protected, secara default mode protectednya aktif, artinya walaupun redis bisa diakses dari manapun, tapi redis hanya mau menerima request dari 127.0.0.1 (localhost)

```py
# beri komentar ini jika redis ingin diakses dari semua ip address
bind 127.0.0.1 -::1

# beri komentar ini jika redis tidak pada mode protected, tapi jangan dilakukan karena bahaya, pakai auth aja
protected-mode yes
```
- `Detailnya lihat video tutorial`

## 16. Authentication
- Redis memiliki fitur authentication, dan kita bisa menambahkannya di file konfigurasi di server redis
- Namun perlu diingat, proses authentication di redis itu sangat cepat, jadi pastikan gunakan password sepanjang mungkin agar tidak mudah untuk di brute force 
- Cara: 
    - pada redis.conf:
        ```py
        # user <nama-user> on <hak-akses> ><password>
        user anto on +@all >h453257@4325gfs$
        ```
    - pada server client:
        ```py
        # untuk login menggunakan username password
        auth anto h453257@4325gfs$   
        ```

## 17. Authorization
- Cara:
    - pada redis.conf untuk konfigurasi:
        ```py        
        # untuk lihat semua kategori
        acl cat
        # untuk lihat semua perintah pada kategori
        acl cat read
        # user <nama-user> on <hak-akses> ~<key-yang-diperbolehkan> > <password>
        user anto on +@all ~* >h453257@4325gfs$
        
        ```

## 18. Persistence
- Kita bisa menyimpan data di memory redis tersebut di disk jika kita mau
Namun perlu diingat proses penyimpanan data ke disk redis tidak realtime, dia dilakukan secara scheduler dengan konfigurasi tertentu
- Jadi jangan jadikan redis sebagai media penyimpanan persistence, gunakan redis sebagai database untuk membantu database persistence lainnya
- Di redis.conf:
    ```py
    # save <cek-tiap-berapa-detik> <jumlah-data-yang berubah>
    save 10 1
    # Tapi cara ini single thread, jadi ketika backup, request lain akan menunggu sampai selesai. Tapi biasany Pak Eko tidak melakukan persistance data    
    ```
- Di CLI:
    ```py
    # simpan semua data
    save # simpan secara syncronous
    bgsave # simpan secara asyncronous 
    ```

## 18. Eviction
- Ketika memory redis penuh, maka redis secara default akan mereject semua request penyimpanan data
- Hal ini mungkin menjadi masalah ketika kita hanya menggunakan redis sebagai cache untuk media penyimpanan sementara
- Kadang akan sangat berguna jika memory penuh, redis bisa secara otomatis menghapus data yang sudah jarang digunakan
- Redis mendukung fitur eviction (menghapus data lama, dan menerima data baru)
- Namun untuk mengaktifkan fitur ini, kita perlu memberi tahu redis, maximum memory yang boleh digunakan, dan bagaimana strategi untuk melakukan eviction nya
- di redis.conf
    ```py
    # maxmemory <bytes>

    # MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
    # is reached. You can select one from the following behaviors:
    #
    # volatile-lru -> Evict using approximated LRU, only keys with an expire set.
    # allkeys-lru -> Evict any key using approximated LRU.
    # volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
    # allkeys-lfu -> Evict any key using approximated LFU.
    # volatile-random -> Remove a random key having an expire set.
    # allkeys-random -> Remove a random key, any key.
    # volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
    # noeviction -> Don't evict anything, just return an error on write operations.
    #
    # LRU means Least Recently Used
    # LFU means Least Frequently Used

    maxmemory 100mb

    maxmemory-policy allkeys-lfu
    ```

## 19. Next..
1. Redis Collection
2. Redis PubSub
3. Redis Scalability

---

# 8 Kesalahan Menggunakan Redis
1. Terburu-buru dalam menggunakan redis
2. Digunakan pada data yang sering berubah
3. Caching data yang jarang diakses
4. Caching data pencarian
5. Cache data dari aplikasi lain
6. Cache data yang terlalu besar
7. menghidupkan feature persistance
8. Tidak menentukan waktu expired