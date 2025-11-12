# Percobaan 5: Unit Tests and pytest

## Deskripsi Singkat
Percobaan ini bertujuan untuk mengimplementasikan **unit testing** pada proyek Pyramid menggunakan **pytest**. Dengan menambahkan `pytest` ke dalam dependensi proyek, pengujian kode dapat dilakukan secara otomatis tanpa harus menjalankan aplikasi di browser. Pengujian ini memastikan setiap fungsi dalam aplikasi berjalan sesuai harapan dan membantu mendeteksi bug sejak dini.

---

## Analisis
Dalam percobaan ini, dilakukan pengujian fungsi `hello_world` menggunakan `pytest` dan modul `unittest`. Framework `pytest` membantu mempermudah proses testing dengan hasil yang lebih informatif dan efisien. Pada pengujian, digunakan `DummyRequest` dari modul `pyramid.testing` untuk mensimulasikan request tanpa perlu menjalankan server sebenarnya. Dengan cara ini, kita dapat memastikan bahwa fungsi view mengembalikan **HTTP status code 200**, yang berarti fungsi berjalan sesuai ekspektasi.

Selain itu, penggunaan `testing.setUp()` dan `testing.tearDown()` memastikan setiap pengujian dilakukan dalam kondisi yang bersih dan terisolasi, mencegah konfigurasi antar-test saling memengaruhi. Pengujian unit semacam ini penting untuk menjaga kualitas kode, terutama saat aplikasi berkembang dan mengalami perubahan signifikan di masa depan.

---

## Kesimpulan
Melalui percobaan ini, dapat disimpulkan bahwa **unit testing** merupakan bagian penting dalam proses pengembangan perangkat lunak untuk memastikan keandalan dan kestabilan kode. Dengan bantuan **pytest**, proses pengujian menjadi lebih mudah, cepat, dan terstruktur. Pendekatan ini membantu developer menemukan kesalahan sejak awal tanpa harus menjalankan aplikasi secara manual di browser.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 15 52 28_600fa7e9](https://github.com/user-attachments/assets/41577438-9690-4ada-8885-f55f884b06ee)
