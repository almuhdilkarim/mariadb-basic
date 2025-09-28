---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Menguji Pengaruh Index pada Query"
short: "UjiIndex"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 14
lister: 3
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
description: "Modul ini memperlihatkan perbedaan performa query dengan dan tanpa index. Peserta memahami bagaimana index dapat mempercepat pencarian data dalam tabel besar."
---

## Pendahuluan

Uji performa adalah tahap penting untuk memastikan index benar-benar bermanfaat. Banyak pemula membuat index tanpa menguji dampaknya, sehingga mereka tidak tahu apakah index digunakan oleh query atau tidak. Dalam sistem perpustakaan digital, kecepatan pencarian buku atau data pinjaman sangat menentukan kualitas layanan. Tanpa pengujian, sistem bisa terlihat berjalan normal tetapi sebenarnya bekerja lambat di belakang layar. Oleh karena itu, pengujian performa harus menjadi kebiasaan baik setiap kali menambahkan index.

Perbedaan utama yang akan kita lihat adalah antara **full table scan** dan **index scan**. Full table scan berarti MariaDB membaca seluruh isi tabel untuk menemukan data yang cocok. Index scan berarti MariaDB menggunakan struktur index untuk langsung melompat ke lokasi baris tertentu. Analogi sederhananya, full table scan seperti membuka rak demi rak di perpustakaan, sementara index scan seperti melihat katalog terlebih dahulu.

Tujuan modul ini adalah memberikan pemahaman praktis tentang bagaimana cara menguji pengaruh index pada query. Kita akan membuat tabel, mengisi dengan data yang cukup banyak, lalu menjalankan query sebelum dan sesudah index dibuat. Dengan begitu, kita bisa membandingkan waktu eksekusi dan query plan.

Selain itu, kita akan menggunakan perintah `EXPLAIN` untuk memvisualisasikan cara MariaDB mengeksekusi query. Perintah ini akan menunjukkan apakah query menggunakan index atau tidak. Dengan bukti nyata ini, kamu tidak hanya percaya pada teori, tetapi juga melihat sendiri efek index.

Mari kita mulai modul ini dengan semangat mencoba. Jangan takut dengan jumlah data besar, karena itu justru membuat hasil uji lebih realistis. Kerja bagus, kamu sudah berada di tahap optimasi performa yang lebih serius!

---

## Langkah-langkah Praktik

Pertama, mari kita buat tabel `buku` dan isi dengan data dalam jumlah besar agar uji performa terasa nyata:

```sql
CREATE TABLE buku (
    id_buku INT PRIMARY KEY AUTO_INCREMENT,
    judul VARCHAR(255),
    pengarang VARCHAR(255)
);

-- contoh isi data
INSERT INTO buku (judul, pengarang)
SELECT CONCAT('Judul ', n), 'Pengarang X'
FROM seq_1_to_10000; -- anggap ada generator angka
```

Sekarang, jalankan query pencarian tanpa index:

```sql
SELECT * FROM buku WHERE judul = 'Judul 5000';
```

Query ini akan membutuhkan full table scan karena MariaDB belum memiliki index pada kolom `judul`. Selanjutnya, mari kita buat index:

```sql
CREATE INDEX idx_judul ON buku(judul);
```

Kerja bagus! Sekarang jalankan ulang query yang sama:

```sql
SELECT * FROM buku WHERE judul = 'Judul 5000';
```

Terakhir, gunakan `EXPLAIN` untuk membandingkan query plan sebelum dan sesudah index dibuat:

```sql
EXPLAIN SELECT * FROM buku WHERE judul = 'Judul 5000';
```

Dengan langkah ini, kamu bisa melihat secara langsung bahwa index membuat MariaDB menggunakan **index lookup** daripada full table scan.

---

## Kesalahan Umum

### 1. Tidak Mengisi Tabel dengan Data yang Cukup

```sql
INSERT INTO buku (judul, pengarang) VALUES ('Satu Buku', 'Andi');
```

Jika hanya ada sedikit data, perbedaan performa antara query dengan dan tanpa index tidak akan terasa. Banyak pemula salah menilai karena menggunakan dataset terlalu kecil. Akibatnya, mereka mengira index tidak ada gunanya.

### 2. Mengira Performa Lebih Cepat Tanpa Pembuktian

```sql
SELECT * FROM buku WHERE judul = 'Judul 5';
```

Menjalankan query sekali lalu merasa “sudah lebih cepat” adalah kesalahan. Tanpa menggunakan `EXPLAIN` atau pengukuran waktu, kesimpulan seperti ini hanya berdasarkan perasaan. Database optimasi harus selalu berbasis data yang objektif.

### 3. Membuat Index tetapi Query Tidak Memanfaatkannya

```sql
SELECT * FROM buku WHERE UPPER(judul) = 'JUDUL 5000';
```

Jika query menggunakan fungsi pada kolom index, MariaDB tidak akan menggunakan index tersebut. Kesalahan ini sering terjadi karena developer tidak menyadari bahwa fungsi dapat membatalkan penggunaan index.

### 4. Mengira Semua Query Akan Terpengaruh Index

```sql
SELECT * FROM buku;
```

Query tanpa filter tidak akan menggunakan index. Index hanya berguna jika query memiliki kondisi pencarian (`WHERE`) atau penghubung (`JOIN`). Mengharapkan semua query menjadi lebih cepat adalah kesalahpahaman umum.

### 5. Menghapus Index Tanpa Analisis

```sql
DROP INDEX idx_judul ON buku;
```

Menghapus index tanpa menganalisis query yang berjalan bisa berakibat fatal. Query yang tadinya cepat bisa tiba-tiba menjadi lambat. Selalu cek query log sebelum memutuskan menghapus index.

---

## Best Practice

### 1. Gunakan Dataset Besar untuk Uji Performa

```sql
INSERT INTO buku (judul, pengarang)
SELECT CONCAT('Judul ', n), 'Anonim'
FROM seq_1_to_100000;
```

Semakin besar dataset, semakin jelas terlihat perbedaan performa antara query dengan dan tanpa index. Uji coba pada dataset besar memberikan gambaran yang lebih realistis.

### 2. Gunakan `EXPLAIN` untuk Membuktikan Penggunaan Index

```sql
EXPLAIN SELECT * FROM buku WHERE judul = 'Judul 7000';
```

`EXPLAIN` memberikan bukti objektif apakah index digunakan. Dengan cara ini, kita tidak hanya mengandalkan waktu eksekusi, tetapi juga melihat strategi eksekusi query.

### 3. Uji Query Sebelum dan Sesudah Index Dibuat

```sql
SELECT * FROM buku WHERE judul = 'Judul 8000';
CREATE INDEX idx_judul ON buku(judul);
SELECT * FROM buku WHERE judul = 'Judul 8000';
```

Perbandingan langsung antara sebelum dan sesudah index adalah cara terbaik untuk melihat manfaatnya. Praktik ini juga membangun pemahaman intuitif mengenai peran index.

### 4. Catat Hasil Uji untuk Referensi

```sql
SHOW INDEX FROM buku;
```

Mencatat hasil pengujian performa membuat proses optimasi lebih terarah. Catatan ini bisa digunakan kembali saat sistem berkembang dan data semakin besar.

### 5. Fokus pada Query yang Paling Sering Digunakan

```sql
CREATE INDEX idx_idanggota ON pinjaman(id_anggota);
```

Tidak semua query perlu diuji, cukup fokus pada query yang paling sering digunakan. Dengan demikian, optimasi memberikan dampak nyata pada kinerja sistem perpustakaan.

---

## Studi Kasus

Bayangkan tabel `pinjaman` yang menyimpan jutaan catatan transaksi di perpustakaan. Seorang admin ingin melihat semua pinjaman dari anggota dengan `id_anggota = 1234`. Tanpa index, query berikut akan berjalan lambat:

```sql
SELECT * FROM pinjaman WHERE id_anggota = 1234;
```

Sekarang tambahkan index pada kolom `id_anggota`:

```sql
CREATE INDEX idx_idanggota ON pinjaman(id_anggota);
```

Jalankan ulang query yang sama:

```sql
SELECT * FROM pinjaman WHERE id_anggota = 1234;
```

Gunakan `EXPLAIN` untuk membuktikan bahwa index benar-benar digunakan:

```sql
EXPLAIN SELECT * FROM pinjaman WHERE id_anggota = 1234;
```

Kerja bagus! Hasilnya akan menunjukkan bahwa query sekarang menggunakan **ref** atau **index lookup**, bukan full table scan. Studi kasus ini membuktikan manfaat nyata index dalam mempercepat query di dunia nyata.

---

## Kesimpulan

Menguji pengaruh index pada query adalah langkah penting dalam optimasi database. Dengan membandingkan performa sebelum dan sesudah index dibuat, kita bisa membuktikan manfaat index secara nyata. `EXPLAIN` adalah alat yang wajib digunakan untuk memverifikasi strategi eksekusi query. Praktik ini menunjukkan bahwa index mempercepat pencarian pada dataset besar, tetapi tidak otomatis membuat semua query lebih cepat. Dengan uji rutin, sistem perpustakaan digital dapat tetap responsif meski jumlah data terus bertambah.

---

## Referensi

* Özsu, M. T., & Valduriez, P. (2020). *Principles of Distributed Database Systems*. Springer.
* Garcia-Molina, H., Ullman, J. D., & Widom, J. (2008). *Database Systems: The Complete Book*. Pearson.
* Abiteboul, S., Hull, R., & Vianu, V. (1995). *Foundations of Databases*. Addison-Wesley.

