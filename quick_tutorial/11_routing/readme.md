# Percobaan 11 - Dispatching URLs To Views With Routing

## Deskripsi Singkat
Percobaan ini berfokus pada mekanisme **routing** di framework Pyramid, yaitu proses mencocokkan pola URL dengan fungsi atau kelas view yang sesuai. Dalam praktik ini, kita membuat route yang dapat menangkap bagian tertentu dari URL dan mengubahnya menjadi data yang bisa diakses melalui `request.matchdict`. Dengan begitu, bagian URL seperti `/howdy/Jane/Doe` dapat secara otomatis diuraikan menjadi nilai `first = "Jane"` dan `last = "Doe"` yang kemudian ditampilkan di halaman web menggunakan template Chameleon.

---

## Analisis
Percobaan ini memperlihatkan bagaimana Pyramid menangani **replacement pattern** dalam deklarasi route menggunakan sintaks `{}`. Pada file `__init__.py`, route `/howdy/{first}/{last}` membuat Pyramid secara otomatis memetakan bagian URL ke dalam dictionary `matchdict`. Nilai tersebut dapat diambil di view menggunakan `self.request.matchdict['first']` dan `self.request.matchdict['last']`.  
Hal ini membuat sistem routing lebih dinamis dan fleksibel karena URL bisa membawa informasi secara langsung tanpa perlu query string. Dari sisi tampilan, data tersebut diteruskan ke template dan dirender ke halaman HTML.  
Pengujian memastikan bahwa data dari URL (`Jane`, `Doe`) berhasil ditangkap dan ditampilkan dengan benar di halaman. Mekanisme ini menunjukkan kekuatan Pyramid dalam memisahkan logika URL dan tampilan secara bersih serta mudah diatur.

---

## Kesimpulan
Percobaan ini membuktikan bahwa sistem **routing Pyramid** sangat efisien untuk menangani URL dinamis menggunakan pola pengganti. Dengan `matchdict`, data dari URL dapat diakses dengan mudah dan digunakan di dalam view. Pendekatan ini membantu membangun aplikasi web yang lebih modular, mudah diatur, dan mendukung desain URL yang lebih bersih serta informatif.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 19 05 27_69551574](https://github.com/user-attachments/assets/5f12a3f7-487b-45fb-a4c3-7a59c6fb1139)

![Gambar WhatsApp 2025-11-12 pukul 19 06 17_d4075409](https://github.com/user-attachments/assets/30f03815-3ce7-48af-8f5b-9c17a55240a7)

