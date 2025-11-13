# Percobaan 10 - Handling Web Requests and Responses

## Deskripsi Singkat
Percobaan ini mempelajari cara kerja **request** dan **response** pada framework Pyramid. Fokusnya adalah bagaimana aplikasi web menerima data dari pengguna (melalui parameter URL atau form) dan mengembalikannya sebagai respons HTTP.  
Dalam percobaan ini, digunakan library **WebOb** yang menjadi dasar sistem request-response di Pyramid. View menangani permintaan (request), mengakses data dari parameter URL, dan mengatur respons yang dikirim kembali, termasuk jenis konten dan header.

---

## Analisis
1. Request dan WebOb:
- Pyramid menggunakan library WebOb untuk objek request. Ini memberikan antarmuka yang kuat dan standar untuk mengakses data request.
- Mengambil Parameter URL: Data query string (misalnya ?name=alice) diakses melalui self.request.params. Metode .get('name', 'No Name Provided') memastikan nilai default digunakan jika parameter tidak ada, menghindari KeyError.
2. Response dan Redirect:
- Redirect (View home): View home mengembalikan objek HTTPFound(location='/plain'). Ini adalah response khusus dari pyramid.httpexceptions yang menghasilkan response HTTP 302 Found (Redirect), mengarahkan browser ke URL /plain.
- Response Kustom (View plain): View plain secara eksplisit membuat objek Response baru:
a. content_type='text/plain' mengatur header Content-Type, memberi tahu browser cara menampilkan konten (teks biasa).
b. body=body mengatur isi response.
3. Pengujian yang Komprehensif:
- Uji Redirect: Unit Test memverifikasi bahwa view home mengembalikan status code 302 Found.
- Uji Parameter: Unit dan Functional Test memverifikasi bahwa view plain bekerja dengan benar, baik saat parameter name disediakan maupun tidak, memastikan logika default berjalan.

Jawaban Extra Credit
1. Bisakah kita juga raise HTTPFound(location='/plain') alih-alih me-return-nya? Jika ya, apa perbedaannya?
Ya, bisa.
- Fungsi: Kedua metode (return HTTPFound(...) dan raise HTTPFound(...)) mencapai hasil yang sama: menghasilkan response HTTP 302 Found.
- Perbedaan:
a. return: Ini adalah gaya yang lebih Pythonic dan direkomendasikan jika redirect adalah hasil normal dari view tersebut (seperti dalam contoh ini).
b. raise: Ini lebih sering digunakan jika redirect adalah hasil dari suatu kondisi pengecualian atau kegagalan (misalnya, pengguna mencoba mengakses halaman yang tidak berhak, dan Anda ingin mengarahkannya ke halaman login). Secara internal, Pyramid menangkap pengecualian ini dan menggunakannya sebagai response.

---

## Kesimpulan
Percobaan ini menunjukkan bagaimana Pyramid menangani **HTTP request dan response** secara efisien melalui integrasi dengan **WebOb**. Kita dapat melakukan redirect, membaca parameter dari URL, serta mengatur jenis dan isi response dengan mudah. Dengan pemahaman ini, pengembang dapat membangun alur komunikasi web yang lebih interaktif dan dinamis di aplikasi Pyramid.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 18 42 47_deee92e9](https://github.com/user-attachments/assets/f64b1243-9491-499e-9641-8a6e5b5497d9)

