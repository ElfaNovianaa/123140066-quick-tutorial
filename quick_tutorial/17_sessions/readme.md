# Percobaan 17 – Transient Data Using Sessions

Dokumen ini menjelaskan cara Pyramid menangani data transien (sementara) yang perlu dipertahankan antara *request* pengguna yang berbeda menggunakan **Pyramid Sessions**.

---

## Deskripsi Singkat

Percobaan ini mengimplementasikan fitur *session* bawaan Pyramid menggunakan **`SignedCookieSessionFactory`**. Pabrik *session* ini didaftarkan ke `Configurator` di `__init__.py`. Setelah diaktifkan, *view* dapat menyimpan dan mengambil data sementara melalui objek **`request.session`**, yang bertindak seperti *dictionary* Python. Kami menggunakannya untuk membuat *counter* sederhana yang nilainya terus bertambah meskipun pengguna berpindah halaman atau me-*reload* aplikasi.

---

## Analisis 
1. Aktivasi Session
Pyramid tidak secara otomatis menyediakan *session* hingga Anda mendaftarkan **Session Factory**. Kami menggunakan:

* **`SignedCookieSessionFactory('itsaseekreet')`**: Ini adalah implementasi *session* bawaan Pyramid yang menyimpan data **di sisi klien** (sebagai *cookie* di *browser*). Data tersebut dienkripsi dan ditandatangani menggunakan *secret key* (`itsaseekreet`) untuk mencegah manipulasi data oleh klien.
* Pabrik *session* kemudian diteruskan ke `Configurator`: `config = Configurator(..., session_factory=my_session_factory)`.

2. Penggunaan `request.session`
Setelah Session Factory didaftarkan, setiap objek `request` memiliki atribut **`.session`** yang dapat digunakan seperti *dictionary* standar Python untuk menyimpan data *session* spesifik pengguna.

* Dalam *view class*, *counter* disimpan di `session['counter']`. Setiap *request* memicu properti `@property counter` yang mengambil nilai *session*, menaikkannya, dan menyimpannya kembali.
* Karena *session* terikat pada *browser* (melalui *cookie*), *counter* tetap bertambah meskipun pengguna berpindah antara `/` dan `/howdy`, membuktikan bahwa data *session* bersifat **global per pengguna**, bukan per URL.

3. Flash Messages (Data Antar Request)
Konsep *session* sangat penting untuk **Flash Messages**. Ini adalah pesan yang hanya ditampilkan sekali pada *request* berikutnya setelah *request* saat ini. Contohnya, setelah pengguna melakukan *POST* formulir, *server* biasanya merespons dengan **HTTP Redirect** ke halaman baru (*GET*). *Session* digunakan untuk membawa pesan ("Item berhasil ditambahkan!") melintasi dua *request* tersebut. Pyramid menyediakan API khusus untuk menangani pesan kilat (flash messages) ini.

4. Persistensi Data
Karena kami menggunakan *session* berbasis *signed cookie*, data *session* (yaitu nilai *counter*) disimpan di *browser* pengguna. Hal ini menyebabkan *counter* **tetap bertambah** bahkan jika aplikasi *server* Pyramid di-*restart*, karena *server* hanya perlu membaca dan mendekripsi *cookie* yang dikirim kembali oleh *browser*.

---

## Kesimpulan

Penggunaan **Session Factory** di Pyramid menyediakan cara yang andal dan mudah untuk mengelola data transien yang perlu dipertahankan di antara *request* seorang pengguna. Dengan mengimplementasikan `SignedCookieSessionFactory`, kita dapat menyimpan *state* pengguna secara aman (meskipun terbatas) dan mendukung fitur penting seperti *shopping cart* dan *flash messages*, yang merupakan kebutuhan dasar dalam pengembangan aplikasi web.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 21 57 02_44e6abc9](https://github.com/user-attachments/assets/30a7efc8-a76d-4189-b443-8bbaca46c5b0)

![Gambar WhatsApp 2025-11-12 pukul 21 57 32_ff221ba5](https://github.com/user-attachments/assets/2bb7f27b-0736-478a-8092-67062448a687)

![Gambar WhatsApp 2025-11-12 pukul 21 58 16_f92e1305](https://github.com/user-attachments/assets/ac392f5c-1cca-46d7-a048-a38dce6adcca)

