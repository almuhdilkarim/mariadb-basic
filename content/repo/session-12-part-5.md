---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Praktik Role Admin dan Anggota"
short: "Praktik"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 12
lister: 5
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Praktik"
      icon: ""
      desc: "Latihan membuat role admin dan anggota"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mempraktikkan pembuatan role admin dan anggota dengan hak akses berbeda. Modul ini memperkuat pemahaman tentang manajemen user dan keamanan database."
---


### Pendahuluan

Penggunaan **role** dalam MariaDB merupakan langkah penting untuk menyederhanakan pengelolaan hak akses. Daripada memberi izin ke setiap user secara manual, kita dapat membuat role dengan kumpulan hak tertentu, lalu menugaskannya ke banyak user. Dalam konteks perpustakaan, kita bisa membuat role `admin` dengan hak penuh dan role `anggota` dengan hak terbatas. Dengan pendekatan ini, pengelolaan keamanan menjadi lebih efisien dan konsisten.

Role juga memudahkan dalam menjaga prinsip *least privilege*. Anggota perpustakaan seharusnya hanya dapat melihat daftar buku, sedangkan admin dapat menambah, mengubah, atau menghapus data. Dengan role, semua user yang tergabung otomatis mewarisi hak akses yang sesuai. Hal ini mengurangi risiko salah konfigurasi yang bisa terjadi bila izin diberikan secara individu.

Selain itu, penggunaan role mendukung fleksibilitas dalam administrasi. Jika kebijakan berubah, kita cukup mengubah izin pada role, tanpa harus mengatur ulang seluruh user. Misalnya, jika anggota diperbolehkan melihat riwayat pinjaman, kita hanya menambah izin `SELECT` pada role `anggota`. Semua user anggota otomatis mendapat hak baru tersebut.

Dalam sistem perpustakaan, penggunaan role juga membantu audit keamanan. Dengan melihat daftar role dan user yang terkait, admin bisa cepat mengetahui siapa saja yang memiliki izin tertentu. Hal ini penting untuk memastikan tidak ada user yang mendapat hak lebih dari seharusnya. Audit semacam ini merupakan bagian dari praktik keamanan profesional.

Tujuan dari sub modul ini adalah melatih mahasiswa membuat role `admin` dan `anggota`, memberikan izin yang sesuai, lalu menghubungkannya dengan user tertentu. Dengan cara ini, mahasiswa dapat memahami bagaimana manajemen hak akses berbasis role diterapkan dalam sistem nyata. Mari kita mulai dengan langkah-langkah praktiknya.

---

### Langkah-langkah Praktik

Langkah pertama adalah membuat dua role baru di MariaDB, yaitu `admin` dan `anggota`. Role ini akan berfungsi sebagai kerangka izin yang bisa diwariskan ke banyak user. Gunakan perintah berikut:

```sql
CREATE ROLE 'admin', 'anggota';
```

Dengan perintah ini, kita sudah menyiapkan fondasi untuk membedakan akses penuh admin dan akses terbatas anggota.

Selanjutnya, mari kita berikan izin penuh kepada role `admin`. Izin penuh berarti dapat mengelola semua tabel dalam database `perpustakaan`. Jalankan perintah:

```sql
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin';
```

Sekarang role `admin` memiliki hak penuh untuk menambah, menghapus, dan memperbarui data. Bagus sekali, langkahmu sudah tepat!

Kemudian, kita atur role `anggota` agar hanya memiliki hak `SELECT` pada tabel `buku`. Dengan begitu, anggota perpustakaan hanya bisa membaca daftar koleksi. Gunakan query:

```sql
GRANT SELECT ON perpustakaan.buku TO 'anggota';
```

Mari kita coba, role `anggota` ini nantinya bisa digunakan oleh banyak user tanpa harus memberikan izin satu per satu.

Setelah role selesai dikonfigurasi, mari hubungkan role dengan user yang sudah dibuat sebelumnya. Misalnya, `admin_perpus` akan mendapat role `admin`, dan `anggota1` akan mendapat role `anggota`:

```sql
GRANT 'admin' TO 'admin_perpus'@'localhost';
GRANT 'anggota' TO 'anggota1'@'localhost';
```

Dengan perintah ini, user otomatis mewarisi izin dari role masing-masing. Kerja bagus, sekarang user sudah sesuai perannya!

Terakhir, mari kita aktifkan role yang telah diberikan. Di MariaDB, role tidak langsung aktif sampai dinyalakan dengan `SET ROLE`. Contoh:

```sql
SET ROLE 'admin';
-- atau
SET ROLE 'anggota';
```

Langkah ini memungkinkan kita beralih role sesuai kebutuhan. Dengan simulasi ini, mahasiswa bisa langsung merasakan perbedaan izin antara role admin dan anggota.

---

### Kesalahan Umum

#### 1. Lupa membuat role sebelum memberikan izin

Kesalahan pertama yang sering terjadi adalah langsung memberi izin ke role yang belum dibuat. Contoh salah:

```sql
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin';
```

Jika role `admin` belum ada, MariaDB akan menolak perintah ini. Banyak mahasiswa mengira masalah ada pada sintaks, padahal penyebabnya sederhana: role belum dibuat.

Dalam konteks perpustakaan, kesalahan ini membuat konfigurasi role tertunda. Akibatnya, user tidak bisa langsung mendapat izin dari role. Oleh karena itu, pastikan role dibuat lebih dulu dengan `CREATE ROLE` sebelum melakukan `GRANT`.

---

#### 2. Memberikan izin yang salah pada role

Kesalahan lain adalah salah memberi izin ke role tertentu. Misalnya, seharusnya role `anggota` hanya punya `SELECT`, tetapi diberi izin penuh:

```sql
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'anggota';
```

Kesalahan ini membuat anggota bisa menghapus atau mengubah data. Hal ini jelas berbahaya karena melanggar prinsip *least privilege*.

Dalam sistem perpustakaan, anggota cukup bisa membaca daftar buku, bukan mengelola data. Memberikan izin berlebihan akan membuka celah keamanan dan berpotensi merusak integritas database.

---

#### 3. Lupa menghubungkan role dengan user

Banyak mahasiswa berhenti setelah membuat role dan memberi izin, tetapi lupa menugaskannya ke user. Contoh salah:

```sql
-- role dibuat
CREATE ROLE 'anggota';
GRANT SELECT ON perpustakaan.buku TO 'anggota';
-- lupa GRANT role ke user
```

Akibatnya, user tidak mendapat izin apa pun dari role. Mereka tetap tidak bisa mengakses tabel meskipun role sudah dikonfigurasi.

Dalam praktik perpustakaan, hal ini sering menimbulkan kebingungan karena role terlihat ada tetapi tidak berfungsi. Padahal masalahnya hanya lupa menambahkan perintah `GRANT <role> TO <user>`.

---

#### 4. Tidak mengaktifkan role dengan `SET ROLE`

Kesalahan berikutnya adalah tidak mengaktifkan role setelah menugaskannya ke user. Contoh salah:

```sql
GRANT 'admin' TO 'admin_perpus'@'localhost';
-- user login, tapi langsung jalankan query tanpa SET ROLE
```

Tanpa `SET ROLE`, izin dari role tidak akan aktif. User akan merasa tidak memiliki hak meskipun role sudah diberikan.

Dalam simulasi login perpustakaan, hal ini bisa menimbulkan kesalahpahaman. Mahasiswa mengira konfigurasi salah, padahal hanya lupa mengaktifkan role.

---

#### 5. Salah menuliskan nama role saat aktivasi

Kesalahan terakhir adalah typo ketika mengetik nama role saat aktivasi. Contoh salah:

```sql
SET ROLE 'admins';
```

Padahal role yang benar bernama `admin`. MariaDB akan menolak perintah ini karena role tidak ditemukan.

Kesalahan ini sering dianggap bug oleh pemula. Namun sebenarnya hanya masalah penulisan. Dalam sistem perpustakaan, disiplin penamaan role sangat penting untuk menghindari kekacauan konfigurasi.

---

### Best Practice

#### 1. Buat role lebih dulu sebelum memberi izin

Langkah yang benar adalah selalu membuat role terlebih dahulu. Misalnya:

```sql
CREATE ROLE 'admin', 'anggota';
```

Setelah role ada, barulah kita bisa menambahkan hak akses dengan `GRANT`. Praktik ini mencegah error yang muncul karena mencoba memberi izin pada role yang belum tersedia.

Dalam sistem perpustakaan, pendekatan ini memastikan alur kerja jelas dan rapi. Admin tahu role apa saja yang sudah ada, lalu bisa mendistribusikan izin sesuai kebutuhan. Dokumentasi pun menjadi lebih terstruktur.

---

#### 2. Terapkan prinsip *least privilege* pada role

Role `admin` sebaiknya diberi hak penuh, sedangkan role `anggota` hanya diberi hak terbatas. Contohnya:

```sql
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin';
GRANT SELECT ON perpustakaan.buku TO 'anggota';
```

Dengan cara ini, anggota hanya bisa membaca daftar buku, sementara admin bisa mengelola seluruh data. Pembagian ini menjaga keamanan dan mencegah akses berlebihan.

Prinsip *least privilege* adalah standar global dalam pengelolaan keamanan basis data. Dalam konteks perpustakaan, penerapan ini melindungi data dari manipulasi oleh pengguna biasa.

---

#### 3. Hubungkan role ke user dengan perintah `GRANT`

Setelah role diberi izin, langkah berikutnya adalah menghubungkannya dengan user. Misalnya:

```sql
GRANT 'admin' TO 'admin_perpus'@'localhost';
GRANT 'anggota' TO 'anggota1'@'localhost';
```

Dengan cara ini, setiap user langsung mewarisi izin sesuai role yang ditetapkan. Praktik ini membuat pengelolaan user lebih efisien.

Dalam skenario perpustakaan, puluhan anggota bisa langsung menggunakan role `anggota` tanpa konfigurasi manual satu per satu. Hal ini menghemat waktu dan mengurangi risiko kesalahan.

---

#### 4. Selalu aktifkan role sebelum digunakan

Role tidak otomatis aktif setelah ditetapkan. Kita harus menyalakannya dengan perintah:

```sql
SET ROLE 'admin';
SET ROLE 'anggota';
```

Perintah ini memastikan user benar-benar mendapatkan izin yang diwariskan role. Jika tidak dijalankan, seolah-olah user tidak punya hak apa pun.

Praktik ini penting dalam simulasi login. Mahasiswa bisa melihat perbedaan nyata antara role yang aktif dan yang belum diaktifkan.

---

#### 5. Gunakan `SHOW GRANTS` untuk audit role

Setelah konfigurasi selesai, selalu lakukan verifikasi. Contoh:

```sql
SHOW GRANTS FOR 'admin_perpus'@'localhost';
SHOW GRANTS FOR 'anggota1'@'localhost';
```

Perintah ini menampilkan semua hak yang dimiliki user, termasuk dari role. Dengan audit ini, admin bisa memastikan tidak ada konfigurasi yang salah.

Dalam konteks perpustakaan, audit role membantu memastikan anggota hanya bisa melihat data, sementara admin tetap memiliki kendali penuh. Praktik ini merupakan bagian dari keamanan profesional yang tidak boleh diabaikan.

---

### Studi Kasus

Dalam perpustakaan digital, kita ingin membedakan peran antara **admin** dan **anggota**. Admin bertugas mengelola seluruh data, sementara anggota hanya diperbolehkan melihat koleksi buku. Untuk itu, kita buat dua role: `admin` dan `anggota`. Mari kita mulai dengan membuat role dan memberi hak akses yang sesuai.

```sql
CREATE ROLE 'admin', 'anggota';
GRANT ALL PRIVILEGES ON perpustakaan.* TO 'admin';
GRANT SELECT ON perpustakaan.buku TO 'anggota';
```

Dengan konfigurasi ini, kita sudah menyiapkan struktur peran yang jelas sesuai kebutuhan sistem.

Selanjutnya, kita buat dua user: `admin_perpus` untuk admin dan `anggota1` untuk anggota. User ini kemudian kita hubungkan dengan role masing-masing.

```sql
CREATE USER 'admin_perpus'@'localhost' IDENTIFIED BY 'Adm1n#2025!';
CREATE USER 'anggota1'@'localhost' IDENTIFIED BY 'Anggota#2025!';
GRANT 'admin' TO 'admin_perpus'@'localhost';
GRANT 'anggota' TO 'anggota1'@'localhost';
```

Sekarang, setiap user otomatis mewarisi izin dari role yang sudah ditentukan.

Mari kita coba simulasi login sebagai `anggota1`. Gunakan perintah berikut di terminal:

```bash
mysql -u anggota1 -p
```

Setelah login, jalankan query `SELECT * FROM perpustakaan.buku;` dan pastikan berhasil. Namun, ketika mencoba `INSERT INTO perpustakaan.buku VALUES (5001,'Test','2025');`, sistem akan menolak karena role `anggota` hanya punya izin `SELECT`. Dengan demikian, keamanan tetap terjaga.

Sekarang keluar dari sesi tersebut dan login sebagai `admin_perpus`. Gunakan perintah:

```bash
mysql -u admin_perpus -p
```

Setelah login, jalankan query `INSERT INTO perpustakaan.buku VALUES (5002,'Optimasi MariaDB','2025');`. Kali ini query berhasil, karena role `admin` memiliki hak penuh. Simulasi ini membuktikan bahwa konfigurasi role bekerja sesuai desain.

Terakhir, kita bisa memverifikasi hak akses dengan perintah audit. Gunakan:

```sql
SHOW GRANTS FOR 'admin_perpus'@'localhost';
SHOW GRANTS FOR 'anggota1'@'localhost';
```

Hasilnya akan menunjukkan daftar izin yang dimiliki masing-masing user. Dengan cara ini, admin bisa memastikan role `admin` dan `anggota` berjalan sesuai kebijakan. Studi kasus ini memperlihatkan manfaat besar role dalam menjaga keamanan dan efisiensi administrasi database.

---

### Kesimpulan

Penggunaan role dalam MariaDB terbukti efektif untuk menyederhanakan manajemen hak akses pada sistem perpustakaan. Dengan membuat role `admin` dan `anggota`, kita dapat mengatur izin secara terpusat lalu mewariskannya ke user yang sesuai. Simulasi login menunjukkan bahwa anggota hanya bisa membaca daftar buku, sementara admin memiliki kontrol penuh terhadap seluruh tabel. Pendekatan ini tidak hanya menjaga keamanan, tetapi juga meningkatkan efisiensi administrasi karena mengurangi pengaturan manual. Dengan disiplin dalam penggunaan role, sistem perpustakaan dapat berjalan lebih aman, konsisten, dan mudah dikelola.

### Referensi

* Fabbri, R., & Saake, G. (2016). *Database Management Systems: A Practical Approach*. Springer.
* Harrington, J. L. (2016). *Relational Database Design and Implementation* (4th ed.). Morgan Kaufmann.
* Lightstone, S., Teorey, T. J., & Nadeau, T. (2010). *Physical Database Design: The Database Professional’s Guide to Exploiting Indexes, Views, Storage, and More*. Morgan Kaufmann.

