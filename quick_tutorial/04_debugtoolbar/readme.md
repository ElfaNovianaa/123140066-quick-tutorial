# Percobaan 04 – Easier Development with Debug Toolbar

Dokumen ini mencakup langkah-langkah dan analisis untuk mengintegrasikan **`pyramid_debugtoolbar`** ke dalam proyek Pyramid, sebuah *add-on* esensial yang meningkatkan pengalaman pengembangan dan *debugging* aplikasi.

---

## Deskripsi Singkat

**`pyramid_debugtoolbar`** adalah *add-on* resmi Pyramid yang menyediakan *toolbar* interaktif di *browser*. Toolbar ini menampilkan informasi penting secara visual dan informatif mengenai setiap *request*, *response*, konfigurasi aplikasi, serta menyediakan *traceback* error yang interaktif.

---

## Langkah-Langkah Integrasi

Berikut adalah langkah-langkah untuk menambahkan dan mengaktifkan `pyramid_debugtoolbar`:

1. Salin dan Siapkan Proyek
```bash
cd ..
cp -r ini debugtoolbar
cd debugtoolbar
```
2. Tambahkan Dependency di setup.py
```bash
from setuptools import setup

requires = [
    'pyramid',
    'waitress',
    # ... dependencies lainnya
]

dev_requires = [
    # Tambahkan di sini
    'pyramid_debugtoolbar',
]

setup(
    name='tutorial',
    install_requires=requires,
    extras_require={
        'dev': dev_requires, # Didefinisikan di sini
    },
    entry_points={
        'paste.app_factory': [
            'main = tutorial:main'
        ],
    },
)
```
3. Instal Proyek dan Dependency Development
Gunakan Setuptools Extras untuk menginstal dependency pengembangan (dev) secara opsional:
```Bash
$VENV/bin/pip install -e ".[dev]"
```
4. Tambahkan Konfigurasi di development.ini
Aktifkan add-on melalui file konfigurasi .ini menggunakan direktif pyramid.includes.
```bash
Ini, TOML

[app:main]
use = egg:tutorial
pyramid.includes =
    pyramid_debugtoolbar ; Aktifkan toolbar di sini

[server:main]
use = egg:waitress#main
listen = localhost:6543
```
5. Jalankan Server
Jalankan server dengan auto reload dan akses aplikasi:
```Bash
$VENV/bin/pserve development.ini --reload
```
Akses http://localhost:6543/. Toolbar akan muncul di sisi kanan layar browser.

## Analisis
1. Fungsi utama pyramid_debugtoolbar
pyramid_debugtoolbar adalah add-on resmi Pyramid yang mempermudah proses debugging dengan menyediakan tampilan visual yang interaktif di browser. Toolbar ini menampilkan informasi penting seperti request, response, konfigurasi route, dan traceback error.
2. Integrasi dengan Pyramid
Add-on ini dimasukkan melalui konfigurasi file .ini menggunakan direktif:
```bash
pyramid.includes = pyramid_debugtoolbar
```
Selain itu, dapat juga dilakukan secara imperatif di __init__.py menggunakan:
```bash
config.include('pyramid_debugtoolbar')
```
Namun pendekatan konfigurasi .ini lebih disukai karena tidak perlu mengubah kode saat ingin mengaktifkan atau menonaktifkan toolbar.
3. Fungsi Setuptools Extras
extras_require di setup.py memungkinkan kita menambahkan dependency opsional seperti pyramid_debugtoolbar hanya untuk lingkungan pengembangan (dev).
Dengan perintah:
```bash
pip install -e ".[dev]"
```
kita menginstal dependency tambahan tanpa memengaruhi dependency utama proyek produksi.
4. Perilaku Debug Toolbar
Toolbar menambahkan potongan kecil HTML/CSS ke halaman (tepat sebelum </body>) untuk menampilkan tombol debug di sisi kanan layar.
Jika terjadi error, toolbar akan menampilkan traceback interaktif yang memudahkan analisis kesalahan.

## Extra Credit
1. Mengapa pyramid_debugtoolbar ditambahkan ke dev_requires bukan ke requires?
Karena pyramid_debugtoolbar hanya dibutuhkan dalam tahap pengembangan, bukan di produksi.
Menempatkannya di dev_requires membuat dependensi ini opsional, menjaga agar versi produksi tetap ringan dan bebas dari tool debugging.
2. Eksperimen Bug:
Ubah kode di __init__.py atau file view Anda dari:
```bash
def hello_world(request):
    return Response('<body><h1>Hello World!</h1></body>')
```
menjadi:
```bash
def hello_world(request):
    return xResponse('<body><h1>Hello World!</h1></body>')
```
Lalu buka kembali:
```bash
http://localhost:6543/
```

## Kesimpulan
- pyramid_debugtoolbar adalah add-on penting untuk debugging selama pengembangan.
- Dapat diaktifkan hanya melalui konfigurasi .ini tanpa ubah kode.
- extras_require memungkinkan pemisahan dependensi development dan production.
- Toolbar memberikan visualisasi error dan request-response yang sangat membantu developer.
