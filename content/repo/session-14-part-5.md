---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Latihan Mandiri Optimasi Query"
short: "Latihan"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 14
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
      desc: "Melatih kemandirian optimisasi query dengan index"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta berlatih secara mandiri menambahkan index dan membandingkan performa query. Modul ini membantu membangun kebiasaan optimisasi database sejak awal."
---


## Pendahuluan

Optimasi query adalah langkah penting untuk memastikan performa sistem database tetap cepat meskipun data semakin besar. Dalam sistem perpustakaan digital, pengguna mengharapkan pencarian buku atau riwayat pinjaman berlangsung dalam hitungan detik. Query yang tidak dioptimalkan bisa menyebabkan server lambat, bahkan hingga gagal merespons. Hal ini akan menurunkan pengalaman pengguna dan membuat sistem terlihat tidak profesional. Oleh karena itu, setiap developer perlu memahami dasar-dasar optimasi query.

Banyak orang beranggapan bahwa optimasi hanya berarti menambahkan index. Padahal, cara menulis query juga sangat berpengaruh. Query yang salah bisa membuat MariaDB mengabaikan index yang sudah dibuat. Sama seperti pustakawan yang menggunakan katalog dengan cara keliru, index yang sudah ada pun tidak bermanfaat jika query tidak ditulis dengan tepat.

Salah satu alat penting dalam optimasi query adalah perintah `EXPLAIN`. Dengan perintah ini, kita bisa melihat bagaimana MariaDB mengeksekusi sebuah query. Hasil dari `EXPLAIN` membantu developer menentukan apakah query sudah efisien atau masih perlu perbaikan. Analisis semacam ini membuat optimasi lebih terukur dan tidak hanya berdasarkan dugaan.

Dalam modul ini, kita akan mempraktikkan beberapa cara optimasi query pada tabel `buku` dan `pinjaman`. Kita akan membandingkan hasil query sebelum dan sesudah optimasi untuk melihat perbedaannya secara nyata. Dengan latihan ini, kamu akan memahami strategi dasar yang bisa diterapkan dalam kehidupan nyata.

Mari kita mulai praktik ini dengan semangat. Ingat, optimasi adalah seni sekaligus ilmu. Kerja bagus sudah sampai di tahap ini, sekarang kita akan belajar cara membuat query lebih cepat dan efisien!

---

## Langkah-langkah Praktik

Langkah pertama adalah menyiapkan tabel `buku` dan `pinjaman`. Berikut struktur sederhana:

```sql
CREATE TABLE buku (
    id_buku INT PRIMARY KEY AUTO_INCREMENT,
    judul VARCHAR(255),
    pengarang VARCHAR(255)
);

CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY AUTO_INCREMENT,
    id_buku INT,
    id_anggota INT,
    tanggal_pinjam DATE,
    FOREIGN KEY (id_buku) REFERENCES buku(id_buku)
);
```

Isi tabel dengan data dummy agar lebih realistis:

```sql
INSERT INTO buku (judul, pengarang)
VALUES ('Basis Data Lanjut', 'Andi'), ('Algoritma Pemrograman', 'Budi');
```

Sekarang jalankan query tanpa optimasi:

```sql
SELECT * FROM pinjaman WHERE id_anggota = 123;
```

Mari kita tambahkan index untuk mempercepat pencarian:

```sql
CREATE INDEX idx_idanggota ON pinjaman(id_anggota);
```

Kerja bagus! Jalankan ulang query dan gunakan `EXPLAIN`:

```sql
EXPLAIN SELECT * FROM pinjaman WHERE id_anggota = 123;
```

Kamu akan melihat perbedaan strategi eksekusi query sebelum dan sesudah index ditambahkan.

---

## Kesalahan Umum

### 1. Menggunakan `SELECT *`

```sql
SELECT * FROM pinjaman WHERE id_anggota = 123;
```

Banyak pemula mengambil semua kolom padahal hanya butuh beberapa saja. Hal ini memperlambat query karena MariaDB harus membaca data lebih banyak dari yang diperlukan. Sama seperti meminjam seluruh rak buku hanya untuk membaca satu judul.

### 2. Kondisi `WHERE` Tidak Sesuai Index

```sql
SELECT * FROM pinjaman WHERE YEAR(tanggal_pinjam) = 2023;
```

Jika kita membuat index pada `tanggal_pinjam`, tetapi query menggunakan fungsi `YEAR()`, index tidak akan digunakan. MariaDB akan tetap melakukan full table scan, membuat pencarian lambat.

### 3. Menggunakan Fungsi pada Kolom yang Diindeks

```sql
SELECT * FROM buku WHERE UPPER(judul) = 'BASIS DATA LANJUT';
```

Fungsi seperti `UPPER` atau `LOWER` membuat MariaDB tidak bisa menggunakan index. Kesalahan ini umum terjadi ketika ingin membuat pencarian case-insensitive.

### 4. JOIN Tanpa Index di Kolom Penghubung

```sql
SELECT * FROM pinjaman p JOIN buku b ON p.id_buku = b.id_buku;
```

Jika kolom `id_buku` tidak memiliki index, JOIN ini akan lambat pada tabel besar. MariaDB harus mencocokkan baris satu per satu tanpa bantuan index.

### 5. Tidak Menggunakan `LIMIT`

```sql
SELECT * FROM pinjaman ORDER BY tanggal_pinjam DESC;
```

Query ini akan menampilkan semua data meski pengguna hanya butuh beberapa baris terakhir. Tanpa `LIMIT`, query membaca seluruh tabel sehingga performa menurun.

---

## Best Practice

### 1. Gunakan Kolom yang Dibutuhkan Saja

```sql
SELECT id_pinjaman, tanggal_pinjam FROM pinjaman WHERE id_anggota = 123;
```

Dengan hanya mengambil kolom yang diperlukan, query menjadi lebih cepat. Praktik ini mengurangi beban transfer data dan pemrosesan.

### 2. Pastikan Kondisi `WHERE` Sesuai Index

```sql
SELECT * FROM pinjaman WHERE tanggal_pinjam = '2023-05-01';
```

Jika index dibuat pada kolom `tanggal_pinjam`, maka query sederhana seperti ini bisa memanfaatkannya. Hindari penggunaan fungsi yang menghalangi optimasi.

### 3. Hindari Fungsi pada Kolom yang Diindeks

```sql
SELECT * FROM buku WHERE judul = 'Basis Data Lanjut';
```

Dengan menulis kondisi langsung, index pada `judul` dapat digunakan dengan efektif. Praktik ini memastikan pencarian berjalan cepat.

### 4. Optimalkan JOIN dengan Index

```sql
CREATE INDEX idx_idbuku ON pinjaman(id_buku);

SELECT * FROM pinjaman p
JOIN buku b ON p.id_buku = b.id_buku;
```

Dengan index pada `id_buku`, JOIN akan berjalan lebih efisien. Ini sangat penting pada tabel besar yang sering digabungkan.

### 5. Gunakan LIMIT Saat Mengambil Data Besar

```sql
SELECT * FROM pinjaman ORDER BY tanggal_pinjam DESC LIMIT 10;
```

Dengan menambahkan `LIMIT`, query hanya mengambil sebagian kecil data. Hal ini menghemat waktu eksekusi dan beban server.

---

## Studi Kasus

Bayangkan admin perpustakaan ingin melihat daftar pinjaman terbaru dari anggota tertentu. Query awal ditulis seperti berikut:

```sql
SELECT * FROM pinjaman WHERE id_anggota = 101 ORDER BY tanggal_pinjam DESC;
```

Query ini berjalan lambat karena tabel `pinjaman` sudah berisi ratusan ribu baris. Mari kita optimasi dengan menambahkan index dan memperbaiki query. Pertama, buat index:

```sql
CREATE INDEX idx_idanggota ON pinjaman(id_anggota);
```

Lalu tulis ulang query dengan `LIMIT`:

```sql
SELECT id_pinjaman, tanggal_pinjam 
FROM pinjaman 
WHERE id_anggota = 101 
ORDER BY tanggal_pinjam DESC 
LIMIT 5;
```

Gunakan `EXPLAIN` untuk memverifikasi:

```sql
EXPLAIN SELECT id_pinjaman, tanggal_pinjam 
FROM pinjaman 
WHERE id_anggota = 101 
ORDER BY tanggal_pinjam DESC 
LIMIT 5;
```

Kerja bagus! Dengan optimasi ini, pencarian pinjaman anggota menjadi jauh lebih cepat. Studi kasus ini membuktikan pentingnya menulis query dengan benar selain menambahkan index.

---

## Kesimpulan

Optimasi query adalah kunci untuk menjaga performa database tetap cepat dan efisien. Tidak cukup hanya membuat index, tetapi cara menulis query juga harus benar. Dengan menggunakan praktik terbaik seperti menghindari `SELECT *`, memastikan kondisi sesuai dengan index, dan menambahkan `LIMIT`, performa bisa meningkat signifikan. Alat `EXPLAIN` membantu memverifikasi strategi eksekusi query. Dengan optimasi rutin, sistem perpustakaan digital akan tetap responsif meski jumlah data terus bertambah.

---

## Referensi

* Snodgrass, R. T. (2009). *Developing Time-Oriented Database Applications in SQL*. Morgan Kaufmann.
* Karwin, B. (2010). *SQL Antipatterns: Avoiding the Pitfalls of Database Programming*. Pragmatic Bookshelf.
* Trudy Pelzer & Hugh Darwen. (1995). *An Introduction to Relational Database Theory*. Bookboon.
