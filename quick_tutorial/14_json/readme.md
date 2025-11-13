# Percobaan 14 – AJAX Development Dengan JSON Renderers

Dokumen ini menjelaskan implementasi *renderer* **JSON** di Pyramid untuk mendukung aplikasi web modern berbasis AJAX, yang meminta data mentah (*data API*) dari *server*.

---

## Deskripsi Singkat

Percobaan ini mendemonstrasikan bagaimana Pyramid dapat melayani format *response* yang berbeda (HTML dan JSON) dari **satu fungsi *view* yang sama**. Dengan mendaftarkan *route* baru dan menggunakan dekorator **`@view_config`** secara berlapis (*stacking*), kita mengarahkan *request* ke URL `/howdy.json` agar menggunakan *renderer* `json`, yang secara otomatis mengubah *dictionary* Python menjadi *string* JSON.

---

## Analisis dan Konsep Kunci
1. View Layer yang Berorientasi Data
Prinsip dasar di sini adalah: **View code hanya mengembalikan data Python** (`dictionary` atau `list`). Karena *view* `hello()` mengembalikan *dictionary*, Pyramid dapat menggunakan *renderer* yang berbeda pada *output* yang sama:
* Jika *route* **`hello`** cocok, ia menggunakan *renderer* *default* **`home.pt`** (HTML).
* Jika *route* **`hello_json`** cocok, ia menggunakan *renderer* **`json`** untuk menghasilkan JSON.
2. Stacked View Configuration
Dekorator **`@view_config`** dapat ditumpuk (*stacked*) pada satu fungsi *view* yang sama. Setiap dekorator menetapkan aturan *matching* (seperti `route_name`) dan *response* (seperti `renderer='json'`) yang berbeda. Ini adalah cara yang efisien untuk membuat *endpoint* API (JSON) dan *endpoint* halaman (HTML) tanpa duplikasi logika bisnis.
3. Fungsi JSON Renderer
JSON *renderer* melakukan tiga tugas utama:
1.  Mengambil *dictionary* Python yang dikembalikan oleh *view*.
2.  Mengubahnya menjadi *string* JSON.
3.  Secara otomatis mengatur *header* **`Content-Type`** *response* ke **`application/json`**.

---

## Extra Credit
1. Dependency:** Kita menginstal `pyramid_jinja2` secara manual. Cara lain untuk membuat asosiasi dependency adalah menambahkannya ke daftar `install_requires` di `setup.py` atau menggunakannya sebagai *extra* yang diperlukan dalam distribusi *package* lain.
2. Konfigurasi Imperatif vs. Deklaratif:** Kita menggunakan `config.include` (imperatif) untuk memuat konfigurasi `pyramid_jinja2`. Cara lain (deklaratif) adalah menambahkannya ke direktif `pyramid.includes` di file konfigurasi `.ini` (misalnya `development.ini`).

---

## Kesimpulan
Penggunaan *JSON Renderer* di Pyramid adalah cara yang ringkas dan efisien untuk membuat *endpoint* API (Data Services). Dengan memanfaatkan fitur *stacked view configuration*, kita dapat melayani berbagai format *response* (HTML untuk *browser* dan JSON untuk *client* AJAX) dari satu fungsi *view* yang berorientasi data, menjaga kode aplikasi tetap bersih dan modular.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 21 24 18_5ef99149](https://github.com/user-attachments/assets/13968c89-e66e-42a8-8a7b-e9795695c640)

![Gambar WhatsApp 2025-11-12 pukul 21 27 25_9235c28f](https://github.com/user-attachments/assets/bb67cbef-0e5c-4831-a239-87f9ae04e29f)
