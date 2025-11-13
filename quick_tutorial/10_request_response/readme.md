# Percobaan 10 – Handling Web Requests and Responses

Dokumen ini menjelaskan bagaimana Pyramid memfasilitasi penanganan *request* yang masuk dan pembuatan *response* yang keluar menggunakan objek yang kuat dari library **WebOb**.

---

## Deskripsi Singkat

Percobaan ini mendemonstrasikan cara kerja *request* dan *response* di Pyramid. Kami menggunakan objek **`request`** (yang berasal dari WebOb) untuk mendapatkan informasi dari *query string* (`request.params.get('name')`) dan URL saat ini (`request.url`). Selain itu, kami menunjukkan dua cara utama untuk menghasilkan *response*: *redirect* menggunakan **`pyramid.httpexceptions.HTTPFound`** dan menghasilkan *response* teks biasa dengan objek **`pyramid.response.Response`**.

---

## Analisis dan Konsep Kunci
1. Fondasi WebOb
Pyramid menggunakan library **WebOb** untuk menangani *request* dan *response*. Hal ini memberikan *developer* objek *request* yang **andal** dan **matang** yang berisi semua informasi HTTP yang masuk (header, body, parameter) dan objek *response* yang mudah dimanipulasi untuk mengatur *header*, *body*, dan status keluar.

2. Mengakses Data Request dan URL
Objek `request` menyediakan cara yang mudah untuk mengakses data:

* **`request.url`**: Mengembalikan **URL lengkap** yang saat ini sedang diakses (misalnya, `http://localhost:6543/plain?name=alice`).
* **`request.params`**: Ini adalah objek seperti *dictionary* yang digunakan untuk mengambil **input pengguna** dari *query string* (GET) dan *form body* (POST) secara seragam, seperti yang ditunjukkan oleh `request.params.get('name')`.

3. Dua Cara Menghasilkan Response
Pyramid mendukung dua pendekatan utama untuk menghasilkan *response* yang efektif:

* **Redirects (Mengembalikan Pengecualian HTTP):** *View* `home()` mengembalikan **`pyramid.httpexceptions.HTTPFound`**. Ini adalah objek khusus yang bertindak sebagai *response* dengan status **302 Found** dan *header* `Location` yang memicu *redirect* di *browser*.
* **Response Langsung dan Kustom:** *View* `plain()` secara eksplisit membuat dan mengembalikan objek **`pyramid.response.Response`**. Ini memungkinkan kontrol penuh atas *header* *response*, seperti mengatur **`content_type='text/plain'`** dan menentukan *body* *response*.

---

## Extra Credit
**Bisakah kita juga `raise HTTPFound(location='/plain')` alih-alih mengembalikannya? Jika ya, apa perbedaannya?**
Ya, Anda **bisa** menggunakan `raise HTTPFound(location='/plain')` alih-alih `return HTTPFound(location='/plain')`.
* **Fungsionalitas:** Keduanya memiliki **efek yang sama** di Pyramid: mereka menghasilkan *response* HTTP 302 Redirect. Hal ini karena kelas pengecualian HTTP di Pyramid (`pyramid.httpexceptions.HTTPException`) juga bertindak sebagai objek *response* yang sah.
* **Perbedaan Konseptual:** Menggunakan **`raise`** lebih akurat secara konseptual ketika *redirect* terjadi sebagai pengalihan alur kontrol, yang menyiratkan bahwa kode *view* setelahnya tidak akan dieksekusi. Namun, **`return`** adalah pola yang paling umum dan mudah dibaca untuk *response* sukses di Pyramid, termasuk *redirect*.

---

## Kesimpulan

Pyramid memanfaatkan library **WebOb** yang matang untuk menyediakan objek **`request`** dan **`response`** yang kuat dan fleksibel. Hal ini memudahkan tugas-tugas penting seperti mengakses parameter *query* (`request.params`) dan mengelola *response* HTTP, termasuk *redirect* (`HTTPFound`) dan *response* dengan *content type* yang dikustomisasi (`Response`). Fleksibilitas ini membuat pengembangan aplikasi web menjadi lebih efisien dan *testable*.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 18 42 47_deee92e9](https://github.com/user-attachments/assets/f64b1243-9491-499e-9641-8a6e5b5497d9)


