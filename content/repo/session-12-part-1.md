---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Membuat User Baru di MariaDB"
short: "User"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 12
lister: 1
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: ""
      name: ""
      icon: ""
      desc: ""
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mempraktikkan transaksi peminjaman buku menggunakan perintah START, COMMIT, dan ROLLBACK. Modul ini memperkuat pemahaman konsep ACID melalui latihan nyata."
---

### Pendahuluan

Keamanan database merupakan aspek penting dalam sistem manajemen perpustakaan, karena akses data harus dibatasi sesuai peran pengguna. Setiap pengguna tidak boleh memiliki izin yang sama, sebab kebutuhan akses anggota berbeda dengan admin. Dalam MariaDB, konsep ini diwujudkan melalui manajemen user dan hak akses (privileges). Pendekatan ini melindungi data sensitif seperti riwayat pinjaman, data anggota, maupun koleksi buku yang hanya boleh diubah oleh pihak tertentu. Dengan demikian, keamanan bukan sekadar tambahan, melainkan fondasi yang memastikan sistem berjalan secara berkelanjutan.

Dalam konteks perpustakaan, admin biasanya memiliki akses penuh untuk menambah, menghapus, atau memperbarui data. Sementara itu, anggota hanya memerlukan akses terbatas seperti melihat daftar buku atau status pinjaman. Hal ini mencerminkan prinsip *least privilege* yang menyarankan agar setiap user diberikan hak minimal sesuai kebutuhannya. Jika prinsip ini dilanggar, risiko kebocoran data dan manipulasi informasi akan meningkat. Maka dari itu, konsep dasar keamanan perlu dipahami sejak awal implementasi.

MariaDB menyediakan sintaks SQL khusus untuk mengatur pembuatan user dan pemberian izin. Dengan memanfaatkan perintah `CREATE USER` dan `GRANT`, kita bisa mendefinisikan hak akses yang berbeda untuk tiap pengguna. Misalnya, seorang anggota hanya diberi izin `SELECT` pada tabel `buku`, sedangkan admin diberi izin `INSERT`, `UPDATE`, dan `DELETE`. Contoh ini menggambarkan bagaimana keamanan sederhana bisa diterapkan dalam praktik sehari-hari. Dengan demikian, database dapat digunakan oleh banyak pihak tanpa mengorbankan konsistensi maupun kerahasiaan data.

Selain itu, penerapan role seperti `admin` dan `anggota` memungkinkan manajemen akses menjadi lebih terstruktur. Alih-alih memberi izin satu per satu pada tiap user, kita bisa mengelompokkan hak akses berdasarkan role. Pendekatan ini lebih efisien dan memudahkan pemeliharaan jangka panjang. Role juga membantu dalam skenario simulasi login, sehingga kita dapat menguji apakah konfigurasi hak akses sudah sesuai. Inilah yang akan kita praktikkan pada modul ini.

Tujuan dari modul ini adalah melatih mahasiswa agar terbiasa mengelola keamanan database MariaDB. Dengan memahami cara membuat user, memberikan izin, serta melakukan simulasi login, mahasiswa akan mampu membedakan level akses antara admin dan anggota. Selain itu, mereka akan belajar mendeteksi kesalahan umum yang sering muncul saat konfigurasi user. Harapannya, praktik ini akan menumbuhkan kesadaran pentingnya keamanan pada sistem basis data. Mari kita mulai langkah-langkah praktiknya.

---

### Langkah-langkah Praktik

Langkah pertama adalah membuat user baru di MariaDB untuk mensimulasikan akses anggota perpustakaan. Misalnya, kita ingin membuat user `anggota1` dengan password khusus. Gunakan perintah berikut:

```sql
CREATE USER 'anggota1'@'localhost' IDENTIFIED BY 'password123';
```

Perintah ini memastikan bahwa `anggota1` hanya bisa login dari `localhost` dan membutuhkan kata sandi untuk autentikasi.

Selanjutnya, mari kita berikan izin terbatas kepada user `anggota1`. Kita hanya ingin user ini dapat melihat daftar buku tanpa bisa mengubah data. Jalankan perintah berikut:

```sql
GRANT SELECT ON perpustakaan.buku TO 'anggota1'@'localhost';
```

Dengan perintah ini, user hanya dapat melakukan query `SELECT` pada tabel `buku`. Kerja bagus, sekarang kita berhasil membatasi akses sesuai kebutuhan!

Berikutnya, kita buat user `admin1` yang akan berperan sebagai administrator sistem perpustakaan. Gunakan perintah:

```sql
CREATE USER 'admin1'@'localhost' IDENTIFIED BY 'adminpass';
```

Kemudian beri hak akses penuh dengan perintah:

```sql
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin1'@'localhost';
```

Dengan cara ini, `admin1` dapat mengelola seluruh tabel dalam database `perpustakaan`.

Mari kita coba simulasi login dengan user berbeda. Untuk login sebagai `anggota1`, gunakan:

```bash
mysql -u anggota1 -p
```

Setelah memasukkan password, coba jalankan `SELECT * FROM perpustakaan.buku;`. Jika berhasil, berarti konfigurasi izin sudah tepat. Kemudian, login sebagai `admin1` dan coba lakukan `INSERT` atau `DELETE` untuk menguji hak akses penuh.

Terakhir, kita bisa membuat role untuk menyederhanakan pengelolaan izin. Misalnya:

```sql
CREATE ROLE 'admin', 'anggota';
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin';
GRANT SELECT ON perpustakaan.buku TO 'anggota';
GRANT 'admin' TO 'admin1'@'localhost';
GRANT 'anggota' TO 'anggota1'@'localhost';
```

Dengan role ini, kita tidak perlu memberi izin manual pada tiap user. Semua hak akses bisa diwariskan sesuai role yang ditetapkan. Langkah ini membuat sistem lebih rapi dan mudah dikelola.

---

### Kesalahan Umum

#### 1. Membuat user tanpa password

Kesalahan yang sering terjadi adalah membuat user baru tanpa memberikan password. Contohnya:

```sql
CREATE USER 'anggota2'@'localhost';
```

User ini akan terbentuk, tetapi tanpa autentikasi sehingga siapa pun bisa login. Hal ini sangat berbahaya karena keamanan sistem langsung lemah. Dalam konteks perpustakaan, data anggota dan pinjaman bisa diakses pihak yang tidak berwenang. Praktik seperti ini jelas tidak sesuai dengan standar keamanan dasar.

Selain itu, user tanpa password juga berpotensi digunakan oleh bot atau serangan brute-force. Dengan akses login yang terbuka, database bisa disusupi query yang merusak. Untuk itu, selalu pastikan user baru dibuat dengan password yang kuat. Perintah `IDENTIFIED BY` adalah komponen wajib yang tidak boleh diabaikan.

---

#### 2. Memberikan izin tanpa menyebut database

Beberapa mahasiswa pemula lupa menuliskan nama database saat memberi izin. Contoh kesalahan:

```sql
GRANT SELECT ON buku TO 'anggota1'@'localhost';
```

Perintah ini tidak akan berjalan karena MariaDB membutuhkan format `database.tabel`. Akibatnya, user tidak akan mendapatkan izin yang seharusnya.

Kesalahan kecil ini sering menimbulkan kebingungan karena sistem tidak memberi pesan yang jelas. Untuk menghindarinya, selalu tuliskan nama database dengan lengkap, misalnya `perpustakaan.buku`. Dengan cara itu, hak akses dapat diterapkan dengan benar sesuai tabel target.

---

#### 3. Memberi hak penuh kepada semua user

Ada juga kasus di mana semua user diberikan izin penuh tanpa mempertimbangkan perannya. Contoh kesalahan:

```sql
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'anggota1'@'localhost';
```

Padahal `anggota1` seharusnya hanya memiliki hak akses `SELECT`. Jika semua user diberi `ALL PRIVILEGES`, maka keamanan sistem hilang.

Dalam praktik nyata, anggota bisa saja menghapus seluruh tabel hanya dengan query `DROP`. Kesalahan ini mencerminkan kurangnya pemahaman terhadap prinsip *least privilege*. Oleh karena itu, pemberian izin harus benar-benar dibatasi sesuai kebutuhan.

---

#### 4. Lupa menjalankan `FLUSH PRIVILEGES`

Setelah melakukan perubahan user dan izin, terkadang admin lupa menjalankan perintah:

```sql
FLUSH PRIVILEGES;
```

Tanpa ini, perubahan tidak akan langsung berlaku. Akibatnya, user lama tetap memiliki izin lama meskipun kita sudah melakukan update.

Kesalahan ini sering muncul saat simulasi login, di mana hasilnya tidak sesuai harapan. Mahasiswa bisa mengira query `GRANT` gagal, padahal hanya lupa melakukan sinkronisasi. Itulah mengapa `FLUSH PRIVILEGES` menjadi langkah penting yang tidak boleh dilewatkan.

---

#### 5. Tidak membatasi host saat membuat user

Kesalahan lain adalah membuat user dengan akses dari host mana saja, contohnya:

```sql
CREATE USER 'anggota1'@'%';
```

Simbol `%` berarti user bisa login dari semua host. Ini berbahaya jika server database dapat diakses dari jaringan publik. Penyerang bisa mencoba login dari luar server.

Dalam konteks perpustakaan, kita biasanya hanya ingin user mengakses dari `localhost`. Dengan begitu, akses hanya terbuka dari server tempat aplikasi berjalan. Membiarkan `%` tanpa alasan jelas sama saja membuka celah keamanan yang besar.

---


### Best Practice

#### 1. Membuat user dengan password yang kuat

Langkah terbaik saat membuat user baru adalah selalu menyertakan password yang kompleks. Contoh:

```sql
CREATE USER 'anggota2'@'localhost' IDENTIFIED BY 'Anggota#2025!';
```

Password ini menggunakan kombinasi huruf besar, kecil, angka, dan simbol. Dengan demikian, upaya serangan brute force menjadi lebih sulit. Praktik ini menjaga keamanan database perpustakaan agar hanya user sah yang dapat mengakses.

Selain itu, penggunaan password kompleks mengurangi risiko kebocoran data anggota dan pinjaman buku. Admin perlu menerapkan kebijakan agar password diperbarui secara berkala. Ini akan semakin memperkuat sistem keamanan MariaDB di lingkungan perpustakaan.

---

#### 2. Memberikan izin spesifik sesuai kebutuhan

Alih-alih memberikan hak penuh, sebaiknya izin diberikan secara spesifik. Contoh:

```sql
GRANT SELECT ON perpustakaan.buku TO 'anggota2'@'localhost';
```

Dengan perintah ini, `anggota2` hanya dapat melihat daftar buku tanpa bisa mengubah data. Prinsip *least privilege* diterapkan untuk mencegah akses berlebihan.

Penerapan izin spesifik membantu menjaga integritas database. Data koleksi buku tetap aman, sementara anggota tetap bisa mencari informasi yang dibutuhkan. Inilah praktik standar dalam manajemen keamanan MariaDB.

---

#### 3. Menggunakan role untuk pengelolaan izin

Penggunaan role membuat pengelolaan user lebih efisien. Contoh penerapannya:

```sql
CREATE ROLE 'admin', 'anggota';
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin';
GRANT SELECT ON perpustakaan.buku TO 'anggota';
```

Dengan cara ini, kita cukup menambahkan user ke role tertentu tanpa memberi izin satu per satu. Hal ini mempermudah pemeliharaan jangka panjang.

Role juga membantu memastikan konsistensi hak akses di antara user dengan peran sama. Misalnya, semua anggota mendapat izin yang identik tanpa ada yang berlebih. Pendekatan ini mencerminkan praktik profesional dalam pengelolaan keamanan database.

---

#### 4. Menyertakan `FLUSH PRIVILEGES` setelah perubahan

Setiap kali ada perubahan user atau izin, selalu jalankan:

```sql
FLUSH PRIVILEGES;
```

Perintah ini memastikan perubahan segera diterapkan dalam sistem. Tanpa langkah ini, konfigurasi baru mungkin tidak langsung aktif.

Kebiasaan menjalankan `FLUSH PRIVILEGES` mengurangi kebingungan saat simulasi login. Mahasiswa dapat langsung melihat efek dari perubahan izin. Praktik ini memperlihatkan pemahaman mendalam terhadap mekanisme internal MariaDB.

---

#### 5. Membatasi host untuk login

Langkah terbaik adalah membatasi host agar user hanya bisa login dari sumber tertentu. Contoh:

```sql
CREATE USER 'admin2'@'localhost' IDENTIFIED BY 'Admin#2025!';
```

Dengan membatasi `localhost`, user hanya bisa login dari server aplikasi. Hal ini mencegah akses dari luar jaringan yang berpotensi berbahaya.

Pembatasan host membantu mengurangi celah keamanan dalam sistem. Dalam konteks perpustakaan, hanya server resmi yang dapat digunakan untuk login admin. Dengan demikian, risiko penyusupan dari luar dapat ditekan secara signifikan.

---


### Studi Kasus

Dalam sistem perpustakaan, terdapat dua jenis pengguna utama: **admin** dan **anggota**. Admin memiliki tugas penuh untuk mengelola database, mulai dari menambah buku baru hingga memperbarui status pinjaman. Anggota hanya membutuhkan izin untuk membaca daftar buku atau memeriksa riwayat pinjamannya. Untuk itu, kita dapat membuat role yang sesuai agar lebih mudah dikelola. Mari kita mulai dengan membuat role `admin` dan `anggota` terlebih dahulu:

```sql
CREATE ROLE 'admin', 'anggota';
```

Langkah ini menyiapkan dua kerangka izin yang akan kita berikan pada user tertentu.

Selanjutnya, mari kita tentukan hak akses untuk role tersebut. Admin diberikan hak penuh pada seluruh tabel di database `perpustakaan`. Anggota hanya diberi izin `SELECT` pada tabel `buku` agar bisa melihat daftar koleksi. Berikut adalah query-nya:

```sql
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin';
GRANT SELECT ON perpustakaan.buku TO 'anggota';
```

Dengan pengaturan ini, kita telah membedakan tanggung jawab masing-masing role. Prinsip *least privilege* tetap terjaga.

Kemudian, kita buat user baru untuk masing-masing role. Misalnya, `admin_perpus` untuk admin dan `anggota_perpus` untuk anggota. Perintah berikut digunakan untuk membuat keduanya:

```sql
CREATE USER 'admin_perpus'@'localhost' IDENTIFIED BY 'Admin#2025!';
CREATE USER 'anggota_perpus'@'localhost' IDENTIFIED BY 'Anggota#2025!';
```

User ini secara default belum memiliki izin, sehingga kita perlu mengaitkan mereka dengan role yang sudah dibuat.

Untuk menghubungkan user dengan role, gunakan perintah `GRANT <role> TO <user>`. Contohnya:

```sql
GRANT 'admin' TO 'admin_perpus'@'localhost';
GRANT 'anggota' TO 'anggota_perpus'@'localhost';
```

Dengan cara ini, `admin_perpus` otomatis memiliki semua izin sebagai admin, sedangkan `anggota_perpus` hanya bisa melihat tabel buku. Pendekatan berbasis role membuat administrasi keamanan lebih efisien.

Terakhir, mari kita uji dengan simulasi login. Cobalah login sebagai `anggota_perpus` dan jalankan query:

```sql
SELECT * FROM perpustakaan.buku;
```

Query ini seharusnya berhasil. Namun, jika mencoba `INSERT` atau `DELETE`, akan muncul error karena izin tidak ada. Sebaliknya, login sebagai `admin_perpus` akan memungkinkan kita melakukan semua operasi. Simulasi ini membuktikan bahwa sistem keamanan sederhana sudah berjalan sesuai desain.

---

### Kesimpulan

Penerapan keamanan sederhana dalam MariaDB sangat penting untuk melindungi sistem perpustakaan dari akses yang tidak sah. Dengan membuat user baru dan memberikan izin sesuai kebutuhan, prinsip *least privilege* dapat dijalankan secara konsisten. Simulasi login membuktikan bahwa anggota hanya bisa membaca data, sementara admin memiliki kontrol penuh atas database. Selain itu, penggunaan role membuat pengelolaan hak akses menjadi lebih efisien dan terstruktur. Praktik ini tidak hanya meningkatkan keamanan, tetapi juga menjaga integritas dan keberlanjutan sistem perpustakaan dalam jangka panjang.

### Referensi

* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2020). *Database System Concepts* (7th ed.). McGraw-Hill.
* Connolly, T., & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management* (6th ed.). Pearson Education.
* Coronel, C., & Morris, S. (2017). *Database Systems: Design, Implementation, & Management* (12th ed.). Cengage Learning.


