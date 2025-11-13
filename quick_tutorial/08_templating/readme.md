# Percobaan 8 – HTML Generation With Templating

## Deskripsi Singkat
Percobaan ini membahas penggunaan sistem templating pada Pyramid menggunakan *pyramid_chameleon* untuk memisahkan logika program dan tampilan HTML. Hal ini membuat kode lebih bersih, mudah dikelola, dan lebih fleksibel.

---

## Analisis
1. Fungsi Utama Renderer
Renderer adalah mekanisme kunci di Pyramid yang memungkinkan view fokus pada logika.
- View mengembalikan data (misalnya {'name': 'Hello View'}).
- Pyramid melihat argumen renderer='home.pt' pada @view_config.
- Renderer memproses data menggunakan template home.pt.
- Renderer secara otomatis mengemas hasil HTML ke dalam objek pyramid.response.Response HTTP yang dikirim ke client.
2. Pengujian yang Berfokus pada Data
Penerapan templating memengaruhi Unit Test:
- Unit Test: Berubah dari memeriksa body HTML menjadi hanya memeriksa struktur dan nilai data yang dikembalikan oleh view (kontrak data).
```bash
self.assertEqual('Home View', response['name'])
```
Functional Test: Tetap menguji hasil akhir HTML, memverifikasi bahwa renderer telah menyuntikkan data dengan benar
3. Keuntungan Modularitas
Memisahkan view dan template menciptakan kode yang lebih modular dan lebih mudah dirawat (Maintenance). Perubahan desain HTML tidak memerlukan modifikasi pada kode Python, dan sebaliknya.

## Kesimpulan
Percobaan ini menunjukkan bahwa penggunaan templating pada Pyramid sangat membantu dalam memisahkan logika program dari tampilan HTML. Pendekatan ini tidak hanya membuat kode lebih bersih dan mudah dipelihara, tetapi juga mempercepat proses pengembangan. Dengan memanfaatkan `pyramid_chameleon`, setiap perubahan pada tampilan dapat dilakukan langsung di file template tanpa perlu mengubah logika Python.

---

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 17 05 04_eef682be](https://github.com/user-attachments/assets/ab8a5945-de97-4865-834f-c9d6e806c548)

![Gambar WhatsApp 2025-11-12 pukul 17 06 35_01ac303a](https://github.com/user-attachments/assets/a010bde3-335b-4e33-bfdf-5c6070d53208)

![Gambar WhatsApp 2025-11-12 pukul 17 06 48_0e5b8c9c](https://github.com/user-attachments/assets/da33c417-824c-4edb-98f9-0d57415a09e7)


