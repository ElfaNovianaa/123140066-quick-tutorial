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

## Analisis Kode Pyramid "Hello World"

### **Line 11 (`if __name__ == '__main__':`)**
Bagian ini menunjukkan desain modular pada Pyramid. Dengan memisahkan titik eksekusi utama dari definisi modul, developer bisa menjalankan aplikasi secara langsung **atau** mengimpor file tersebut ke proyek lain tanpa menjalankan server otomatis.  
➡️ Analisis: ini mencerminkan prinsip **reusability** dan **encapsulation**, memungkinkan pengembangan skala besar tanpa saling ketergantungan antar-komponen.

### **Lines 12–14 (context manager Configurator)**
Penggunaan *context manager* (`with Configurator() as config:`) memastikan konfigurasi aplikasi dilakukan dalam ruang lingkup yang aman dan terkontrol. Setelah konteks selesai, semua pengaturan akan dirapikan secara otomatis oleh Pyramid.  
➡️ Analisis: pendekatan ini mencerminkan **resource safety** dan **explicit configuration**, mencegah konfigurasi bersifat global atau tidak sengaja terbawa antar-modul. Hal ini mendukung maintainability jangka panjang.

### **Lines 6–8 (fungsi view `hello_world`)**
Fungsi ini menjadi titik utama interaksi antara pengguna dan aplikasi. View hanya fokus pada satu tugas: menerima request dan menghasilkan response.  
➡️ Analisis: ini menegaskan prinsip **separation of concerns**, di mana logika bisnis dan logika routing dipisahkan. Selain itu, penggunaan `Response` secara eksplisit menunjukkan bahwa Pyramid memberi kontrol penuh terhadap isi dan format HTTP response — cocok untuk arsitektur modern seperti RESTful API.

### **Lines 15–17 (server Waitress menjalankan WSGI app)**
Bagian ini menampilkan bagaimana Pyramid memisahkan tanggung jawab antara server dan aplikasi. Waitress berfungsi sebagai *HTTP server* yang hanya menyalurkan request ke aplikasi WSGI Pyramid.  
➡️ Analisis: desain ini mencerminkan filosofi **WSGI decoupling** — artinya aplikasi tidak terikat pada satu server tertentu, dan bisa dipindahkan ke Gunicorn, uWSGI, atau server lain tanpa ubahan kode. Ini meningkatkan **portabilitas dan fleksibilitas deployment**.

---

## Extra Credit — Eksperimen

Beberapa eksperimen tambahan untuk memperdalam pemahaman:

### A. Why do we do this `print('Incoming request')` ...instead of: `print 'Incoming request'`?
Karena Python 3 mengharuskan penggunaan tanda kurung pada fungsi `print()`.  
Tanpa tanda kurung (`print '...'`) hanya valid di Python 2.

### B. What happens if you return a string of HTML? A sequence of integers?
- Jika return berupa **string HTML**, Pyramid otomatis mengubahnya menjadi `Response`.
- Jika return berupa **angka atau list**, akan muncul **TypeError**, karena tidak bisa dikonversi menjadi respons HTTP yang valid.

### C. Put something invalid, such as print xyz, in the view function. Kill your python app.py with ctrl-C and restart, then reload your browser. See the exception in the console?
Masukkan baris seperti `print xyz` di dalam fungsi view, lalu jalankan ulang server (`Ctrl+C` → `python app.py`).
Ketika browser di-*reload*, error akan muncul di terminal (traceback), menunjukkan mekanisme debug Pyramid.

### D. The GI in WSGI stands for "Gateway Interface". What web standard is this modelled after?
**GI = Gateway Interface**, yaitu standar komunikasi antara server web dan aplikasi Python.
WSGI terinspirasi oleh model lama CGI (*Common Gateway Interface*) dari web server tradisional.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 15 07 08_fd03bfc2](https://github.com/user-attachments/assets/8ebe1833-35c2-4b20-8068-95073275118e)



