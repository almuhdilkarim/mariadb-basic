---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Praktik View Mandiri"
short: "Praktik"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 11
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
      desc: "Latihan membuat transaksi peminjaman buku"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mempraktikkan transaksi peminjaman buku menggunakan perintah START, COMMIT, dan ROLLBACK. Modul ini memperkuat pemahaman konsep ACID melalui latihan nyata."
---



### Pertemuan 10 — Submodul 5

### Latihan Mandiri View (Laporan Perpustakaan)

---

#### Pendahuluan

Latihan mandiri ini bertujuan untuk melatih keterampilan pustakawan dalam memanfaatkan view untuk membuat laporan perpustakaan yang lebih profesional. View dapat digunakan untuk berbagai kebutuhan, mulai dari laporan harian, bulanan, hingga tahunan. Dengan membuat view, pustakawan tidak perlu lagi menulis query panjang setiap kali laporan dibutuhkan. Hal ini sangat menghemat waktu dan mengurangi risiko kesalahan penulisan query【Elmasri & Navathe, 2016】.

Selain efisiensi, latihan mandiri ini juga menekankan pentingnya konsistensi. Dengan menggunakan view yang sama, laporan yang dibuat oleh pustakawan berbeda akan tetap memiliki hasil seragam. Konsistensi ini menjadi sangat penting terutama ketika laporan digunakan untuk pengambilan keputusan manajemen. Dengan latihan ini, pustakawan belajar membangun standar pelaporan yang lebih dapat diandalkan【Connolly & Begg, 2015】.

Latihan ini juga melatih pemahaman pustakawan tentang keamanan data. Dengan membuat view yang spesifik, pustakawan dapat mengendalikan data apa saja yang bisa diakses pengguna lain. Misalnya, anggota hanya boleh mengakses daftar buku yang tersedia tanpa melihat data internal seperti harga pengadaan. Praktik ini menjaga privasi sekaligus mendukung prinsip keamanan sistem informasi【Rob & Coronel, 2007】.

Selain itu, pustakawan akan berlatih membuat view yang dapat mendukung analisis tren. Misalnya, laporan pinjaman bulanan dapat digunakan untuk melihat pola minat anggota dari waktu ke waktu. Data ini sangat berguna untuk perencanaan koleksi dan penyesuaian kebijakan layanan. Dengan latihan ini, pustakawan belajar menghubungkan data operasional dengan analisis strategis【Date, 2004】.

Secara ringkas, latihan mandiri pembuatan view memberikan manfaat ganda: menguasai keterampilan teknis SQL sekaligus memahami penerapannya dalam manajemen perpustakaan. Dengan menguasai latihan ini, pustakawan akan lebih percaya diri dalam menghasilkan laporan yang relevan, akurat, dan bermanfaat. Hal ini mendukung pengelolaan perpustakaan yang berbasis data【Kroenke & Auer, 2015】.

---

#### Langkah-langkah Praktik

Mari kita coba latihan pertama: membuat view ringkasan bulanan yang menampilkan jumlah pinjaman dan keterlambatan.

```sql
CREATE VIEW latihan_ringkasan_bulanan AS
SELECT YEAR(p.tanggal_pinjam) AS tahun, MONTH(p.tanggal_pinjam) AS bulan,
       COUNT(*) AS total_pinjam,
       SUM(CASE WHEN p.tanggal_kembali > p.tanggal_jatuh_tempo THEN 1 ELSE 0 END) AS total_terlambat
FROM peminjaman p
GROUP BY YEAR(p.tanggal_pinjam), MONTH(p.tanggal_pinjam);
```

Sekarang jalankan:

```sql
SELECT * FROM latihan_ringkasan_bulanan WHERE tahun=2025;
```

Kerja bagus! Kamu sudah bisa menampilkan laporan bulanan dengan cepat.

Latihan kedua, buat view untuk laporan anggota dengan total pinjaman.

```sql
CREATE VIEW latihan_anggota_pinjaman AS
SELECT a.id_anggota, a.nama_anggota, COUNT(p.id_peminjaman) AS jumlah_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota
GROUP BY a.id_anggota, a.nama_anggota;
```

Lalu coba panggil:

```sql
SELECT * FROM latihan_anggota_pinjaman ORDER BY jumlah_pinjam DESC LIMIT 5;
```

Bagus sekali, sekarang kamu bisa mengetahui anggota paling aktif!

Latihan ketiga, buat view buku populer berdasarkan kategori.

```sql
CREATE VIEW latihan_buku_populer AS
SELECT b.kategori, b.judul_buku, COUNT(p.id_peminjaman) AS total_pinjam
FROM buku b
JOIN peminjaman p ON b.id_buku = p.id_buku
GROUP BY b.kategori, b.judul_buku
ORDER BY total_pinjam DESC;
```

Kemudian jalankan:

```sql
SELECT * FROM latihan_buku_populer WHERE kategori='Teknologi' LIMIT 5;
```

Hebat, kamu sudah bisa menampilkan buku populer per kategori!

Latihan keempat, buat view laporan keterlambatan dengan denda.

```sql
CREATE VIEW latihan_keterlambatan_denda AS
SELECT a.nama_anggota, b.judul_buku,
       GREATEST(DATEDIFF(COALESCE(p.tanggal_kembali, NOW()), p.tanggal_jatuh_tempo),0) AS hari_terlambat,
       GREATEST(DATEDIFF(COALESCE(p.tanggal_kembali, NOW()), p.tanggal_jatuh_tempo),0) * 1000 AS total_denda
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
JOIN buku b ON b.id_buku = p.id_buku;
```

Lalu coba:

```sql
SELECT * FROM latihan_keterlambatan_denda ORDER BY total_denda DESC LIMIT 10;
```

Bagus, laporanmu kini bisa langsung menunjukkan keterlambatan terbesar!

Latihan terakhir, buat view gabungan untuk dashboard ringkasan tahunan.

```sql
CREATE VIEW latihan_dashboard_tahunan AS
SELECT YEAR(p.tanggal_pinjam) AS tahun,
       COUNT(*) AS total_pinjam,
       SUM(CASE WHEN p.tanggal_kembali > p.tanggal_jatuh_tempo THEN 1 ELSE 0 END) AS jumlah_terlambat,
       SUM(GREATEST(DATEDIFF(COALESCE(p.tanggal_kembali, NOW()), p.tanggal_jatuh_tempo),0) * 1000) AS total_denda
FROM peminjaman p
GROUP BY YEAR(p.tanggal_pinjam);
```

Sekarang jalankan:

```sql
SELECT * FROM latihan_dashboard_tahunan;
```

Kerja luar biasa! Kamu sudah membuat view yang bisa dipakai untuk evaluasi tahunan.

---

#### Kesimpulan

Latihan mandiri ini membuktikan bahwa view sangat efektif dalam mendukung pelaporan perpustakaan. Dengan membuat view, pustakawan dapat menghasilkan laporan bulanan, keaktifan anggota, buku populer, keterlambatan, hingga dashboard tahunan. Kesalahan umum seperti menggunakan `SELECT *` atau membuat view tanpa filter harus dihindari agar laporan tetap relevan dan aman. Dengan best practice seperti penamaan jelas, kolom spesifik, dan filter bawaan, view dapat dijadikan fondasi pelaporan rutin. Secara keseluruhan, latihan ini meningkatkan keterampilan teknis SQL sekaligus mendukung manajemen perpustakaan berbasis data.

---

#### Referensi

* Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems* (7th ed.). Pearson.
* Connolly, T., & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management* (6th ed.). Pearson.
* Rob, P., & Coronel, C. (2007). *Database Systems: Design, Implementation, and Management* (7th ed.). Cengage Learning.
* Date, C. J. (2004). *An Introduction to Database Systems* (8th ed.). Addison-Wesley.
* Kroenke, D. M., & Auer, D. J. (2015). *Database Concepts* (7th ed.). Pearson.
