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

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 15 52 26_22e51bf2](https://github.com/user-attachments/assets/057fa3fb-3daa-4301-a811-cd1ad0c6aac1)
