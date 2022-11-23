# Microservices
- Slides: https://docs.google.com/presentation/d/1lKnpXhljmYwKgdnb1K8GDrpCYuao_ZHHGn24gX_um2M/edit#slide=id.p

---
## 4. Database Per Service

## 5. Shared Database

## 6. NoSQL
- Jenis-jenis NoSQL:
    - Document Oriented Database
    - Key-Value  Database
    - Column Families Database
    - Graph Database
    - Search Database
    - Time Series Database
    - Dan lain-lain
- Contoh:
    - MongoDB : Document Oriented Database 
    - Elasticsearch : Search Database
    - Redis : Key-Value Database
    - Apache Cassandra : Column Families Database
    - Neo4J : Graph Database
    - InfluxDB : Time Series Database


---
## 7. Remote Procedural Invocation
- Cara-cara komunikasi antar service:
    1. RPI
    2. Message
- RPI (Remote Procedural Invocation) (biasanya Sync) 
    - 2 yang popular adalah RESTful API dan gRPC

## 8. Messaging
- Message (Async) 
    - Komunikasi messaging membutuhkan message channel sebagai jembatan untuk mengirim dan menerima data
    - Direkomendasikan menggunakan aplikasi message broker untuk melakukan management message channel
    - Contoh:
        - Redis (PubSub)
        - Apache Kafka
        - RabbitMQ
        - NSQ

---
## 9. Types Microservices
- Ada 3 jenis:
    1. Stateless Microservice
    2. Persistence Microservice
    3. Aggregation Microservices
### 1. Stateless Microservice
- Biasanya tidak memiliki database
- Digunakan untuk melakukan tugas sederhana
- Biasa digunakan juga sebagai utility untuk microservice lain
- Tidak bergantung dengan microservice lain
- Contoh: Email Service dan SMS Service
### 2. Persistence (Stateful) Microservice
- Biasanya memiliki database 
- Bisa juga disebut sebagai Master Data Microservice
- Biasa digunakan untuk mengolah data di database (CRUD)
### 3. Aggregation Microservices
- Tergantung dengan microservice lain
- Biasa digunakan sebagai pusat business logic aplikasi
- Boleh memiliki database ataupun tidak
- Tidak bisa berdiri sendiri
- Contoh: Cart Service, Payment Service
- `Pusat alur ada disini, dia yang ngatur services lain`
![Contoh Aggregation Microservices](http://www.plantuml.com/plantuml/png/VPBVQkim3CRlynIYztc1GocSiZ462tHaVG35rjn0OWz9soXZxpudcLSxqEOkqa-V_dn6eu5XSnJY3Q30kn1N5PDHz6uWs6x_EyVaKFlWeUaDXqXyz8PajuEl2kBdWoNOl31wGJDpxOBCYdg9Lz-bz85OUb7oXcvADpHsn8NgE8Tcng9YXp9nv_RvAKlRBXFPuu3UKA7IBR6Lp268EgQebKE5M4DiJkXrDTCIN4yLl0jt-mAntVeMhQBIG28tBt4_OZyKJMcAP4JRj4LUHfbQJRv2pRqxQRtBFO5_SC0pPEvLURQET1eweW-ayBVxetyDp2DUAPezIoNpprHmXKzNZWN7DujAyElN6bmMZbXBti9oZoeLrpPy-iXSB5l6I8dBkqWxfKpkpBgZZF83)

---
## 10. Services Orchestration
- Sebelumnya kita sudah bahas tentang tipe Aggregation Microservices
- Cara Aggregation Microservices berkomunikasi dengan Microservices lain, jika menggunakan Remote Procedure Invocation, maka dinamakan Service Orchestration Pattern
- Dalam Service Orchestration Pattern, Aggregation Microservices bertugas untuk mengatur alur business logic sistem.
- Keuntungan Service Orchestration:
    - Mudah dibuat, karena kode business logic akan terpusat di Aggregation Microservices
    - Mudah dimengerti, karena kode business logic akan terpusat di Aggregation Microservices
- Kekurangan Service Orchestration:
    - Aggregation Microservices terlalu ketergantungan dengan Microservices lain
    - Aggregation Microservices akan lebih lambat karena harus terkoneksi dengan Microservices lain
    - Aggregation Microservices akan lebih mudah error jika di Microservices lain terdapat masalah
    - Jika perlu Microservices baru, perlu dilakukan perubahan di Aggregation Microservices

## 11. Services Choreopraphy
- `Alternatif dari Services Orchestration`
- Service Choreography berbeda dengan Service Orchestration. 
- Dalam Service Choreography, komunikasi Aggregation Service dengan Microservices lainnya menggunakan Messaging.
- Dalam Service Orchestration, Aggregation Microservice adalah service yang sangat kompleks dan mengerti semua alur business logic, sedangkan berbeda dengan Service Choreography, semua Microservices dituntut untuk menjadi pintar, tidak hanya diperintah oleh Aggregation Microservices.
![Contoh Aggregation Microservices](http://www.plantuml.com/plantuml/png/bPBXIWCn48J_vocM_hyNa7BKIdyM2jK7sARhDPXBT3UfYFZkSad5pM4B_fsR-NOot4eISigZDsXJP5Wy2V42K20BiJ5CDc4OFC5oUJyCJ0Cc5mDidUKen6TdIVeUFWq0G8X7WiKZOn2qnRUlba9ClxhvQj4xOd6IA5YwYTxIU21kg6EHb6UD7YUEDXsgrf3OdZ2a6QkAythxX8ayYTmijndH-OP7apB1tZBbSbG41u8rnvVBpFLBnJmxFvjlSwQGvkCLKGNgACfR9sbjwiNQMAMYJ3sp44F7RZYbhjStuGvs-06gcz5VksDy3ssYCATOFdWd9nq5GsMgPOtzbNg8GV98LmzUelgfAbKNkH9GJsuYLJUNN-M_vSmF6BhyslNj7JM3gOshIzLN7hEYSCoZ_mO0)
- `Cara kerjanya adalah, services akan melihat event2 yang ada pada message broker. Jika ada yang butuh data transaction event, bisa dia ambil`
- Keuntungan Service Choreography
    - Aggregation Microservices tidak tergantung dengan Microservices lainnya
    - Aggregation Microservice akan lebih cepat, karena tidak perlu berkomunikasi dengan Microservices lainnya
    - Jika ada Microservice baru, Aggregation Microservice tidak perlu melakukan perubahan lagi
- Kekurangan Service Choreography
    - Lebih sulit di-debug ketika terjadi masalah
    - Business logic akan terdistribusi di semua Microservices, sehingga sulit untuk dimengerti secara keseluruhan
    - Sangat bergantung pada message broker

---
## 12. API Gateway
- Masalah Mengekspos Microservices:    
    - Semua service bisa diakses dari luar    
    - Jika butuh Autentikasi, harus diimplementasikan di semua service
    - Rawan terjadi kebocoran data
- API Gateway adalah aplikasi yang bertugas sebagai gerbang dari luar ke dalam
- Luar adalah akses dari internet, dan Dalam adalah aplikasi microservices
- API Gateway bertugas sebagai proxy server ke semua aplikasi microservices
- Aplikasi microservices hanya bisa diakses dari luar melalui API Gateway
- Keuntungan API Gateway:
    - Lebih aman karena satu gerbang
    - Service tidak perlu mengimplementasikan proses Autentikasi, cukup dilakukan di API Gateway
    - API Gateway juga bisa digunakan sebagai load balancer
    - Bisa digunakan sebagai rate limiter
    - Bisa digunakan sebagai pengaman sehingga error dari service tidak terekspos
- Contoh API Gateway:
    - Nginx
    - Apache HTTPD
    - Kong
    - Netflix Zuul
    - Spring Cloud Gateway

## 13. Authetication dan Authorization
- Lihat Video
- Teknologi Pendukung:
    - Secure Cookie
    - Oauth
    - JSON Web Token
    - Basic Auth
    - API Key / Secret Key

---
## 14. Backend for Frontend
- Liat slides

---
## 15. CQRS (Command Query Responsibility Segregation)
- CQRS adalah proses membedakan operasi Command dan operasi Query
- Operasi Command adalah operasi mengubah data (Create, Update, Delete)
- Operasi Query adalah operasi mengambil data (Get, Search) 
- Dalam CQRS, biasanya service atau database dibedakan untuk kebutuhan Command dan kebutuhan Query
- `Jika data sudah gede, bisa pakai cara ini`

![Contoh CQRS](http://www.plantuml.com/plantuml/png/NOvT2i8m48JVSuhGzzn0f22-ALXwWZLPi92VDja8HRoxSHEaDK_Bp3SpwHD1fEoi04qXokva9_JKIKXIyobyC2YxMmmcQv8ZnkUaaO6vQIyXozF1pS6NH2a9pe4tjQNU_yYGCQuCBDzBl8K1WZkaidLTn-72dblJOZVEsKAYIIU4g1zCu5OHelvPAdNy3JVfe5IRQWP3TM2hx0ivmTcztgCtsTaF)
- Keuntungan CQRS:
    - Bisa memilih database berbeda yang optimal untuk proses Command dan Query, sehingga operasi Command dan Search bisa lebih cepat, karena database nya sudah disesuaikan dengan kebutuhan
    - Membedakan model untuk Command dan Query di aplikasi akan lebih mudah dibanding digabung di satu model yang sama untuk proses Command dan Query
    - Performa aplikasi akan lebih baik, karena kita membedakan component untuk Command dan Query    
- Idealnya pakai ini:
![Contoh CQRS Menggunakan Messaging](http://www.plantuml.com/plantuml/png/RP1DJWCn38NtEKKq-xa1GXMeEv2eUW8tCJ0YFwsTL1eXxaxg19HHcfNpsNxFzeuJKChUAJ0fafwUtO8XJHfO6mbLY1Rrz4RHPfFq4Ucw69I2SsFVIfTdeZ_7K3gAIFGUWgqgDLO_Wn2G-TpeiQ1Hxf2HLgcutABHBN3sIDMkVOgVXGGduEBvkXuFBvBsArzSbzaxuBIOwYmLs1DL3FDD09dX0_KJnQzcv2iw2Maplo-kst1_nP3wGmaeWwbwMJtFZ__dQUXXdjkIBI55CdLFjef_EqlttOpq39lHAXrRliDWitU_)
- Keutungan CQRS Menggunakan Messaging
    - Aplikasi Command dan Query terpisah, sehingga bisa dikerjakan oleh tim yang berbeda secara paralel
    - Aplikasi Command tidak perlu pusing memikirkan struktur data Aplikasi Query, hanya cukup mengirim datanya ke Message Broker
    - Scaling aplikasi bisa sesuai dengan kebutuhan, baik itu Command atau Query
    - Jika Aplikasi Query sedang stop atau error, data dari Aplikasi Command akan tetap aman tersimpan di Message Broker
    - Mekanisme retry akan lebih mudah dilakukan jika melalui Message Broker

---
## 16. Server Side Discovery
- Membuat server khusus sebagai router atau load balancer ke service
- Client hanya butuh terkoneksi ke router atau load balancer
- Jika jumlah node service bertambah atau berkurang, router yang hanya perlu dirubah, client tidak perlu berubah
![Server Side Discovery](http://www.plantuml.com/plantuml/png/VP1D3e8m48NtSueNzYpQ0nZY6XCJJr1AWyJoarBS6Ezkh1MOL5dEc-zDlamnUUNyt5dgXzeWEncdhwhtk1XtRxN9e2PqCOpCsGtrh1S4vJ5Gjg9HwPjgKYJ3Wm3WTr-4-lX9nGuejw2aP_GfXwQTVAiJePYlOenDpT9BWXqIUwXawDkEldz3dXzWApPlxMmLakK3V9Qqegmf_Yqa5QQlvyw-0000)
- Contoh Load Balancer:
    - Nginx
    - Apache HTTPD
    - Traefik
- Kekurangan:
    - Tiap service harus memiliki router atau load balancer
    - Agar tidak terjadi single point of failure, maka router atau load balancer harus di setup sebanyak 2 instance
    - Cost biaya akan lebih mahal, karena 1 service harus menjalankan 2 router

## 17. Client Side Discovery
- Client side discovery adalah solusi agar client harus bisa tahu lokasi semua lokasi service yang akan dituju
- Tidak perlu lagi ada router atau load balancer untuk berkomunikasi dengan Service lain
- Semua logic untuk mendistribusikan traffic harus dilakukan di client atau service yang akan melakukan request
![Client Side Discovery](http://www.plantuml.com/plantuml/png/VP113eCW403l-ugDTm_mWCO7j4cJle0i6qkgO01xQVhtQhLK397Zx6G7Q49KFevz1s2TPgmkxEckRCGR-wSXhb05x5S8WwA7QYVjwfqUQMEz0AUpxDWDKNoN30iL1wBSYXBU_zxPjIP4G-LWckR5RiNYI9MPEaVXzroiwrEwTf7AdAPW6Kk0XgsUV_i3)
- Kekurangan:
    - Client harus tahu lokasi semua service
    - Jika jumlah node service bertambah atau berkurang, client harus diubah untuk lokasi baru nya
    - Jika client salah mengimplementasikan logic untuk load balancer, maka traffic ke service yang dituju bisa tidak merata pembagiannya

## 18. Service Registry
- Service Registry  adalah aplikasi yang digunakan sebagai tempat untuk menyimpan semua informasi yang berhubungan dengan lokasi service
- Semua service akan meregistrasikan alamat lokasi nya di Service Registry  ketika pertama kali nyala
- Semua service akan laporan ke Service Registry  jika akan berhenti beroperasi, sehingga - Service Registry  akan menghilangkan informasi service tersebut agar tidak mendapat traffic dari service yang bertanya
- Registrasi ke Service Registry
![Registrasi ke Service Registry](http://www.plantuml.com/plantuml/png/XP3F3e8m38VlUue6pnpOqOFX0JGn-WJDrk0Y3D9j9iRuxl9dA1B17VjzwwzjxZoo3rKfRAq9aZFnpIY24nN6URudW0wSXat1H3PA1s9rGUiXshrKnQ9eK5snQBKZrpgeYVKGqBwXnmn2rZTfXcgs8igfACNpaxUlkvcD-Xqufp6nZELiJPLVQXSgndKX3Kswwqwq7O-6peXnaNKi5_1xg3zywNzELeUgV040)
- Bertanya ke Service Registry
![Bertanya ke Service Registry](http://www.plantuml.com/plantuml/png/VP71geCm44Nt-Oh1jtPXbczHf6iBfVs28PdQe2QIn8AK_dkZZJHICSipXxbpIUayMZzqBTD64cHNiWyb22vKA-Vjc04wS1B-QakaetGYA-weFKHxbhe8MO-YmJfePsla81BhoI2yiEDt2CMyfF_GOaVagSZPwvjNcUbc9RO3ascQs4PSxAbyfKwXRAaBUKitdn_0IVY78rcFUaIeJ8DE9LOmbdARbAzeSjOcvgmaBSEtlG40)
- Health Check di Service Registry
![Health Check di Service Registry](http://www.plantuml.com/plantuml/png/XT2n3e8m40RWlKznmPc1ZiQ1nCL14wDFqBGN89H2hgs9CRwxW4MH27Iy-tB_NLFh13bVQMPgXH1TaMw5HBXWLbX7zmY41QjWan6Y2UY497DX70JTeoeKMIDaARAMdQbMNexKWq7x-XdJ9YJzaEPTJHOW7qFEdj-yOztjNCZ_WTDkIDIXd2nH_aExK3QS2xlODt7tHLiueD0kXtsEicJ3AMk0puBFQgjYVaQAHlbAlm00)
---
## 19. Centralized Configuration
- Dimana menyimpan konfigurasi?
    - Konfigurasi adalah sesuatu yang tidak asing lagi saat membuat aplikasi
    - Tiap aplikasi biasanya memiliki konfigurasi, seperti konfigurasi database misalnya
    - Pertanyaannya, dimana sebaiknya menyimpan konfigurasi? Agar mudah untuk di maintain dan digunakan oleh aplikasi kita?
- Contoh lokasi: 
    - Database
    - File
    - Environment Variable
- Centralized Configuration
    - Centralized Configuration adalah pattern dimana kita menyimpan semua konfigurasi di sebuah aplikasi atau service
    - Service yang butuh konfigurasi akan bertanya ke aplikasi tersebut untuk mendapatkan data konfigurasinya
- Contoh Aplikasi Centralized Configuration:
    - Hashicorp Consul https://www.consul.io/
    - Hashicorp Vault https://www.vaultproject.io/
    - Etcd https://etcd.io/
    - Zookeeper https://zookeeper.apache.org/
    - Doozerd https://github.com/ha/doozerd
---