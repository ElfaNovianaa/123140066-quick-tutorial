# Percobaan 12 – Templating with Jinja2

## Deskripsi Singkat
Pada percobaan ini, kita membuktikan bahwa framework **Pyramid** mendukung berbagai sistem templating, salah satunya **Jinja2**. Jinja2 adalah sistem templating populer yang digunakan di Flask dan memiliki sintaks mirip dengan Django Template. Melalui percobaan ini, kita belajar cara menambahkan **pyramid_jinja2** sebagai add-on Pyramid untuk mengaktifkan dukungan Jinja2 sebagai renderer pada aplikasi Pyramid.

Langkah-langkah utamanya meliputi:
- Menyalin direktori `view_classes` menjadi `jinja2`.
- Menambahkan dependensi `pyramid_jinja2` ke file `setup.py`.
- Mengaktifkan modul `pyramid_jinja2` di file `__init__.py`.
- Mengubah renderer view menjadi `.jinja2`.
- Menambahkan file template baru `home.jinja2`.
- Menjalankan pengujian dan memastikan semua test lulus.

---

## Analisis
Proses integrasi add-on di Pyramid sangat sederhana. Cukup dengan menambahkan paket menggunakan alat instalasi Python seperti `pip`, lalu menyertakannya ke dalam konfigurasi aplikasi menggunakan `config.include`. Pada kasus ini, `pyramid_jinja2` memperkenalkan renderer baru yang mengenali file berformat `.jinja2`.  

Kode utama pada view tidak banyak berubah — hanya renderer-nya yang diganti dari `.pt` menjadi `.jinja2`. Sintaks dasar antara Chameleon dan Jinja2 juga sangat mirip, terutama dalam hal penyisipan variabel ke dalam template. Hal ini menunjukkan fleksibilitas Pyramid dalam mendukung berbagai sistem templating tanpa perlu banyak penyesuaian kode.

---

## Kesimpulan
Percobaan ini menunjukkan bahwa **Pyramid** memiliki arsitektur yang fleksibel untuk mendukung berbagai sistem templating, termasuk **Jinja2**. Integrasi add-on seperti `pyramid_jinja2` dapat dilakukan dengan mudah melalui instalasi paket dan konfigurasi singkat pada `__init__.py`. Dengan perubahan minimal pada kode view, aplikasi sudah dapat berjalan menggunakan renderer baru berbasis Jinja2.
