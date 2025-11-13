# Percobaan 08 – HTML Generation With Templating

Dokumen ini menjelaskan cara Pyramid memisahkan logika aplikasi dari presentasi HTML menggunakan sistem *templating* yang *pluggable*, dengan fokus pada integrasi **Chameleon**.

---

## Deskripsi Singkat

Percobaan ini mengubah aplikasi dari *view* yang mengembalikan *string* HTML langsung, menjadi *view* yang hanya mengembalikan **data** (sebuah *dictionary*). Kami mengintegrasikan *add-on* **`pyramid_chameleon`** dan menggunakannya sebagai **Renderer**. Dekorator **`@view_config(renderer='home.pt')`** menghubungkan data yang dikembalikan oleh *view* ke *template* `home.pt`. Ini adalah praktik terbaik untuk menjaga kode Python tetap bersih dan fokus pada logika bisnis.

---

## Analisis dan Konsep Kunci
1. Konsep Renderer Pyramid
Pyramid tidak memaksakan bahasa *templating* tertentu. Sebaliknya, ia menggunakan konsep **Renderer** yang *pluggable* untuk menangani *output* selain *string* atau objek `Response`.

* **Integrasi Renderer:** Kami menambahkan `pyramid_chameleon` sebagai *dependency* di `setup.py` dan mengaktifkannya di `__init__.py` menggunakan `config.include('pyramid_chameleon')`.
* **Penggunaan Renderer:** Di *view*, kita menunjuk file *template* melalui argumen **`renderer='home.pt'`** pada `@view_config`.

2. Pemisahan Logika dan Presentasi
Perubahan kunci adalah pada *view code* (`views.py`):
* **Sebelum:** *View* membuat objek `Response` dan mengisi *body* dengan *string* HTML.
* **Sesudah:** *View* hanya mengembalikan **Python dictionary** (misalnya, `{'name': 'Home View'}`).
* **Proses di Belakang Layar:** Pyramid melihat *dictionary* yang dikembalikan, melihat konfigurasi `renderer`, meneruskan *dictionary* tersebut ke *template* `home.pt`, dan mengembalikan hasil HTML sebagai *response*.

3. Templating Language (Chameleon)
Kami menggunakan **Chameleon** yang syntax-nya ditunjukkan dalam `home.pt`:

```html
<title>Quick Tutorial: ${name}</title>
<body><h1>Hi ${name}</h1></body>
```
---

## Kesimpulan 

Penggunaan sistem Templating melalui Renderer Pyramid adalah praktik fundamental dalam pengembangan web modern. Dengan mengintegrasikan pyramid_chameleon, kami berhasil memisahkan concern antara logika aplikasi (di Python view yang hanya mengembalikan data) dan presentasi (di file template HTML), menghasilkan kode yang lebih bersih, mudah dikelola, dan lebih mudah diuji.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 17 05 04_eef682be](https://github.com/user-attachments/assets/ab8a5945-de97-4865-834f-c9d6e806c548)

![Gambar WhatsApp 2025-11-12 pukul 17 06 35_01ac303a](https://github.com/user-attachments/assets/a010bde3-335b-4e33-bfdf-5c6070d53208)

![Gambar WhatsApp 2025-11-12 pukul 17 06 48_0e5b8c9c](https://github.com/user-attachments/assets/da33c417-824c-4edb-98f9-0d57415a09e7)



