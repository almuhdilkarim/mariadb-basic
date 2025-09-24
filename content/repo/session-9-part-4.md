---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Bentuk Normal Kedua (2NF)"
short: "2NF"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 9
lister: 4
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Desain"
      icon: ""
      desc: "Menghilangkan ketergantungan parsial antar kolom"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta belajar bentuk normal kedua (2NF) untuk menghindari ketergantungan parsial. Modul ini membantu mendesain tabel yang lebih efisien dan konsisten."
---


### Pertemuan 8 — Submodul 4

**Bentuk Normal Kedua (2NF) dalam Database Perpustakaan**

---

#### Pendahuluan

Bentuk Normal Kedua (2NF) adalah tahap lanjutan setelah 1NF yang berfokus pada eliminasi ketergantungan parsial dalam tabel. Menurut Harrington (2016), sebuah tabel dikatakan memenuhi 2NF jika sudah berada dalam 1NF dan semua atribut non-key bergantung sepenuhnya pada primary key, bukan hanya sebagian dari composite key. Dalam sistem perpustakaan, hal ini penting untuk memastikan bahwa data seperti alamat anggota tidak bergantung pada sebagian atribut saja. Dengan demikian, struktur tabel akan lebih konsisten dan terhindar dari redundansi yang tidak perlu.

Salah satu masalah yang sering muncul ketika belum mencapai 2NF adalah adanya atribut yang bergantung hanya pada sebagian dari kunci gabungan. Misalnya, jika tabel pinjaman menggunakan composite key berupa `(id_anggota, id_buku)`, maka atribut seperti `nama_anggota` hanya bergantung pada `id_anggota` saja. Situasi ini menimbulkan anomali ketika data diperbarui, karena `nama_anggota` akan diulang berkali-kali. Menurut Pratt dan Last (2015), hal ini menyebabkan pemborosan ruang penyimpanan dan meningkatkan risiko inkonsistensi data.

Dengan menerapkan 2NF, tabel besar biasanya dipecah menjadi tabel-tabel yang lebih kecil berdasarkan dependensi penuh terhadap primary key. Dalam perpustakaan, data anggota disimpan di tabel **anggota**, sedangkan data pinjaman hanya menyimpan referensi berupa `id_anggota` dan `id_buku`. Dengan cara ini, perubahan pada nama anggota cukup dilakukan di satu tabel saja. Menurut Kroenke dan Auer (2014), praktik ini sangat penting untuk menjaga kualitas database relasional.

Selain mengurangi redundansi, 2NF juga meningkatkan fleksibilitas database dalam menangani transaksi baru. Misalnya, ketika anggota berganti alamat, perubahan tersebut tidak mempengaruhi tabel pinjaman. Hal ini membuat operasi update menjadi lebih efisien dan aman. Menurut Stephens dan Plew (2013), pemisahan data sesuai dependensinya akan membantu pengembang menjaga integritas sistem dalam jangka panjang.

Tahap 2NF ini sangat penting karena menjadi fondasi untuk menuju bentuk normal yang lebih tinggi, yaitu 3NF. Tanpa 2NF, database akan terus mengalami masalah redundansi data meskipun sudah memenuhi 1NF. Menurut Adamski dan Pratt (2011), normalisasi ke 2NF memastikan setiap atribut benar-benar melekat pada identitas unik baris tabel. Oleh karena itu, penerapan 2NF dalam database perpustakaan akan menjamin keakuratan dan kehandalan sistem.

---

#### Langkah-Langkah Praktik

Mari kita coba membuat contoh tabel pinjaman yang belum memenuhi 2NF. Perhatikan desain berikut:

```sql
CREATE TABLE pinjaman (
    id_anggota INT,
    id_buku INT,
    nama_anggota VARCHAR(100),
    alamat VARCHAR(200),
    tanggal_pinjam DATE,
    PRIMARY KEY (id_anggota, id_buku)
);
```

Sekarang, jalankan query berikut untuk memasukkan data:

```sql
INSERT INTO pinjaman VALUES
(1, 101, 'Ahmad Fauzi', 'Jl. Melati No.1', '2025-09-01'),
(1, 102, 'Ahmad Fauzi', 'Jl. Melati No.1', '2025-09-05');
```

Perhatikan bahwa `nama_anggota` dan `alamat` diulang pada setiap baris meskipun identitas anggota sama. Inilah contoh pelanggaran 2NF karena atribut tersebut hanya bergantung pada `id_anggota`, bukan pada kombinasi `(id_anggota, id_buku)`.

Mari kita pecah tabel agar sesuai dengan 2NF. Pertama, buat tabel **anggota**:

```sql
CREATE TABLE anggota (
    id_anggota INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    alamat VARCHAR(200)
);
```

Lalu, perbaiki tabel **pinjaman** agar hanya menyimpan referensi `id_anggota`:

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    id_anggota INT,
    id_buku INT,
    tanggal_pinjam DATE,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Sekarang coba isi tabel dan jalankan query `SELECT *` untuk memverifikasi. Kerja bagus, langkahmu sudah tepat menuju database yang lebih efisien.

---

#### Kesalahan Umum

##### 1. Menyimpan Data Anggota Berulang Kali di Tabel Pinjaman

```sql
INSERT INTO pinjaman VALUES
(2, 201, 'Budi Santoso', 'Jl. Mawar No.2', '2025-09-03');
```

Kesalahan ini menyebabkan `nama_anggota` dan `alamat` diulang di setiap baris. Pratt dan Last (2015) menekankan bahwa data berulang menimbulkan inkonsistensi saat ada perubahan. Hal ini juga membuat tabel semakin membengkak.

##### 2. Menggunakan Composite Key tanpa Memperhatikan Dependensi

```sql
PRIMARY KEY (id_anggota, id_buku)
```

Composite key ini benar secara teknis, tetapi atribut `nama_anggota` hanya bergantung pada `id_anggota`. Menurut Harrington (2016), hal ini melanggar aturan 2NF. Database menjadi tidak efisien.

##### 3. Tidak Memecah Tabel Meski Ada Ketergantungan Parsial

```sql
-- Semua atribut disimpan di tabel pinjaman
```

Tidak ada upaya memisahkan atribut ke tabel yang sesuai. Kroenke dan Auer (2014) menyebut ini sebagai praktik buruk dalam desain database. Hal ini membuat update data menjadi rumit.

##### 4. Mengandalkan Atribut Non-Key sebagai Identitas

```sql
PRIMARY KEY (nama_anggota, id_buku)
```

Menggunakan `nama_anggota` sebagai bagian dari primary key tidak sesuai standar. Stephens dan Plew (2013) menjelaskan bahwa nama tidak selalu unik. Ini bisa menimbulkan data duplikat.

##### 5. Tidak Menentukan Foreign Key

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    id_anggota INT,
    id_buku INT
);
```

Tanpa foreign key, database tidak menjamin bahwa `id_anggota` benar-benar ada di tabel anggota. Adamski dan Pratt (2011) menegaskan bahwa foreign key sangat penting untuk integritas referensial. Kesalahan ini rawan menyebabkan data yatim.

---

#### Best Practice

##### 1. Pisahkan Data Anggota ke Tabel Tersendiri

```sql
CREATE TABLE anggota (
    id_anggota INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    alamat VARCHAR(200)
);
```

Dengan cara ini, data anggota hanya disimpan sekali. Harrington (2016) menyebut pemisahan ini sebagai inti 2NF.

##### 2. Gunakan Primary Key Tunggal pada Tabel Pinjaman

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    id_anggota INT,
    id_buku INT
);
```

Primary key tunggal lebih jelas dan menghindari dependensi parsial. Pratt dan Last (2015) menekankan pentingnya struktur ini untuk efisiensi.

##### 3. Hubungkan Tabel dengan Foreign Key

```sql
ALTER TABLE pinjaman
ADD CONSTRAINT fk_anggota FOREIGN KEY (id_anggota)
REFERENCES anggota(id_anggota);
```

Foreign key menjaga integritas antar tabel. Kroenke dan Auer (2014) menyebutnya sebagai praktik wajib dalam desain relasional.

##### 4. Hindari Redundansi Data dalam Tabel Pinjaman

```sql
-- Simpan hanya id_anggota, jangan nama dan alamat
```

Data anggota cukup direferensikan melalui ID. Stephens dan Plew (2013) menjelaskan bahwa strategi ini mengurangi anomali update.

##### 5. Selalu Cek Dependensi Atribut terhadap Primary Key

```sql
-- Pastikan semua atribut non-key bergantung penuh pada primary key
```

Adamski dan Pratt (2011) menegaskan pentingnya analisis dependensi. Praktik ini menjamin tabel benar-benar sesuai 2NF.

---

#### Studi Kasus

Misalkan sebuah perpustakaan menyimpan data pinjaman dengan mengulang nama dan alamat anggota di setiap baris. Hal ini membuat laporan menjadi boros dan rawan salah. Untuk memperbaikinya, mari kita terapkan 2NF.

Pertama, buat tabel **anggota**:

```sql
CREATE TABLE anggota (
    id_anggota INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    alamat VARCHAR(200)
);
```

Lalu, buat tabel **pinjaman**:

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    id_anggota INT,
    id_buku INT,
    tanggal_pinjam DATE,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Sekarang jalankan query untuk memasukkan data:

```sql
INSERT INTO anggota VALUES (1, 'Ahmad Fauzi', 'Jl. Melati No.1');
INSERT INTO pinjaman VALUES (101, 1, 301, '2025-09-01');
INSERT INTO pinjaman VALUES (102, 1, 302, '2025-09-05');
```

Kerja bagus! Dengan desain ini, perubahan alamat Ahmad Fauzi cukup dilakukan di tabel anggota, tanpa memengaruhi tabel pinjaman.

---

#### Kesimpulan

Bentuk Normal Kedua (2NF) memastikan bahwa setiap atribut non-key bergantung penuh pada primary key, bukan hanya sebagian. Dalam sistem perpustakaan, penerapan 2NF mengurangi redundansi dengan memisahkan data anggota dari tabel pinjaman. Praktik ini mempermudah proses pembaruan data dan menjaga integritas database. Dengan 2NF, database menjadi lebih efisien dan konsisten. Tahap ini juga menyiapkan dasar untuk normalisasi ke 3NF.

---

#### Referensi

* Harrington, J. L. (2016). *Relational Database Design and Implementation*. Morgan Kaufmann.
* Pratt, P. J., & Last, M. J. (2015). *Concepts of Database Management*. Cengage Learning.
* Kroenke, D. M., & Auer, D. J. (2014). *Database Concepts*. Pearson.
* Stephens, R., & Plew, R. (2013). *Database Design*. Pearson Education.
* Adamski, J. J., & Pratt, P. J. (2011). *Database Systems: A Practical Approach*. Cengage Learning.

