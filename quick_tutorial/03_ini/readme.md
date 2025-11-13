# Percobaan 3 – Application Configuration with .ini Files

## Deskripsi Singkat
Pada percobaan ini, kita mulai menggunakan file konfigurasi `.ini` dan perintah `pserve` milik Pyramid.  
Pendekatan ini memisahkan konfigurasi dari kode, menjadikan aplikasi lebih mudah dikelola, fleksibel, dan sesuai dengan standar pengembangan aplikasi berbasis WSGI.

---

## Tujuan
1. Menambahkan entry point dalam `setup.py` untuk mendefinisikan lokasi WSGI app.  
2. Membuat file konfigurasi `.ini` untuk menjalankan aplikasi.  
3. Memindahkan kode inisialisasi aplikasi ke dalam `__init__.py`.  
4. Menjalankan aplikasi menggunakan perintah `pserve`.  

---

## Langkah-langkah Percobaan

### 1. Menyalin hasil percobaan sebelumnya
```bash
cd ..
cp -r package ini
cd ini
```
2. Membuat file setup.py
```bash
from setuptools import setup

requires = [
    'pyramid',
    'waitress',
]

setup(
    name='tutorial',
    install_requires=requires,
    entry_points={
        'paste.app_factory': [
            'main = tutorial:main'
        ],
    },
)
```
3. Instalasi proyek
```bash
$VENV/bin/pip install -e .
```
5. Membuat file konfigurasi development.ini
[app:main]
```bash
use = egg:tutorial

[server:main]
use = egg:waitress#main
listen = localhost:6543

5. Memindahkan kode aplikasi ke tutorial/__init__.py
from pyramid.config import Configurator
from pyramid.response import Response

def hello_world(request):
    return Response('<body><h1>Hello World!</h1></body>')

def main(global_config, **settings):
    config = Configurator(settings=settings)
    config.add_route('hello', '/')
    config.add_view(hello_world, route_name='hello')
    return config.make_wsgi_app()
```
6. Menghapus file app.py
```bash
rm tutorial/app.py
```
8. Menjalankan aplikasi dengan pserve
```bash
$VENV/bin/pserve development.ini --reload
```
Buka browser dan akses:
```bash
http://localhost:6543/
```

# Analisis Konfigurasi Aplikasi Pyramid (development.ini & Struktur Kode)

Dokumen ini menjelaskan fungsi utama dari file konfigurasi `development.ini`, alasan pemindahan kode *startup* ke `__init__.py`, dan menjawab beberapa pertanyaan kunci terkait struktur proyek berbasis Pyramid dan Setuptools.

---

## Konsep Dasar `development.ini` dan `pserve`

File `development.ini` adalah *blueprint* yang digunakan oleh perintah `pserve` untuk mem-*boot* aplikasi.

* **[app:main]:** Menunjuk ke *entry point* aplikasi WSGI yang sebenarnya.
    * `use = egg:tutorial` → Memanggil *entry point* bernama `main` yang didefinisikan dalam paket `tutorial` di file `setup.py`.

* **[server:main]:** Mengatur server WSGI yang akan digunakan (misalnya, Waitress).
    * `listen = localhost:6543` → Mengatur *host* dan *port* untuk server.

File ini juga bertanggung jawab untuk menangani **konfigurasi logging** yang digunakan oleh kerangka kerja Pyramid.

> **Opsi `--reload`:** Penggunaan `--reload` pada `pserve` selama pengembangan sangat penting, karena membuat aplikasi otomatis *restart* setiap kali ada perubahan pada file Python atau `.ini`.

---

## Mengapa Kode Startup Dipindahkan ke `__init__.py`

Kode *startup* utama (yang biasanya ada di `app.py`) dipindahkan ke `__init__.py` untuk mengikuti **struktur standar paket Python** dan memudahkan integrasi dengan Setuptools:

1.  **Sesuai Struktur Pyramid:** Memungkinkan Pyramid untuk mengetahui titik awal aplikasi (`main`) langsung dari *package* (`tutorial`) tanpa perlu mengeksekusi file Python spesifik.
2.  **Entry Point Setuptools:** Gaya ini mempermudah proses instalasi dan integrasi aplikasi melalui *entry point* yang didefinisikan di `setup.py`.

---

## Pertanyaan Ekstra (Extra Credit)

### 1. Bisakah Konfigurasi `.ini` Diganti dengan Python?

Ya, konfigurasi dari `.ini` **bisa** diganti sepenuhnya dengan kode Python, menggunakan konstruktor `Configurator(settings=...)`.

Namun, menggunakan file `.ini` sangat direkomendasikan karena:
* **Pemisahan:** Memisahkan kode logika dan konfigurasi.
* **Fleksibilitas:** Konfigurasi dapat diubah tanpa menyentuh dan menyebarkan ulang logika program.
* **Pengelolaan Lingkungan:** Memungkinkan pengelolaan konfigurasi yang berbeda untuk lingkungan **development**, **testing**, dan **production**.

### 2. Apakah Mungkin Memiliki Beberapa File `.ini`?

**Ya**, ini adalah praktik yang umum dan dianjurkan.
Contoh:
* `development.ini`: Untuk pengembangan lokal, dengan mode *debug* aktif.
* `production.ini`: Untuk server produksi, dengan pengaturan *logging* dan *performa* yang berbeda.

Hal ini memudahkan *switching* antara konfigurasi tanpa mengubah kode dan menyesuaikan parameter seperti *port*, *database*, atau *middleware* sesuai lingkungan kerja.

### 3. Mengapa `__init__.py` Tidak Disebutkan di `setup.py`?

Dalam *entry point* Setuptools (`main = tutorial:main`), kita tidak perlu menyebutkan `__init__.py` karena:

* **Konvensi Python:** Ketika kita mereferensikan modul atau fungsi di tingkat *package* (misalnya `tutorial:main`), Python secara otomatis mencari fungsi `main()` di dalam modul utama *package* tersebut, yaitu `tutorial/__init__.py`.

### 4. Apa Fungsi dari `**settings` dalam Fungsi `main()`?

* **Fungsi:** Parameter `**settings` menerima semua pasangan *key-value* konfigurasi dari file `.ini` (di bawah bagian `[app:main]`) dan meneruskannya ke fungsi `main()` aplikasi dalam bentuk sebuah **dictionary**.
* **Arti Tanda `**`:** Simbol `**` dalam Python adalah operator untuk **dictionary unpacking** (*membongkar dictionary*), yang memungkinkan argumen pasangan *key-value* diteruskan sebagai argumen bernama (*keyword arguments*) ke dalam sebuah fungsi.

Dalam konteks Pyramid, `**settings` berisi konfigurasi dinamis (seperti *host*, *port*, atau pengaturan *logging*) yang siap digunakan oleh `Configurator` untuk mengatur aplikasi.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 15 39 12_6193edf2](https://github.com/user-attachments/assets/2172c301-3ecc-4535-95ce-5f99d2fa21d7)

