# Percobaan 15 – Advanced View Classes dan View Predicates

Dokumen ini menjelaskan penggunaan fitur *View Class* Pyramid secara mendalam untuk mengelompokkan *view*, berbagi *state* dan logika, serta menggunakan **View Predicates** untuk mengarahkan satu URL ke berbagai metode *view* berdasarkan kondisi *request*.

---

## Deskripsi Singkat

Percobaan ini merestrukturisasi *view* untuk menangani empat operasi berbeda (GET halaman utama, GET formulir, POST Save/Edit, dan POST Delete) menggunakan **satu *route* utama** (`/howdy/{first}/{last}`). Semua logika ini dikelompokkan dalam kelas **`TutorialViews`**. Kami menggunakan dekorator **`@view_defaults`** untuk *default route* dan **View Predicates** (`request_method`, `request_param`) pada `@view_config` untuk menentukan metode mana yang akan dieksekusi berdasarkan *request* yang masuk.

---

## Analisis 
1. Pengelompokan dan Berbagi State
Seluruh *view* yang terkait (`hello`, `edit`, `delete`) dikelompokkan dalam **`TutorialViews`**, sementara `home` tetap didefinisikan secara terpisah. Kelas ini memungkinkan pembagian informasi:

* **Berbagi State (`__init__`)**: Variabel seperti `self.view_name = 'TutorialViews'` didefinisikan di `__init__` dan tersedia di semua metode *view* dan *template* (diakses sebagai `${view.view_name}`).
* **Berbagi Logika (`@property`)**: Metode Python **`@property`** (`full_name`) digunakan untuk menghitung nilai dinamis (`first + ' ' + last`) berdasarkan `request.matchdict`.

View Predicates untuk Dispatching
Tujuan utama dari percobaan ini adalah menggunakan **View Predicates** untuk memetakan satu URL *path* (`/howdy/first/last`) ke beberapa metode *view* berbeda:

* **Predikat `request_method='POST'`**: Metode `edit()` akan dieksekusi hanya jika *request* menggunakan metode `POST`.
* **Predikat `request_param='form.delete'`**: Metode `delete()` hanya dieksekusi jika *request* adalah `POST` **dan** mengandung parameter `form.delete` (yang dikirim saat tombol "Delete" diklik). Predikat ini bertindak sebagai filter yang lebih spesifik daripada predikat `POST` biasa.

3. Centralized Configuration (`@view_defaults`)
Dekorator `@view_defaults(route_name='hello')` di tingkat kelas menetapkan bahwa semua metode *view* di dalamnya (kecuali yang di-*override*) akan menggunakan *route* bernama **`hello`** secara *default*. Ini menyederhanakan deklarasi pada setiap metode *view*.

4. URL Generation Fleksibel
Pyramid menyediakan *helper* `request.route_url()` untuk menghasilkan URL secara aman. Contoh: `${request.route_url('hello', first='jane', last='doe')}`. Ini memastikan bahwa URL yang dihasilkan selalu sinkron dengan definisi *route* yang ada dan bebas dari kesalahan penulisan *path* manual.

---

## Extra Credit
1. Mengapa `template` dapat memanggil `${view.full_name}` tanpa kurung?**
Karena `full_name` didekorasi dengan **`@property`** di Python. Dekorator ini mengubah metode menjadi atribut yang diakses seolah-olah itu adalah variabel. Ketika *template* mengaksesnya, kode di balik metode tersebut dieksekusi.

2. Mengapa *view* `edit` tidak menangkap `POST` yang digunakan oleh `delete`?**
Meskipun `edit()` juga memiliki `request_method='POST'`, *view* **`delete()`** memiliki predikat yang **lebih spesifik** (`request_param='form.delete'`). Pyramid selalu mencocokkan *view* berdasarkan urutan spesifisitas. Karena `delete` memiliki predikat *request parameter* yang lebih ketat, ia diprioritaskan dan dieksekusi sebelum *view* `edit`.

3. Cache untuk `@property`?**
Ya. Pyramid menyediakan dekorator **`@reify`** (atau seringkali menggunakan `functools.cached_property` di Python modern) yang dapat digunakan pada properti *view class*. Dekorator ini menghitung nilai properti hanya **sekali** per *request* dan menyimpan hasilnya sebagai atribut *instance* (`self.full_name`), memastikan komputasi ulang tidak terjadi meskipun dipanggil berkali-kali dalam *view* atau *template* yang sama.

4. Bisakah mengasosiasikan lebih dari satu *route* dengan *view* yang sama?**
Ya. Hal ini dilakukan persis seperti yang kita lihat di Percobaan 14 dan Percobaan ini: dengan menumpuk beberapa dekorator **`@view_config`** di atas satu metode *view* yang sama, masing-masing menunjuk ke nama *route* yang berbeda.

5. Perbedaan `request.route_path` dan `request.route_url`?**
* **`request.route_url`**: Menghasilkan **URL lengkap** (termasuk skema dan *hostname*, e.g., `http://localhost:6543/howdy/jane/doe`).
* **`request.route_path`**: Menghasilkan **path relatif** (e.g., `/howdy/jane/doe`). Ini lebih umum digunakan di *template* untuk tautan internal.

---

## Kesimpulan
Penggunaan **View Classes** adalah pola kunci dalam Pyramid untuk membangun aplikasi yang terstruktur. Kelas memungkinkan *view* yang terkait untuk berbagi *state* dan logika, sementara **View Predicates** memberikan kontrol granular atas *dispatching* *request*. Ini memungkinkan satu *route* URL melayani banyak fungsi (GET, POST Edit, POST Delete) dengan kode yang bersih dan terpusat.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 21 35 26_541a054d](https://github.com/user-attachments/assets/336a5ef1-8b78-496b-a455-9ea9b818c00f)

![Gambar WhatsApp 2025-11-12 pukul 21 39 50_df25b770](https://github.com/user-attachments/assets/72cb5398-08da-4b46-90c6-2936f4650f13)

![Gambar WhatsApp 2025-11-12 pukul 21 39 34_ca280040](https://github.com/user-attachments/assets/5591a133-2b7e-4392-981c-831d2666b6b6)

