# Percobaan 12 – Templating With Jinja2

## Deskripsi Singkat
Dokumen ini mendemonstrasikan fleksibilitas **Pyramid** dalam mendukung berbagai sistem *templating* dengan mengintegrasikan **Jinja2** sebagai *renderer* baru, menggantikan Chameleon.
mendemonstrasikan fleksibilitas Pyramid dalam mendukung berbagai sistem templating dengan mengganti Chameleon dengan Jinja2 menggunakan add-on pyramid_jinja2. Proses ini membuktikan bahwa view code Pyramid sebagian besar tidak terikat pada sistem templating tertentu.

---

##  Analisis
1. Fleksibilitas Renderer Pyramid
Percobaan ini menunjukkan bahwa Pyramid memiliki konsep Renderer yang abstrak. View hanya berinteraksi dengan renderer melalui nama file (ekstensi) atau nama renderer yang terdaftar.
- View (Logika): Mengembalikan {'name': 'Home View'}.
- Renderer (Jembatan): Mendeteksi ekstensi .jinja2 atau .pt dan menggunakan mesin templating yang sesuai untuk memproses data.
2. Pola Konfigurasi Add-on
Untuk mengaktifkan fitur dari add-on pihak ketiga (seperti pyramid_jinja2), Anda harus:
- Menginstal paket (via pip).
- Memanggil config.include(package_name) atau menambahkannya ke pyramid.includes di .ini.
3. Extra Credit: Opsi Konfigurasi
- Dependency: pyramid_jinja2 dapat dimasukkan ke dalam daftar install_requires jika merupakan dependency wajib (seperti yang sudah dilakukan).
- Konfigurasi Deklaratif: Selain config.include(...) (imperatif), kita bisa memuat konfigurasi add-on secara deklaratif di development.ini:
```bash
[app:main]
# ...
pyramid.includes =
    pyramid_debugtoolbar
    pyramid_jinja2
```
Hasil Akhir
Aplikasi berhasil menyajikan halaman menggunakan renderer Jinja2 yang baru, tanpa mengubah logika view Unit Test.
```Bash
$VENV/bin/pytest tutorial/tests.py -q
....
4 passed in 0.xx seconds
```

## Kesimpulan 
percobaan ini berhasil menunjukkan evolusi proyek Pyramid dari setup dasar menjadi aplikasi web yang modular, teruji, dan fleksibel. Kami mengawali dengan konfigurasi server melalui development.ini dan entry point di setup.py, lalu meningkatkan kualitas pengembangan dengan mengintegrasikan pyramid_debugtoolbar dan memisahkan dependency dengan extras_require. Struktur kode ditingkatkan dengan memindahkan view ke views.py dan mengelompokkannya ke dalam Kelas View menggunakan dekorator @view_defaults dan @view_config. Kami menerapkan pengujian komprehensif (Unit dan Fungsional) menggunakan pytest dan WebTest untuk memastikan keandalan. Selain itu, kami mempelajari penanganan Request yang kuat dengan WebOb, termasuk mengekstrak data dari URL path menggunakan request.matchdict melalui Replacement Patterns pada routing. Akhirnya, kami mendemonstrasikan fleksibilitas Pyramid dalam lapisan presentasi dengan berhasil beralih antara renderer Chameleon dan Jinja2, membuktikan bahwa view code dapat berfokus murni pada kontrak data tanpa terikat pada teknologi templating tertentu.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 19 54 41_d1017a24](https://github.com/user-attachments/assets/103133db-eb27-4c04-b3b9-66235a5e934c)

![Gambar WhatsApp 2025-11-12 pukul 19 55 34_7d717948](https://github.com/user-attachments/assets/333492c9-b22f-48b0-b27b-81c21ee2b35b)
