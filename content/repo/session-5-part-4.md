---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Mengurutkan Data dengan ORDER BY"
short: "Urut"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 5
lister: 4
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
description: "Modul ini mengajarkan cara mengurutkan hasil query dengan ORDER BY. Peserta belajar menampilkan data berdasarkan urutan abjad atau angka sesuai kebutuhan."
---


Mengurutkan data adalah kebutuhan mendasar dalam pengolahan informasi. Tanpa pengurutan, hasil query SQL hanya akan menampilkan data sesuai urutan fisik di tabel, yang tidak selalu sesuai dengan kebutuhan analisis atau laporan. SQL menyediakan klausa `ORDER BY` untuk memberikan kontrol penuh atas urutan hasil. Dengan klausa ini, pengguna dapat menampilkan data berdasarkan urutan abjad, kronologis, atau bahkan berdasarkan kriteria khusus yang ditentukan. Dalam konteks perpustakaan, kemampuan ini penting karena pustakawan sering kali membutuhkan daftar buku dalam urutan tertentu agar lebih mudah dipahami oleh anggota.

Sebelum mencoba praktik `ORDER BY`, pastikan database yang digunakan adalah `perpustakaan`. Gunakan perintah `USE perpustakaan;` untuk mengaktifkan database ini. Selanjutnya, verifikasi dengan `SELECT DATABASE();` agar yakin bahwa Anda berada di database yang tepat. Jika langkah ini dilewati, ada risiko query dijalankan pada database lain yang mungkin kosong atau memiliki tabel berbeda. Hal ini bisa menimbulkan kebingungan karena hasil query tidak sesuai dengan yang diharapkan. Maka dari itu, disiplin dalam memastikan konteks database aktif adalah langkah dasar yang harus dijaga.

Tabel yang akan digunakan dalam praktik ini adalah tabel `buku`. Tabel ini berisi kolom `id_buku`, `judul`, `penulis`, `tahun_terbit`, dan `isbn`. Kolom-kolom tersebut dapat dijadikan dasar pengurutan sesuai kebutuhan laporan. Misalnya, pustakawan bisa mengurutkan berdasarkan `judul` untuk membuat katalog abjad, atau berdasarkan `tahun_terbit` untuk membuat daftar koleksi terbaru. Dengan memahami struktur tabel, pengguna bisa memilih kolom yang paling relevan untuk pengurutan. Inilah alasan mengapa pemahaman tabel menjadi bagian penting dalam persiapan praktik.

Langkah persiapan berikutnya adalah menampilkan isi tabel tanpa pengurutan dengan perintah `SELECT * FROM buku;`. Dari hasil tampilan, pengguna bisa melihat apakah data sudah sesuai untuk dicoba diurutkan. Jika terlihat tidak rapi, maka klausa `ORDER BY` menjadi solusi. Praktik ini bukan hanya teknis, tetapi juga membiasakan diri untuk selalu mengamati data sebelum melakukan manipulasi. Dengan demikian, pengurutan yang dilakukan akan lebih tepat sasaran dan berguna.

Akhirnya, penting memahami tujuan praktis dari pengurutan dalam kasus nyata. Pustakawan mungkin ingin membuat katalog abjad agar anggota lebih mudah mencari koleksi. Di sisi lain, mereka bisa mengurutkan berdasarkan `tahun_terbit` untuk menyoroti buku terbaru yang dimiliki perpustakaan. Tujuan ini membuat penggunaan `ORDER BY` lebih bermakna karena setiap query ditulis dengan alasan yang jelas.

---

## Perintah Dasar `ORDER BY`

Klausa `ORDER BY` ditambahkan di akhir query `SELECT`. Secara default, klausa ini mengurutkan data secara menaik (*ascending*), tetapi pengguna dapat menambahkan kata kunci `DESC` untuk menampilkan data secara menurun (*descending*). Misalnya, jika ingin menampilkan judul buku dalam urutan abjad, query yang digunakan adalah:

```sql
SELECT judul, penulis FROM buku ORDER BY judul ASC;
```

Query ini akan menghasilkan daftar judul dari A ke Z. Jika ingin membalik urutannya, gunakan kata kunci `DESC`:

```sql
SELECT judul, penulis FROM buku ORDER BY judul DESC;
```

Pengurutan tidak terbatas pada kolom teks. Kolom angka atau tahun juga bisa dijadikan dasar pengurutan. Misalnya, untuk menampilkan koleksi terbaru terlebih dahulu, gunakan:

```sql
SELECT judul, tahun_terbit FROM buku ORDER BY tahun_terbit DESC;
```

Query ini membuat pustakawan bisa segera melihat koleksi terbaru. Sebaliknya, dengan `ASC`, mereka bisa menampilkan koleksi lama terlebih dahulu untuk keperluan arsip. Fleksibilitas ini membuat `ORDER BY` menjadi alat yang sangat berguna dalam berbagai konteks analisis.

SQL juga mendukung pengurutan berdasarkan lebih dari satu kolom. Misalnya, pustakawan ingin mengurutkan daftar berdasarkan `penulis` lalu berdasarkan `tahun_terbit`. Query yang digunakan adalah:

```sql
SELECT penulis, judul, tahun_terbit FROM buku ORDER BY penulis ASC, tahun_terbit DESC;
```

Dengan cara ini, data dikelompokkan berdasarkan penulis dan setiap kelompok diurutkan berdasarkan tahun terbaru. Query ini menunjukkan bagaimana `ORDER BY` dapat menghasilkan laporan yang lebih terstruktur.

Selain itu, Anda bisa mengombinasikan `ORDER BY` dengan klausa lain seperti `WHERE`. Misalnya, pustakawan hanya ingin menampilkan buku terbitan setelah 2015, lalu mengurutkannya berdasarkan judul. Dengan kombinasi ini, hasil query lebih relevan sekaligus rapi. Kemampuan menggabungkan klausa inilah yang membuat SQL sangat kuat dalam pengelolaan data.

---

## 3. Kesalahan Umum

### 3.1 Salah Menulis Nama Kolom

Salah ketik nama kolom adalah kesalahan klasik dalam penggunaan `ORDER BY`. Misalnya, menulis `ORDER BY judull` dengan tambahan huruf “l” akan membuat query gagal dijalankan. MariaDB akan mengembalikan pesan error bahwa kolom tidak ditemukan. Kesalahan kecil ini bisa membuang waktu karena pengguna harus kembali memeriksa struktur tabel. Cara terbaik untuk mencegahnya adalah menggunakan `DESCRIBE buku;` atau fitur *autocomplete* pada editor SQL.

### 3.2 Mengurutkan dengan Kolom yang Tidak Ditampilkan

Kesalahan lain adalah menggunakan kolom untuk `ORDER BY` tanpa menampilkannya di hasil query. Walaupun query tetap bisa berjalan, hasilnya bisa membingungkan. Misalnya, pustakawan mengurutkan berdasarkan `isbn` tetapi hanya menampilkan `judul` dan `penulis`. Bagi pembaca hasil, daftar terlihat acak karena mereka tidak tahu bahwa data diurutkan berdasarkan `isbn`. Oleh karena itu, sebaiknya tampilkan juga kolom yang digunakan dalam `ORDER BY`.

### 3.3 Salah Memahami Urutan Default

Secara default, MariaDB menggunakan urutan menaik (*ascending*) jika pengguna tidak menuliskan kata `ASC` atau `DESC`. Banyak pemula mengira default-nya adalah menurun sehingga hasilnya berbeda dari yang diharapkan. Hal ini menimbulkan kebingungan dan membuat pengguna mengira query salah. Untuk menghindarinya, biasakan menuliskan arah pengurutan dengan jelas. Dengan begitu, query lebih mudah dibaca dan dipahami siapa pun.

### 3.4 Mengabaikan Kinerja pada Data Besar

Kesalahan yang sering diabaikan adalah aspek performa saat mengurutkan tabel besar. Pada tabel kecil, query terasa cepat. Namun, pada tabel dengan jutaan baris, `ORDER BY` tanpa indeks bisa membuat query sangat lambat. Pemula sering tidak menyadari pentingnya indeks untuk pengurutan. Dalam lingkungan produksi, praktik ini bisa mengakibatkan bottleneck sistem. Karena itu, selalu pertimbangkan indeks saat mengurutkan tabel besar.

---

## 4. Best Practice

### 4.1 Verifikasi Nama Kolom Sebelum Query

Pastikan nama kolom benar sebelum digunakan dalam `ORDER BY`. Verifikasi ini bisa dilakukan dengan `DESCRIBE` atau melihat hasil `SELECT *`. Dengan memastikan nama kolom, Anda bisa menghindari error penulisan. Praktik ini sederhana tetapi penting untuk menjaga konsistensi hasil.

### 4.2 Selalu Tampilkan Kolom yang Digunakan untuk Urutan

Jika Anda menggunakan kolom tertentu untuk `ORDER BY`, sertakan kolom tersebut dalam hasil query. Dengan begitu, pembaca dapat langsung memahami pola urutan. Praktik ini meningkatkan transparansi dan mempermudah interpretasi hasil.

### 4.3 Tuliskan Arah Urutan Secara Eksplisit

Meskipun `ASC` adalah default, tetap tuliskan kata kunci `ASC` atau `DESC`. Dengan cara ini, query lebih jelas dan tidak membingungkan. Penulisan eksplisit juga membuat query lebih mudah dipahami orang lain yang membaca kode Anda. Hal ini sejalan dengan prinsip keterbacaan dalam penulisan SQL profesional.

### 4.4 Gunakan Indeks untuk Data Besar

Pada tabel besar, pengurutan bisa sangat lambat jika tidak ada indeks. Karena itu, pastikan kolom yang sering digunakan dalam `ORDER BY` memiliki indeks. Dengan indeks, performa query meningkat signifikan. Praktik ini penting untuk menjaga kecepatan sistem dalam skala produksi.

---

## Studi Kasus Perpustakaan

Seorang pustakawan ingin membuat katalog buku dalam urutan abjad berdasarkan judul. Ia menjalankan query:

```sql
SELECT judul, penulis FROM buku ORDER BY judul ASC;
```

Hasil query menampilkan daftar buku dari A sampai Z, sehingga anggota lebih mudah mencari koleksi. Dalam kasus lain, pustakawan ingin mengetahui buku terbaru. Ia menjalankan query berikut:

```sql
SELECT judul, tahun_terbit FROM buku ORDER BY tahun_terbit DESC;
```

Hasil query menunjukkan daftar koleksi terbaru yang siap dipromosikan. Studi kasus ini menunjukkan bagaimana `ORDER BY` membantu pustakawan menyusun informasi lebih rapi dan relevan. Dengan pengurutan yang tepat, data mentah bisa berubah menjadi laporan yang bermakna.

---

## Kesimpulan

Modul ini membahas penggunaan klausa `ORDER BY` dalam SQL untuk mengurutkan data. Peserta mempelajari sintaks dasar, kesalahan umum, praktik terbaik, dan penerapannya dalam studi kasus perpustakaan. Dengan `ORDER BY`, data bisa disajikan lebih terstruktur, rapi, dan mudah dipahami. Pemahaman ini penting untuk membuat laporan profesional dan mendukung pengambilan keputusan. Pada modul berikutnya, peserta akan mempraktikkan pencarian buku berdasarkan penulis untuk melanjutkan penguasaan keterampilan SQL dasar.

---

## Referensi

* Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems*. Pearson.
* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2020). *Database System Concepts*. McGraw-Hill.
