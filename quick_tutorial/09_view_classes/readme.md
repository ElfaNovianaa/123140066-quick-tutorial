# Percobaan 09 – Organizing Views With View Classes

Dokumen ini menjelaskan restrukturisasi *view* Pyramid dari fungsi mandiri menjadi **metode dalam *view class*** untuk pengelompokan logis dan berbagi konfigurasi.

---

## Deskripsi Singkat

Percobaan ini mengubah fungsi *view* individual menjadi metode di dalam kelas **`TutorialViews`**. Tujuan utamanya adalah untuk mengelompokkan *view* yang terkait. Kami menggunakan dekorator tingkat kelas **`@view_defaults(renderer='home.pt')`** untuk memusatkan konfigurasi *renderer* yang sama, sehingga menghindari pengulangan pada setiap metode *view* individual. Perubahan ini memerlukan pembaruan pada unit tes untuk mencerminkan pola instansiasi kelas.

---

## Analisis 
1. Tujuan View Classes
Mengelompokkan *view* ke dalam kelas menawarkan beberapa keuntungan struktural:
* **Pengelompokan Logis:** *View* yang terkait dengan sumber daya atau operasi yang sama (misalnya, `home` dan `hello`) menjadi terorganisir dalam satu unit (`TutorialViews`).
* **Berbagi State & Helpers (Di Eksplorasi Lebih Lanjut):** Meskipun tidak digunakan di langkah ini, *view class* memungkinkan penyimpanan *state* (di `__init__`) dan *helper method* yang dapat dibagikan oleh semua *view method*.
* **Sentralisasi Konfigurasi:** Mengurangi pengulangan deklarasi.

2. Centralized Configuration (`@view_defaults`)
Dekorator **`@view_defaults`** ditempatkan pada level kelas. Ini menetapkan nilai *default* untuk konfigurasi *view* pada semua metode di dalamnya. Dalam kasus ini, semua metode *view* di `TutorialViews` secara *default* akan menggunakan **`renderer='home.pt'`**, kecuali jika di-*override* secara eksplisit pada metode tertentu.

3. Konvensi Instansiasi
Ketika Pyramid mencocokkan *route* ke metode *view* di dalam kelas, prosesnya melibatkan dua langkah:
1. Pyramid membuat *instance* dari `TutorialViews`, meneruskan objek `request` yang masuk ke metode **`__init__(self, request)`**.
2. Kemudian, Pyramid memanggil metode *view* yang sesuai pada *instance* tersebut (misalnya, `inst.home()`).

4. Pembaruan Unit Testing
Pola *view class* mengharuskan perubahan pada *unit test*. Alih-alih memanggil fungsi secara langsung, tes kini harus:
1. Membuat `request` tiruan (`testing.DummyRequest()`).
2. Membuat *instance* dari *view class* dengan *request* tiruan (`inst = TutorialViews(request)`).
3. Memanggil metode *view* yang diuji pada *instance* (`response = inst.home()`).

---

## Kesimpulan

Konversi dari fungsi *view* ke **View Classes** adalah langkah awal menuju struktur aplikasi Pyramid yang lebih ambisius dan terorganisir. Pola ini memungkinkan pengelompokan *view* yang terkait secara logis dan menyederhanakan konfigurasi melalui penggunaan **`@view_defaults`** di tingkat kelas, meletakkan dasar untuk fitur berbagi logika dan *state* yang akan dieksplorasi di langkah tutorial berikutnya.

--- 

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 18 35 11_78496e1b](https://github.com/user-attachments/assets/95f6b38b-112b-4f10-a55c-a5cd655f71f6)

![Gambar WhatsApp 2025-11-12 pukul 18 36 05_a7347379](https://github.com/user-attachments/assets/db12a7be-84d1-46ac-92da-898774da26a4)

![Gambar WhatsApp 2025-11-12 pukul 18 36 18_c0cf85d8](https://github.com/user-attachments/assets/91a89f2e-de46-4219-acea-7d9baf193131)




