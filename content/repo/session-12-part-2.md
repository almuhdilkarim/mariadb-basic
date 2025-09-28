---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Memberi Hak Akses SELECT"
short: "HakAkses"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 12
lister: 2
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Keamanan"
      icon: ""
      desc: "Membatasi user hanya untuk membaca data"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Pelajari bagaimana memberikan hak akses SELECT agar user hanya bisa membaca data. Modul ini memperkenalkan konsep otorisasi sederhana pada MariaDB."
---

### Pendahuluan

Keamanan database tidak hanya berhenti pada pemberian hak akses, tetapi juga mencakup bagaimana izin tersebut dapat dicabut kembali ketika sudah tidak relevan. Dalam sistem perpustakaan, terkadang ada anggota atau petugas yang sudah tidak aktif, sehingga hak akses mereka harus dicabut untuk mencegah penyalahgunaan. MariaDB menyediakan perintah `REVOKE` yang berfungsi membatalkan izin tertentu dari user. Dengan demikian, administrator memiliki kendali penuh terhadap siapa saja yang dapat mengakses data. Hal ini memastikan bahwa database selalu dalam kondisi aman dan terkontrol.

Selain pencabutan izin, penting juga bagi admin untuk memahami perbedaan pemberian hak akses pada tingkat tabel dan tingkat database. Misalnya, seorang anggota mungkin hanya boleh membaca data dari tabel `buku`, tetapi tidak boleh menambahkan data baru ke tabel `pinjaman`. Pengaturan ini menuntut ketelitian karena kesalahan kecil dapat berdampak besar pada keamanan. Oleh karena itu, memahami sintaks `GRANT` dan `REVOKE` menjadi keterampilan penting yang wajib dikuasai. Modul ini akan membantu mahasiswa melatih keterampilan tersebut melalui studi kasus nyata.

Dalam praktik sehari-hari, pengelolaan hak akses juga berhubungan erat dengan dinamika organisasi. Admin perpustakaan bisa berganti, anggota bisa bertambah atau berkurang, dan kebijakan keamanan bisa berubah seiring waktu. Karena itu, fleksibilitas dalam memberikan dan mencabut izin menjadi kunci agar sistem tetap relevan dengan kebutuhan. MariaDB memberikan keleluasaan dalam hal ini, namun tetap menuntut disiplin dalam penerapannya. Kedisiplinan inilah yang akan menjadi nilai tambah bagi mahasiswa saat mengelola database.

Penerapan `REVOKE` juga berguna untuk simulasi keamanan. Misalnya, kita bisa mencabut izin `INSERT` dari anggota lalu menguji apakah sistem benar-benar menolak perintah tersebut. Praktik seperti ini bukan hanya latihan teknis, tetapi juga melatih pola pikir kritis terhadap keamanan. Mahasiswa akan terbiasa memverifikasi konfigurasi, bukan hanya sekadar menuliskan query. Dengan begitu, kualitas administrasi database akan meningkat.

Tujuan dari modul ini adalah memperkenalkan bagaimana izin dapat dikelola secara lebih dinamis. Mahasiswa akan belajar tidak hanya memberi, tetapi juga mencabut hak akses sesuai kebutuhan. Selain itu, mereka akan memahami bagaimana perintah `GRANT` dan `REVOKE` bekerja sama untuk menciptakan sistem yang aman. Pada akhirnya, mahasiswa mampu membedakan izin yang tepat bagi admin dan anggota dalam berbagai skenario. Mari kita lanjutkan ke langkah-langkah praktik agar pemahaman ini lebih mendalam.

---


### Langkah-langkah Praktik

Langkah pertama adalah membuat user baru untuk simulasi, misalnya `petugas1`. User ini akan kita beri izin terbatas, kemudian kita cabut kembali untuk melihat efeknya. Gunakan perintah berikut:

```sql
CREATE USER 'petugas1'@'localhost' IDENTIFIED BY 'Petugas#2025!';
```

Dengan cara ini, kita menyiapkan akun uji coba yang aman dengan password yang cukup kuat.

Sekarang mari kita berikan izin `SELECT` dan `INSERT` pada tabel `pinjaman`. Hal ini mensimulasikan seorang petugas yang berhak menambahkan catatan pinjaman baru dan juga melihat daftar pinjaman. Jalankan perintah:

```sql
GRANT SELECT, INSERT ON perpustakaan.pinjaman TO 'petugas1'@'localhost';
```

Kerja bagus! Dengan perintah ini, `petugas1` bisa melakukan query untuk melihat data serta menambahkan baris baru ke tabel `pinjaman`.

Untuk menguji, login sebagai `petugas1` menggunakan terminal:

```bash
mysql -u petugas1 -p
```

Kemudian coba jalankan query `INSERT INTO pinjaman VALUES (...);`. Jika berhasil, berarti izin sudah berfungsi dengan benar. Mari juga coba `SELECT * FROM perpustakaan.pinjaman;` untuk memastikan akses baca tersedia. Ini penting agar kita yakin bahwa konfigurasi sudah berjalan sesuai desain.

Selanjutnya, mari kita cabut izin `INSERT` dari `petugas1` agar ia hanya bisa membaca data pinjaman. Gunakan perintah berikut:

```sql
REVOKE INSERT ON perpustakaan.pinjaman FROM 'petugas1'@'localhost';
```

Setelah perintah ini dijalankan, user tetap bisa melakukan `SELECT` tetapi tidak lagi bisa `INSERT`. Silakan coba login kembali dan jalankan perintah `INSERT`. Anda akan melihat pesan error yang menandakan izin sudah berhasil dicabut.

Terakhir, kita bisa menguji perbedaan role dengan menambahkan `petugas1` ke dalam role `anggota`. Role `anggota` hanya memiliki izin `SELECT` pada tabel `buku`. Gunakan perintah:

```sql
GRANT 'anggota' TO 'petugas1'@'localhost';
```

Dengan demikian, `petugas1` sekarang hanya dapat membaca data buku seperti halnya anggota biasa. Praktik ini memperlihatkan bagaimana role dan perintah `REVOKE` bekerja sama untuk menciptakan sistem keamanan yang fleksibel.

---
### Kesalahan Umum

#### 1. Tidak menggunakan kata kunci `REVOKE` dengan benar

Kesalahan umum pertama adalah lupa menuliskan format lengkap `REVOKE`. Contoh salah:

```sql
REVOKE INSERT 'petugas1'@'localhost';
```

Query ini akan menghasilkan error karena MariaDB membutuhkan format `REVOKE <privileges> ON <database.table> FROM <user>`. Tanpa bagian `ON`, perintah tidak dapat dieksekusi.

Akibatnya, izin tidak akan tercabut dan user tetap memiliki akses penuh. Hal ini berbahaya bila user sudah tidak lagi berwenang. Dalam sistem perpustakaan, petugas yang sudah keluar masih bisa menambahkan data pinjaman baru, yang jelas berisiko manipulasi.

---

#### 2. Lupa membatasi cakupan tabel saat mencabut izin

Kesalahan lain adalah mencabut izin tanpa menyebutkan tabel yang spesifik. Contoh salah:

```sql
REVOKE INSERT ON perpustakaan FROM 'petugas1'@'localhost';
```

Perintah ini tidak benar karena `INSERT` tidak bisa diterapkan langsung pada level database, hanya pada tabel tertentu. Akibatnya, perintah gagal dieksekusi.

Jika mahasiswa tidak memperhatikan hal ini, mereka bisa mengira izin sudah dicabut padahal belum. Dalam kasus perpustakaan, `petugas1` tetap bisa menambahkan data ke tabel `pinjaman`. Ini membuat kebijakan keamanan menjadi tidak efektif.

---

#### 3. Memberi `GRANT` ulang sebelum `REVOKE`

Ada yang beranggapan bahwa untuk mencabut izin, cukup menimpa dengan perintah `GRANT`. Contoh salah:

```sql
GRANT SELECT ON perpustakaan.pinjaman TO 'petugas1'@'localhost';
```

Meskipun perintah ini berhasil, sebenarnya user masih menyimpan izin `INSERT` lama. MariaDB tidak otomatis mencabut izin lama jika tidak dinyatakan dengan `REVOKE`.

Kesalahan ini menyebabkan user tetap bisa menambahkan data pinjaman meskipun niatnya hanya memberi izin baca. Tanpa verifikasi login, kesalahan ini tidak langsung terlihat. Karena itu, pemahaman tentang mekanisme `REVOKE` menjadi sangat penting.

---

#### 4. Tidak melakukan `FLUSH PRIVILEGES` setelah mencabut izin

Kesalahan klasik lain adalah lupa melakukan sinkronisasi. Contoh alurnya:

```sql
REVOKE INSERT ON perpustakaan.pinjaman FROM 'petugas1'@'localhost';
-- lupa FLUSH PRIVILEGES;
```

Tanpa `FLUSH PRIVILEGES`, perubahan tidak langsung diterapkan. User masih bisa melakukan `INSERT` meski seharusnya sudah dilarang.

Dalam simulasi login, hal ini membuat mahasiswa kebingungan karena hasil tidak sesuai ekspektasi. Kesalahan ini sederhana, tetapi bisa mengacaukan proses uji coba. Oleh sebab itu, `FLUSH PRIVILEGES` wajib dilakukan setiap kali ada perubahan izin.

---

#### 5. Salah menuliskan nama user atau host

Kesalahan terakhir adalah salah ketik nama user atau host. Contoh:

```sql
REVOKE INSERT ON perpustakaan.pinjaman FROM 'petugas'@'localhost';
```

Padahal user yang benar adalah `'petugas1'@'localhost'`. Karena salah penulisan, MariaDB tidak akan mengubah izin apa pun.

Akibatnya, admin merasa sudah mencabut izin, padahal sebenarnya tidak ada perubahan. Hal ini bisa berbahaya jika user tersebut masih aktif. Di perpustakaan, kesalahan ini bisa membuat anggota tetap bisa menambah data tanpa batas.

---


### Best Practice

#### 1) Tulis `REVOKE` dengan format lengkap dan spesifik

Gunakan pola lengkap `REVOKE <privileges> ON <db>.<table> FROM <user>@<host>;` agar tidak ambigu.

```sql
REVOKE INSERT ON perpustakaan.pinjaman FROM 'petugas1'@'localhost';
```

Format ini memastikan cakupan pencabutan izin tepat pada tabel yang dimaksud. Menuliskan objek yang spesifik mencegah salah cabut di skema lain dengan nama mirip. Setelahnya, verifikasi dengan uji login singkat agar hasilnya terukur. Kebiasaan disiplin ini membuat kebijakan akses tetap konsisten.

#### 2) Cabut izin bertahap sebelum menghapus user

Saat user sudah tidak aktif, cabut seluruh izin terlebih dahulu, lalu hapus akunnya.

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'petugas1'@'localhost';
DROP USER 'petugas1'@'localhost';
```

Langkah bertahap membantu audit menyeluruh terhadap sisa hak yang mungkin tertinggal. Ini juga mengurangi risiko “orphan privilege” yang mengikat ke objek baru. Dengan urutan ini, jejak administrasi lebih rapi bila perlu ditinjau ulang. Praktik ini penting dalam lingkungan perpustakaan yang multi-pengguna.

#### 3) Gunakan role sebagai sumber kebenaran izin

Kelola izin pada level role, bukan langsung di banyak user.

```sql
CREATE ROLE IF NOT EXISTS 'petugas_pinjaman';
GRANT SELECT, INSERT ON perpustakaan.pinjaman TO 'petugas_pinjaman';
GRANT 'petugas_pinjaman' TO 'petugas_baru'@'localhost';
```

Dengan pola ini, perubahan cukup dilakukan sekali pada role. Seluruh user yang terkait otomatis menerima pembaruan izin. Ini mengurangi human error saat skala pengguna bertambah. Selain itu, dokumentasi kebijakan akses menjadi lebih mudah dibaca.

#### 4) Audit dan verifikasi pasca-penyesuaian izin

Setelah `GRANT`/`REVOKE`, lakukan uji fungsional minimal.

```sql
-- sebagai user uji
SELECT * FROM perpustakaan.pinjaman;
INSERT INTO perpustakaan.pinjaman (id_pinjaman,id_anggota,id_buku,tgl_pinjam) VALUES (10001, 7, 42, CURRENT_DATE());
```

Uji ini memastikan izin efektif sesuai rancangan—misalnya hanya `SELECT` yang lolos dan `INSERT` ditolak. Verifikasi berbasis tindakan nyata lebih kuat daripada asumsi. Sertakan skenario positif dan negatif agar bukti lengkap. Simpan hasil uji sebagai catatan audit singkat.

#### 5) Sinkronisasi metadata hak akses dan rapikan host

Setelah perubahan akses, selaraskan metadata dan batasi host secara ketat.

```sql
FLUSH PRIVILEGES;
ALTER USER 'anggota_readonly'@'localhost' IDENTIFIED BY 'Anggota#2025!';
```

Menyegarkan metadata membantu memastikan perilaku konsisten di sesi berikutnya. Membatasi host ke `localhost` menutup permukaan serangan jarak jauh yang tidak perlu. Di sistem perpustakaan, aplikasi biasanya berjalan di server yang sama dengan DB. Kombinasi keduanya menjaga integritas dan mengerem eskalasi risiko.

---

### Studi Kasus

Perpustakaan ingin menurunkan hak `INSERT` dari petugas magang menjadi hanya `SELECT` pada tabel `pinjaman`, sambil memastikan ia tetap dapat melihat data `buku` seperti anggota; mari kita lakukan konfigurasi berbasis role agar mudah dirawat dan diaudit. Pertama, buat role `petugas_magang` yang awalnya memiliki `SELECT, INSERT` pada `pinjaman`, lalu atur akses baca ke `buku`. Sekarang jalankan perintah untuk membuat role dan memberi haknya agar dapat diuji bersama tim. Setelah itu, tetapkan user `magang1` ke role tersebut untuk memulai simulasi. Praktik pembatasan berbasis peran ini sejalan dengan prinsip *least privilege* untuk menjaga integritas data peminjaman di lingkungan multiuser yang dinamis (Elmasri & Navathe, 2016; Sandhu et al., 1996).

```sql
CREATE ROLE IF NOT EXISTS 'petugas_magang', 'anggota';
GRANT SELECT, INSERT ON perpustakaan.pinjaman TO 'petugas_magang';
GRANT SELECT ON perpustakaan.buku TO 'anggota';
CREATE USER IF NOT EXISTS 'magang1'@'localhost' IDENTIFIED BY 'Magang#2025!';
GRANT 'petugas_magang', 'anggota' TO 'magang1'@'localhost';
```

Mari kita uji perilaku saat ini: login sebagai `magang1` dan coba tambahkan satu catatan peminjaman baru, kemudian baca daftar pinjaman dan buku. Uji positif dan negatif penting untuk memastikan kebijakan benar-benar efektif di eksekusi, bukan hanya di atas kertas. Jika `INSERT` dan `SELECT` pada `pinjaman` berhasil, dan `SELECT` pada `buku` juga berhasil, berarti baseline kita benar. Dokumentasikan hasil uji sebagai bahan audit agar perubahan berikutnya dapat ditelusuri dan direproduksi. Pendekatan berbasis pengujian ini direkomendasikan untuk administrasi DB yang dapat dipertanggungjawabkan (Ramakrishnan & Gehrke, 2003; Kifer, Bernstein, & Lewis, 2006).

```bash
# sebagai magang1
mysql -u magang1 -p
# uji
INSERT INTO perpustakaan.pinjaman (id_pinjaman,id_anggota,id_buku,tgl_pinjam)
VALUES (20001, 9, 77, CURRENT_DATE());
SELECT * FROM perpustakaan.pinjaman;
SELECT * FROM perpustakaan.buku;
```

Sekarang kebijakan berubah: magang hanya boleh membaca, tidak boleh menambahkan peminjaman; mari kita cabut `INSERT` dari role agar semua pemegang role ikut diperbarui. Mencabut pada level role memastikan tidak ada user yang tertinggal dengan izin lama yang berbahaya. Setelah `REVOKE`, jalankan sinkronisasi dan ulangi uji untuk memverifikasi bahwa `INSERT` ditolak namun `SELECT` tetap berjalan. Kerja bagus—pendekatan ini memotong risiko *orphan privilege* dan memudahkan rollback bila diperlukan. Praktik pengelolaan siklus hidup izin seperti ini dianjurkan dalam pengendalian akses berbasis peran yang terdokumentasi baik (Sandhu et al., 1996; Thuraisingham, 2008).

```sql
REVOKE INSERT ON perpustakaan.pinjaman FROM 'petugas_magang';
FLUSH PRIVILEGES;
```

Mari perluas studi kasus: admin mendapati bahwa `magang1` juga tidak boleh melihat denda, sehingga akses ke tabel `denda` perlu diatur eksplisit. Kita akan menahan akses apa pun ke `denda` bagi role `petugas_magang`, sambil memastikan admin tetap penuh. Lakukan verifikasi negatif: sebagai `magang1`, coba `SELECT` pada `denda` dan pastikan ditolak dengan pesan *access denied*. Lalu sebagai admin, pastikan operasi `SELECT` dan `INSERT` pada `denda` tetap sukses. Segmentasi objek seperti ini mencegah *data leakage* dari domain yang sensitif (Elmasri & Navathe, 2016; Thuraisingham, 2008).

```sql
-- pastikan tidak ada GRANT ke denda untuk petugas_magang
REVOKE ALL PRIVILEGES ON perpustakaan.denda FROM 'petugas_magang';
-- (abaikan error jika belum ada izin)
```

Terakhir, mari rapikan atribusi role agar jelas siapa pengguna aktif dan siapa yang perlu dinonaktifkan; ini penting untuk audit akhir semester. Jika magang selesai masa tugas, cabut semua izin dan hapus user—lakukan bertahap agar tercatat bersih. Dokumentasikan *who/when/why* di buku log administrasi untuk kepatuhan internal. Langkah komprehensif ini menutup siklus akses sekaligus menjaga arsip audit untuk pemeriksaan mendatang. Prosedur pencabutan bertahap dan penghapusan akun tercatat luas dalam praktik pengelolaan keamanan database yang sistematis (Ramakrishnan & Gehrke, 2003; Kifer, Bernstein, & Lewis, 2006).

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'magang1'@'localhost';
DROP USER 'magang1'@'localhost';
```

### Kesimpulan

Modul ini menunjukkan bagaimana `GRANT` dan `REVOKE` bekerja sama membentuk kontrol akses yang hidup dan dapat diaudit pada sistem perpustakaan, dari pemberian wewenang awal hingga pencabutan terarah. Pendekatan berbasis role menyederhanakan administrasi, mengurangi kesalahan manual, dan memastikan konsistensi saat jumlah pengguna bertambah. Uji positif dan negatif setelah setiap perubahan menjadikan kebijakan dapat dibuktikan secara operasional, bukan asumsi. Pencabutan bertahap sebelum penghapusan akun menutup celah *orphan privilege* dan memperkuat jejak audit. Prinsip-prinsip ini sejalan dengan literatur akses berbasis peran dan praktik keamanan database modern (Sandhu et al., 1996; Thuraisingham, 2008).

### Referensi

* Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems* (7th ed.). Pearson.
* Ramakrishnan, R., & Gehrke, J. (2003). *Database Management Systems* (3rd ed.). McGraw-Hill.
* Kifer, M., Bernstein, A., & Lewis, P. M. (2006). *Database Systems: An Application-Oriented Approach* (2nd ed.). Addison-Wesley.
* Sandhu, R. S., Coyne, E. J., Feinstein, H. L., & Youman, C. E. (1996). Role-Based Access Control. *ACM Computing Surveys, 28*(1), 3–26.
* Thuraisingham, B. (2008). *Database and Applications Security* (2nd ed.). CRC Press.
