# Percobaan 06 – Functional Testing dengan WebTest

## Deskripsi Singkat
Percobaan ini menjelaskan implementasi **Pengujian Fungsional (Functional Testing)** menggunakan library **`WebTest`** di proyek Pyramid. Pengujian fungsional mensimulasikan interaksi pengguna dengan aplikasi secara menyeluruh, memastikan seluruh alur aplikasi bekerja dengan benar.

**Functional Test** berbeda dari *unit test* karena ia menguji *seluruh alur aplikasi*—dari menerima *request* hingga menghasilkan *response* HTML lengkap—tanpa perlu menjalankan server HTTP aktual (seperti Waitress atau Gunicorn).

`WebTest` berperan sebagai **simulasi *browser*/** *client* yang berinteraksi langsung dengan aplikasi WSGI, menghasilkan pengujian *end-to-end* yang efisien.

---

## Langkah-Langkah Implementasi

1. Salin dan Siapkan Proyek
```bash
cd ..
cp -r unit_testing functional_testing
cd functional_testing
```
Tambahkan WebTest ke Dependency di setup.py
Tambahkan webtest ke dalam bagian dependency pengembangan (dev_requires).
File: setup.py
```bash
from setuptools import setup

# ...

dev_requires = [
    'pyramid_debugtoolbar',
    'pytest',
    'webtest', # Tambahkan webtest di sini
]

setup(
    # ...
    extras_require={
        'dev': dev_requires,
    },
    # ...
)
```
3. Instal Proyek dan Dependency Pengujian Fungsional
Instal package proyek beserta dependency pengembangan opsional (dev):
```Bash
$VENV/bin/pip install -e ".[dev]"
```
4. Tambahkan Functional Test ke tutorial/tests.py
Tambahkan kelas TutorialFunctionalTests untuk pengujian fungsional.
File: tutorial/tests.py (Hanya bagian Functional Test)
```bash
# ... (Bagian Unit Test yang sudah ada)

class TutorialFunctionalTests(unittest.TestCase):
    def setUp(self):
        # 1. Import entry point aplikasi (fungsi main)
        from tutorial import main
        # 2. Buat instance aplikasi WSGI
        app = main({})
        # 3. Import TestApp dari WebTest
        from webtest import TestApp
        # 4. Inisialisasi TestApp dengan aplikasi WSGI
        self.testapp = TestApp(app)

    def test_hello_world(self):
        # Kirim GET request ke root URL ('/') dan harapkan status 200
        res = self.testapp.get('/', status=200)
        
        # Asersi: Pastikan body response mengandung HTML yang diharapkan
        self.assertIn(b'<h1>Hello World!</h1>', res.body)
```
5. Jalankan Pengujian
Jalankan pytest. Ia akan secara otomatis menjalankan Unit Test dan Functional Test.
```Bash
$VENV/bin/pytest tutorial/tests.py -q
# Output: ..
#         2 passed in 0.xx seconds
```

## Analisis

1. Perbedaan Unit Test dan Functional Test
- Unit Test hanya menguji fungsi atau komponen tunggal secara terpisah.
- Functional Test menguji seluruh aplikasi dari sudut pandang pengguna (end-to-end).
Dengan WebTest, kita bisa mengirim request HTTP tiruan (self.testapp.get('/')) dan memeriksa hasil response tanpa menjalankan server sebenarnya.
Ini memberikan efisiensi tinggi untuk pengujian skala penuh.
2. Fungsi WebTest
WebTest membuat lapisan simulasi WSGI yang berperan seperti browser mini.
Ia mengirimkan request ke aplikasi, mengeksekusi semua middleware dan view, lalu mengembalikan response lengkap.
Kita kemudian bisa memeriksa status, headers, dan body dari response tersebut.
3. Struktur Kelas Pengujian
- TutorialViewTests → menguji fungsi view secara langsung (unit test).
- TutorialFunctionalTests → menguji seluruh aplikasi dengan request-response nyata (functional test).
4. Integrasi dengan pytest
pytest mendeteksi semua class turunan unittest.TestCase dan menjalankan keduanya bersama-sama.
Hasil test ditampilkan dengan laporan yang ringkas dan waktu eksekusi total.

## Extra Credit
1. Mengapa Functional Test Menggunakan b''?
Dalam kode:
```bash
self.assertIn(b'<h1>Hello World!</h1>', res.body)
```
Kita menggunakan prefix b karena properti res.body pada WebTest mengembalikan data dalam tipe bytes, bukan string (str).
Hal ini karena:
- HTTP response body secara default dikirim sebagai byte stream, bukan teks Python.
- WebTest meniru perilaku server WSGI yang memproses data dalam format byte-level, agar kompatibel dengan standar web.
- Jadi, saat kita ingin membandingkan isi HTML-nya, string pembanding juga harus dalam bentuk bytes (b'') agar Python tidak mengeluarkan TypeError.

## Kesimpulan
- WebTest memungkinkan pengujian end-to-end tanpa menjalankan server HTTP nyata.
- Functional test memastikan seluruh stack aplikasi (routing, view, response) berfungsi dengan baik.
- Prefix b'' digunakan karena response body dikembalikan dalam bentuk bytes.
- Menggabungkan unit test dan functional test memberi cakupan pengujian yang lebih komprehensif.
- Semua test dapat dijalankan otomatis dengan pytest, mempercepat proses debugging dan validasi kode.

## Output Percobaan 
![Gambar WhatsApp 2025-11-12 pukul 16 10 52_1bb65599](https://github.com/user-attachments/assets/9d63a140-7cc8-4e91-bdb2-c4a2a165266e)



