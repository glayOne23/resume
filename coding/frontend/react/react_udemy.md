1. Kenapa tiap component dalam react harus dibungkus dengan 1 element?
    - Karena react mengeksekusi komponen menjadi fungsi yang terdiri dari 3 komponen:
        1. tag pembuka penutupnya
        2. parameter2 tagnya
        3. isi tagnya
2. kenapa fungsi dalam event tidak menggunakan ()?
    - contoh: `<button onClick={eventHandler}> Click </button>`
    - Karena jika diberi (), maka itu akan dieksekusi ketika render component tersebut. Jika tidak diberi (), maka react akan mengingat isi pada event tersebut dan akan dieksekusi ketika event terjadi
3. Guna state?
    - Untuk set nilai variabel baru dan mentrigger rerender komponen yang menggunakan state tersebut
4. Lempar tangkap data antar parent dan child:
    - Dari parent ke child -> pakai props
    - dari children ke parent -> buat prop untuk childen component di parent yang manggil function pada parent. Pada children, prop ini dipanggil untuk manggil funtion pada parent
5. UseEffect:
    - Codingan didalamnya akan selalu dijalankan setiap ada perubahan pada component
    - Jika list dalam useEffect ada, maka kodingan dalam effect akan dijalankan ketika load pertama kali dan ketika anggota dalam list berubah
6. UseReducer:
    - Digunakan ketika ada state yang kompleks atau jika ada state yang ingin diupdate berhubungan dengan state lain
    - Summary di chapter 120


