# Percobaan 3 – Application Configuration with .ini Files

## Deskripsi Singkat
Percobaan ini memperkenalkan cara **menjalankan aplikasi Pyramid menggunakan file konfigurasi `.ini`** dan perintah `pserve`.  
Pendekatan ini memisahkan konfigurasi dari kode, membuat aplikasi lebih mudah diatur, dikelola, dan dijalankan secara profesional.

---

## Arsitektur Dasar
Pada tahap ini, kita menambahkan konsep **konfigurasi eksternal** ke dalam struktur proyek.  
File penting yang digunakan adalah:

- **`setup.py`** → Menentukan *entry point* `paste.app_factory` untuk memberi tahu Pyramid di mana menemukan aplikasi WSGI.
- **`development.ini`** → File konfigurasi utama yang berisi pengaturan server, port, dan aplikasi.
- **`__init__.py`** → Tempat mendefinisikan fungsi `main()` yang membuat dan mengembalikan instance aplikasi Pyramid.

---

## Penjelasan Konsep Utama
- **Konfigurasi berbasis `.ini`**  
  Pyramid membaca pengaturan aplikasi dan server dari file `.ini`, bukan dari kode langsung. Ini memisahkan logika aplikasi dari pengaturan runtime.
- **Entry Point pada `setup.py`**  
  Dengan menambahkan *entry point* `paste.app_factory`, Pyramid tahu di mana menemukan fungsi `main()` yang membangun aplikasi.
- **Fungsi `main()` di `__init__.py`**  
  Fungsi ini menggantikan peran `app.py` dari percobaan sebelumnya. Di sinilah konfigurasi route, view, dan pembuatan WSGI app dilakukan.
- **Menjalankan Aplikasi dengan `pserve`**  
  Perintah `pserve development.ini --reload` digunakan untuk menjalankan server. Opsi `--reload` memungkinkan aplikasi otomatis memuat ulang saat terjadi perubahan file.

---

## Analisis

Percobaan ini menunjukkan peralihan dari pendekatan manual ke sistem konfigurasi yang lebih terintegrasi.  
Dengan `.ini`, Pyramid menerapkan **pemisahan antara kode dan konfigurasi**, yang membuat aplikasi lebih mudah diatur dan di-*deploy*.

Analisis poin penting:
- File `development.ini` bertindak sebagai **pusat kontrol**. Ia menentukan aplikasi mana yang dijalankan (`[app:main]`) dan bagaimana server dikonfigurasi (`[server:main]`).
- File `setup.py` memperkenalkan **entry point**, konsep yang memungkinkan Pyramid memanggil fungsi utama tanpa hardcoding.
- Pemindahan fungsi `main()` ke `__init__.py` memperkuat prinsip **modularitas** — kode lebih rapi dan siap dikembangkan menjadi aplikasi besar.
- Fitur `--reload` saat menjalankan `pserve` meningkatkan **efisiensi pengembangan**, karena perubahan otomatis diterapkan tanpa restart manual.
- Pendekatan ini juga memudahkan penggunaan **beberapa file konfigurasi** (misalnya untuk mode development, testing, dan production).

Secara keseluruhan, percobaan ini memperlihatkan bagaimana Pyramid mengintegrasikan manajemen konfigurasi tingkat lanjut untuk mendukung pengembangan aplikasi yang **fleksibel, scalable, dan maintainable**.

---

## Output Percobaan

![Gambar WhatsApp 2025-11-12 pukul 15 39 14_65c33026](https://github.com/user-attachments/assets/1e7e897b-86bd-4bac-a867-facbeca51128)

---

**Kesimpulan:**  
Pyramid kini beralih ke sistem berbasis konfigurasi `.ini` yang membuat proses pengaturan aplikasi lebih efisien dan terstandarisasi.  
