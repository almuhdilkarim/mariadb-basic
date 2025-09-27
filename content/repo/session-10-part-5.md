---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Laporan Keterlambatan"
short: "Praktik"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 10
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
      desc: "Latihan membuat query lebih cepat dengan index"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta melakukan praktik optimisasi query pencarian judul buku dengan menggunakan index. Modul ini memperkuat pemahaman tentang performa database secara nyata."
---


#### Pendahuluan

Laporan keterlambatan merupakan salah satu aspek penting dalam manajemen perpustakaan karena terkait langsung dengan kedisiplinan anggota dan rotasi koleksi. Data keterlambatan membantu pustakawan menentukan tingkat kepatuhan anggota terhadap aturan pengembalian. Dengan informasi ini, perpustakaan dapat mengambil tindakan yang sesuai, seperti memberikan peringatan atau denda. SQL menyediakan fungsi tanggal dan kondisi yang dapat dimanfaatkan untuk menghasilkan laporan ini. Dengan demikian, laporan keterlambatan menjadi sarana kontrol operasional sekaligus evaluasi kebijakan【Silberschatz et al., 2020】.

Selain aspek pengendalian, laporan keterlambatan juga bermanfaat untuk perencanaan sumber daya. Buku yang sering terlambat dikembalikan biasanya merupakan koleksi populer. Dengan informasi ini, pustakawan dapat menambah jumlah eksemplar untuk memenuhi permintaan. Analisis keterlambatan juga dapat menjadi dasar dalam menentukan lama periode peminjaman yang ideal. Oleh sebab itu, laporan ini mendukung kebijakan berbasis data yang lebih efektif【Ramakrishnan & Gehrke, 2003】.

Laporan keterlambatan juga berperan dalam komunikasi dengan anggota. Pustakawan dapat mengirimkan notifikasi kepada anggota yang terlambat mengembalikan buku. Dengan adanya laporan yang jelas, anggota akan lebih sadar untuk mengembalikan buku tepat waktu. Transparansi ini meningkatkan hubungan antara perpustakaan dan pengguna. Hal ini juga menumbuhkan budaya disiplin yang mendukung keberlangsungan layanan【Garcia-Molina et al., 2008】.

Dari perspektif keuangan, laporan keterlambatan memberikan informasi tentang potensi pendapatan dari denda. Denda keterlambatan bisa menjadi sumber dana tambahan untuk operasional perpustakaan. Dengan laporan yang akurat, perhitungan denda dapat dilakukan secara otomatis dan adil. Hal ini mengurangi kesalahan manual serta meningkatkan kepercayaan anggota terhadap sistem. Oleh karena itu, aspek finansial juga diuntungkan dari laporan keterlambatan【Karwin, 2017】.

Secara keseluruhan, laporan keterlambatan bukan hanya soal menghitung siapa yang terlambat, tetapi juga bagaimana data tersebut digunakan untuk memperbaiki layanan. Dengan penguasaan query SQL yang tepat, pustakawan dapat menghasilkan laporan yang komprehensif dan mudah dipahami. Laporan ini mendukung aspek disiplin, operasional, komunikasi, hingga finansial. Oleh sebab itu, keterampilan menyusun laporan keterlambatan wajib dikuasai pustakawan dalam sistem informasi modern【Celko, 2014】.

---

#### Langkah-langkah Praktik

Mari kita mulai dengan membuat laporan anggota yang terlambat mengembalikan buku pada bulan September 2025.

```sql
SELECT a.nama_anggota, p.judul_buku, p.tanggal_jatuh_tempo, p.tanggal_kembali
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE p.tanggal_kembali > p.tanggal_jatuh_tempo
  AND p.tanggal_pinjam >= '2025-09-01'
  AND p.tanggal_pinjam <  '2025-10-01';
```

Query ini menampilkan detail keterlambatan per anggota. Bagus, langkah awalmu sudah tepat!

Selanjutnya, mari kita hitung jumlah hari keterlambatan untuk setiap peminjaman.

```sql
SELECT a.nama_anggota, p.judul_buku,
       DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo) AS hari_terlambat
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE p.tanggal_kembali > p.tanggal_jatuh_tempo;
```

Dengan query ini, pustakawan dapat langsung melihat durasi keterlambatan. Bagus sekali, datanya makin jelas!

Sekarang kita hitung total denda yang harus dibayar anggota.

```sql
SELECT a.nama_anggota,
       SUM(GREATEST(DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo),0) * 1000) AS total_denda
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE p.tanggal_kembali > p.tanggal_jatuh_tempo
GROUP BY a.nama_anggota;
```

Laporan ini memberikan informasi finansial dari keterlambatan. Kerja bagus, laporanmu semakin lengkap!

Mari kita kategorikan tingkat keterlambatan anggota.

```sql
SELECT a.nama_anggota,
       CASE
           WHEN DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo) BETWEEN 1 AND 3 THEN 'Ringan'
           WHEN DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo) BETWEEN 4 AND 7 THEN 'Sedang'
           WHEN DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo) > 7 THEN 'Berat'
           ELSE 'Tepat Waktu'
       END AS kategori
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota;
```

Query ini membuat laporan lebih informatif dengan klasifikasi. Bagus sekali, kamu sudah bisa membuat kategori!

Terakhir, mari buat laporan agregasi jumlah anggota per kategori keterlambatan.

```sql
SELECT kategori, COUNT(*) AS jumlah
FROM (
    SELECT CASE
               WHEN DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo) BETWEEN 1 AND 3 THEN 'Ringan'
               WHEN DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo) BETWEEN 4 AND 7 THEN 'Sedang'
               WHEN DATEDIFF(p.tanggal_kembali, p.tanggal_jatuh_tempo) > 7 THEN 'Berat'
               ELSE 'Tepat Waktu'
           END AS kategori
    FROM peminjaman p
) x
GROUP BY kategori;
```

Laporan ini membantu pustakawan memahami distribusi keterlambatan secara keseluruhan. Hebat, sekarang kamu sudah bisa menghasilkan laporan komprehensif!

---

#### Kesalahan Umum

##### 1) Tidak Memeriksa `NULL` pada Tanggal Kembali

```sql
-- SALAH: akan gagal identifikasi peminjaman yang belum kembali
WHERE tanggal_kembali > tanggal_jatuh_tempo;
```

Jika `tanggal_kembali` bernilai NULL (belum dikembalikan), kondisi ini gagal mendeteksi keterlambatan aktif. Laporan pun tidak lengkap. Gunakan `COALESCE` atau `IS NULL` agar semua kasus tercakup.

##### 2) Salah Menggunakan `DATEDIFF` (Terbalik)

```sql
-- SALAH: hasil negatif
DATEDIFF(tanggal_jatuh_tempo, tanggal_kembali)
```

Bila urutan argumen dibalik, hasil menjadi negatif. Angka keterlambatan salah tafsir, bahkan bisa terlihat anggota lebih awal padahal terlambat.

##### 3) Tidak Membatasi Periode

```sql
-- SALAH: menghitung semua keterlambatan sepanjang tahun
WHERE tanggal_kembali > tanggal_jatuh_tempo;
```

Tanpa filter bulan, laporan terlalu luas. Pustakawan tidak bisa fokus pada periode analisis tertentu, misalnya September 2025.

##### 4) Mengalikan Denda Tanpa Validasi Hari Terlambat

```sql
-- SALAH: denda jadi negatif
hari_terlambat * 1000
```

Tanpa `GREATEST(...,0)`, kasus non-terlambat menghasilkan nilai negatif. Hal ini menurunkan validitas laporan keuangan.

##### 5) Menggunakan `LIKE` untuk Filter Tanggal

```sql
-- SALAH: tidak efisien dan bisa salah hasil
WHERE tanggal_pinjam LIKE '2025-09%';
```

Pendekatan string rawan error format dan tidak memanfaatkan indeks. Query jadi lambat pada dataset besar.

---

#### Best Practice

##### 1) Tangani Kasus Belum Dikembalikan dengan `COALESCE`

```sql
DATEDIFF(COALESCE(tanggal_kembali, NOW()), tanggal_jatuh_tempo)
```

Dengan ini, buku yang belum kembali dihitung terlambat hingga hari ini. Laporan jadi lebih akurat.

##### 2) Pastikan Urutan Argumen `DATEDIFF` Benar

```sql
DATEDIFF(tanggal_kembali, tanggal_jatuh_tempo)
```

Urutan ini menghasilkan nilai positif ketika terlambat. Praktik sederhana ini mencegah salah tafsir.

##### 3) Batasi Periode dengan Rentang Tanggal

```sql
WHERE tanggal_pinjam >= '2025-09-01' 
  AND tanggal_pinjam <  '2025-10-01'
```

Filter rentang memastikan laporan fokus pada periode analisis. Laporan jadi lebih relevan.

##### 4) Validasi Hari Terlambat dengan `GREATEST`

```sql
GREATEST(DATEDIFF(tanggal_kembali, tanggal_jatuh_tempo), 0) * 1000
```

Pendekatan ini mencegah nilai negatif pada perhitungan denda. Laporan keuangan lebih kredibel.

##### 5) Gunakan Fungsi Tanggal, Bukan String Matching

```sql
WHERE tanggal_pinjam BETWEEN '2025-09-01' AND '2025-09-30'
```

Dengan fungsi tanggal, query lebih efisien dan hasilnya lebih tepat. Optimasi indeks pun tetap terjaga.

---

#### Studi Kasus

Seorang pustakawan ingin mengevaluasi keterlambatan di bulan September 2025. Pertama, ia menjalankan query berikut:

```sql
SELECT COUNT(*) AS jumlah_terlambat
FROM peminjaman
WHERE tanggal_kembali > tanggal_jatuh_tempo
  AND tanggal_pinjam >= '2025-09-01'
  AND tanggal_pinjam <  '2025-10-01';
```

Hasilnya menunjukkan jumlah total transaksi terlambat bulan tersebut.

Selanjutnya, ia ingin mengetahui siapa saja anggota yang paling sering terlambat.

```sql
SELECT a.nama_anggota, COUNT(*) AS jumlah_terlambat
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE p.tanggal_kembali > p.tanggal_jatuh_tempo
  AND p.tanggal_pinjam >= '2025-09-01'
  AND p.tanggal_pinjam <  '2025-10-01'
GROUP BY a.nama_anggota
ORDER BY jumlah_terlambat DESC
LIMIT 10;
```

Dengan laporan ini, pustakawan bisa memberikan peringatan kepada anggota tertentu.

Kemudian, pustakawan menghitung total hari keterlambatan.

```sql
SELECT SUM(GREATEST(DATEDIFF(tanggal_kembali, tanggal_jatuh_tempo),0)) AS total_hari
FROM peminjaman
WHERE tanggal_pinjam >= '2025-09-01'
  AND tanggal_pinjam <  '2025-10-01';
```

Informasi ini membantu menghitung dampak keterlambatan terhadap rotasi koleksi.

Pustakawan juga ingin menghitung total denda dari keterlambatan.

```sql
SELECT SUM(GREATEST(DATEDIFF(tanggal_kembali, tanggal_jatuh_tempo),0) * 1000) AS total_denda
FROM peminjaman
WHERE tanggal_pinjam >= '2025-09-01'
  AND tanggal_pinjam <  '2025-10-01';
```

Laporan ini memberikan proyeksi pendapatan dari denda.

Akhirnya, pustakawan membuat laporan distribusi kategori keterlambatan.

```sql
SELECT kategori, COUNT(*) AS jumlah
FROM (
    SELECT CASE
               WHEN DATEDIFF(COALESCE(tanggal_kembali, NOW()), tanggal_jatuh_tempo) BETWEEN 1 AND 3 THEN 'Ringan'
               WHEN DATEDIFF(COALESCE(tanggal_kembali, NOW()), tanggal_jatuh_tempo) BETWEEN 4 AND 7 THEN 'Sedang'
               WHEN DATEDIFF(COALESCE(tanggal_kembali, NOW()), tanggal_jatuh_tempo) > 7 THEN 'Berat'
               ELSE 'Tepat Waktu'
           END AS kategori
    FROM peminjaman
    WHERE tanggal_pinjam >= '2025-09-01'
      AND tanggal_pinjam <  '2025-10-01'
) x
GROUP BY kategori;
```

Dengan laporan ini, pustakawan mendapatkan gambaran lengkap tentang pola keterlambatan anggota.

---

#### Kesimpulan

Laporan keterlambatan adalah instrumen penting dalam manajemen perpustakaan yang mencakup aspek disiplin, layanan, hingga keuangan. Dengan SQL, pustakawan dapat menghitung jumlah keterlambatan, durasi, hingga total denda secara akurat. Kesalahan umum seperti lupa menangani `NULL`, salah urutan `DATEDIFF`, atau tidak membatasi periode bisa menyesatkan laporan. Best practice seperti penggunaan `COALESCE`, `GREATEST`, dan rentang tanggal memastikan hasil lebih valid. Secara keseluruhan, laporan keterlambatan membantu perpustakaan menjaga kedisiplinan anggota, rotasi koleksi, serta memberikan dasar untuk evaluasi kebijakan.

---

#### Referensi

* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2020). *Database System Concepts* (7th ed.). McGraw-Hill.
* Ramakrishnan, R., & Gehrke, J. (2003). *Database Management Systems* (3rd ed.). McGraw-Hill.
* Garcia-Molina, H., Ullman, J. D., & Widom, J. (2008). *Database Systems: The Complete Book* (2nd ed.). Pearson.
* Karwin, B. (2017). *SQL Antipatterns, Volume 1: Avoiding the Pitfalls of Database Programming* (2nd ed.). Pragmatic Bookshelf.
* Celko, J. (2014). *SQL for Smarties: Advanced SQL Programming* (5th ed.). Morgan Kaufmann.

---

