---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Operator Perbandingan"
short: "Operator"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 5
lister: 3
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Dasar"
      icon: ""
      desc: "Memahami penggunaan operator perbandingan data"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Pelajari operator perbandingan seperti =, >, <, dan LIKE. Modul ini memperkenalkan cara menyaring data dengan logika sederhana dalam query SQL."
---

## Persiapan Praktik

Operator perbandingan adalah salah satu fitur inti dari SQL yang membuat klausa `WHERE` semakin kuat dan berguna. Tanpa operator ini, penyaringan data hanya terbatas pada kecocokan sederhana. Dengan operator seperti `=`, `>`, `<`, dan `LIKE`, pengguna dapat menyeleksi data secara lebih presisi sesuai dengan kebutuhan analisis. Contohnya, pustakawan bisa mencari buku dengan tahun terbit tertentu atau buku dengan judul yang mengandung kata kunci tertentu. Hal ini memperlihatkan bahwa operator perbandingan bukan hanya fungsi teknis, tetapi juga alat praktis untuk mendukung pengambilan keputusan.

Sebelum melakukan praktik, penting memastikan bahwa Anda menggunakan database yang benar. Dalam konteks studi kasus ini, database yang digunakan adalah `perpustakaan`. Perintah `USE perpustakaan;` harus dijalankan agar semua query berikutnya diarahkan ke database yang sesuai. Setelah itu, Anda bisa memverifikasi dengan `SELECT DATABASE();` untuk memastikan posisi Anda sudah benar. Jika database yang aktif tidak sesuai, semua query yang dijalankan bisa gagal atau menghasilkan data yang salah. Persiapan kecil ini sangat penting untuk mencegah kesalahan besar di tahap selanjutnya.

Selain memastikan database aktif, periksa juga struktur tabel yang akan digunakan. Pada tabel `buku`, terdapat kolom `id_buku`, `judul`, `penulis`, `tahun_terbit`, dan `isbn`. Masing-masing kolom memiliki tipe data yang berbeda, sehingga operator tertentu hanya bisa digunakan pada kolom tertentu. Misalnya, operator `>` cocok untuk `tahun_terbit`, tetapi tidak tepat jika digunakan pada `judul`. Tanpa memahami tipe data, penggunaan operator bisa berakhir dengan error. Dengan memeriksa struktur terlebih dahulu menggunakan `DESCRIBE buku;`, kesalahan seperti ini dapat dihindari.

Langkah persiapan lainnya adalah menampilkan isi tabel untuk memahami data yang tersedia. Gunakan `SELECT * FROM buku;` agar Anda bisa melihat nilai yang ada sebelum mencoba operator perbandingan. Jika ingin menggunakan `LIKE`, Anda perlu tahu teks apa yang ada di kolom `judul` atau `penulis`. Jika ingin menggunakan `>`, Anda harus tahu rentang nilai pada kolom `tahun_terbit`. Dengan cara ini, praktik akan lebih bermakna dan relevan. Menampilkan data sebelum menyaring adalah kebiasaan yang baik bagi praktisi SQL.

Terakhir, pahami tujuan dari penggunaan operator perbandingan dalam kasus nyata. Operator ini digunakan untuk membuat query yang menjawab pertanyaan spesifik, misalnya “Buku apa saja yang terbit setelah tahun 2015?” atau “Buku mana yang judulnya mengandung kata ‘Pemrograman’?”. Dengan memahami tujuan ini, Anda tidak hanya menulis query demi latihan, tetapi juga menghasilkan informasi nyata yang bermanfaat. Pada konteks perpustakaan, hasil dari query bisa dipakai untuk promosi koleksi baru, penyusunan laporan, atau pelayanan anggota.

---

## Perintah Dasar Operator Perbandingan

Operator `=` adalah operator paling sederhana dalam SQL. Fungsinya adalah mencari data yang nilainya sama persis dengan syarat tertentu. Misalnya, jika pustakawan ingin menampilkan semua buku yang terbit pada tahun 2020, query yang digunakan adalah:

```sql
SELECT judul, penulis FROM buku WHERE tahun_terbit = 2020;
```

Perintah ini akan menampilkan hanya baris data yang memiliki tahun terbit 2020. Semua baris lain yang tidak sesuai akan diabaikan. Operator `=` biasanya digunakan untuk pencarian sederhana yang membutuhkan kecocokan persis. Penggunaannya sangat umum dalam semua aplikasi SQL.

Selain `=`, operator `>` digunakan untuk mencari nilai yang lebih besar. Misalnya, untuk menampilkan semua buku yang terbit setelah 2018, pustakawan dapat menjalankan:

```sql
SELECT judul, tahun_terbit FROM buku WHERE tahun_terbit > 2018;
```

Query ini hanya akan menampilkan buku yang lebih baru dari tahun 2018. Sebaliknya, operator `<` digunakan untuk mencari nilai yang lebih kecil. Contoh:

```sql
SELECT judul, tahun_terbit FROM buku WHERE tahun_terbit < 2000;
```

Query tersebut akan menampilkan koleksi buku lama yang terbit sebelum tahun 2000. Dengan demikian, operator ini sangat bermanfaat untuk analisis berdasarkan rentang waktu.

Operator `LIKE` berfungsi untuk pencarian pola teks. Misalnya, untuk menampilkan semua buku dengan judul yang mengandung kata “Pemrograman”, query yang digunakan adalah:

```sql
SELECT judul FROM buku WHERE judul LIKE '%Pemrograman%';
```

Simbol `%` adalah wildcard yang mewakili nol atau lebih karakter. Dengan `LIKE`, pencarian teks menjadi lebih fleksibel. Operator ini sangat penting dalam kasus nyata ketika pengguna hanya tahu sebagian dari judul buku atau nama penulis.

---

## 3. Kesalahan Umum

### 3.1 Menggunakan Operator pada Kolom yang Salah

Salah satu kesalahan paling umum adalah menggunakan operator numerik pada kolom teks. Misalnya, mencoba `penulis > 'Andi'` tidak akan menghasilkan hasil yang logis. MariaDB akan memprosesnya secara alfabetis, tetapi hasilnya tidak sesuai dengan maksud pengguna. Kesalahan ini muncul karena kurang memahami tipe data dari kolom.

```sql
-- Salah: operator numerik untuk kolom teks
SELECT * FROM buku WHERE penulis > 'Andi';
```

Untuk menghindari kesalahan ini, biasakan memeriksa tipe data kolom dengan `DESCRIBE`. Dengan memahami tipe data, Anda bisa memilih operator yang sesuai. Jika kolom adalah teks, gunakan `=` atau `LIKE`. Jika kolom adalah angka atau tahun, barulah gunakan `>` atau `<`.

---

### 3.2 Salah Menempatkan Wildcard pada `LIKE`

Kesalahan lain adalah salah menempatkan simbol `%` dalam pola `LIKE`. Jika Anda menulis `LIKE 'Pemrograman%'`, maka hasil hanya menampilkan judul yang diawali dengan kata "Pemrograman". Namun, jika maksud Anda adalah menemukan kata “Pemrograman” di mana saja dalam teks, query ini salah. Akibatnya, hasil query kosong atau tidak lengkap.

```sql
-- Salah: pola tidak sesuai
SELECT * FROM buku WHERE judul LIKE 'Pemrograman%';
```

Agar hasil sesuai, wildcard harus ditempatkan di posisi yang benar. Gunakan `%Pemrograman%` untuk mencari kata "Pemrograman" di tengah judul. Dengan pemahaman ini, kesalahan dalam pencarian teks dapat dihindari.

---

### 3.3 Membandingkan Data dengan Format Tidak Sesuai

Kesalahan berikutnya adalah menuliskan nilai dengan format yang tidak sesuai tipe data. Misalnya, mencoba membandingkan kolom `tahun_terbit` dengan teks “Dua Ribu Dua Puluh”. MariaDB tidak bisa membandingkan angka dengan teks, sehingga query gagal. Kesalahan ini terjadi karena pengguna tidak disiplin dalam menuliskan format data.

```sql
-- Salah: data tidak sesuai format
SELECT * FROM buku WHERE tahun_terbit = 'Dua Ribu Dua Puluh';
```

Untuk menghindari hal ini, pastikan Anda selalu menulis angka tanpa kutip untuk kolom numerik dan teks dengan kutip tunggal untuk kolom string. Konsistensi format adalah prinsip dasar dalam penulisan query SQL.

---

### 3.4 Mengabaikan Sensitivitas Pola Teks

Walaupun MariaDB biasanya tidak peka huruf besar kecil dalam perbandingan teks, ada konfigurasi tertentu yang membuatnya case-sensitive. Misalnya, jika Anda menulis `LIKE 'pemrograman%'` sementara data tersimpan dengan huruf besar "Pemrograman", hasilnya bisa berbeda. Hal ini membuat pengguna mengira data tidak ada padahal sebenarnya ada.

```sql
-- Salah: perbedaan huruf besar kecil
SELECT * FROM buku WHERE judul LIKE 'pemrograman%';
```

Untuk mengatasi masalah ini, gunakan fungsi `LOWER()` atau `UPPER()` untuk menyamakan format teks sebelum dibandingkan. Dengan cara ini, pencarian teks menjadi lebih konsisten dan akurat.

---

## 4. Best Practice

### 4.1 Gunakan Operator Sesuai Tipe Data

Pastikan operator yang digunakan sesuai dengan tipe data kolom. Jika kolom berupa angka atau tanggal, gunakan `>` atau `<`. Jika kolom berupa teks, gunakan `=` atau `LIKE`. Pemilihan operator yang tepat akan menghasilkan query yang akurat dan mencegah error.

```sql
SELECT judul FROM buku WHERE tahun_terbit > 2015;
```

---

### 4.2 Gunakan Wildcard Secara Tepat

Gunakan simbol `%` pada `LIKE` sesuai dengan pola yang diinginkan. Jika ingin mencari teks di mana saja, gunakan `%teks%`. Jika hanya di awal, gunakan `teks%`. Dengan menempatkan wildcard secara tepat, pencarian teks akan lebih sesuai dengan tujuan.

```sql
SELECT * FROM buku WHERE judul LIKE '%Basis Data%';
```

---

### 4.3 Gunakan Fungsi Tambahan untuk Konsistensi

Untuk menghindari perbedaan huruf besar kecil, gunakan fungsi `LOWER()` atau `UPPER()`. Dengan cara ini, pencarian teks tidak akan terpengaruh oleh format huruf. Praktik ini sangat membantu ketika data dimasukkan dengan variasi penulisan yang tidak konsisten.

```sql
SELECT * FROM buku WHERE LOWER(judul) LIKE '%pemrograman%';
```

---

### 4.4 Gunakan Operator untuk Analisis Data

Operator perbandingan tidak hanya berguna untuk pencarian, tetapi juga analisis data. Anda bisa mencari jumlah buku baru dengan `>`, atau jumlah buku lama dengan `<`. Dengan cara ini, operator menjadi alat analisis yang bermanfaat bagi manajemen perpustakaan.

```sql
SELECT COUNT(*) FROM buku WHERE tahun_terbit > 2015;
```

---

## Studi Kasus Perpustakaan

Dalam sebuah perpustakaan, admin ingin mencari semua buku yang memiliki kata "Pemrograman" pada judulnya. Ia menjalankan query berikut:

```sql
SELECT judul, penulis FROM buku WHERE judul LIKE '%Pemrograman%';
```

Hasilnya menampilkan semua buku yang relevan, termasuk “Pemrograman Python untuk Pemula”. Dengan data ini, admin bisa membuat daftar katalog tematik yang akan dipajang di perpustakaan. Daftar semacam ini memudahkan anggota menemukan koleksi sesuai minat mereka.

Dalam kasus lain, admin ingin mengetahui koleksi terbaru yang terbit setelah tahun 2018. Ia menggunakan query:

```sql
SELECT judul, tahun_terbit FROM buku WHERE tahun_terbit > 2018;
```

Hasilnya adalah daftar buku terbaru yang bisa dipromosikan kepada anggota. Dengan cara ini, perpustakaan dapat selalu menampilkan koleksi terkini yang menarik perhatian pembaca. Studi kasus ini memperlihatkan betapa bergunanya operator perbandingan untuk pelayanan sehari-hari.

---

## Kesimpulan

Modul ini membahas penggunaan operator perbandingan `=`, `>`, `<`, dan `LIKE` dalam SQL. Peserta mempelajari cara menggunakan operator untuk menyaring data, kesalahan umum yang sering terjadi, dan praktik terbaik yang sebaiknya diikuti. Studi kasus perpustakaan memperlihatkan manfaat operator ini dalam pencarian koleksi dan analisis data. Dengan menguasai operator perbandingan, peserta semakin siap melanjutkan ke modul berikutnya tentang pengurutan data dengan `ORDER BY`.

---

## Referensi

* Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems*. Pearson.
* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2020). *Database System Concepts*. McGraw-Hill.


