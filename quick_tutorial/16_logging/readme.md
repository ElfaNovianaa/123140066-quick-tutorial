# Percobaan 16 – Collecting Application Info With Logging

Dokumen ini menjelaskan cara Pyramid mengintegrasikan *standard* **Python Logging** untuk menangkap informasi *debugging* dan *error* dari aplikasi web, baik selama pengembangan maupun di lingkungan produksi.

---

## Deskripsi Singkat

Percobaan ini berfokus pada aktivasi dan konfigurasi sistem *logging* Python di dalam proyek Pyramid. Kami mendefinisikan *logger* spesifik untuk *package* `tutorial` dan mengaturnya ke level **`DEBUG`** di file konfigurasi **`development.ini`**. Kemudian, kami menambahkan pernyataan *logging* (`log.debug(...)`) di dalam metode *view* untuk memverifikasi bahwa pesan berhasil ditangkap dan ditampilkan di konsol serta *debug toolbar*.

---

## Analisis 
1. Standarisasi Logging Python
Pyramid mengadopsi mekanisme *logging* standar Python. Ini memungkinkan *developer* untuk menggunakan semua fitur bawaan Python (`logging.getLogger`, `log.debug`, `log.info`, dll.) tanpa perlu mempelajari API *logging* khusus *framework*.

2. Konfigurasi Hierarki Logging
File `development.ini` bertindak sebagai pusat konfigurasi *logging*. Konfigurasi dipecah menjadi beberapa bagian:

* **`[loggers]`**: Mendaftarkan nama *logger* yang digunakan, seperti `root` (untuk *logger* bawaan) dan `tutorial` (untuk *package* aplikasi kita).
* **`[logger_tutorial]`**: Mendefinisikan *logger* spesifik untuk *package* kita (`qualname = tutorial`) dan menentukan **level minimum** (`level = DEBUG`). Ini memastikan pesan *debug* yang kita tambahkan (`log.debug('In home view')`) akan diproses dan dikeluarkan.
* **`[handlers]` dan `[formatters]`**: Mendefinisikan cara output dikeluarkan (misalnya, `handler_console` untuk mengirim output ke `sys.stderr`) dan format pesan yang ditampilkan (`formatter_generic`).

3. Mengaktifkan Logging di View Code
Untuk menggunakan *logging* di *view code* (`tutorial/views.py`), dua langkah diperlukan:
1.  Mengimpor modul *logging*: `import logging`.
2.  Mendapatkan *logger* berdasarkan nama modul (atau *package*): `log = logging.getLogger(__name__)`. Variabel `__name__` secara otomatis akan bernilai `'tutorial.views'`, yang kemudian dicocokkan dengan konfigurasi `[logger_tutorial]` di `development.ini`.

4. Visibilitas Pesan
Pesan *logging* tidak hanya terlihat di konsol *server* (`pserve`), tetapi juga di **Pyramid Debug Toolbar**. Ini sangat berguna selama pengembangan karena pesan *logging* yang terkait dengan *request* tertentu mudah diisolasi dan diperiksa.

---

## Kesimpulan

Integrasi **standard Python Logging** dalam Pyramid memungkinkan *developer* memiliki kontrol penuh atas informasi yang dikumpulkan dari aplikasi. Dengan konfigurasi yang jelas di `development.ini`, kita dapat mengatur level *logging* yang berbeda untuk *package* aplikasi kita (seperti `DEBUG`) dan *logger* lainnya, memastikan visibilitas penuh terhadap alur eksekusi aplikasi untuk tujuan *debugging* dan pemantauan.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 21 53 10_dd5eae18](https://github.com/user-attachments/assets/d63595e0-398e-4424-bdeb-047df2f5a2a8)

![Gambar WhatsApp 2025-11-12 pukul 21 54 17_5410ff66](https://github.com/user-attachments/assets/b16f8c18-2966-45ec-bde5-26e22344c841)

![Gambar WhatsApp 2025-11-12 pukul 21 54 33_e3e4a1f7](https://github.com/user-attachments/assets/bb4a4aba-a1f0-4323-86a7-059c4bf4e504)
