# Percobaan 7 - Basic Web Handling With Views

## Deskripsi Singkat
Percobaan ini membahas cara mengorganisasi **views** dalam framework Pyramid agar lebih terstruktur dan mudah dikembangkan. Sebelumnya, fungsi view, route, dan konfigurasi aplikasi masih berada dalam satu file. Pada percobaan ini, semua fungsi view dipindahkan ke modul terpisah `views.py`, kemudian dihubungkan dengan konfigurasi utama melalui **decorator** dan proses **auto scanning** menggunakan `config.scan('.views')`. Selain itu, ditambahkan dua view berbeda yang saling terhubung, yaitu `/` (home) dan `/howdy` (hello).

---

## Analysis
Dalam percobaan ini, proses penanganan web pada Pyramid diatur lebih rapi dengan memisahkan fungsi view dari file utama aplikasi. Modul `views.py` kini berisi dua fungsi utama — `home` dan `hello` — yang masing-masing dihubungkan ke URL berbeda menggunakan **decorator** `@view_config`. Dekorator ini memungkinkan konfigurasi dilakukan secara **declarative**, sehingga tidak perlu lagi menulis konfigurasi manual dengan `config.add_view`.  

Kemudian, konfigurasi utama pada `__init__.py` menjadi lebih ringkas, hanya mengatur route dan memanggil `config.scan('.views')` untuk mendeteksi decorator yang digunakan pada modul `views.py`. Pengujian dilakukan untuk memastikan bahwa setiap view memberikan respons yang benar dan menampilkan konten HTML yang diharapkan. Selain itu, konsep perbedaan antara nama view, nama route, dan URL juga diperlihatkan dalam percobaan ini, menegaskan fleksibilitas Pyramid dalam pemetaan rute.

---

## Kesimpulan
Percobaan ini menunjukkan pentingnya pemisahan struktur kode pada aplikasi web Pyramid. Dengan menempatkan fungsi view di modul terpisah dan menggunakan decorator `@view_config`, pengelolaan aplikasi menjadi lebih efisien, mudah dibaca, dan siap untuk pengembangan skala besar. Pendekatan declarative configuration membuat kode lebih bersih sekaligus mempermudah pengujian dan pemeliharaan aplikasi.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 16 55 20_c2dace2f](https://github.com/user-attachments/assets/9625801a-868d-4141-a731-5db888065d74)

![Gambar WhatsApp 2025-11-12 pukul 16 58 37_4fb735f7](https://github.com/user-attachments/assets/11027f50-299a-4620-bc4d-2a820b76ded1)

![Gambar WhatsApp 2025-11-12 pukul 16 58 54_3bf7501e](https://github.com/user-attachments/assets/b4dba03a-46b1-45b1-9d95-8ae85f65d64b)


