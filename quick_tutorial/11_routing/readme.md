# Percobaan 11 - Dispatching URLs To Views With Routing

## Deskripsi Singkat
Percobaan ini berfokus pada mekanisme **routing** di framework Pyramid, yaitu proses mencocokkan pola URL dengan fungsi atau kelas view yang sesuai. Dalam praktik ini, kita membuat route yang dapat menangkap bagian tertentu dari URL dan mengubahnya menjadi data yang bisa diakses melalui `request.matchdict`. Dengan begitu, bagian URL seperti `/howdy/Jane/Doe` dapat secara otomatis diuraikan menjadi nilai `first = "Jane"` dan `last = "Doe"` yang kemudian ditampilkan di halaman web menggunakan template Chameleon.

---

## Analisis
1. Apa itu `request.matchdict`?
`request.matchdict` adalah sebuah **dictionary** khusus yang diisi secara otomatis oleh *router* Pyramid.
* **Fungsi:** Ia menyimpan nilai-nilai yang diekstrak dari *path* URL berdasarkan ***Replacement Patterns*** (ditandai dengan kurung kurawal `{...}`) yang didefinisikan dalam `config.add_route()`.
* **Contoh:** Untuk *route* yang didefinisikan sebagai `/howdy/{first}/{last}`, URL `/howdy/John/Smith` akan menghasilkan `matchdict` sebagai berikut:
    ```python
    {'first': 'John', 'last': 'Smith'}
    ```
2. Perbedaan Kunci: `matchdict` vs. `params`
Sangat penting untuk membedakan sumber data yang berbeda yang tersedia pada objek *request*:
| Fitur | `request.matchdict` | `request.params` |
| :--- | :--- | :--- |
| **Sumber Data** | **URL Path** (misalnya, `/user/{id}`) | **Query String** (misalnya, `?name=alice`) |
| **Definisi** | Didefinisikan secara eksplisit di `config.add_route()` | Disediakan secara implisit oleh *client* (browser) |
### 3. Penanganan URL yang Tidak Cocok
Router Pyramid sangat spesifik dalam pencocokan *path* URL:
* Jika *route* didefinisikan sebagai `/howdy/{first}/{last}`, mengunjungi URL dasar `/howdy` akan menghasilkan *status code* **404 Not Found**.
* Hal ini terjadi karena URL tersebut secara eksplisit mengharuskan adanya dua segmen tambahan (`{first}` dan `{last}`).

---

## Dampak pada Pengujian Unit

Saat menulis Unit Test untuk *view* yang bergantung pada data dari URL *path*, kita harus mensimulasikan hasil dari proses *routing* yang dilakukan Pyramid:

* Karena Unit Test tidak melalui proses *routing* sesungguhnya, data harus **disuntikkan secara manual** ke `request.matchdict` pada `testing.DummyRequest()` sebelum memanggil *view*:

```python
request.matchdict['first'] = 'First'
request.matchdict['last'] = 'Last'
```
Hasil Akhir
Aplikasi berhasil membaca data yang disematkan langsung di URL path (/howdy/nama/belakang) dan menampilkannya di halaman web. Semua pengujian Unit dan Fungsional yang memvalidasi ekstraksi data URL berhasil dijalankan.
```Bash
$VENV/bin/pytest tutorial/tests.py -q
..
2 passed in 0.xx seconds
```
## Extra Credit
Apa yang terjadi jika Anda pergi ke URL http://localhost:6543/howdy? Apakah ini hasil yang Anda harapkan?
* Hasil: Pyramid akan mengembalikan status code 404 Not Found.
* Ekspektasi: Hasil ini sudah benar dan diharapkan, karena route yang didefinisikan (/howdy/{first}/{last}) memerlukan dua segmen tambahan. URL /howdy tidak cocok dengan pattern yang memerlukan tiga segmen. Jika kita ingin /howdy juga bekerja, kita perlu mendefinisikan route tambahan (misalnya config.add_route('howdy_base', '/howdy')).

---

## Kesimpulan
Percobaan ini membuktikan bahwa sistem **routing Pyramid** sangat efisien untuk menangani URL dinamis menggunakan pola pengganti. Dengan `matchdict`, data dari URL dapat diakses dengan mudah dan digunakan di dalam view. Pendekatan ini membantu membangun aplikasi web yang lebih modular, mudah diatur, dan mendukung desain URL yang lebih bersih serta informatif.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 19 05 27_69551574](https://github.com/user-attachments/assets/5f12a3f7-487b-45fb-a4c3-7a59c6fb1139)

![Gambar WhatsApp 2025-11-12 pukul 19 06 17_d4075409](https://github.com/user-attachments/assets/30f03815-3ce7-48af-8f5b-9c17a55240a7)




