# Percobaan 05 – Unit Tests dan pytest

## Deskripsi Singkat

Percobaan ini menjelaskan integrasi **Unit Testing** ke dalam proyek Pyramid menggunakan *framework* **`pytest`** dan modul `pyramid.testing`. Tujuannya adalah memastikan kebenaran fungsi kode tanpa perlu menjalankan aplikasi secara penuh di *browser*.

**Unit Test** adalah proses pengujian bagian terkecil (unit) dari kode, seperti fungsi *view*, untuk memastikan ia bekerja sesuai kontrak yang diharapkan. Dengan bantuan `pytest`, proses pengujian menjadi **otomatis, efisien, dan memberikan laporan *error* yang sangat informatif**.

---

## Langkah-Langkah Implementasi

1. Salin dan Siapkan Proyek
```bash
cd ..
cp -r debugtoolbar unit_testing
cd unit_testing
```
2. Tambahkan pytest ke Dependency di setup.py
Tambahkan pytest ke dalam bagian dependency pengembangan (dev_requires).
```bash
from setuptools import setup

# ...

dev_requires = [
    'pyramid_debugtoolbar',
    'pytest', # Tambahkan pytest di sini
]

setup(
    # ...
    extras_require={
        'dev': dev_requires,
    },
    # ...
)
```
3. Instal Proyek dan Dependency Pengujian
Instal package proyek beserta dependency pengembangan opsional (dev):
```Bash
$VENV/bin/pip install -e ".[dev]"
```
4. Buat File Test Baru
Buat file tutorial/tests.py untuk menampung kelas pengujian.
File: tutorial/tests.py
```bash
import unittest
from pyramid import testing

class TutorialViewTests(unittest.TestCase):
    def setUp(self):
        # Menyiapkan konfigurasi Pyramid sebelum setiap test
        self.config = testing.setUp()

    def tearDown(self):
        # Membersihkan konfigurasi setelah setiap test selesai
        testing.tearDown()

    def test_hello_world(self):
        # Import fungsi view di dalam test untuk menjaga isolasi
        from tutorial import hello_world 

        # Membuat permintaan tiruan (dummy request)
        request = testing.DummyRequest()
        
        # Memanggil fungsi view
        response = hello_world(request)
        
        # Asersi: Memastikan kode status HTTP adalah 200 (OK)
        self.assertEqual(response.status_code, 200)
```
5. Jalankan Pengujian dengan pytest
Jalankan pytest dengan menunjuk ke file test:
```Bash
$VENV/bin/pytest tutorial/tests.py -q
```

## Analisis Konsep Unit Testing
1. Modul pyramid.testing
Modul ini adalah alat bantu utama dari Pyramid untuk pengujian. Ia menyediakan fungsi-fungsi untuk:
- testing.setUp() / testing.tearDown(): Menyiapkan dan membersihkan state konfigurasi Pyramid agar setiap test berjalan dalam lingkungan yang terisolasi.
- testing.DummyRequest(): Membuat objek request tiruan yang dapat diberikan ke fungsi view aplikasi, meniru request HTTP sungguhan.
2. Prinsip Isolasi (Isolation)
Mengimpor fungsi hello_world di dalam metode test_hello_world (bukan di bagian atas file) adalah praktik yang baik. Ini memastikan:
- Unit Murni: Setiap unit test benar-benar terpisah dan tidak ada side-effect dari import yang dapat memengaruhi test lain atau konfigurasi global.
- Modularitas: Pengujian lebih modular dan mudah dipindahkan.
3. Kelebihan Menggunakan pytest
pytest meningkatkan proses pengujian dibandingkan framework standar unittest karena:
- Output Informatif: Laporan error lebih jelas, berwarna, dan mudah dibaca.
- Traceback Detail: Memberikan traceback yang lebih ringkas dan fokus saat terjadi kegagalan (failure).
- Konfigurasi Minimal: Mudah digunakan untuk proyek kecil hingga besar.

## Eksperimen Debugging dan Asersi
- Eksperimen 1: Kegagalan Asersi (Assertion Failure)
Mengubah asersi yang diharapkan dari 200 ke 404:
```bash
self.assertEqual(response.status_code, 404
```
Hasil: pytest akan menampilkan AssertionError yang menunjukkan dengan jelas bahwa nilai aktual (200) tidak sesuai dengan nilai yang diharapkan (404).
- Eksperimen 2: Menguji Isi Body Response
Untuk memastikan konten HTML yang dikembalikan benar, gunakan asersi assertIn pada properti response.body (yang merupakan bytes):
```bash
self.assertIn(b"<h1>Hello World!</h1>", response.body)
```
Pengujian ini memverifikasi kontrak fungsional view, bukan hanya status HTTP-nya.
- Eksperimen 3: Menguji Kegagalan Kode (Bug)
Memasukkan bug (misalnya, memanggil fungsi yang tidak ada) ke dalam view akan menyebabkan test gagal. pytest akan segera menampilkan traceback terperinci, yang jauh lebih efisien daripada menjalankan server dan memeriksa browser secara manual.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 15 52 26_22e51bf2](https://github.com/user-attachments/assets/057fa3fb-3daa-4301-a811-cd1ad0c6aac1)

## Kesimpulan
Unit testing, didukung oleh pytest dan pyramid.testing, adalah bagian krusial dalam siklus pengembangan. Metode ini memastikan bahwa setiap komponen kode (unit) berfungsi sesuai ekspektasi, mencegah regresi kode (kerusakan kode lama akibat perubahan baru), dan sangat meningkatkan efisiensi proses debugging.
