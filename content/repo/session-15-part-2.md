---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Membuat Stored Procedure Sederhana"
short: "Procedure"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 15
lister: 2
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Otomatisasi"
      icon: ""
      desc: "Latihan membuat prosedur tersimpan sederhana"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mempelajari cara membuat stored procedure sederhana untuk menjalankan query otomatis. Modul ini menekankan langkah dasar pembuatan prosedur SQL."
---

## Pendahuluan

Stored procedure merupakan cara efektif untuk mengelompokkan logika SQL ke dalam satu unit yang bisa dipanggil berulang kali. Dalam sistem perpustakaan digital, banyak operasi rutin yang selalu dilakukan, seperti menampilkan daftar buku atau menambah pinjaman baru. Jika semua dilakukan dengan query manual, resiko kesalahan dan pengulangan kerja menjadi besar. Stored procedure hadir sebagai solusi untuk membuat proses tersebut lebih konsisten.

Konsep sederhana ini mirip dengan SOP (Standard Operating Procedure) dalam layanan perpustakaan. Setiap pustakawan mengikuti langkah yang sama untuk melayani anggota, sehingga kualitas layanan tetap konsisten. Dengan stored procedure, database mengikuti pola yang sama setiap kali menjalankan operasi tertentu.

Dalam modul ini, kita akan mempelajari cara membuat stored procedure sederhana di MariaDB. Fokusnya adalah prosedur yang menampilkan daftar buku dari tabel `buku`. Langkah ini akan menjadi dasar untuk membangun prosedur yang lebih kompleks di modul berikutnya.

Selain membuat, kita juga akan mencoba memanggil stored procedure tersebut. Dengan satu perintah sederhana `CALL`, database akan menjalankan semua perintah SQL yang ada di dalam prosedur. Praktik ini akan menunjukkan manfaat nyata dari stored procedure.

Mari kita siapkan waktu untuk mencoba praktik ini. Jangan khawatir, langkah-langkahnya akan disusun dengan jelas dan mudah diikuti. Kerja bagus, kamu sudah siap belajar hal baru yang sangat berguna!

---

## Langkah-langkah Praktik

Langkah pertama adalah memastikan tabel `buku` sudah tersedia di database. Jika belum ada, buatlah tabel berikut:

```sql
CREATE TABLE buku (
    id_buku INT PRIMARY KEY AUTO_INCREMENT,
    judul VARCHAR(255),
    pengarang VARCHAR(255)
);

INSERT INTO buku (judul, pengarang) VALUES
('Basis Data Lanjut', 'Andi'),
('Algoritma Pemrograman', 'Budi'),
('Sistem Operasi', 'Citra');
```

Sekarang mari kita buat stored procedure sederhana untuk menampilkan daftar buku:

```sql
DELIMITER //
CREATE PROCEDURE tampilkan_buku()
BEGIN
    SELECT * FROM buku;
END //
DELIMITER ;
```

Kerja bagus! Prosedur `tampilkan_buku` sudah tersimpan di database. Untuk menjalankannya, gunakan perintah berikut:

```sql
CALL tampilkan_buku();
```

Hasilnya adalah daftar seluruh buku yang ada di tabel `buku`. Dengan stored procedure ini, kamu tidak perlu lagi mengetik query `SELECT * FROM buku` setiap kali. Satu kali definisi, bisa dipakai berkali-kali.

---

## Kesalahan Umum

### 1. Lupa Menetapkan Delimiter

```sql
CREATE PROCEDURE tampilkan_buku()
BEGIN
    SELECT * FROM buku;
END;
```

Tanpa mengubah `DELIMITER`, MariaDB akan mengira prosedur selesai di titik koma pertama. Hal ini membuat prosedur gagal dibuat. Pemula sering lupa bahwa stored procedure membutuhkan `DELIMITER` khusus.

### 2. Tidak Menutup Blok dengan Benar

```sql
DELIMITER //
CREATE PROCEDURE tampilkan_buku()
BEGIN
    SELECT * FROM buku;
-- lupa menutup END
DELIMITER ;
```

Jika `END` tidak ditulis, MariaDB akan menolak pembuatan prosedur. Kesalahan ini umum terjadi saat developer terburu-buru.

### 3. Salah Menulis Nama Tabel

```sql
DELIMITER //
CREATE PROCEDURE tampilkan_buku()
BEGIN
    SELECT * FROM bukus; -- typo
END //
DELIMITER ;
```

Typo kecil seperti ini akan membuat prosedur error saat dijalankan. Karena prosedur tetap tersimpan, kesalahan hanya muncul saat `CALL`.

### 4. Tidak Menggunakan Kata Kunci CALL

```sql
tampilkan_buku();
```

Memanggil prosedur tanpa `CALL` tidak akan berhasil. MariaDB tidak mengenali sintaks ini. Pemula sering menyamakan prosedur dengan fungsi biasa.

### 5. Membuat Prosedur Tanpa Nama Jelas

```sql
DELIMITER //
CREATE PROCEDURE p1()
BEGIN
    SELECT * FROM buku;
END //
DELIMITER ;
```

Nama seperti `p1` tidak deskriptif, sehingga sulit dikelola jika jumlah prosedur banyak. Gunakan nama yang jelas agar memudahkan administrasi database.

---

## Best Practice

### 1. Selalu Gunakan DELIMITER

```sql
DELIMITER //
CREATE PROCEDURE tampilkan_buku()
BEGIN
    SELECT * FROM buku;
END //
DELIMITER ;
```

Menggunakan `DELIMITER` memastikan prosedur dibuat dengan benar. Ini adalah langkah pertama yang wajib dilakukan setiap kali membuat prosedur.

### 2. Pastikan Blok Ditutup dengan Benar

```sql
DELIMITER //
CREATE PROCEDURE tampilkan_buku()
BEGIN
    SELECT * FROM buku;
END //
DELIMITER ;
```

Menutup dengan `END` membuat prosedur valid. Praktik ini menghindari error sintaks yang sering terjadi.

### 3. Gunakan Nama Prosedur yang Deskriptif

```sql
CREATE PROCEDURE tampilkan_buku();
```

Nama `tampilkan_buku` langsung menjelaskan fungsinya. Praktik ini memudahkan pengelolaan database, terutama dalam sistem besar.

### 4. Gunakan CALL untuk Menjalankan Prosedur

```sql
CALL tampilkan_buku();
```

Menggunakan `CALL` adalah cara resmi menjalankan stored procedure di MariaDB. Praktik ini memastikan prosedur berjalan sesuai harapan.

### 5. Uji Prosedur Setelah Dibuat

```sql
CALL tampilkan_buku();
```

Segera jalankan prosedur setelah dibuat untuk memastikan hasil sesuai. Praktik ini penting agar kesalahan bisa ditemukan lebih awal.

---

## Studi Kasus

Bayangkan seorang pustakawan sering kali diminta menampilkan daftar semua buku di perpustakaan. Tanpa stored procedure, ia harus mengetik query berikut setiap kali:

```sql
SELECT * FROM buku;
```

Untuk menghemat waktu, mari kita buat stored procedure:

```sql
DELIMITER //
CREATE PROCEDURE daftar_buku()
BEGIN
    SELECT * FROM buku;
END //
DELIMITER ;
```

Sekarang, setiap kali ingin menampilkan daftar buku, pustakawan cukup menjalankan:

```sql
CALL daftar_buku();
```

Kerja bagus! Dengan cara ini, pustakawan tidak perlu mengetik query panjang berulang kali. Studi kasus ini menunjukkan manfaat langsung stored procedure sederhana dalam mempermudah pekerjaan rutin.

---

## Kesimpulan

Membuat stored procedure sederhana di MariaDB adalah langkah awal untuk memahami bagaimana logika SQL bisa dikelola lebih baik. Dengan `CREATE PROCEDURE`, `BEGIN ... END`, dan `CALL`, kita bisa menyimpan query yang dapat dijalankan berulang kali. Stored procedure memastikan konsistensi, mengurangi kesalahan, dan menghemat waktu. Praktik ini sangat relevan untuk sistem perpustakaan digital yang sering menjalankan operasi berulang. Dengan penguasaan dasar ini, kita siap melangkah ke prosedur yang lebih kompleks.

---

## Referensi

* Murach, J. (2019). *Murach’s MySQL*. Mike Murach & Associates.
* Gulutzan, P., & Pelzer, T. (1999). *SQL-99 Complete, Really*. R\&D Books.
* Bell, C., Kindahl, F., & Thalmann, L. (2012). *MySQL High Availability*. O’Reilly Media.

