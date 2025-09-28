---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Menampilkan Kolom Tertentu"
short: "Kolom"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 5
lister: 1
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Teknis"
      icon: ""
      desc: "Menampilkan hanya kolom yang dibutuhkan"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Pelajari cara memilih kolom tertentu dengan perintah SELECT. Modul ini membantu pengguna menampilkan hanya informasi yang relevan dari tabel."
---


Menampilkan kolom tertentu dari sebuah tabel adalah langkah penting untuk memahami bagaimana data bisa diolah dengan lebih efektif. Pada tahap ini, kita akan berlatih menggunakan tabel `buku` dalam database `perpustakaan` yang sudah kita buat sebelumnya. Perintah `SELECT *` memang bisa menampilkan semua kolom, tetapi sering kali hal itu tidak efisien dan menampilkan terlalu banyak informasi yang tidak relevan. Oleh karena itu, kita perlu memahami bagaimana menampilkan hanya kolom tertentu sesuai kebutuhan. Konsep ini akan menjadi bekal penting dalam analisis data yang lebih kompleks di masa depan.

Sebelum memulai praktik, pastikan Anda sudah masuk ke dalam database yang benar. Perintah `USE perpustakaan;` digunakan untuk berpindah ke database `perpustakaan`, sedangkan `SELECT DATABASE();` dipakai untuk memverifikasi nama database aktif. Jika Anda tidak berhati-hati, query yang dijalankan bisa diarahkan ke database lain yang memiliki struktur tabel berbeda atau bahkan kosong. Hal ini tentu bisa membingungkan, terutama jika hasil query tidak sesuai dengan harapan. Oleh karena itu, disiplin dalam memeriksa database aktif adalah langkah yang tidak boleh diabaikan.

Tabel `buku` sendiri memiliki kolom yang cukup representatif untuk studi kasus perpustakaan. Kolom tersebut adalah `id_buku`, `judul`, `penulis`, `tahun_terbit`, dan `isbn`. Dengan struktur ini, kita bisa menampilkan data yang relevan sesuai kebutuhan, seperti hanya `judul` dan `penulis` untuk katalog, atau `judul` dan `tahun_terbit` untuk analisis umur koleksi. Fleksibilitas dalam memilih kolom membuat perintah `SELECT` lebih bermanfaat. Jika semua kolom ditampilkan tanpa tujuan yang jelas, data bisa sulit dipahami dan digunakan secara efektif.

Selain itu, perlu juga dipahami bahwa tabel dalam sistem nyata sering memiliki kolom yang sangat banyak. Dalam tabel transaksi misalnya, kolom bisa mencapai puluhan, sehingga menampilkan semua kolom sekaligus akan menghasilkan data yang berlebihan. Dengan menampilkan hanya kolom tertentu, sistem akan bekerja lebih efisien dan pengguna hanya mendapatkan informasi yang relevan. Inilah alasan mengapa keterampilan ini sangat penting bagi seorang praktisi database. Kebiasaan ini juga akan mempercepat analisis dan meningkatkan kualitas laporan.

Terakhir, buatlah pertanyaan sederhana sebelum menulis query. Contoh pertanyaan yang bisa diajukan adalah: "Siapa penulis buku dalam koleksi?" atau "Apa saja judul buku yang tersedia?". Pertanyaan-pertanyaan ini bisa langsung dijawab dengan memilih kolom yang sesuai menggunakan `SELECT`. Dengan demikian, praktik ini bukan sekadar latihan teknis, melainkan latihan berpikir kritis tentang bagaimana data bisa menjawab kebutuhan informasi.

---

## Perintah Dasar Menampilkan Kolom Tertentu

Perintah dasar untuk menampilkan kolom tertentu sangat sederhana, tetapi memiliki kegunaan yang luas. Sintaksnya adalah `SELECT kolom1, kolom2 FROM nama_tabel;`. Dengan menuliskan kolom secara eksplisit, sistem hanya akan menampilkan data dari kolom tersebut. Misalnya, jika kita ingin menampilkan `judul` dan `penulis` dari tabel `buku`, maka query-nya adalah `SELECT judul, penulis FROM buku;`. Perintah ini akan menghasilkan daftar ringkas yang langsung menjawab pertanyaan tentang siapa penulis dari setiap buku.

```sql
SELECT judul, penulis FROM buku;
```

Query di atas akan menampilkan daftar judul dan penulis tanpa kolom lain. Hasil ini lebih fokus dan mudah dipahami daripada menampilkan semua kolom sekaligus. Efisiensi semacam ini penting terutama ketika data dalam tabel sudah sangat banyak. Dengan begitu, pengguna tidak perlu memilah informasi yang relevan secara manual. Hal ini menunjukkan bagaimana SQL bisa memberikan kontrol penuh terhadap data.

Anda juga dapat menggunakan alias untuk membuat hasil lebih ramah pengguna. Alias ditulis dengan kata kunci `AS` dan digunakan untuk mengganti nama kolom sementara pada hasil tampilan. Contoh query dengan alias adalah `SELECT judul AS "Judul Buku", penulis AS "Nama Penulis" FROM buku;`. Dengan cara ini, laporan menjadi lebih profesional karena kolom memiliki nama yang lebih deskriptif. Praktik ini sering digunakan dalam pembuatan laporan untuk pemangku kepentingan non-teknis.

```sql
SELECT judul AS "Judul Buku", penulis AS "Nama Penulis" FROM buku;
```

Penggunaan alias bukan hanya memperindah tampilan, tetapi juga meningkatkan keterbacaan. Dalam banyak organisasi, laporan dari database digunakan oleh orang yang tidak terbiasa dengan istilah teknis. Dengan alias, data menjadi lebih komunikatif dan siap dipresentasikan. Langkah sederhana ini memberikan nilai tambah besar bagi profesionalisme sebuah sistem.

Dengan menguasai perintah dasar ini, pengguna akan lebih siap untuk menulis query yang kompleks di masa depan. Menampilkan kolom tertentu adalah pondasi untuk belajar menyaring data dengan `WHERE` atau mengurutkan dengan `ORDER BY`. Jika dasar ini sudah dikuasai, maka langkah selanjutnya akan terasa lebih mudah. Oleh karena itu, penting untuk benar-benar memahami sintaks ini sebelum melangkah lebih jauh.

---

## 3. Kesalahan Umum

### 3.1 Salah Menuliskan Nama Kolom

Kesalahan ini sering terjadi pada pemula, yaitu salah ketik nama kolom. Misalnya, menulis `penuliss` alih-alih `penulis`. MariaDB akan langsung mengembalikan pesan error karena kolom tidak ditemukan. Masalah ini terlihat sepele, tetapi bisa membuang waktu saat debugging. Untuk mencegahnya, biasakan mengecek struktur tabel dengan `DESCRIBE`. Dengan begitu, Anda tidak perlu menebak-nebak nama kolom.

```sql
-- Salah: kolom tidak ada
SELECT judul, penuliss FROM buku;
```

### 3.2 Menggunakan `SELECT *` Padahal Tidak Perlu

Banyak pengguna terbiasa menulis `SELECT *` meskipun hanya membutuhkan beberapa kolom saja. Akibatnya, hasil query menjadi terlalu besar dan membingungkan. Praktik ini juga tidak efisien, terutama jika tabel berisi ribuan baris dengan banyak kolom. Kebiasaan menggunakan `SELECT *` membuat sistem bekerja lebih keras dari yang diperlukan. Untuk kebutuhan analisis, cara ini justru memperlambat kerja Anda.

```sql
-- Salah: menampilkan semua kolom
SELECT * FROM buku;
```

### 3.3 Lupa Menyebutkan Nama Tabel

Ada juga kesalahan ketika pengguna lupa menuliskan nama tabel setelah kata kunci `FROM`. Akibatnya, MariaDB tidak tahu dari mana data harus diambil. Kesalahan ini memang sederhana, tetapi cukup sering terjadi ketika seseorang terburu-buru menulis query. Solusi terbaik adalah membiasakan diri menulis query dengan tenang dan rapi. Dengan begitu, kesalahan kecil ini bisa dihindari.

```sql
-- Salah: lupa menuliskan nama tabel
SELECT judul, penulis;
```

### 3.4 Tidak Memanfaatkan Alias

Meskipun tidak menimbulkan error, tidak menggunakan alias adalah kehilangan kesempatan untuk membuat hasil lebih jelas. Hasil query tanpa alias bisa terlihat terlalu teknis bagi pengguna non-teknis. Alias membuat laporan lebih ramah dan komunikatif. Mengabaikan fitur sederhana ini membuat data sulit dipahami oleh orang awam. Oleh karena itu, sebaiknya biasakan menggunakan alias dalam laporan formal.

---

## 4. Best Practice

### 4.1 Tentukan Kolom Secara Spesifik

Selalu pilih kolom yang benar-benar dibutuhkan. Dengan menuliskan nama kolom secara spesifik, hasil query lebih efisien dan mudah dibaca. Praktik ini juga mengurangi beban server. Misalnya, pustakawan hanya perlu menampilkan `judul` dan `penulis`.

```sql
SELECT judul, penulis FROM buku;
```

### 4.2 Gunakan Alias untuk Kerapihan

Alias membantu membuat hasil lebih profesional. Nama kolom bisa diganti sementara agar lebih komunikatif. Hal ini sangat berguna dalam laporan resmi atau presentasi. Dengan alias, data lebih mudah dipahami bahkan oleh orang yang tidak paham teknis SQL.

```sql
SELECT judul AS "Judul Buku", penulis AS "Nama Penulis" FROM buku;
```

### 4.3 Verifikasi Nama Kolom dengan `DESCRIBE`

Sebelum menulis query, biasakan menggunakan `DESCRIBE` untuk memverifikasi nama kolom. Hal ini membantu mencegah salah ketik. Kebiasaan ini membuat Anda lebih percaya diri dalam menulis query. Dengan begitu, kesalahan teknis bisa ditekan seminimal mungkin.

```sql
DESCRIBE buku;
```

### 4.4 Sesuaikan Kolom dengan Kebutuhan Laporan

Pilih kolom sesuai dengan tujuan laporan. Jika ingin mengetahui tren penerbitan, cukup tampilkan `judul` dan `tahun_terbit`. Dengan cara ini, hasil query lebih relevan. Praktik ini membuat data lebih bermakna sesuai konteks analisis.

```sql
SELECT judul, tahun_terbit FROM buku;
```

---

## 5. Studi Kasus Perpustakaan

Seorang pustakawan ingin membuat katalog sederhana berisi judul dan penulis. Untuk itu, ia menggunakan query berikut:

```sql
SELECT judul, penulis FROM buku;
```

Hasil query menampilkan daftar ringkas yang siap digunakan sebagai katalog cetak. Selanjutnya, pustakawan ingin membuat laporan dengan nama kolom lebih deskriptif. Ia menambahkan alias agar hasil lebih komunikatif:

```sql
SELECT judul AS "Judul Buku", penulis AS "Nama Penulis" FROM buku;
```

Dengan alias, laporan menjadi lebih profesional dan mudah dipahami anggota perpustakaan. Studi kasus ini memperlihatkan bahwa menampilkan kolom tertentu bukan hanya soal teknis, tetapi juga soal komunikasi informasi. Database menjadi lebih bermanfaat ketika hasil query disesuaikan dengan kebutuhan pengguna.

---

## Kesimpulan

Modul ini membahas bagaimana cara menampilkan kolom tertentu dengan perintah `SELECT`. Peserta mempelajari sintaks dasar, kesalahan umum, praktik terbaik, serta studi kasus perpustakaan. Dengan keterampilan ini, peserta mampu menyesuaikan tampilan data sesuai kebutuhan, sehingga analisis menjadi lebih efisien dan komunikatif. Pemahaman ini juga menjadi dasar untuk mempelajari penyaringan data dengan `WHERE` pada modul berikutnya.

---

## Referensi

* Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems*. Pearson.
* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2020). *Database System Concepts*. McGraw-Hill.
