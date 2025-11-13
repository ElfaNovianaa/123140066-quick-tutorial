# Percobaan 13 – Static Assets (CSS, JS, dan Gambar)

Dokumen ini menjelaskan cara Pyramid menangani file statis (CSS, JavaScript, dan gambar) dengan mendaftarkan direktori aset ke *URL path* publik.

---

## Tujuan

* Mendaftarkan direktori file statis dari *package* ke *URL path* menggunakan `config.add_static_view()`.
* Menggunakan *helper* **`request.static_url`** di *template* untuk menghasilkan URL aset secara fleksibel.
* Menampilkan *asset* CSS sederhana pada halaman web.

---

## Analisis 
Percobaan 13 memperkenalkan mekanisme penting dalam Pyramid untuk melayani aset statis seperti CSS, JS, dan gambar. Inti dari proses ini adalah fungsi config.add_static_view yang mendaftarkan direktori file statis dari package ke URL path publik. Misalnya, config.add_static_view(name='static', path='tutorial:static') memberitahu Pyramid untuk menyajikan semua file di dalam folder tutorial/static ketika browser mengakses URL yang dimulai dengan /static/.

Untuk memastikan fleksibilitas deployment, kita menggunakan helper request.static_url di dalam template (seperti href="${request.static_url('tutorial:static/app.css') }"). Helper ini secara dinamis menghasilkan URL yang benar (misalnya, /static/app.css) berdasarkan konfigurasi yang telah didaftarkan. Dengan pola ini, jika path publik (nilai name) diubah di masa mendatang, semua tautan di template akan otomatis diperbarui, sehingga menghindari broken link dan meningkatkan refactoring flexibility.

Perlu dicatat perbedaan antara request.static_url dan request.static_path. request.static_url menghasilkan URL yang dapat diakses oleh browser, dan karenanya digunakan di client-side (template HTML). Sementara itu, request.static_path mengembalikan path fisik file di server, yang berguna untuk kode Python server-side yang mungkin perlu membaca atau memproses file tersebut secara langsung, bukan untuk ditampilkan ke client.

Pengujian fungsional juga membuktikan bahwa view ini bekerja secara independen dari view aplikasi lainnya, dengan WebTest berhasil meminta /static/app.css dan memverifikasi kontennya disajikan dengan status 200 OK.

## Kesimpulan
Penanganan Static Assets di Pyramid sangat efisien berkat config.add_static_view dan request.static_url. Pola ini memastikan pemisahan yang jelas antara lokasi fisik aset dan URL path yang digunakan client, meningkatkan fleksibilitas deployment dan kemudahan perawatan kode.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 21 17 45_5026f22a](https://github.com/user-attachments/assets/451bcb76-4373-40d5-808a-9316e989729c)

