# Percobaan 2 – Arsitektur Dasar Pyramid

## Deskripsi Singkat
Percobaan ini membahas dasar **arsitektur proyek Pyramid** dengan fokus pada pembentukan struktur paket (`package`) dan penggunaan **file setup.py** untuk pengaturan proyek.  
Langkah ini merupakan kelanjutan dari percobaan pertama yang sebelumnya hanya berisi file tunggal tanpa struktur modular.

---

## Arsitektur Dasar
Struktur proyek dibuat dengan pendekatan **Python Package**, di mana seluruh kode aplikasi dikelompokkan dalam satu direktori utama bernama `tutorial`.  
File penting yang digunakan pada tahap ini adalah `setup.py`, yang berfungsi untuk:
- Menghubungkan proyek dengan sistem manajemen paket Python (setuptools)
- Mengatur instalasi dependensi dan konfigurasi aplikasi
- Memungkinkan instalasi dalam mode pengembangan menggunakan `pip install -e .`

## Penjelasan Kode Utama
Kode utama masih berisi aplikasi Pyramid sederhana, serupa dengan percobaan pertama.  
Perbedaannya hanya terletak pada struktur dan cara instalasinya, bukan pada logika aplikasi.

- **`tutorial/app.py`** berisi konfigurasi Pyramid serta fungsi utama untuk menjalankan server.
- **`__init__.py`** menandai direktori `tutorial` sebagai package Python yang dapat diimpor.
- **`setup.py`** berisi metadata proyek seperti nama, versi, dependensi, dan entry point untuk eksekusi.

## Analisis
- Struktur modular: Pembuatan package tutorial membuat proyek lebih terorganisir dan mudah dikembangkan.
- setup.py sebagai pengatur proyek: File ini menjadi jembatan antara kode dan sistem manajemen paket Python, mendukung instalasi dengan pip install -e . (editable mode).
- Konsistensi logika: Walau struktur berubah menjadi package, fungsionalitas aplikasi tetap sama seperti sebelumnya.
- Pendekatan profesional: Langkah ini merupakan transisi dari proyek sederhana menuju struktur aplikasi Pyramid yang sesuai standar industri.
- Eksekusi sementara: Menjalankan python tutorial/app.py masih diperbolehkan di tahap pembelajaran, tetapi di dunia nyata, Pyramid sebaiknya dijalankan melalui pserve untuk keamanan dan kompatibilitas yang lebih baik.

## Kesimpulan
Percobaan ini menekankan pentingnya struktur paket dan pengaturan proyek menggunakan setup.py dalam pengembangan aplikasi Pyramid.
Dengan cara ini, proyek menjadi lebih terkelola, fleksibel, serta siap untuk dikembangkan ke tahap berikutnya.

--

## Output percobaan
![Uploading Gambar WhatsApp 2025-11-12 pukul 15.17.27_ef247bf5.jpg…]()
