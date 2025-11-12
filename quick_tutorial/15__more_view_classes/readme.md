# Percobaan 8 - HTML Generation With Templating

## Deskripsi Singkat
Percobaan ini membahas penggunaan sistem **templating** dalam framework Pyramid untuk menghasilkan halaman HTML secara dinamis tanpa harus menulis kode HTML langsung di dalam file Python. Pyramid sendiri bersifat fleksibel dan tidak memaksa penggunaan template engine tertentu. Dalam percobaan ini digunakan **pyramid_chameleon** sebagai add-on untuk menghubungkan view dengan file template `.pt`.  
Setiap view kini hanya mengembalikan data dalam bentuk dictionary, sementara proses render HTML dilakukan oleh template yang terpisah. Dengan cara ini, struktur aplikasi menjadi lebih bersih, mudah dikelola, dan sesuai dengan prinsip pemisahan antara **logic** dan **presentation layer**.

---

## Analisis
Pada percobaan ini, penerapan sistem templating membuat kode lebih terorganisir dan profesional. View function kini fokus pada logika Python, sedangkan bagian tampilan ditangani oleh file template `home.pt`. Decorator `@view_config` digunakan untuk menghubungkan view dengan template melalui parameter `renderer`, sehingga data yang dikembalikan dari view otomatis diteruskan ke file HTML untuk dirender.  
Selain itu, penggunaan template yang sama (`home.pt`) untuk dua route (`/` dan `/howdy`) menunjukkan fleksibilitas sistem templating Pyramid. Pengujian unit juga menjadi lebih sederhana karena view hanya perlu diuji berdasarkan data yang dikembalikannya, bukan isi HTML-nya. Dengan menambahkan `pyramid.reload_templates = true` pada konfigurasi, template akan otomatis dimuat ulang tanpa perlu restart server, sangat membantu dalam tahap pengembangan.

---

## Kesimpulan
Percobaan ini menunjukkan bahwa penggunaan templating pada Pyramid sangat membantu dalam memisahkan logika program dari tampilan HTML. Pendekatan ini tidak hanya membuat kode lebih bersih dan mudah dipelihara, tetapi juga mempercepat proses pengembangan. Dengan memanfaatkan `pyramid_chameleon`, setiap perubahan pada tampilan dapat dilakukan langsung di file template tanpa perlu mengubah logika Python.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 17 05 04_eef682be](https://github.com/user-attachments/assets/ab8a5945-de97-4865-834f-c9d6e806c548)

![Gambar WhatsApp 2025-11-12 pukul 17 06 35_01ac303a](https://github.com/user-attachments/assets/a010bde3-335b-4e33-bfdf-5c6070d53208)

![Gambar WhatsApp 2025-11-12 pukul 17 06 48_0e5b8c9c](https://github.com/user-attachments/assets/da33c417-824c-4edb-98f9-0d57415a09e7)
