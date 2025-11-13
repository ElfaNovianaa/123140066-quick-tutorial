# Percobaan 2 – Python Packages for Pyramid Applications

## Deskripsi Singkat
Percobaan ini melanjutkan proyek **Pyramid Hello World** sebelumnya, dengan fokus pada **pengorganisasian kode menggunakan Python package dan project structure yang benar**.  
Tujuannya adalah memahami bagaimana modul Pyramid dikemas dalam bentuk *package* dan diinstal secara lokal (*editable mode*) agar mudah dikembangkan.

---

## Tujuan
1. Membuat direktori Python package yang berisi `__init__.py`.
2. Membuat file `setup.py` untuk mendefinisikan project.
3. Menginstal project dalam *development mode* menggunakan `pip install -e .`.
4. Menjalankan aplikasi Pyramid dengan struktur proyek yang terorganisir.

---

## Langkah-langkah Percobaan

1. Membuat Direktori Proyek
```bash
cd ..
mkdir package
cd package
```
2. Membuat File Setup.py
```bash
from setuptools import setup

requires = [
    'pyramid',
    'waitress',
]

setup(
    name='tutorial',
    install_requires=requires,
)
```
3. Instalasi Proyek dalam Mode Pengembangan
```bash
$VENV/bin/pip install -e .
```
4. Membuat direktori package
```bash
mkdir tutorial
```
5. Membuat file tutorial/__init__.py
```bash
# package
```
6. Membuat file aplikasi tutorial/app.py
```bash
from waitress import serve
from pyramid.config import Configurator
from pyramid.response import Response

def hello_world(request):
    print('Incoming request')
    return Response('<body><h1>Hello World!</h1></body>')

if __name__ == '__main__':
    with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
        app = config.make_wsgi_app()
    serve(app, host='0.0.0.0', port=6543)
```
7. Menjalankan aplikasi
```bash
$VENV/bin/python tutorial/app.py
```
Kemudian buka browser dan akses:
http://localhost:6543/

## Analisis

Percobaan ini menunjukkan pentingnya struktur proyek yang terorganisir dalam pengembangan Python modern.
Sebelumnya kita hanya menjalankan satu file sederhana, sekarang proyek dibentuk dalam struktur package agar dapat diinstal, diuji, dan dikembangkan lebih lanjut.
1. Peran __init__.py
File ini membuat direktori dikenali sebagai package oleh Python. Tanpa file ini, direktori tutorial hanya dianggap sebagai folder biasa, bukan bagian dari namespace Python.
2. Fungsi setup.py
File ini digunakan untuk mendeskripsikan project, seperti nama dan dependensi.
Dengan menjalankan pip install -e ., proyek akan diinstal dalam mode editable, artinya setiap perubahan kode langsung diterapkan tanpa instal ulang.
Hal ini sangat membantu selama proses pengembangan.
3. Menjalankan python tutorial/app.py
Cara ini digunakan di percobaan untuk mempermudah pemahaman alur program, tapi bukan praktik yang disarankan dalam proyek sebenarnya.
Menjalankan modul di dalam package secara langsung bisa menyebabkan masalah pada namespace dan impor relatif.
Pada tahap lebih lanjut, aplikasi Pyramid sebaiknya dijalankan menggunakan command seperti pserve agar lebih sesuai standar WSGI.
4. Perbedaan antara module, package, dan project
- Module adalah satu file Python (.py) seperti app.py.
- Package adalah direktori yang memiliki __init__.py, seperti tutorial.
- Project adalah keseluruhan struktur yang berisi package dan file konfigurasi seperti setup.py.

Kesimpulan Analisis
- Struktur package membuat kode lebih rapi dan mudah dikembangkan.
- File setup.py memungkinkan integrasi dengan sistem packaging Python dan mempermudah instalasi.
- Menjalankan aplikasi langsung dari file masih digunakan hanya untuk pembelajaran.
- Percobaan ini menjadi dasar untuk langkah selanjutnya, yaitu menjalankan aplikasi Pyramid dengan konfigurasi yang lebih profesional.

