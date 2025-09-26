---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Praktik Mencari Buku Berdasarkan Penulis"
short: "Praktik"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 5
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
      desc: "Latihan query sederhana dengan kondisi penulis"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta akan melakukan praktik membuat query untuk mencari daftar buku berdasarkan nama penulis. Modul ini memperkuat pemahaman penggunaan SELECT, WHERE, dan ORDER BY."
---


## 1. Persiapan Praktik

Mencari buku berdasarkan penulis adalah fitur yang sangat penting dalam sistem manajemen perpustakaan. Anggota perpustakaan sering kali hanya mengingat nama penulis favorit mereka, bukan judul buku secara lengkap. Hal ini membuat klausa `WHERE` dan operator teks seperti `=` dan `LIKE` menjadi sangat relevan untuk kebutuhan sehari-hari. Dengan memahami pencarian berdasarkan penulis, kita bisa menghadirkan layanan yang lebih ramah bagi pengguna. Pustakawan dapat langsung membantu anggota menemukan buku yang mereka cari tanpa membuang waktu.

Langkah pertama dalam persiapan adalah memastikan Anda berada pada database yang benar. Gunakan perintah `USE perpustakaan;` untuk berpindah ke database yang berisi tabel `buku`. Setelah itu, jalankan `SELECT DATABASE();` untuk memverifikasi bahwa Anda sudah bekerja pada database yang sesuai. Banyak pemula sering melupakan tahap sederhana ini, sehingga query dijalankan pada database yang salah. Akibatnya, hasil tidak muncul atau bahkan menimbulkan error. Konsistensi dalam memeriksa konteks database sangat penting dalam praktik profesional.

Selain itu, penting juga untuk meninjau struktur tabel sebelum memulai query. Gunakan perintah `DESCRIBE buku;` untuk melihat kolom apa saja yang tersedia. Dengan melihat struktur tabel, Anda akan menemukan bahwa kolom `penulis` adalah kolom teks yang bisa dijadikan filter utama. Informasi ini memastikan Anda memilih operator yang tepat. Jika hanya tahu sebagian nama penulis, operator `LIKE` lebih sesuai, sedangkan operator `=` cocok untuk pencarian nama lengkap. Tanpa pengetahuan ini, query bisa gagal memberikan hasil yang diharapkan.

Langkah berikutnya adalah meninjau data yang ada di tabel `buku`. Jalankan perintah `SELECT * FROM buku;` untuk melihat daftar lengkap koleksi. Dari hasil ini, Anda bisa menentukan nama penulis mana yang akan digunakan dalam contoh pencarian. Misalnya, jika terlihat ada nama “Sinta Dewi”, maka nama tersebut bisa dijadikan contoh kasus. Dengan meninjau data terlebih dahulu, Anda menghindari risiko menulis kondisi dengan nilai yang tidak ada di tabel. Ini merupakan kebiasaan baik yang akan memperkuat akurasi praktik SQL Anda.

Persiapan ini penting bukan hanya untuk keperluan teknis, tetapi juga untuk melatih kedisiplinan berpikir sistematis. Seorang database engineer yang baik tidak hanya menulis query secara cepat, tetapi juga mempertimbangkan konteks, data yang tersedia, serta tujuan analisis. Dengan persiapan yang matang, hasil praktik menjadi lebih bermakna. Maka, mari kita lanjutkan ke langkah-langkah pencarian yang lebih spesifik.

---

## 2. Langkah-Langkah Pencarian Berdasarkan Penulis

Langkah paling sederhana adalah mencari buku berdasarkan nama penulis lengkap dengan operator `=`. Misalnya, jika pustakawan ingin mencari semua buku karya “Sinta Dewi”, query yang digunakan adalah:

```sql
SELECT judul, tahun_terbit FROM buku WHERE penulis = 'Sinta Dewi';
```

Query ini akan menampilkan semua judul dan tahun terbit dari penulis yang dimaksud. Hasilnya ringkas, jelas, dan langsung menjawab pertanyaan. Dengan praktik ini, pustakawan dapat membantu anggota perpustakaan secara cepat. Anda bisa mencoba query ini sekarang, dan jika berhasil, selamat karena Anda sudah menguasai pencarian dasar.

Namun, dalam banyak kasus, anggota tidak mengingat nama penulis lengkap. Untuk situasi ini, operator `LIKE` menjadi lebih berguna. Misalnya, jika anggota hanya ingat nama belakang “Dewi”, maka query-nya adalah:

```sql
SELECT judul, penulis FROM buku WHERE penulis LIKE '%Dewi%';
```

Query ini menampilkan semua buku yang memiliki kata “Dewi” dalam kolom `penulis`. Dengan simbol `%`, SQL menjadi fleksibel dalam menangani variasi teks. Inilah salah satu keunggulan SQL yang membuatnya sangat berguna untuk data realitas sehari-hari.

Selain itu, Anda dapat menggabungkan pencarian dengan pengurutan. Misalnya, pustakawan ingin menampilkan semua karya Sinta Dewi berdasarkan urutan tahun terbit terbaru. Query-nya adalah:

```sql
SELECT judul, tahun_terbit FROM buku WHERE penulis = 'Sinta Dewi' ORDER BY tahun_terbit DESC;
```

Dengan query ini, pustakawan bisa langsung melihat perkembangan karya penulis dari tahun ke tahun. Hasilnya tidak hanya menampilkan data, tetapi juga memberi wawasan yang lebih dalam. Anda bisa mencoba kombinasi ini untuk memperluas keterampilan Anda.

Langkah terakhir adalah menguji berbagai variasi. Cobalah mencari penulis lain atau gunakan bagian nama yang berbeda dalam pola `LIKE`. Semakin sering Anda berlatih, semakin terbiasa Anda dalam menulis query fleksibel. Jangan ragu untuk bereksperimen karena praktik adalah cara terbaik untuk memperkuat pemahaman.

---

## 3. Kesalahan Umum

### 3.1 Lupa Menambahkan Kutip pada Nama Penulis

Kesalahan paling sering terjadi adalah tidak menambahkan tanda kutip pada nilai teks. SQL akan salah menafsirkan nilai tersebut sebagai nama kolom. Akibatnya, query gagal dijalankan dan mengembalikan error. Kesalahan ini seharusnya mudah dihindari, tetapi sering dilakukan oleh pemula karena terburu-buru. Membiasakan diri menulis tanda kutip pada nilai teks adalah kunci keberhasilan dalam query SQL.

### 3.2 Salah Mengetik Nama Penulis

Kesalahan kedua adalah salah menuliskan ejaan nama penulis. Misalnya, mengetik “Cinta Dewi” alih-alih “Sinta Dewi”. SQL tidak bisa menoleransi kesalahan ejaan seperti ini karena perbandingan dilakukan secara presisi. Hasil query menjadi kosong, membuat pengguna menyangka data tidak ada. Untuk menghindarinya, biasakan mengecek daftar penulis terlebih dahulu dengan `SELECT DISTINCT penulis FROM buku;`.

### 3.3 Menggunakan `=` untuk Nama Parsial

Kesalahan berikutnya adalah menggunakan operator `=` ketika hanya tahu sebagian nama. Misalnya, `WHERE penulis = 'Dewi'` tidak akan mengembalikan hasil jika data sebenarnya adalah “Sinta Dewi”. Operator `=` hanya cocok untuk kecocokan penuh, bukan parsial. Untuk kasus seperti ini, `LIKE` adalah pilihan yang lebih tepat. Kesalahan pemilihan operator bisa menyebabkan hasil yang tidak relevan.

### 3.4 Lupa Menambahkan Wildcard dalam `LIKE`

Kesalahan lain adalah menggunakan `LIKE` tanpa menambahkan wildcard `%`. Misalnya, `WHERE penulis LIKE 'Dewi'` hanya mencari nilai yang persis sama dengan “Dewi”. Jika maksudnya adalah mencari penulis dengan kata “Dewi” di dalam namanya, query ini salah. Tanpa wildcard, pencarian menjadi terbatas. Oleh karena itu, selalu tambahkan `%` di depan dan belakang teks untuk pencarian fleksibel.

---

## 4. Best Practice

### 4.1 Gunakan Kutip untuk Nilai Teks

Setiap nilai teks dalam kondisi `WHERE` harus ditulis dengan tanda kutip tunggal. Hal ini membuat SQL mengenali nilai tersebut sebagai string. Praktik ini sederhana tetapi mencegah error yang sering dialami pemula. Dengan konsistensi ini, Anda akan menulis query lebih lancar.

```sql
SELECT * FROM buku WHERE penulis = 'Sinta Dewi';
```

### 4.2 Gunakan `LIKE` untuk Nama Parsial

Jika anggota hanya mengingat sebagian nama, gunakan `LIKE` dengan wildcard `%`. Cara ini membuat pencarian lebih fleksibel dan sesuai dengan kenyataan. Pengguna tidak harus mengingat nama lengkap untuk bisa menemukan buku.

```sql
SELECT * FROM buku WHERE penulis LIKE '%Dewi%';
```

### 4.3 Kombinasikan dengan `ORDER BY`

Penggunaan `WHERE` bisa dipadukan dengan `ORDER BY` untuk menyusun hasil lebih rapi. Dengan kombinasi ini, laporan bisa disajikan dalam bentuk yang lebih profesional. Pustakawan bisa memilih menampilkan data berdasarkan urutan abjad atau tahun terbit.

```sql
SELECT judul, tahun_terbit FROM buku WHERE penulis = 'Sinta Dewi' ORDER BY tahun_terbit DESC;
```

### 4.4 Verifikasi Data Sebelum Query

Sebelum menulis query, biasakan memverifikasi data yang ada. Gunakan `SELECT DISTINCT penulis FROM buku;` untuk memastikan nama penulis valid. Dengan langkah ini, risiko salah ketik atau pencarian nilai yang tidak ada bisa diminimalkan. Praktik ini adalah bagian dari etika profesional dalam pengelolaan database.

---

## 5. Studi Kasus Perpustakaan

Seorang anggota perpustakaan datang dan menanyakan semua buku karya Sinta Dewi. Pustakawan kemudian menjalankan query berikut:

```sql
SELECT judul, tahun_terbit FROM buku WHERE penulis = 'Sinta Dewi';
```

Hasil query langsung menampilkan daftar buku karya penulis tersebut. Anggota merasa puas karena bisa mendapatkan informasi dengan cepat.

Dalam kasus lain, anggota hanya mengingat nama belakang “Dewi”. Pustakawan menggunakan query berikut:

```sql
SELECT judul, penulis FROM buku WHERE penulis LIKE '%Dewi%';
```

Dengan query ini, pustakawan bisa menemukan semua buku yang penulisnya memiliki kata “Dewi” dalam namanya. Hal ini membuat layanan perpustakaan lebih efisien dan responsif. Praktik ini memperlihatkan bagaimana SQL benar-benar mendukung kebutuhan sehari-hari.

---

## Kesimpulan

Modul ini membimbing peserta untuk melakukan pencarian buku berdasarkan nama penulis. Peserta mempelajari cara dasar dengan operator `=`, cara fleksibel dengan `LIKE`, serta kombinasi dengan `ORDER BY`. Kesalahan umum dan praktik terbaik juga dijelaskan agar peserta dapat menghindari jebakan pemula. Studi kasus perpustakaan memperlihatkan manfaat nyata dari keterampilan ini. Dengan menguasai pencarian berbasis penulis, peserta siap memberikan layanan database yang lebih profesional.

---

## Referensi

* Connolly, T., & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management*. Pearson.
* Coronel, C., & Morris, S. (2019). *Database Systems: Design, Implementation, & Management*. Cengage Learning.

