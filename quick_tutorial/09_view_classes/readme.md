# Percobaan 9 - Organizing Views With View Classes

## Deskripsi Singkat
Percobaan ini bertujuan untuk mengorganisir fungsi-fungsi view menjadi **kelas view** dalam framework Pyramid. Sebelumnya, setiap view ditulis sebagai fungsi terpisah, namun dalam percobaan ini, view digabungkan dalam satu kelas agar lebih terstruktur, mudah dikelola, dan meminimalkan pengulangan konfigurasi. Dengan menggunakan **@view_defaults**, konfigurasi umum seperti renderer dapat didefinisikan di tingkat kelas dan berlaku untuk seluruh metode di dalamnya.  
Selain itu, pendekatan ini memungkinkan setiap instance view menyimpan state melalui `__init__`, misalnya untuk menyimpan objek `request`.

---

## Analisis
1. Penggunaan @view_defaults
Dekorator @view_defaults berfungsi untuk menentukan parameter konfigurasi yang umum dan akan berlaku pada semua metode yang didekorasi dengan @view_config di dalam kelas tersebut.
- Dalam kasus ini, renderer='home.pt' tidak perlu diulang pada @view_config(route_name='home') dan @view_config(route_name='hello').
2. Siklus Hidup Kelas View
Ketika sebuah request masuk:
- Pyramid mencocokkan route (/ atau /howdy) dengan metode di TutorialViews.
- Pyramid membuat instance dari TutorialViews, meneruskan objek request ke __init__.
- Pyramid memanggil metode yang sesuai (inst.home() atau inst.hello()).
Konsep ini memungkinkan metode view berbagi state dan helper melalui self.request atau variabel instance lainnya.
3. Dampak pada Pengujian (Unit Test)
Unit Test kini harus secara eksplisit meniru siklus hidup Pyramid. Ini memastikan bahwa:
- View diuji sebagai metode yang membutuhkan objek self (yaitu, instance kelas).
- Request tersedia dan terpasang dengan benar ke instance (inst.request).
Hasil Akhir
Aplikasi berfungsi penuh, dan semua pengujian berhasil dijalankan, memvalidasi struktur kode yang baru.
```Bash
$VENV/bin/pytest tutorial/tests.py -q
....
4 passed in 0.xx seconds
---
```

## Kesimpulan
Melalui percobaan ini, kita belajar bagaimana **view classes** di Pyramid membantu meningkatkan keteraturan dan efisiensi kode. Pendekatan ini memudahkan pengelolaan banyak view yang memiliki konfigurasi serupa serta memungkinkan berbagi state antar metode. Dengan refactoring ini, aplikasi menjadi lebih modular dan mudah dikembangkan di

--- 

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 18 35 11_78496e1b](https://github.com/user-attachments/assets/95f6b38b-112b-4f10-a55c-a5cd655f71f6)

![Gambar WhatsApp 2025-11-12 pukul 18 36 05_a7347379](https://github.com/user-attachments/assets/db12a7be-84d1-46ac-92da-898774da26a4)

![Gambar WhatsApp 2025-11-12 pukul 18 36 18_c0cf85d8](https://github.com/user-attachments/assets/91a89f2e-de46-4219-acea-7d9baf193131)



