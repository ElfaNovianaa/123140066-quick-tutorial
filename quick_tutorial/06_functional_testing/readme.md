# Percobaan 6 - Functional Testing with WebTest

## Deskripsi Singkat
Percobaan ini bertujuan untuk melakukan **pengujian fungsional (functional testing)** pada aplikasi Pyramid menggunakan **WebTest**. Berbeda dengan unit test yang hanya menguji potongan kecil kode, functional test memeriksa keseluruhan alur aplikasi dari awal hingga akhir, termasuk respons HTML yang dikembalikan. WebTest mensimulasikan permintaan HTTP tanpa perlu menjalankan server sesungguhnya, sehingga proses pengujian tetap cepat dan efisien.

---

## Analysis
Dalam percobaan ini, WebTest digunakan untuk menguji aplikasi Pyramid secara **end-to-end** dengan cara membuat permintaan HTTP langsung ke aplikasi WSGI. Setiap respons diuji untuk memastikan status kode HTTP serta konten HTML yang dihasilkan sudah sesuai. Misalnya, pengujian dilakukan untuk memastikan bahwa halaman utama (`/`) benar-benar menampilkan elemen `<h1>Hello World!</h1>`.

Keunggulan dari WebTest adalah kemampuannya melakukan pengujian penuh terhadap antarmuka web tanpa perlu server berjalan, sehingga mempercepat waktu eksekusi dan integrasi dengan pytest. Dengan pendekatan ini, developer dapat memastikan bahwa semua komponen — mulai dari routing, view, hingga template — bekerja secara konsisten dan benar. Selain itu, penggunaan `b''` dalam pengujian menunjukkan bahwa respons yang diuji berupa **byte string**, bukan teks biasa, karena data HTTP dikembalikan dalam bentuk byte.

---

## Kesimpulan
Percobaan ini menunjukkan pentingnya **functional testing** dalam memastikan kualitas keseluruhan aplikasi web. Dengan menggunakan WebTest, proses pengujian menjadi lebih menyeluruh tanpa mengorbankan kecepatan. Integrasi dengan pytest juga memungkinkan pengujian unit dan fungsional dijalankan bersama dalam satu sistem. Hasilnya, developer dapat dengan mudah menjaga stabilitas aplikasi dari sisi tampilan dan logika secara bersamaan.

## Output Percobaan 
![Gambar WhatsApp 2025-11-12 pukul 16 10 52_1bb65599](https://github.com/user-attachments/assets/9d63a140-7cc8-4e91-bdb2-c4a2a165266e)

