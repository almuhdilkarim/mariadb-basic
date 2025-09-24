---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Bentuk Normal Pertama (1NF)"
short: "1NF"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 9
lister: 3
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Desain"
      icon: ""
      desc: "Mengubah tabel agar tiap kolom atomik"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Pelajari prinsip bentuk normal pertama (1NF) yang memastikan setiap kolom berisi nilai tunggal. Modul ini membahas cara membuat tabel lebih rapi dan mudah dikelola."
---

### Pertemuan 8 — Submodul 3

**Bentuk Normal Pertama (1NF) dalam Database Perpustakaan**

---

#### Pendahuluan

Bentuk Normal Pertama (1NF) adalah tahap dasar dalam proses normalisasi yang bertujuan memastikan setiap tabel memiliki struktur data yang rapi dan terorganisir. Menurut Ramakrishnan dan Gehrke (2003), 1NF menekankan bahwa semua nilai atribut harus bersifat atomik, yaitu tidak dapat dipecah lebih lanjut. Dalam konteks perpustakaan, hal ini berarti setiap kolom hanya menyimpan satu informasi tunggal, misalnya satu nama anggota atau satu judul buku. Jika sebuah kolom berisi daftar nilai, maka tabel tersebut belum memenuhi 1NF. Prinsip ini menjaga data agar lebih mudah diolah dan dianalisis.

Masalah umum yang melatarbelakangi 1NF adalah penyimpanan data yang berulang atau berupa set dalam satu kolom. Misalnya, satu anggota meminjam beberapa buku sekaligus, lalu data semua judul buku disimpan dalam satu kolom. Hal ini menimbulkan kesulitan ketika ingin melakukan pencarian atau perhitungan jumlah pinjaman. Menurut Elmasri dan Navathe (2015), struktur tabel yang melanggar 1NF akan mempersulit sistem dalam melakukan query SQL. Oleh karena itu, penerapan 1NF menjadi solusi penting untuk mencegah data yang sulit diakses.

Selain mempermudah query, 1NF juga membantu mencegah anomali data saat melakukan operasi insert, update, dan delete. Jika tabel masih menyimpan data berulang dalam satu kolom, pembaruan informasi menjadi sangat rumit. Misalnya, jika ada perubahan judul buku, maka seluruh entri dalam satu kolom panjang harus diedit. Rob dan Coronel (2016) menegaskan bahwa 1NF merupakan dasar untuk menjaga konsistensi data dalam sistem informasi. Dengan memenuhi 1NF, sistem perpustakaan dapat lebih stabil.

Implementasi 1NF juga membuat relasi antar tabel menjadi lebih jelas. Sebagai contoh, ketika data anggota dipisahkan dari data pinjaman, sistem bisa lebih fleksibel dalam menambahkan atau menghapus data. Menurut Atzeni, Ceri, Paraboschi, dan Torlone (1999), struktur tabel yang mengikuti 1NF lebih mudah diperluas untuk mendukung kebutuhan baru. Dengan demikian, 1NF bukan hanya tentang menghindari data ganda, tetapi juga tentang membuat sistem lebih berkelanjutan.

Secara keseluruhan, penerapan 1NF pada sistem perpustakaan akan memastikan bahwa data anggota, buku, dan pinjaman tercatat dengan format yang sederhana dan konsisten. Setiap kolom hanya menyimpan satu nilai yang jelas, tanpa ada daftar atau nested data. Tahap ini adalah pondasi sebelum melanjutkan ke bentuk normal yang lebih kompleks. Menurut Coronel dan Morris (2015), tanpa 1NF, proses normalisasi berikutnya tidak dapat dilakukan. Oleh karena itu, 1NF adalah titik awal menuju desain database perpustakaan yang profesional.

---

#### Langkah-Langkah Praktik

Mari kita coba membuat sebuah tabel pinjaman yang belum memenuhi 1NF. Perhatikan contoh berikut:

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    judul_buku VARCHAR(500)
);

INSERT INTO pinjaman VALUES
(1, 'Ahmad Fauzi', 'Database Systems, SQL Fundamentals, Data Management');
```

Dalam contoh ini, kolom **judul\_buku** berisi lebih dari satu judul dalam satu baris. Struktur seperti ini melanggar aturan 1NF. Sekarang jalankan query berikut untuk melihat hasilnya:

```sql
SELECT * FROM pinjaman;
```

Kamu akan melihat bahwa data sulit dipisahkan per judul buku. Mari kita perbaiki tabel agar sesuai dengan 1NF. Caranya adalah dengan memecah setiap judul buku ke dalam baris terpisah.

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    judul_buku VARCHAR(200)
);

INSERT INTO pinjaman VALUES
(1, 'Ahmad Fauzi', 'Database Systems'),
(2, 'Ahmad Fauzi', 'SQL Fundamentals'),
(3, 'Ahmad Fauzi', 'Data Management');
```

Sekarang coba jalankan query `SELECT * FROM pinjaman;` lagi. Hasilnya akan lebih rapi, setiap baris hanya berisi satu informasi judul buku. Kerja bagus! Kamu baru saja memastikan tabel pinjaman memenuhi 1NF.

---

#### Kesalahan Umum

##### 1. Menyimpan Banyak Nilai dalam Satu Kolom

```sql
INSERT INTO pinjaman VALUES
(1, 'Siti Aminah', 'Buku A, Buku B, Buku C');
```

Kesalahan ini membuat data sulit diproses dengan query. Menurut Elmasri dan Navathe (2015), format seperti ini melanggar prinsip atomisitas data. Akibatnya, sistem sulit melakukan filter berdasarkan satu judul tertentu.

##### 2. Membuat Kolom Duplikat untuk Data Berulang

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    buku1 VARCHAR(200),
    buku2 VARCHAR(200),
    buku3 VARCHAR(200)
);
```

Struktur ini menyalahi 1NF karena menambahkan kolom baru untuk data berulang. Rob dan Coronel (2016) menekankan bahwa kolom ganda justru menimbulkan anomali ketika jumlah buku bertambah. Database menjadi tidak fleksibel.

##### 3. Mengabaikan Pemisahan Entitas

```sql
CREATE TABLE transaksi (
    id INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    daftar_buku VARCHAR(500)
);
```

Penyimpanan anggota dan daftar buku dalam tabel tunggal melanggar prinsip 1NF. Atzeni dkk. (1999) menjelaskan bahwa entitas harus dipisahkan sesuai fungsi. Jika tidak, data sulit dipelihara.

##### 4. Tidak Menentukan Primary Key dengan Benar

```sql
CREATE TABLE pinjaman (
    nama_anggota VARCHAR(100),
    judul_buku VARCHAR(200)
);
```

Tanpa primary key, tabel tidak bisa menjamin keunikan data. Coronel dan Morris (2015) menyebut bahwa setiap tabel wajib memiliki identitas unik. Jika tidak, data pinjaman bisa tercatat ganda.

##### 5. Memasukkan Data Nested ke dalam JSON String

```sql
INSERT INTO pinjaman VALUES
(1, 'Budi Santoso', '{"buku":["Buku A","Buku B"]}');
```

Menyimpan daftar buku dalam format JSON dalam kolom tunggal juga melanggar 1NF. Ramakrishnan dan Gehrke (2003) menjelaskan bahwa setiap atribut harus berisi nilai sederhana. JSON hanya cocok untuk sistem NoSQL, bukan relasional.

---

#### Best Practice

##### 1. Pastikan Kolom Menyimpan Nilai Atomik

```sql
INSERT INTO pinjaman VALUES
(1, 'Rina Kusuma', 'Pemrograman Database');
```

Setiap kolom hanya menyimpan satu informasi tunggal. Menurut Elmasri dan Navathe (2015), ini adalah dasar 1NF. Dengan cara ini, query menjadi lebih mudah.

##### 2. Gunakan Primary Key untuk Identitas Unik

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    judul_buku VARCHAR(200)
);
```

Primary key mencegah data ganda. Rob dan Coronel (2016) menegaskan bahwa kunci unik adalah syarat tabel yang valid.

##### 3. Pisahkan Data Berulang ke dalam Baris Tersendiri

```sql
INSERT INTO pinjaman VALUES
(2, 'Ahmad Fauzi', 'Database Systems'),
(3, 'Ahmad Fauzi', 'SQL Fundamentals');
```

Setiap judul buku disimpan dalam baris baru. Atzeni dkk. (1999) menyebut cara ini paling efisien untuk data relasional.

##### 4. Hindari Kolom Duplikat

```sql
-- Salah: buku1, buku2, buku3
-- Benar: satu kolom judul_buku, banyak baris
```

Dengan satu kolom saja, data lebih fleksibel. Coronel dan Morris (2015) menekankan bahwa desain minimalis lebih mudah dipelihara.

##### 5. Gunakan Foreign Key untuk Hubungan Data

```sql
CREATE TABLE anggota (
    id_anggota INT PRIMARY KEY,
    nama VARCHAR(100)
);

CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    id_anggota INT,
    judul_buku VARCHAR(200),
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Relasi ini menjaga konsistensi data antar tabel. Ramakrishnan dan Gehrke (2003) menegaskan bahwa foreign key penting untuk integritas referensial.

---

#### Studi Kasus

Misalkan sebuah perpustakaan awalnya menyimpan data pinjaman dengan format tidak atomik, seperti daftar judul buku dalam satu kolom. Untuk memperbaikinya, kita ubah tabel agar sesuai dengan 1NF.

```sql
CREATE TABLE pinjaman (
    id_pinjaman INT PRIMARY KEY,
    nama_anggota VARCHAR(100),
    judul_buku VARCHAR(200)
);

INSERT INTO pinjaman VALUES
(1, 'Siti Aminah', 'Buku A'),
(2, 'Siti Aminah', 'Buku B'),
(3, 'Siti Aminah', 'Buku C');
```

Sekarang, setiap baris menyimpan satu judul buku. Mari kita coba query berikut:

```sql
SELECT nama_anggota, COUNT(*) AS jumlah_pinjam
FROM pinjaman
GROUP BY nama_anggota;
```

Hasilnya akan menunjukkan jumlah pinjaman per anggota dengan akurat. Langkahmu sudah tepat, kamu baru saja memastikan tabel pinjaman memenuhi prinsip 1NF.

---

#### Kesimpulan

Bentuk Normal Pertama (1NF) adalah dasar dari normalisasi yang memastikan data disimpan dalam format atomik. Dalam sistem perpustakaan, 1NF membantu mencegah data berulang dalam satu kolom dan memastikan struktur tabel tetap rapi. Dengan 1NF, proses pencarian, pembaruan, dan penghapusan data menjadi lebih mudah. Penerapan 1NF juga membuka jalan untuk normalisasi ke tingkat berikutnya. Dengan demikian, 1NF merupakan fondasi utama dalam desain database relasional.

---

#### Referensi

* Ramakrishnan, R., & Gehrke, J. (2003). *Database Management Systems*. McGraw-Hill.
* Elmasri, R., & Navathe, S. B. (2015). *Fundamentals of Database Systems*. Pearson.
* Rob, P., & Coronel, C. (2016). *Database Systems: Design, Implementation, & Management*. Cengage Learning.
* Atzeni, P., Ceri, S., Paraboschi, S., & Torlone, R. (1999). *Database Systems: Concepts, Languages and Architectures*. McGraw-Hill.
* Coronel, C., & Morris, S. (2015). *Database Systems: Design, Implementation, and Management*. Cengage Learning.

---

Apakah mau saya lanjutkan dengan **Submodul 4: Bentuk Normal Kedua (2NF)** menggunakan format praktikal yang sama?
