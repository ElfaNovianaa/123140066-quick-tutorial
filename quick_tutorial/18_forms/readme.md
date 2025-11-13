# Percobaan 18 – Forms and Validation with Deform

Dokumen ini menjelaskan integrasi library **Deform** dan **Colander** ke dalam aplikasi Pyramid untuk membuat *form* dinamis, *schema-driven*, dan dilengkapi dengan validasi yang kuat.

---

## Deskripsi Singkat

Percobaan ini berfokus pada penanganan *form* dan validasi. Kami menginstal `Deform` dan *dependency*-nya `Colander`. **Colander** digunakan untuk mendefinisikan *schema* data (`WikiPage`), sementara **Deform** menggunakan *schema* tersebut untuk menghasilkan *form* HTML secara otomatis, termasuk *widget* kaya seperti `RichTextWidget`. Kami menerapkan pola **Self-Posting Form** di mana satu *view* menangani *GET* (menampilkan *form*) dan *POST* (memproses dan memvalidasi *form*), diikuti dengan *redirect* jika validasi berhasil.

---

## Analisis 
1. Integrasi Deform dan Colander
* **Colander (Schema & Validation):** Colander menyediakan kerangka kerja untuk mendefinisikan struktur data menggunakan `colander.MappingSchema`. Setiap *field* didefinisikan sebagai `colander.SchemaNode` dengan tipe data spesifik (misalnya, `colander.String()`). Ini adalah fondasi untuk validasi data.
* **Deform (Form Rendering):** Deform mengambil *schema* Colander dan mengubahnya menjadi objek *form* yang dapat di-*render* ke HTML (`deform.Form(schema)`). Deform juga memungkinkan penyesuaian *widget* (misalnya, `deform.widget.RichTextWidget()`).

2. Penanganan Aset Statis Pihak Ketiga
Untuk memastikan *form* Deform berfungsi dengan *widget* canggih (yang membutuhkan CSS/JS), kami mendaftarkan aset statisnya:

* `config.add_static_view('deform_static', 'deform:static/')`
* Ini menggunakan **Resource Specification** untuk mengambil aset dari *package* `deform` tanpa perlu tahu lokasi fisiknya di *disk*.

3. Pola Self-Posting Form dan Validation
* *View* **`wikipage_add`** dan **`wikipage_edit`** menangani *GET* dan *POST* di URL yang sama.
* Logika *POST* dikontrol oleh `if 'submit' in self.request.params:`.
* **Validasi:** Data *form* divalidasi dengan `self.wiki_form.validate(controls)`.
    * Jika validasi gagal (`deform.ValidationFailure`), *view* menangkap *exception* dan me-*render* kembali *form* dengan pesan *error* dan data yang dimasukkan.
    * Jika validasi berhasil, *view* memproses data (`appstruct`) dan mengeluarkan **`HTTPFound`** (Redirect) ke halaman tampilan baru.

4. Deform Resource Registry
Deform memiliki **Resource Registry** (diakses melalui `self.wiki_form.get_widget_resources()`). Ini memungkinkan *widget* untuk mendeklarasikan file JS dan CSS yang mereka butuhkan. *Template* kemudian mengulanginya (*iterate*) dan menggunakan `request.static_url` untuk menghasilkan tautan yang diperlukan secara dinamis.

---

## Extra Credit

**Berikan coba pada tombol yang mengarah ke *delete view* untuk halaman *wiki* tertentu.**
Untuk membuat *delete* *view* yang spesifik dan terpisah, kita dapat menggunakan konsep **View Predicates** yang sudah dipelajari (Percobaan 15).
1.  **Route Baru:** Daftarkan *route* baru untuk *delete*:
    ```python
    config.add_route('wikipage_delete', '/{uid}/delete')
    ```
2.  **View Method Baru:** Buat metode *view* `wikipage_delete` di `WikiViews`:
    ```python
    from pyramid.httpexceptions import HTTPFound

    @view_config(route_name='wikipage_delete', request_method='POST')
    def wikipage_delete(self):
        uid = self.request.matchdict['uid']
        # Logika: Hapus halaman dari dictionary 'pages'
        del pages[uid] 
        # Redirect ke halaman utama setelah penghapusan
        return HTTPFound(self.request.route_url('wiki_view'))
    ```
3.  **Form/Tautan:** Pada *template* `wikipage_view.pt`, tambahkan *form* minimal dengan metode `POST` yang mengarah ke URL *delete* yang baru.

---

## Kesimpulan

Integrasi **Deform** dan **Colander** menunjukkan filosofi Pyramid: tidak memaksakan satu library *form* pun, melainkan memfasilitasi integrasi *best-of-breed* *library* eksternal. Dengan mendefinisikan *schema* data dan menggunakan pola **Self-Posting Form**, kita berhasil menciptakan *form* yang kuat, divalidasi, dan mampu menangani *response* yang berbeda (tampilan *form* ulang dengan *error* atau *redirect* setelah sukses). Kemampuan untuk mendaftarkan aset statis dari *package* pihak ketiga (`deform:static/`) juga memperkuat sifat Pyramid sebagai *framework* yang fleksibel dan modular.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 22 11 21_de5f8d66](https://github.com/user-attachments/assets/12d7b039-467d-46fc-aee7-651ae62fa61d)

![Gambar WhatsApp 2025-11-12 pukul 22 11 55_7ab56469](https://github.com/user-attachments/assets/0dd5f2da-537e-400e-b08b-fb2d24c3de1c)

