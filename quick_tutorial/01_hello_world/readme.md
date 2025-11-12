# Percobaan 1 – Single-File Web Application (Pyramid)

**Framework:** Pyramid  
**Server:** Waitress  
**Tujuan:** Membuat dan memahami aplikasi web paling sederhana menggunakan Pyramid hanya dengan satu file (`app.py`).

**Referensi Tutorial Asli:**  
[Pyramid Quick Tutorial – Hello World](https://docs.pylonsproject.org/projects/pyramid/en/latest/quick_tutorial/hello_world.html)

---

## Arsitektur Dasar

Aplikasi “Hello World” di Pyramid sudah mencakup semua komponen penting dari sebuah web app:

- **Routing** → Menentukan URL yang akan ditangani (`/`).
- **View** → Fungsi Python yang menangani permintaan (request) dan mengembalikan respons.
- **Response** → Objek yang dikirim kembali ke browser, berisi hasil dari view (biasanya HTML).
  
---

## Penjelasan Kode Utama

Struktur kode minimal pada percobaan ini bisa dijelaskan seperti berikut:

- `from waitress import serve` → Mengimpor server lokal berbasis WSGI untuk menjalankan aplikasi.
- `Configurator()` → Objek utama Pyramid untuk mengatur route dan view.
- `add_route()` → Mendefinisikan URL pattern yang dikenali aplikasi.
- `add_view()` → Mengaitkan fungsi view dengan route yang sudah didefinisikan.
- `make_wsgi_app()` → Membuat aplikasi WSGI yang siap dijalankan server.
- `serve(app, host="0.0.0.0", port=6543)` → Menjalankan server di alamat dan port tertentu.

---

## Analisis Teknis

- Semua konfigurasi dilakukan terpusat lewat **Configurator**, membuat aplikasi mudah dikembangkan.
- Pyramid sepenuhnya berbasis WSGI → kompatibel dengan berbagai deployment.
- Prinsip **Separation of Concerns** diterapkan: View menangani logika, server dan routing dikelola secara terpisah.
- Meskipun aplikasinya sederhana, strukturnya sudah sesuai dengan pola aplikasi Pyramid besar.

---

## Penjelasan kode

### Baris Penting

- **Line 11:** `if __name__ == '__main__':`  
  Artinya, bagian ini hanya akan dijalankan ketika file dipanggil langsung, bukan saat diimpor oleh modul lain.

- **Lines 12–14:**  
  Menggunakan *context manager* untuk membuat dan mengatur konfigurasi route dan view.

- **Lines 6–8:**  
  Mengimplementasikan view function (`hello_world`) yang menghasilkan `Response`.

- **Lines 15–17:**  
  Menjalankan server HTTP yang menerbitkan aplikasi WSGI ke jaringan lokal.

**Kesimpulan penting:**  
`Configurator` adalah pusat dari pengembangan Pyramid. Aplikasi dibangun dari bagian-bagian yang longgar (*loosely coupled*) melalui *Application Configuration*.

---

## Extra Credit — Eksperimen

Beberapa eksperimen tambahan untuk memperdalam pemahaman:

### A. Kenapa `print('Incoming request')` dan bukan `print 'Incoming request'`?
Karena Python 3 mengharuskan penggunaan tanda kurung pada fungsi `print()`.  
Tanpa tanda kurung (`print '...'`) hanya valid di Python 2.

### B. Apa yang terjadi jika kita return tipe selain `Response`?
- Jika return berupa **string HTML**, Pyramid otomatis mengubahnya menjadi `Response`.
- Jika return berupa **angka atau list**, akan muncul **TypeError**, karena tidak bisa dikonversi menjadi respons HTTP yang valid.

### C. Coba kesalahan sintaks:
Masukkan baris seperti `print xyz` di dalam fungsi view, lalu jalankan ulang server (`Ctrl+C` → `python app.py`).
Ketika browser di-*reload*, error akan muncul di terminal (traceback), menunjukkan mekanisme debug Pyramid.

### D. Arti “GI” pada WSGI:
**GI = Gateway Interface**, yaitu standar komunikasi antara server web dan aplikasi Python.
WSGI terinspirasi oleh model lama CGI (*Common Gateway Interface*) dari web server tradisional.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 15 07 08_fd03bfc2](https://github.com/user-attachments/assets/8ebe1833-35c2-4b20-8068-95073275118e)


