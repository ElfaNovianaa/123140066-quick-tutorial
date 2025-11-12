# Percobaan 10 - Handling Web Requests and Responses

## Deskripsi Singkat
Percobaan ini mempelajari cara kerja **request** dan **response** pada framework Pyramid. Fokusnya adalah bagaimana aplikasi web menerima data dari pengguna (melalui parameter URL atau form) dan mengembalikannya sebagai respons HTTP.  
Dalam percobaan ini, digunakan library **WebOb** yang menjadi dasar sistem request-response di Pyramid. View menangani permintaan (request), mengakses data dari parameter URL, dan mengatur respons yang dikirim kembali, termasuk jenis konten dan header.

---

## Analisis
Pada implementasi kali ini, terdapat dua route: `/` dan `/plain`. Route `/` melakukan **redirect** ke `/plain` menggunakan `HTTPFound`, sedangkan `/plain` menampilkan data berdasarkan parameter `name` yang dikirim melalui URL. Jika parameter tidak ada, maka nilai default yang ditampilkan adalah *“No Name Provided”*.  
Pyramid secara otomatis memetakan objek request dari `self.request` sehingga pengambilan data menjadi mudah dengan `self.request.params.get()`. Selain itu, kita juga dapat menentukan `content_type` respons agar browser menampilkan isi dengan benar.  
Dari sisi pengujian, unit test memastikan redirection bekerja dan data dapat diambil dengan atau tanpa parameter `name`. Semua tes berhasil menunjukkan bahwa sistem request dan response sudah berjalan sebagaimana mestinya.

---

## Kesimpulan
Percobaan ini menunjukkan bagaimana Pyramid menangani **HTTP request dan response** secara efisien melalui integrasi dengan **WebOb**. Kita dapat melakukan redirect, membaca parameter dari URL, serta mengatur jenis dan isi response dengan mudah. Dengan pemahaman ini, pengembang dapat membangun alur komunikasi web yang lebih interaktif dan dinamis di aplikasi Pyramid.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 18 42 47_deee92e9](https://github.com/user-attachments/assets/f64b1243-9491-499e-9641-8a6e5b5497d9)
