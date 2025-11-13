# Percobaan 11 - Dispatching URLs To Views With Routing

## Deskripsi Singkat
Percobaan ini berfokus pada mekanisme **routing** di framework Pyramid, yaitu proses mencocokkan pola URL dengan fungsi atau kelas view yang sesuai. Dalam praktik ini, kita membuat route yang dapat menangkap bagian tertentu dari URL dan mengubahnya menjadi data yang bisa diakses melalui `request.matchdict`. Dengan begitu, bagian URL seperti `/howdy/Jane/Doe` dapat secara otomatis diuraikan menjadi nilai `first = "Jane"` dan `last = "Doe"` yang kemudian ditampilkan di halaman web menggunakan template Chameleon.

---

## Analisis
1. Ekstraksi Data URL (config.add_route):
- Definisi route di __init__.py diubah menjadi config.add_route('home', '/howdy/{first}/{last}').
- Bagian dalam kurung kurawal ({...}) adalah Replacement Patterns. Ketika Pyramid mencocokkan URL seperti /howdy/Jane/Doe, ia secara otomatis mengekstrak Jane dan Doe sebagai data variabel.
2. Akses Data View (request.matchdict):
- Data yang diekstrak oleh router disimpan dalam dictionary khusus pada objek request: self.request.matchdict.
- View mengakses data ini menggunakan kunci yang sesuai dengan nama pattern di route (self.request.matchdict['first']).
3. Penggunaan Data Template:
- Data dari matchdict dilewatkan ke dictionary yang dikembalikan view ({'first': first, 'last': last}).
- Template (home.pt) kemudian menggunakan sintaks Chameleon (${first}, ${last}) untuk menyisipkan data tersebut ke dalam HTML.
4. Dampak pada Pengujian:
- Unit Test: Untuk menguji view yang bergantung pada data URL, DummyRequest harus diinisialisasi secara manual dengan data di request.matchdict sebelum view dipanggil. Ini mensimulasikan hasil dari proses routing Pyramid.
- Functional Test: Test cukup mengirim request ke URL yang memiliki data variabel (misalnya /howdy/Jane/Doe), dan WebTest memastikan response body mengandung data tersebut.

## Extra Credit
- Apa yang terjadi jika Anda pergi ke URL http://localhost:6543/howdy? Apakah ini hasil yang Anda harapkan?
* Hasil: Pyramid akan mengembalikan status code 404 Not Found.
* Ekspektasi: Hasil ini sudah benar dan diharapkan, karena route yang didefinisikan (/howdy/{first}/{last}) memerlukan dua segmen tambahan. URL /howdy tidak cocok dengan pattern yang memerlukan tiga segmen. Jika kita ingin /howdy juga bekerja, kita perlu mendefinisikan route tambahan (misalnya config.add_route('howdy_base', '/howdy')).

---

## Kesimpulan
Percobaan ini membuktikan bahwa sistem **routing Pyramid** sangat efisien untuk menangani URL dinamis menggunakan pola pengganti. Dengan `matchdict`, data dari URL dapat diakses dengan mudah dan digunakan di dalam view. Pendekatan ini membantu membangun aplikasi web yang lebih modular, mudah diatur, dan mendukung desain URL yang lebih bersih serta informatif.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 19 05 27_69551574](https://github.com/user-attachments/assets/5f12a3f7-487b-45fb-a4c3-7a59c6fb1139)

![Gambar WhatsApp 2025-11-12 pukul 19 06 17_d4075409](https://github.com/user-attachments/assets/30f03815-3ce7-48af-8f5b-9c17a55240a7)


