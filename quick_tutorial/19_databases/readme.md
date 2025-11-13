# Percobaan 19 – Databases Menggunakan SQLAlchemy

Dokumen ini menjelaskan integrasi **SQLAlchemy ORM** dengan database **SQLite** untuk menggantikan penyimpanan data berbasis *dictionary* di memori dengan penyimpanan persisten dan transaksional.

---

## Deskripsi Singkat

Percobaan ini mengintegrasikan **SQLAlchemy** sebagai ORM dan menggunakan **SQLite** sebagai *backend* database. Kami menambahkan *dependencies* penting seperti `pyramid_tm` dan `zope.sqlalchemy` untuk mengaktifkan manajemen transaksi otomatis (**Transactional Management**). Kami mendefinisikan model data (`Page`) dan membuat *console script* khusus (`initialize_tutorial_db`) untuk membuat skema database dan mengisi data awal. Akhirnya, *view* yang ada dimodifikasi untuk berinteraksi dengan database menggunakan *query* SQLAlchemy, menggantikan *dictionary* global sebelumnya.

---

## Analisis 
1. Integrasi ORM dan Persistensi
* **SQLAlchemy ORM:** Digunakan untuk memetakan kelas Python (`Page`) ke tabel database (`wikipages`). Ini memungkinkan *developer* berinteraksi dengan database menggunakan objek Python alih-alih menulis SQL mentah.
* **Konfigurasi Database:** URL koneksi database (`sqlite:///%(here)s/sqltutorial.sqlite`) ditentukan di **`development.ini`**, yang merupakan praktik terbaik untuk memisahkan konfigurasi dari kode.
* **Bootstrapping Database:** Fungsi `engine_from_config` di `__init__.py` membaca konfigurasi database dan membuat *engine* koneksi.

2. Manajemen Transaksi Otomatis (Transaction Management)
Pyramid sangat mendukung *transaksi* dengan mengadopsi **Transactional Management (TM)**.

* **Dependencies Kunci:**
    * **`pyramid_tm`**: Menyediakan "Tween" yang secara otomatis mengelola siklus hidup *transaction* di setiap *request* web. Jika *view* berhasil, *transaction* di-*commit* secara otomatis; jika terjadi *error*, *transaction* di-*abort* (di-*rollback*). Ini membebaskan *developer* dari manajemen transaksi manual di *view code*.
    * **`zope.sqlalchemy`**: Menjembatani *transaction manager* SQLAlchemy dengan *transaction manager* yang digunakan oleh `pyramid_tm`.

3. Console Scripts
* **`initialize_tutorial_db`**: Skrip *command-line* yang didaftarkan melalui `console_scripts` di `setup.py`. Skrip ini berfungsi sebagai alat administrasi untuk menginisialisasi database (*create tables*) dan mengisi data awal.
* **Transaction Manual:** Di dalam skrip *console*, karena eksekusi tidak terjadi di dalam *web request* (yang otomatis memiliki *tween* TM), kita harus menggunakan `with transaction.manager:` secara eksplisit untuk menjamin operasi *commit* atau *rollback* yang benar.

4. Peran Model (`models.py`)
File `models.py` mendefinisikan:
* **Session Setup:** Menggunakan `scoped_session` dan `sessionmaker` untuk membuat `DBSession` yang terikat pada *thread* dan mendaftarkannya dengan `zope.sqlalchemy`.
* **Model Deklaratif:** Kelas `Page` diwariskan dari `Base = declarative_base()` untuk mendefinisikan skema tabel.

---

## Extra Credit

1. Mengapa begitu banyak kode? Mengapa tidak bisa *magic* dalam dua baris?**
Banyaknya kode di awal (terutama di `setup.py` dan `__init__.py`) diperlukan karena:
* **Fleksibilitas Pyramid:** Pyramid memilih untuk tidak memaksakan ORM atau mekanisme *transaction* tertentu. *Developer* harus secara eksplisit **memilih** dan **mengintegrasikan** komponen yang dibutuhkan (SQLAlchemy, `pyramid_tm`, `zope.sqlalchemy`).
* **Transactional Safety:** Pengaturan `pyramid_tm` dan `zope.sqlalchemy` menjamin *transactional safety* (keamanan transaksi) yang kuat, memastikan integritas data dalam lingkungan *multithread* web. *Magic* yang sederhana seringkali mengorbankan fleksibilitas dan keamanan ini.

2. Berikan coba pada tombol yang menghapus halaman *wiki*.**
Untuk menghapus halaman, *view* akan menggunakan *query* SQLAlchemy dan kemudian mengeluarkan *redirect*.
1.  **View Method Delete:** Buat metode baru di `WikiViews` dengan predikat `request_method='POST'`:
    ```python
    from pyramid.httpexceptions import HTTPFound
    from .models import DBSession, Page
    # ...

    @view_config(route_name='wikipage_delete', request_method='POST') 
    def wikipage_delete(self):
        uid = int(self.request.matchdict['uid'])
        # Query: Cari objek Page berdasarkan UID
        page = DBSession.query(Page).filter_by(uid=uid).one()
        # Hapus objek dari session
        DBSession.delete(page) 
        # Redirect ke halaman utama (transaction otomatis commit)
        return HTTPFound(self.request.route_url('wiki_view'))
    ```

---

## Kesimpulan

Percobaan ini berhasil memindahkan aplikasi *wiki* dari penyimpanan memori ke penyimpanan persisten berbasis database menggunakan **SQLAlchemy**. Dengan mengintegrasikan **`pyramid_tm`** dan **`zope.sqlalchemy`**, kita menciptakan arsitektur yang kuat di mana manajemen transaksi dilakukan secara otomatis oleh *framework*, memungkinkan *developer* untuk berfokus pada logika aplikasi dan *query* data (CRUD) menggunakan sintaks ORM yang intuitif. Pembuatan *console script* juga memperluas fungsionalitas aplikasi di luar *web request* untuk tugas administrasi.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 22 19 59_f345e168](https://github.com/user-attachments/assets/e0361e8c-200d-4a70-a176-e0b96e78eca8)

![Gambar WhatsApp 2025-11-12 pukul 22 20 58_c7fd3cac](https://github.com/user-attachments/assets/d5397041-cbf7-4e20-afa5-cffd5e4ca5ff)

