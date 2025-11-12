# Percobaan 9 - Organizing Views With View Classes

## Deskripsi Singkat
Percobaan ini bertujuan untuk mengorganisir fungsi-fungsi view menjadi **kelas view** dalam framework Pyramid. Sebelumnya, setiap view ditulis sebagai fungsi terpisah, namun dalam percobaan ini, view digabungkan dalam satu kelas agar lebih terstruktur, mudah dikelola, dan meminimalkan pengulangan konfigurasi. Dengan menggunakan **@view_defaults**, konfigurasi umum seperti renderer dapat didefinisikan di tingkat kelas dan berlaku untuk seluruh metode di dalamnya.  
Selain itu, pendekatan ini memungkinkan setiap instance view menyimpan state melalui `__init__`, misalnya untuk menyimpan objek `request`.

---

## Analisis
Pada percobaan ini, dua view (`home` dan `hello`) digabungkan ke dalam satu kelas bernama `TutorialViews`. Pendekatan ini menyatukan logika yang saling berkaitan dalam satu wadah yang rapi dan efisien. Penggunaan decorator `@view_defaults(renderer='home.pt')` di tingkat kelas membuat kedua metode otomatis menggunakan template yang sama tanpa perlu mendeklarasikannya berulang kali.  
Dari sisi pengujian, perubahan dilakukan agar sesuai dengan struktur baru — yaitu dengan membuat instance kelas view (`inst = TutorialViews(request)`) sebelum memanggil metode yang diuji. Dengan cara ini, testing tetap berjalan lancar, dan hasil menunjukkan bahwa semua fungsi bekerja sebagaimana mestinya setelah refactoring ke bentuk class-based view.

---

## Kesimpulan
Melalui percobaan ini, kita belajar bagaimana **view classes** di Pyramid membantu meningkatkan keteraturan dan efisiensi kode. Pendekatan ini memudahkan pengelolaan banyak view yang memiliki konfigurasi serupa serta memungkinkan berbagi state antar metode. Dengan refactoring ini, aplikasi menjadi lebih modular dan mudah dikembangkan di

--- 

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 18 35 11_78496e1b](https://github.com/user-attachments/assets/95f6b38b-112b-4f10-a55c-a5cd655f71f6)

![Gambar WhatsApp 2025-11-12 pukul 18 36 05_a7347379](https://github.com/user-attachments/assets/db12a7be-84d1-46ac-92da-898774da26a4)

![Gambar WhatsApp 2025-11-12 pukul 18 36 18_c0cf85d8](https://github.com/user-attachments/assets/91a89f2e-de46-4219-acea-7d9baf193131)


