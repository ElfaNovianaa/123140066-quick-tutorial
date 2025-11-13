# Percobaan 20 – Logins dengan Authentication

Dokumen ini menjelaskan implementasi fitur **Authentication (Login/Logout)** di aplikasi Pyramid menggunakan sistem keamanan yang *pluggable* (dapat diganti) dan *cookie* berbasis tiket untuk mempertahankan *state* pengguna.

---

## Deskripsi Singkat

Percobaan ini berfokus pada pengenalan konsep **Authentication** Pyramid. Kami membuat *security policy* khusus (`SecurityPolicy`) yang menggunakan *helper* **`AuthTktCookieHelper`** untuk menyimpan status *login* di *cookie* yang ditandatangani secara aman. Kami mendefinisikan *views* **`login`** dan **`logout`**. Proses *login* memverifikasi *username* dan *password* terhadap daftar pengguna yang *password*-nya telah di-*hash* menggunakan **`bcrypt`**, praktik keamanan terbaik. Setelah berhasil, fungsi **`pyramid.security.remember()`** dipanggil untuk mengatur *cookie* autentikasi pada *response*.

---

## Analisis
1. Security Policy (Authentication)
Authentication di Pyramid bersifat *pluggable* dan dikendalikan oleh **Security Policy** yang harus didaftarkan ke `config.set_security_policy()`. Tugas utama *policy* adalah:

* **`identity(request)`**: Mengidentifikasi pengguna dari *request* yang masuk (misalnya, dari *cookie* yang ada).
* **`authenticated_userid(request)`**: Mengembalikan ID pengguna jika terautentikasi.

2. Cookie-Based State Management (`AuthTktCookieHelper`)
Kami memilih **`AuthTktCookieHelper`** untuk manajemen *session* autentikasi. Helper ini:
* Menyimpan *state* pengguna (tiket autentikasi) di *cookie browser*.
* Mengenkripsi dan menandatangani *cookie* menggunakan *secret key* yang disimpan di `development.ini` (`tutorial.secret`). Ini penting untuk mencegah manipulasi *cookie* di sisi klien.

3. Pengamanan Password (`bcrypt`)
* Password pengguna tidak boleh disimpan dalam *plain text* di *database* (`USERS`).
* Kami menggunakan **`bcrypt`**, algoritma *hashing* satu arah yang kuat dengan *salt* otomatis, melalui fungsi **`hash_password`** dan **`check_password`**. Ini adalah *best practice* keamanan.

4. Siklus Login/Logout
* **Login View (POST):** Memverifikasi `check_password(password, hashed_pw)`. Jika sukses, ia memanggil **`pyramid.security.remember(request, userid)`**, yang menginstruksikan *security policy* (melalui `AuthTktCookieHelper`) untuk membuat dan mengirim *cookie* autentikasi pada *response* HTTP Redirect.
* **Logout View:** Memanggil **`pyramid.security.forget(request)`**, yang menghasilkan *header* untuk menghapus *cookie* autentikasi dari *browser* pengguna.

5. Akses Status Login
Status *login* pengguna dapat diakses di *view* melalui **`request.authenticated_userid`**. Nilai ini kemudian disimpan sebagai *state* di *view class* (`self.logged_in`) untuk digunakan oleh *template* (misalnya, menampilkan tautan "Log In" atau "Logout").

---

## Extra Credit
1. Bisakah saya menggunakan *database* alih-alih `USERS` untuk mengautentikasi pengguna?**
Jawaban: Ya, **Security Policy** dirancang untuk *pluggable*. Daripada mencari pengguna di *dictionary* `USERS`, Anda dapat memodifikasi metode `identity()` atau `check_password()` untuk melakukan *query* ke database (**SQLAlchemy**) untuk memverifikasi *username* dan mengambil *hashed password* yang disimpan.

2. Setelah *login*, apakah informasi spesifik pengguna ditambahkan ke setiap *request*?**
Jawaban: Ya. Setelah *login*, informasi terkait pengguna yang diautentikasi ditambahkan ke objek *request* melalui *security policy*. Secara spesifik:
* **`request.authenticated_userid`**: Berisi ID pengguna yang diautentikasi (misalnya, `'editor'`).
* **`request.identity`**: Berisi *dictionary* identitas penuh yang dikembalikan oleh *security policy* (misalnya, data dari tiket autentikasi, termasuk *userid*).
kamu dapat memeriksa ini dengan *debugger* (`pdb`) pada objek `request` setelah pengguna berhasil *login*.

---

## Kesimpulan

Pengaturan **Authentication** di Pyramid sangat modular dan menekankan praktik keamanan terbaik seperti penggunaan *password hashing* (**`bcrypt`**) dan *session* berbasis *signed cookie* (**`AuthTktCookieHelper`**). Dengan mendefinisikan **Security Policy** dan menggunakan fungsi **`remember`/`forget`**, kita berhasil mengimplementasikan sistem *login/logout* yang aman, meletakkan dasar untuk fitur *authorization* (izin) di langkah selanjutnya.

## Output Percobaaan

![Gambar WhatsApp 2025-11-12 pukul 22 24 05_05e7ba9d](https://github.com/user-attachments/assets/7d7d1c5c-d090-4e01-95ef-2270f2a8810a)

![Gambar WhatsApp 2025-11-12 pukul 22 28 10_805a0882](https://github.com/user-attachments/assets/ca9b8fc7-3dd3-4bc1-8256-40352958ea92)

![Gambar WhatsApp 2025-11-12 pukul 22 28 26_bf84a8d7](https://github.com/user-attachments/assets/3780a02e-f93f-455e-beee-919a0fc7f505)


