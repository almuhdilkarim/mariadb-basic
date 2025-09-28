---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "View untuk Laporan"
short: "report"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 11
lister: 4
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Transaksi"
      icon: ""
      desc: "Memahami konflik transaksi dan solusinya"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Pelajari apa itu deadlock dalam transaksi database dan bagaimana cara menghindarinya. Modul ini membekali peserta strategi mengelola konflik transaksi."
---


#### Kesalahan Umum

##### 1) Menganggap View Selalu Menyimpan Data

```sql
-- SALAH: mengira view menyimpan hasil query
SELECT * FROM laporan_pinjaman_bulanan;
```

Banyak pustakawan mengira view menyimpan data seperti tabel fisik. Padahal, view hanyalah query tersimpan yang dieksekusi saat dipanggil. Jika tabel dasar berubah, hasil view juga ikut berubah. Salah paham ini membuat laporan terlihat "berbeda" dari waktu ke waktu, padahal definisi query tidak berubah【Elmasri & Navathe, 2016】.

##### 2) Menggunakan `ORDER BY` di Dalam View

```sql
-- SALAH: ORDER BY di dalam view tidak selalu berfungsi
CREATE VIEW laporan_buku_populer AS
SELECT b.judul_buku, COUNT(*) AS total_pinjam
FROM buku b
JOIN peminjaman p ON b.id_buku = p.id_buku
GROUP BY b.judul_buku
ORDER BY total_pinjam DESC;
```

`ORDER BY` dalam definisi view sering diabaikan oleh DBMS. Pustakawan yang mengandalkan urutan default bisa salah tafsir. Untuk laporan yang memerlukan urutan, `ORDER BY` sebaiknya ditulis pada query pemanggil view, bukan pada definisi view【Connolly & Begg, 2015】.

##### 3) Membuat View Terlalu Umum

```sql
-- SALAH: view tidak memberi nilai tambah
CREATE VIEW semua_data AS
SELECT * FROM peminjaman;
```

View ini hanya menyalin isi tabel tanpa filter atau ringkasan. Pustakawan tetap harus menambahkan kondisi saat memanggilnya. Akibatnya, tujuan view untuk menyederhanakan laporan hilang. View seharusnya berfungsi sebagai shortcut yang bermanfaat, bukan sekadar salinan【Rob & Coronel, 2007】.

##### 4) Menggunakan `SELECT *` dalam View Laporan

```sql
-- SALAH: SELECT * membuat laporan rentan
CREATE VIEW laporan_anggota AS
SELECT * FROM anggota;
```

Dengan `SELECT *`, perubahan struktur tabel bisa merusak view atau menampilkan data sensitif. Misalnya, alamat atau nomor telepon anggota ikut muncul padahal tidak dibutuhkan dalam laporan. Praktik ini berisiko bagi keamanan dan konsistensi【Date, 2004】.

##### 5) Mengira View Bisa Selalu Diupdate

```sql
-- SALAH: mencoba mengubah view dengan agregasi
UPDATE laporan_ringkasan_bulanan SET total_pinjam = 200 WHERE bulan=9;
```

View yang menggunakan agregasi atau fungsi biasanya tidak bisa diupdate. Pustakawan yang mencoba melakukan update langsung pada view akan menemui error. Kesalahpahaman ini muncul karena view terlihat seperti tabel biasa, padahal fungsinya berbeda【Kroenke & Auer, 2015】.

---

#### Best Practice

##### 1) Gunakan View untuk Ringkasan yang Sering Dipakai

```sql
CREATE VIEW laporan_keterlambatan AS
SELECT a.nama_anggota, b.judul_buku,
       DATEDIFF(COALESCE(p.tanggal_kembali, NOW()), p.tanggal_jatuh_tempo) AS hari_terlambat
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
JOIN buku b ON p.id_buku = b.id_buku
WHERE p.tanggal_kembali > p.tanggal_jatuh_tempo OR p.tanggal_kembali IS NULL;
```

View ini mempercepat akses laporan tanpa harus menulis query panjang.

##### 2) Beri Nama View yang Deskriptif

```sql
CREATE VIEW laporan_pinjaman_bulanan AS ...
CREATE VIEW laporan_buku_populer AS ...
```

Penamaan jelas memudahkan pustakawan memahami fungsi view tanpa membuka definisinya.

##### 3) Tentukan Kolom Secara Spesifik

```sql
CREATE VIEW laporan_anggota_aktif AS
SELECT a.id_anggota, a.nama_anggota, COUNT(p.id_peminjaman) AS jumlah_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota
GROUP BY a.id_anggota, a.nama_anggota;
```

Hanya kolom yang relevan ditampilkan, menjaga keamanan dan kestabilan view.

##### 4) Tambahkan Filter Sesuai Tujuan Laporan

```sql
CREATE VIEW pinjaman_aktif AS
SELECT id_peminjaman, id_anggota, id_buku, tanggal_pinjam, tanggal_jatuh_tempo
FROM peminjaman
WHERE tanggal_kembali IS NULL;
```

Filter bawaan membuat view langsung bermanfaat untuk laporan rutin.

##### 5) Gunakan View untuk Kontrol Akses

```sql
CREATE VIEW daftar_buku_umum AS
SELECT judul_buku, kategori
FROM buku;
```

Dengan cara ini, anggota bisa mengakses daftar buku tanpa melihat data sensitif seperti harga atau stok internal.

---

#### Studi Kasus

Seorang pustakawan diminta menyusun laporan bulanan dengan cepat. Pertama, ia menggunakan view ringkasan bulanan:

```sql
SELECT * FROM laporan_ringkasan_bulanan WHERE tahun=2025 AND bulan=9;
```

Hasil query ini langsung memberikan total pinjaman dan jumlah keterlambatan September 2025.

Kedua, ia ingin melihat buku yang paling sering dipinjam.

```sql
SELECT * FROM laporan_buku_populer LIMIT 10;
```

Dengan view ini, pustakawan tidak perlu menulis query join yang panjang.

Ketiga, ia ingin memantau anggota paling aktif.

```sql
SELECT * FROM laporan_anggota_aktif ORDER BY jumlah_pinjam DESC LIMIT 5;
```

Data ini dapat digunakan untuk memberikan penghargaan pembaca teraktif.

Keempat, ia menyiapkan laporan keterlambatan.

```sql
SELECT * FROM laporan_keterlambatan ORDER BY hari_terlambat DESC LIMIT 10;
```

Hasil laporan ini membantu pustakawan memberi peringatan kepada anggota tertentu.

Terakhir, ia menggabungkan semua laporan ke dalam dashboard. Semua data diambil dari view yang sudah ada, sehingga pembuatan dashboard lebih cepat dan konsisten.

---

#### Kesimpulan

Penggunaan view untuk laporan membuat sistem perpustakaan lebih efisien, konsisten, dan aman. View memungkinkan pustakawan menghasilkan laporan rutin tanpa menulis ulang query yang panjang. Kesalahan umum seperti mengira view menyimpan data atau selalu bisa diupdate harus dihindari. Best practice seperti penamaan deskriptif, filter spesifik, dan pemilihan kolom relevan menjadikan view lebih berguna. Studi kasus menunjukkan bagaimana view mempercepat penyusunan laporan bulanan, keterlambatan, dan koleksi populer. Secara keseluruhan, view adalah fondasi penting dalam pelaporan berbasis SQL di perpustakaan.

---

#### Referensi

* Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems* (7th ed.). Pearson.
* Connolly, T., & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management* (6th ed.). Pearson.
* Rob, P., & Coronel, C. (2007). *Database Systems: Design, Implementation, and Management* (7th ed.). Cengage Learning.
* Date, C. J. (2004). *An Introduction to Database Systems* (8th ed.). Addison-Wesley.
* Kroenke, D. M., & Auer, D. J. (2015). *Database Concepts* (7th ed.). Pearson.

