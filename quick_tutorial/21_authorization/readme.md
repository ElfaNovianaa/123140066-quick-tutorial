# Percobaan 21 – Protecting Resources Dengan Authorization

Dokumen ini menjelaskan implementasi fitur **Authorization (Izin)** di Pyramid untuk melindungi *view* dan sumber daya dengan menentukan izin minimum yang diperlukan untuk mengaksesnya menggunakan **ACLs (Access Control Lists)**.

---

## Deskripsi Singkat

Percobaan ini memperluas sistem keamanan dengan **Authorization**. Kami menetapkan **Root Factory** (`.resources.Root`) yang membawa **ACLs** (`__acl__`) untuk seluruh aplikasi. ACL ini mendefinisikan bahwa *principal* **`group:editors`** memiliki izin **`edit`**. Kami memodifikasi `SecurityPolicy` untuk menentukan grup (`GROUPS`) efektif dari *user* yang *login*. View **`hello`** dilindungi dengan predikat **`permission='edit'`**. Terakhir, **`@forbidden_view_config`** digunakan untuk mengarahkan pengguna tanpa izin (termasuk anonim) ke halaman *login* saat mereka mencoba mengakses sumber daya terlarang.

---

## Analisis 
1. Peran Authorization di Pyramid
Authorization Pyramid bekerja berdasarkan empat konsep utama:

* **Principal:** Identitas yang sedang berinteraksi (e.g., `Everyone`, `Authenticated`, *user ID*, atau *group ID* seperti `group:editors`).
* **Permission:** String yang mendeskripsikan tindakan yang dapat dilakukan (e.g., `'view'`, `'edit'`).
* **Context/Resource:** Sumber daya yang sedang diakses (e.g., *instance* dari `Root` atau objek lain).
* **ACL (Access Control List):** Daftar pernyataan keamanan yang terlampir pada **Context** yang mendefinisikan siapa (`Principal`) yang diizinkan (`Allow`) atau ditolak (`Deny`) untuk melakukan tindakan (`Permission`).

2. Root Factory dan ACLs
Kami mendaftarkan `root_factory='.resources.Root'` di `__init__.py`. Setiap *request* akan menggunakan *instance* `Root` ini sebagai **Context**.

* **`Root.__acl__`**: Menetapkan aturan keamanan dasar:
    * `(Allow, Everyone, 'view')`: Semua orang (anonim atau terautentikasi) diizinkan untuk `'view'`.
    * `(Allow, 'group:editors', 'edit')`: Hanya *principal* yang merupakan bagian dari grup `editors` yang diizinkan untuk `'edit'`.

3. Protection di View Layer
View **`hello`** kini dilindungi dengan predikat **`permission='edit'`**:

* `@view_config(route_name='hello', permission='edit')`
* Ketika *request* tiba di `/howdy`, Pyramid meminta `SecurityPolicy.permits()` untuk memeriksa: "Apakah *user* (Principal) yang ada saat ini diizinkan untuk melakukan **`edit`** pada Context **`Root`**?".

4. Security Policy dan Principals Efektif
Metode **`effective_principals`** di `SecurityPolicy` sangat krusial. Tugasnya adalah menyusun daftar lengkap identitas (principals) yang dimiliki pengguna saat ini.

* Untuk *user* `editor`, daftar *principals* yang efektif mencakup: `[Everyone, Authenticated, 'u:editor', 'group:editors']`.
* Daftar ini kemudian diteruskan ke `ACLHelper` untuk dicocokkan dengan aturan di `Root.__acl__`.

5. Forbidden View
Jika *user* mencoba mengakses *view* yang memerlukan izin (`edit`) tetapi tidak memilikinya (misalnya, *user* anonim), Pyramid akan memicu *exception* **`HTTPForbidden` (403)**.

* Kami menggunakan dekorator **`@forbidden_view_config(renderer='login.pt')`** pada *view* `login()` untuk memberitahu Pyramid: "Setiap kali terjadi *Forbidden* (termasuk *user* anonim yang mencoba mengakses halaman terlindungi), alihkan mereka ke *login view* ini."

---

## Extra Credit
1. Apa perbedaan antara *user* dan *principal*?**
* **User:** Merupakan ID login yang unik (e.g., `'editor'`). Ini adalah identitas yang terautentikasi.
* **Principal:** Adalah **semua identitas** yang dimiliki oleh *user* dalam konteks keamanan. Ini termasuk: **`Everyone`** (setiap orang), **`Authenticated`** (setiap orang yang *login*), ID pengguna (`u:editor`), dan *group* pengguna (`group:editors`). ACLs Pyramid bekerja dengan mencocokkan *principals*.

2. Bisakah saya menggunakan *database* alih-alih `GROUPS` untuk mencari *principals*?**
Ya. akan memodifikasi metode **`effective_principals`** di `SecurityPolicy` untuk melakukan *query* ke database (misalnya, menggunakan **SQLAlchemy**) untuk mengambil daftar grup (`'group:...'`) tempat *user* berada, lalu menambahkan daftar tersebut ke *principals* yang dikembalikan.

3. Apakah harus ada `renderer` di `forbidden_view_config`?**
Tidak. Sama seperti `@view_config`, `renderer` hanya diperlukan jika *view* mengembalikan *dictionary* yang perlu diubah menjadi *response* (HTML). *View* `login()` di percobaan ini memang mengembalikan *dictionary*, jadi *renderer* diperlukan. Jika *forbidden view* hanya mengeluarkan `HTTPFound` (Redirect), `renderer` tidak diperlukan.

4. Bagaimana mengubah *forbidden experience* menjadi lebih kaya?**
kamu dapat menghapus `@forbidden_view_config(renderer='login.pt')` dari *view* `login()` dan membuat fungsi *view* baru yang didekorasi dengan `@forbidden_view_config(renderer='forbidden.pt')`. *View* baru ini dapat menyajikan halaman **403 Forbidden** yang informatif, menjelaskan mengapa *user* tidak dapat mengakses sumber daya tersebut (misalnya, "Anda tidak memiliki izin edit").

5. Bagaimana menyimpan pernyataan keamanan dalam *database* dan mengizinkan pengeditan via *browser*?**
Ini disebut **Dynamic ACLs**. Daripada mendefinisikan `__acl__` statis di `Root` class, Anda akan:
* Mendefinisikan model database untuk menyimpan aturan ACL (`principal`, `permission`, `action`).
* Mengubah properti `__acl__` pada *Context* (atau *Resource*) menjadi *dynamic property* yang melakukan *query* ke database untuk mendapatkan ACL yang berlaku setiap kali diakses.

6. Bagaimana jika kita ingin pernyataan keamanan yang berbeda pada objek yang berbeda?**
Ini adalah tujuan dari **Traversing Pyramid Resources**. Alih-alih hanya menggunakan *Root* sebagai *Context*, Anda akan membangun *Resource Tree* (pohon sumber daya). Setiap *node* dalam pohon tersebut (misalnya, objek *WikiPage*) dapat memiliki `__acl__` sendiri. Pyramid akan mencocokkan izin berdasarkan ACL pada *resource* yang paling spesifik (*deepest* di URL *path*) yang sedang diakses.rdasarkan ACL pada *resource* yang paling spesifik (*deepest* di URL *path*) yang sedang diakses.

## Output Percobaan
![Gambar WhatsApp 2025-11-12 pukul 22 33 00_af73dfd0](https://github.com/user-attachments/assets/84716068-1d74-4322-9652-0662edb9bc6f)

![Gambar WhatsApp 2025-11-12 pukul 22 33 20_24fc874e](https://github.com/user-attachments/assets/b571b9a2-74e0-4324-9499-e6f933ddcad2)

![Gambar WhatsApp 2025-11-12 pukul 22 33 35_e02a1368](https://github.com/user-attachments/assets/eb39ca5e-f331-4958-b6e2-e562399660a3)

![Gambar WhatsApp 2025-11-12 pukul 22 34 11_5459ec2a](https://github.com/user-attachments/assets/dd9cd5b4-a43f-4e79-8698-4aa62bc596dc)

