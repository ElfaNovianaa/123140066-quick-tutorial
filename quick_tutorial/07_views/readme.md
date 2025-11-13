# Percobaan 07 – Basic Web Handling dengan Views Terorganisir

---

## Konsep Dasar
percobaan ini menjelaskan restrukturisasi proyek Pyramid untuk memisahkan fungsi *view* ke dalam modul terpisah (`views.py`) dan menggunakan dekorator `@view_config` untuk konfigurasi deklaratif yang lebih rapi dan modular.

Dalam Pyramid, view adalah komponen utama yang menerima permintaan (request) web dan menghasilkan respons (response).
Sebelumnya, semua logika (fungsi view, route, konfigurasi, dan WSGI app) berada di satu file.
Pada percobaan ini, kita memindahkan views ke dalam modul tersendiri (views.py) dan menggunakan dekorator @view_config agar lebih rapi dan modular.

Pendekatan ini memungkinkan Pyramid melakukan pemindaian otomatis (config.scan) untuk menemukan dan mendaftarkan view berdasarkan dekorator yang digunakan.

---

## Langkah Percobaan
```bash
cd ..; cp -r functional_testing views; cd views
$VENV/bin/pip install -e .
```
File tutorial/__init__.py
```bash
from pyramid.config import Configurator

def main(global_config, **settings):
    config = Configurator(settings=settings)
    config.add_route('home', '/')
    config.add_route('hello', '/howdy')
    config.scan('.views')
    return config.make_wsgi_app()


File tutorial/views.py

from pyramid.response import Response
from pyramid.view import view_config

# First view, available at http://localhost:6543/
@view_config(route_name='home')
def home(request):
    return Response('<body>Visit <a href="/howdy">hello</a></body>')

# /howdy
@view_config(route_name='hello')
def hello(request):
    return Response('<body>Go back <a href="/">home</a></body>')
```
File tutorial/tests.py
```bash
import unittest
from pyramid import testing

class TutorialViewTests(unittest.TestCase):
    def setUp(self):
        self.config = testing.setUp()

    def tearDown(self):
        testing.tearDown()

    def test_home(self):
        from .views import home
        request = testing.DummyRequest()
        response = home(request)
        self.assertEqual(response.status_code, 200)
        self.assertIn(b'Visit', response.body)

    def test_hello(self):
        from .views import hello
        request = testing.DummyRequest()
        response = hello(request)
        self.assertEqual(response.status_code, 200)
        self.assertIn(b'Go back', response.body)

class TutorialFunctionalTests(unittest.TestCase):
    def setUp(self):
        from tutorial import main
        app = main({})
        from webtest import TestApp
        self.testapp = TestApp(app)

    def test_home(self):
        res = self.testapp.get('/', status=200)
        self.assertIn(b'<body>Visit', res.body)

    def test_hello(self):
        res = self.testapp.get('/howdy', status=200)
        self.assertIn(b'<body>Go back', res.body)
```
Menjalankan Pengujian
```bash
$VENV/bin/pytest tutorial/tests.py -q
```
Menjalankan Aplikasi
```bash
$VENV/bin/pserve development.ini --reload
```
Buka di browser:
```bash    
http://localhost:6543/
http://localhost:6543/howdy
```

## Analisis
Pada langkah ini, kita menambahkan dua view baru dan memisahkan kode view dari file __init__.py ke modul views.py.
Pyramid melakukan pemindaian otomatis (auto-scan) terhadap dekorator @view_config di dalam modul views.py melalui perintah config.scan('.views').
Kedua view ini saling terhubung:
- Home (route /) menampilkan link menuju halaman hello.
- hello (route /howdy) memiliki link kembali ke halaman utama.
Pendekatan ini menunjukkan bahwa nama URL, nama route, dan nama fungsi view tidak harus sama, karena mapping antar keduanya diatur melalui konfigurasi route.
Selain itu, Pyramid mendukung dua cara konfigurasi:
- Imperative configuration → menggunakan config.add_view()
- Declarative configuration → menggunakan dekorator @view_config
Kedua metode ini menghasilkan hasil akhir yang sama, hanya berbeda dari segi gaya penulisan.

## Extra Credit
1. Apa arti tanda titik (.) pada .views?
Titik (.) di depan views menandakan relative import, yang berarti Pyramid akan mencari modul views di dalam package saat ini (tutorial).
Jika kita menuliskan config.scan('tutorial.views'), itu berarti menggunakan absolute import yang menunjuk ke path penuh modul.
2. Mengapa assertIn lebih baik daripada assertEqual untuk pengujian isi HTML?
assertIn hanya memastikan bahwa teks tertentu ada di dalam response body, tanpa membandingkan keseluruhan isi HTML.
Hal ini membuat pengujian:
- Lebih toleran terhadap perubahan kecil, seperti whitespace atau urutan atribut.
- Tidak mudah gagal meskipun layout HTML berubah sedikit.
Sementara assertEqual terlalu ketat karena membandingkan seluruh string secara identik.

## Kesimpulan
- View dipisahkan dari __init__.py agar struktur proyek lebih modular dan rapi.
- Pyramid menggunakan config.scan() untuk menemukan dekorator @view_config.
- Dua view (home dan hello) berhasil diuji baik secara unit test maupun functional test.
- assertIn digunakan agar pengujian lebih fleksibel terhadap perubahan tampilan HTML.
- Pendekatan declarative configuration (@view_config) memudahkan manajemen banyak view dalam satu aplikasi.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 16 55 18_3d11d63a](https://github.com/user-attachments/assets/e015d30d-5114-4e38-a13e-83d5fa70d35b)

![Gambar WhatsApp 2025-11-12 pukul 16 58 35_74618349](https://github.com/user-attachments/assets/31ea08a6-cb50-4c38-9dd0-04befd329cb4)

![Gambar WhatsApp 2025-11-12 pukul 16 58 52_fb555c4f](https://github.com/user-attachments/assets/0a504c33-4d96-4264-8521-90c2a3bc9301)


