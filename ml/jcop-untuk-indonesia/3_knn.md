## Apa itu KNN (K Nearest Neighbour) dan Cara Kerjanya
- Memprediksi berdasarkan tetangga terdekatnya
- K disini adalah jumlah tetangga terdekat -> untuk memprediksi -> untuk mencegah data yang ambigu merusak prediksi
    - K biasanya ganjil agar tidak seri
- Hal2 yang bisa dipertimbangkan:
    1. Jumlah K-nya
    2. Jaraknya diberi bobot (weight) -> jadi jarak yang dekat dan jauh, bobotnya beda
    3. Cara menghitung jaraknya:
        - Yang umum pakai 3 cara ini: 
            1. Euclidean Distance (Pythagorean Distance)
            2. Manhattan Distance (Cityblock distance)
            3. Chebyshev Distance (Chessboard Distance)
        - Jarak lain dapat dilihat pada: https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.DistanceMetric.html
