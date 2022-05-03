# 1. Pengantar 
- Data science: interdiciplinary fields to extract insight from data
- Data science dibagi 3 fase:
    1. Analisis (diagnosis)
    2. Modelling (predictive)
    3. Testing (improve)
## 1. Analisis
![](img/1.png?raw=true)
- Actors:
1. Business Analyst -> Menyediakan pertanyaan bisnis
    - Exp: penjualan barang paling sedikit di kota apa?
2. Data Analyst -> Menjawab pertanyaan bisnis yang diberikan berdasarkan data yang telah disediakan dan mempresentasikannya (story telling)
3. Data Architect (blueprint) -> data warehouse -> data dari mana ke mana dan bagaimana prosesnya
4. Data Engineer (pipeline) -> data warehouse -> membuat jalur2 yang sudah dibuat pada blueprint
5. Database Admin (access and backup) -> data warehouse
- Contoh analisis: penjualan barang paling sedikit di kota apa? misal ketemu di Yogyakarta, lanjut pertanyaan, kira2 perlu nyetok berapa untuk bulan depan? karena bersifat prediksi, maka masuk ke fase modelling (membuat model untuk prediksi)
## 2. Modelling
![](img/2.png?raw=true)
- Actors:
1. Data scientist (lebih ke orang statistik yang belajar dikit programming) -> Punya ilmu statistik dan ilmu matematik untuk membuat sebuah model untuk prediksi
    - Exp: Untuk membuat prediksi barang yang dibutuhkan di kota Yogya pada bulan Juli
2. ML Engineer / AI Engineer (orang programming yang belajar dikit statistik)
3. Backend, Frontend, dan Devops Engineer
## 3. Testing

# 2. Pengantar P.2
- Perbedaan Artificial Intelligence dan Machine Learning:
    - AI : Input -> Rules -> Output (kata kunci: Programmed explicitly)
    - ML : Collection of data -> Find patterns -> Rules (kata kunci: cari polanya untuk dijadikan rules)
- Revolutions of data:
    - Previously -> structured data -> ML is good at this
    - Now -> Unstructured data (data macam2-> gambar, video, dll) -> Deep learning
- Batasan untuk ML:
    - Semakin ambigu bagi manusia maka ml semakin tidak reliable
    - Contoh:
        1. Perception (berkaitan dengan persepsi) -> Image clasification, Face recognition -> Berkembang pesat
        2. Judgement (berkaitan dengan penghakiman) -> Copyright violation, Hate speech, Essay grading -> tidak sempurna
        3. Social Outcome -> Terrorist risk -> hasil diragukan
- Penyebab2 kenapa ML bisa gagal:
    1. `Wrong questions`:
        - `Not clearly defined`
        - `No clear lead to business value`
    2. Lack of supports
        - Commitment to implement changes
        - `No proper strategy`
    3. Data problems
        - Data avaibility
        - `Poor data quality`
    4. Incomplete Data Science Team
        - `Seringnya kurang orang business sehingga pertanyaan bisnisnya kurang `
    5. `Over Promising`
        - Solution doesnt apear right away
        - Slow and small refinement
    - `warna ini` menandakan over hype dan over expectation


