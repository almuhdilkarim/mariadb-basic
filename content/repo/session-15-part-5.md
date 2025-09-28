---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Latihan Prosedur Laporan Harian"
short: "Latihan"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 15
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
      desc: "Membuat prosedur laporan harian sederhana"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta berlatih membuat prosedur untuk menghasilkan laporan harian peminjaman. Modul ini memperkuat keterampilan menggabungkan konsep prosedur dan query praktis."
---

## Pendahuluan

Laporan harian adalah salah satu kebutuhan utama dalam sistem perpustakaan digital. Admin perlu mengetahui berapa banyak buku yang dipinjam setiap hari, siapa saja anggotanya, dan judul apa saja yang sedang populer. Jika laporan ini dibuat secara manual dengan query berbeda-beda, proses akan memakan waktu dan rawan kesalahan. Dengan menggunakan stored procedure, laporan harian dapat dihasilkan secara otomatis hanya dengan satu pemanggilan.

Stored procedure untuk laporan harian memudahkan konsistensi data. Setiap kali dijalankan, format laporan akan sama dan hasilnya bisa langsung dipakai untuk evaluasi. Hal ini mengurangi ketergantungan pada admin untuk menulis query baru setiap hari. Dalam jangka panjang, prosedur ini juga mempermudah pembuatan dashboard dan integrasi dengan aplikasi lain.

Kebutuhan laporan ini bisa dianalogikan dengan catatan harian pustakawan. Setiap hari, pustakawan mencatat siapa saja yang meminjam buku dan berapa banyak transaksi yang terjadi. Jika dicatat manual, ada kemungkinan salah hitung. Stored procedure berperan seperti “form laporan digital” yang otomatis terisi berdasarkan data pinjaman di database.

Dalam modul ini, kita akan membuat stored procedure untuk menghasilkan laporan pinjaman harian. Prosedur akan mengambil parameter berupa tanggal dan mengembalikan daftar pinjaman pada hari itu. Dengan pendekatan ini, laporan bisa dijalankan berulang kali untuk tanggal yang berbeda tanpa harus menulis query baru.

Mari kita telusuri praktik ini secara bertahap. Jangan ragu untuk mencoba langsung di database. Kerja bagus, kamu sudah memasuki tahap membuat laporan otomatis yang sangat berguna bagi manajemen perpustakaan!

---

## Langkah-langkah Praktik

Pertama, pastikan tabel `pinjaman` sudah tersedia dan berisi data dengan kolom `id_pinjaman`, `id_buku`, `id_anggota`, dan `tanggal_pinjam`. Tabel ini menjadi sumber utama untuk laporan harian. Jika data masih sedikit, tambahkan beberapa entri dummy agar hasil laporan lebih nyata. Gunakan `INSERT` dengan tanggal berbeda supaya kita bisa menguji laporan.

```sql
INSERT INTO pinjaman (id_buku, id_anggota, tanggal_pinjam) VALUES
(1, 101, '2025-09-20'),
(2, 102, '2025-09-20'),
(3, 103, '2025-09-21'),
(1, 104, '2025-09-21');
```

Kedua, buat stored procedure dengan parameter input berupa tanggal laporan. Prosedur ini akan memilih semua pinjaman yang terjadi pada tanggal tersebut. Pastikan nama prosedur deskriptif agar mudah dipahami oleh admin database.

```sql
DELIMITER //
CREATE PROCEDURE laporan_harian(IN p_tanggal DATE)
BEGIN
    SELECT p.id_pinjaman, p.id_buku, b.judul, p.id_anggota, p.tanggal_pinjam
    FROM pinjaman p
    JOIN buku b ON p.id_buku = b.id_buku
    WHERE p.tanggal_pinjam = p_tanggal;
END //
DELIMITER ;
```

Ketiga, jalankan prosedur dengan parameter tanggal tertentu. Misalnya, untuk menampilkan laporan pada 20 September 2025, gunakan perintah:

```sql
CALL laporan_harian('2025-09-20');
```

Keempat, amati hasil laporan. Hasil query akan menampilkan daftar pinjaman, judul buku, anggota, dan tanggal pinjam. Dengan satu pemanggilan, laporan harian siap pakai tanpa harus menulis query panjang.

Kerja bagus! Dengan langkah ini, kamu sudah memiliki prosedur laporan harian pertama yang bisa dijalankan kapan pun.

---

## Kesalahan Umum

### 1. Tidak Memberi Parameter pada Prosedur

```sql
CREATE PROCEDURE laporan_harian()
BEGIN
    SELECT * FROM pinjaman;
END;
```

Jika prosedur dibuat tanpa parameter, maka laporan tidak bisa difokuskan pada satu tanggal. Akibatnya, hasil laporan terlalu luas dan tidak sesuai kebutuhan. Admin harus tetap menambahkan filter manual, yang menghilangkan manfaat otomatisasi.

### 2. Menggunakan Format Tanggal yang Salah

```sql
CALL laporan_harian('20-09-2025');
```

MariaDB membutuhkan format `YYYY-MM-DD`. Jika format salah, prosedur bisa gagal atau menghasilkan data kosong. Kesalahan ini umum terjadi karena perbedaan format regional.

### 3. Tidak Menggunakan JOIN dengan Tabel Buku

```sql
SELECT * FROM pinjaman WHERE tanggal_pinjam = p_tanggal;
```

Jika hanya mengambil data dari `pinjaman`, laporan tidak menyertakan judul buku. Hal ini membuat laporan kurang informatif karena admin harus mencari judul secara manual.

### 4. Salah Menulis Nama Parameter

```sql
WHERE tanggal_pinjam = tanggal;
```

Jika nama parameter tidak sesuai dengan definisi (`p_tanggal`), maka query gagal dijalankan. Konsistensi penamaan sangat penting agar prosedur berjalan lancar.

### 5. Membuat Prosedur dengan Nama Tidak Jelas

```sql
CREATE PROCEDURE lp1(IN d DATE) ...
```

Nama seperti `lp1` tidak deskriptif, sehingga sulit dipahami saat jumlah prosedur banyak. Gunakan nama yang sesuai seperti `laporan_harian` agar jelas fungsinya.

---

## Best Practice

### 1. Gunakan Parameter Input yang Jelas

```sql
CREATE PROCEDURE laporan_harian(IN p_tanggal DATE) ...
```

Memberikan parameter `p_tanggal` membuat prosedur fleksibel. Admin bisa memilih tanggal mana pun untuk laporan harian.

### 2. Gunakan JOIN untuk Informasi Lengkap

```sql
SELECT p.id_pinjaman, b.judul, p.id_anggota
FROM pinjaman p
JOIN buku b ON p.id_buku = b.id_buku
WHERE p.tanggal_pinjam = p_tanggal;
```

Dengan JOIN, laporan menyertakan judul buku sehingga lebih berguna. Praktik ini meningkatkan kualitas informasi dalam laporan.

### 3. Terapkan Format Tanggal Konsisten

```sql
CALL laporan_harian('2025-09-20');
```

Selalu gunakan format `YYYY-MM-DD` agar MariaDB bisa mengenali input dengan benar. Konsistensi format menghindari error.

### 4. Gunakan Nama Prosedur yang Deskriptif

```sql
CREATE PROCEDURE laporan_harian(...) ...
```

Nama yang jelas membantu manajemen database jangka panjang. Admin baru pun bisa langsung memahami fungsi prosedur.

### 5. Uji Prosedur dengan Data Berbeda

```sql
CALL laporan_harian('2025-09-21');
```

Menggunakan berbagai tanggal untuk uji memastikan prosedur bekerja di semua kondisi. Ini membuat laporan harian lebih andal.

---

## Studi Kasus

Bayangkan seorang admin perpustakaan ingin melihat laporan pinjaman pada tanggal 21 September 2025. Tanpa stored procedure, ia harus menulis query manual. Dengan stored procedure `laporan_harian`, cukup panggil perintah berikut:

```sql
CALL laporan_harian('2025-09-21');
```

Hasil laporan akan menampilkan daftar pinjaman pada hari itu, termasuk judul buku dan identitas anggota. Dengan informasi ini, admin bisa melihat tren peminjaman harian. Jika laporan dihasilkan setiap hari, manajemen bisa menilai hari mana yang paling sibuk.

Selain itu, laporan harian bisa menjadi dasar untuk laporan mingguan dan bulanan. Data yang konsisten dari stored procedure memudahkan pembuatan ringkasan tingkat lanjut. Dalam praktik nyata, laporan ini bisa diekspor ke format PDF atau Excel.

Kerja bagus! Studi kasus ini menunjukkan bagaimana stored procedure memudahkan pembuatan laporan rutin. Dengan cara ini, manajemen perpustakaan memiliki data akurat setiap hari tanpa pekerjaan tambahan.

---

## Kesimpulan

Stored procedure laporan harian adalah cara efektif untuk menghasilkan data rutin dengan cepat dan konsisten. Prosedur ini memungkinkan admin memanggil laporan dengan parameter tanggal tertentu. Dengan tambahan JOIN, laporan lebih informatif karena menampilkan judul buku. Praktik ini mengurangi pekerjaan manual dan meminimalisir kesalahan. Dengan laporan harian otomatis, manajemen perpustakaan bisa mengambil keputusan berdasarkan data akurat setiap hari.

---

## Referensi

* Coronel, C., & Morris, S. (2019). *Database Systems: Design, Implementation, & Management*. Cengage Learning.
* Oppel, A. J. (2009). *Databases: A Beginner’s Guide*. McGraw-Hill.
* van der Lans, R. (2000). *Introduction to SQL*. Addison-Wesley.
