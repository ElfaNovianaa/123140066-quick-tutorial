# Percobaan 4 - Easier Development with Debug Toolbar

## Deskripsi Singkat
Pada percobaan ini, kita menambahkan add-on **pyramid_debugtoolbar** ke dalam proyek Pyramid untuk membantu proses pengembangan dan debugging aplikasi. Add-on ini menampilkan toolbar di sisi kanan browser yang memberikan informasi detail tentang konfigurasi aplikasi, routing, request, dan error yang terjadi.  

## Analisis
Pada percobaan ini, penggunaan pyramid_debugtoolbar membantu proses pengembangan aplikasi Pyramid dengan memberikan tampilan interaktif untuk debugging langsung di browser. Add-on ini menampilkan toolbar di sisi kanan halaman web yang berisi berbagai informasi berguna seperti konfigurasi, route, dan log error. Selain itu, saat terjadi kesalahan dalam aplikasi, toolbar akan menampilkan traceback yang detail, sehingga developer dapat dengan mudah menemukan dan memperbaiki bug.

Dari sisi konfigurasi, pyramid_debugtoolbar diaktifkan melalui development.ini menggunakan pyramid.includes, tanpa perlu mengubah kode utama aplikasi. Dependensi tambahan dikelompokkan di bagian extras_require dengan label [dev] agar hanya digunakan pada tahap pengembangan, bukan produksi. Dengan pendekatan ini, pengelolaan paket menjadi lebih fleksibel dan aman. Toolbar ini menunjukkan bagaimana Pyramid memanfaatkan add-on untuk memperluas fungsionalitas tanpa mengubah arsitektur utama aplikasi.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 16 59 32_eb277d71](https://github.com/user-attachments/assets/d9ef5dd8-0dcf-4778-ac6a-a016cd5d8858)

