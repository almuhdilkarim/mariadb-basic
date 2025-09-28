---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Fungsi Kondisi"
short: "Fulltext"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 10
lister: 3
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Performa"
      icon: ""
      desc: "Memahami pencarian teks dengan fulltext index"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Modul ini membahas fulltext index untuk pencarian teks. Peserta belajar bagaimana MariaDB mengoptimalkan query pencarian kata pada kolom teks panjang."
---



#### Pendahuluan

Fungsi kondisi dalam SQL, seperti `IF` dan `CASE`, berperan penting untuk mengatur logika dalam query. Dalam perpustakaan, pustakawan sering membutuhkan laporan yang menampilkan kondisi tertentu, misalnya status buku apakah “Terlambat” atau “Tepat Waktu”. Dengan fungsi kondisi, sistem dapat mengubah data numerik atau tanggal menjadi informasi yang lebih mudah dipahami. Hal ini membuat hasil query lebih komunikatif bagi pengguna non-teknis. Secara praktis, fungsi kondisi menjembatani antara data mentah dengan kebutuhan informasi sehari-hari【Elmasri & Navathe, 2016】.

Kegunaan fungsi kondisi dapat dilihat pada laporan denda anggota. Alih-alih hanya menampilkan jumlah hari keterlambatan, pustakawan bisa menggunakan fungsi `CASE` untuk menampilkan status deskriptif seperti “Ringan”, “Sedang”, atau “Berat”. Dengan cara ini, laporan menjadi lebih intuitif bagi pembaca. Fungsi kondisi memungkinkan SQL memberikan nilai yang berbeda berdasarkan kriteria yang ditentukan. Hal ini memperkaya fleksibilitas query dalam menjawab kebutuhan operasional perpustakaan【Connolly & Begg, 2015】.

Fungsi `IF` biasanya digunakan untuk kondisi sederhana dengan dua kemungkinan hasil. Misalnya, jika jumlah keterlambatan lebih dari nol, maka anggota diberi label “Terlambat”, jika tidak maka “Tepat Waktu”. Sementara itu, `CASE` lebih fleksibel karena bisa mengakomodasi banyak kondisi sekaligus. Dengan memanfaatkan kedua fungsi ini, pustakawan dapat menghasilkan laporan yang lebih detail dan informatif. Kedua fungsi ini saling melengkapi sesuai kebutuhan【Rob & Coronel, 2007】.

Selain mendukung laporan, fungsi kondisi juga memudahkan dalam pembuatan tampilan data yang ramah pengguna. Misalnya, pustakawan ingin menandai buku “Sedang Dipinjam” atau “Tersedia” berdasarkan data tanggal pengembalian. Dengan fungsi kondisi, status ini dapat ditentukan secara otomatis dalam query. Hal ini mengurangi kesalahan manual dan mempercepat akses informasi. Oleh karena itu, fungsi kondisi meningkatkan kualitas layanan perpustakaan【Date, 2004】.

Secara ringkas, `IF` dan `CASE` adalah alat penting yang harus dikuasai pustakawan dalam bekerja dengan SQL. Dengan menggunakannya, data dapat dikonversi menjadi informasi yang lebih bermakna. Penggunaan fungsi kondisi juga memastikan bahwa laporan yang dihasilkan lebih relevan dan praktis digunakan dalam pengambilan keputusan. Keterampilan ini akan memperkuat profesionalisme pustakawan dalam mengelola data perpustakaan. Hal ini menegaskan bahwa logika kondisional merupakan fondasi penting dalam query SQL【Kroenke & Auer, 2015】.

---

#### Langkah-langkah Praktik

Mari kita mulai dengan contoh penggunaan fungsi `IF`. Misalnya, kita ingin menampilkan status keterlambatan pengembalian buku oleh anggota berdasarkan jumlah hari terlambat.

```sql
SELECT nama_anggota,
    IF( hari_terlambat > 0, 'Terlambat', 'Tepat Waktu') AS status
FROM peminjaman;
```

Query ini akan memberikan status sederhana yang mudah dipahami. Kerja bagus, langkah awalmu sudah tepat!
Sekarang kita lanjut ke `CASE`, yang lebih fleksibel. Misalnya, pustakawan ingin mengategorikan keterlambatan dalam beberapa level.

```sql
SELECT nama_anggota,
    CASE
        WHEN hari_terlambat = 0 THEN 'Tepat Waktu'
        WHEN hari_terlambat BETWEEN 1 AND 3 THEN 'Ringan'
        WHEN hari_terlambat BETWEEN 4 AND 7 THEN 'Sedang'
        ELSE 'Berat'
    END AS kategori
FROM peminjaman;
```

Query ini akan mengelompokkan anggota berdasarkan tingkat keterlambatan. Bagus sekali, kamu sudah mulai memahami fleksibilitas `CASE`.

Langkah berikutnya, mari kita coba menampilkan status buku. Misalnya, apakah buku sedang dipinjam atau tersedia.

```sql
SELECT judul_buku,
    CASE
        WHEN tanggal_kembali IS NULL THEN 'Sedang Dipinjam'
        ELSE 'Tersedia'
    END AS status_buku
FROM buku;
```

Dengan query ini, laporan koleksi menjadi lebih informatif. Kamu hebat, sudah bisa membuat laporan status buku!

Selanjutnya, kita bisa menggabungkan kondisi dengan perhitungan. Misalnya, menentukan apakah anggota mendapat denda atau tidak.

```sql
SELECT nama_anggota,
    CASE
        WHEN hari_terlambat > 0 THEN hari_terlambat * 1000
        ELSE 0
    END AS total_denda
FROM peminjaman;
```

Query ini menghitung denda berdasarkan jumlah hari keterlambatan. Langkahmu sudah mantap, teruskan eksperimen ini!

Terakhir, mari buat laporan yang menggabungkan `IF` dan `CASE`. Misalnya, laporan ringkas keterlambatan anggota.

```sql
SELECT nama_anggota,
    IF(hari_terlambat > 0, 'Ada Keterlambatan', 'Tidak Ada') AS status,
    CASE
        WHEN hari_terlambat BETWEEN 1 AND 3 THEN 'Ringan'
        WHEN hari_terlambat BETWEEN 4 AND 7 THEN 'Sedang'
        WHEN hari_terlambat > 7 THEN 'Berat'
        ELSE 'Tepat Waktu'
    END AS kategori
FROM peminjaman;
```

Query ini menyajikan informasi keterlambatan yang lebih detail. Bagus sekali, kamu sudah berhasil menggabungkan `IF` dan `CASE` dalam satu laporan!


#### Kesalahan Umum

##### 1. Salah Menulis Sintaks IF

Kesalahan umum pertama adalah menulis `IF` tanpa format yang benar. Contoh salah:

```sql
SELECT nama_anggota,
    IF hari_terlambat > 0 'Terlambat' 'Tepat Waktu'
FROM peminjaman;
```

Kesalahan ini terjadi karena `IF` harus selalu diikuti tanda kurung dan pemisah koma untuk argumennya. Tanpa struktur tersebut, query tidak dapat dijalankan dengan benar. Hal ini membingungkan pustakawan karena error yang muncul seringkali tidak jelas. Oleh karena itu, penting untuk selalu memperhatikan tanda kurung dan pemisah dalam penggunaan fungsi `IF`【Elmasri & Navathe, 2016】.

Selain itu, kesalahan sintaks ini sering terjadi pada pemula yang terbiasa dengan bahasa pemrograman lain. Banyak pustakawan yang mengira `IF` SQL sama dengan `IF` di bahasa pemrograman. Padahal, SQL memiliki aturan yang lebih ketat untuk penggunaan kondisional. Akibatnya, query tidak hanya gagal, tetapi juga menghambat proses analisis data. Pemahaman dasar tentang sintaks SQL harus dikuatkan sebelum mencoba fungsi kondisi【Connolly & Begg, 2015】.

---

##### 2. Tidak Menggunakan ELSE pada CASE

Kesalahan lain adalah tidak menyertakan kondisi `ELSE` dalam `CASE`. Contoh salah:

```sql
SELECT nama_anggota,
    CASE
        WHEN hari_terlambat > 0 THEN 'Terlambat'
    END AS status
FROM peminjaman;
```

Tanpa `ELSE`, data yang tidak memenuhi kondisi `WHEN` akan menghasilkan nilai NULL. Hal ini membuat laporan menjadi kurang informatif karena beberapa baris tidak memiliki status. Banyak pustakawan bingung ketika melihat data kosong pada hasil laporan. Padahal masalahnya hanya karena lupa menambahkan `ELSE`【Rob & Coronel, 2007】.

NULL yang muncul dalam laporan juga dapat menimbulkan kesalahan interpretasi. Misalnya, anggota yang sebenarnya tepat waktu bisa terlihat seolah datanya hilang. Jika dibiarkan, hal ini akan menurunkan kepercayaan terhadap kualitas laporan perpustakaan. Oleh karena itu, `ELSE` sebaiknya selalu digunakan sebagai penutup kondisi. Dengan begitu, semua kemungkinan data bisa ditangani dengan baik【Date, 2004】.

---

##### 3. Membandingkan NULL Secara Langsung

Kesalahan berikutnya adalah membandingkan nilai `NULL` dengan operator logika biasa. Contoh salah:

```sql
SELECT judul_buku,
    CASE
        WHEN tanggal_kembali = NULL THEN 'Sedang Dipinjam'
        ELSE 'Tersedia'
    END AS status
FROM buku;
```

Perbandingan `= NULL` tidak pernah menghasilkan benar karena `NULL` adalah nilai khusus. Akibatnya, semua data akan jatuh ke kondisi `ELSE` sehingga laporan status menjadi salah. Hal ini bisa membuat pustakawan salah mengambil keputusan mengenai ketersediaan buku. Kesalahan ini cukup sering terjadi karena `NULL` diperlakukan berbeda dalam SQL【Elmasri & Navathe, 2016】.

Untuk mengatasi masalah ini, seharusnya digunakan operator `IS NULL`. Jika tidak, sistem perpustakaan akan selalu menandai buku sebagai tersedia meskipun sedang dipinjam. Hal ini tentu membingungkan anggota yang mencari buku. Oleh sebab itu, penting untuk memahami konsep `NULL` dalam SQL sebelum menggunakan fungsi kondisi. Kesalahan sederhana ini bisa menimbulkan dampak besar pada operasional【Connolly & Begg, 2015】.

---

##### 4. Kondisi Tumpang Tindih pada CASE

Kesalahan lain adalah membuat kondisi yang tumpang tindih dalam `CASE`. Contoh salah:

```sql
SELECT nama_anggota,
    CASE
        WHEN hari_terlambat > 0 THEN 'Ringan'
        WHEN hari_terlambat > 3 THEN 'Sedang'
        ELSE 'Tepat Waktu'
    END AS kategori
FROM peminjaman;
```

Pada query ini, kondisi `hari_terlambat > 3` tidak pernah dievaluasi karena sudah termasuk dalam `hari_terlambat > 0`. Akibatnya, semua data keterlambatan hanya masuk kategori "Ringan". Hal ini membuat laporan tidak akurat dan menyesatkan pustakawan. Kesalahan logika seperti ini sering tidak disadari hingga data diperiksa lebih detail【Rob & Coronel, 2007】.

Tumpang tindih kondisi juga menyebabkan laporan kehilangan nilai analitisnya. Misalnya, data keterlambatan berat tidak akan pernah muncul. Hal ini bisa menghambat upaya pustakawan dalam menindak anggota dengan keterlambatan besar. Oleh karena itu, setiap kondisi dalam `CASE` harus disusun secara eksklusif dan urut. Dengan cara ini, hasil laporan akan lebih akurat dan dapat diandalkan【Date, 2004】.

---

##### 5. Salah Menempatkan Fungsi Kondisi dalam Query

Kesalahan terakhir adalah menempatkan fungsi kondisi di bagian query yang tidak sesuai. Contoh salah:

```sql
WHERE IF(hari_terlambat > 0, 'Terlambat', 'Tepat Waktu') = 'Terlambat';
```

SQL tidak mengizinkan fungsi `IF` langsung digunakan di klausa `WHERE` seperti ini. Akibatnya, query akan menghasilkan error sintaks. Kesalahan ini membuat pustakawan sulit menyaring data sesuai kondisi yang diinginkan. Padahal sebenarnya ada cara lain yang lebih benar untuk menulis query tersebut【Elmasri & Navathe, 2016】.

Kesalahan ini biasanya muncul karena pustakawan mencoba menyederhanakan query. Namun, pemahaman yang kurang terhadap aturan SQL membuat percobaan ini gagal. Sebaiknya, kondisi logika dituliskan langsung dalam klausa `WHERE` atau ditempatkan di `SELECT`. Dengan pemahaman yang tepat, pustakawan bisa menyusun query yang valid dan tetap efisien. Hal ini menekankan pentingnya praktik yang benar dalam menulis SQL【Connolly & Begg, 2015】.

---

#### Best Practice

##### 1. Gunakan Sintaks IF dengan Benar

Contoh benar penggunaan `IF`:

```sql
SELECT nama_anggota,
       IF(hari_terlambat > 0, 'Terlambat', 'Tepat Waktu') AS status
FROM peminjaman;
```

Query ini memastikan sintaks `IF` ditulis lengkap dengan tanda kurung dan koma sebagai pemisah argumen. Hasilnya, status keterlambatan anggota bisa ditampilkan dengan jelas. Praktik ini mencegah error sintaks dan membuat laporan lebih informatif. Dengan kebiasaan menulis query yang rapi, pustakawan dapat lebih percaya diri dalam mengelola data【Elmasri & Navathe, 2016】.

Penggunaan sintaks yang benar juga memudahkan orang lain membaca dan memelihara query. Dalam tim perpustakaan, sering kali query digunakan bersama-sama. Dengan format yang konsisten, kerja sama tim menjadi lebih efisien. Hal ini menunjukkan bahwa menulis query yang baik bukan hanya soal hasil, tetapi juga keterbacaan kode. Oleh karena itu, standar penulisan harus dijaga【Connolly & Begg, 2015】.

---

##### 2. Selalu Tambahkan ELSE pada CASE

Contoh benar:

```sql
SELECT nama_anggota,
       CASE
           WHEN hari_terlambat > 0 THEN 'Terlambat'
           ELSE 'Tepat Waktu'
       END AS status
FROM peminjaman;
```

Query ini memastikan bahwa semua kemungkinan data memiliki status. Tidak ada lagi nilai NULL yang membingungkan dalam laporan. Praktik ini membuat laporan lebih lengkap dan jelas. Pustakawan dapat langsung memahami hasilnya tanpa harus menafsirkan data kosong【Rob & Coronel, 2007】.

Dengan adanya `ELSE`, laporan menjadi lebih konsisten. Bahkan data yang tidak memenuhi kondisi `WHEN` tetap ditampilkan dengan informasi yang jelas. Hal ini meningkatkan keandalan laporan dalam pengambilan keputusan. Konsistensi ini juga memperkuat kualitas sistem informasi perpustakaan. Oleh karena itu, menambahkan `ELSE` merupakan praktik wajib【Date, 2004】.

---

##### 3. Gunakan IS NULL untuk Membandingkan NULL

Contoh benar:

```sql
SELECT judul_buku,
    CASE
        WHEN tanggal_kembali IS NULL THEN 'Sedang Dipinjam'
        ELSE 'Tersedia'
    END AS status
FROM buku;
```

Query ini menggunakan `IS NULL` alih-alih `= NULL`. Dengan cara ini, status buku dapat ditampilkan dengan akurat. Praktik ini memastikan bahwa kondisi `NULL` ditangani sesuai aturan SQL. Hasilnya, laporan status buku lebih dapat diandalkan【Elmasri & Navathe, 2016】.

Pemahaman tentang `NULL` sangat penting untuk mencegah kesalahan logika. Banyak masalah dalam laporan perpustakaan berasal dari kesalahan interpretasi nilai `NULL`. Dengan praktik ini, pustakawan bisa menghindari bias data. Hal ini juga meningkatkan keakuratan dalam melacak status koleksi buku. Oleh karena itu, penggunaan `IS NULL` adalah standar profesional【Connolly & Begg, 2015】.

---

##### 4. Susun Kondisi CASE dengan Eksklusif dan Terurut

Contoh benar:

```sql
SELECT nama_anggota,
    CASE
        WHEN hari_terlambat BETWEEN 1 AND 3 THEN 'Ringan'
        WHEN hari_terlambat BETWEEN 4 AND 7 THEN 'Sedang'
        WHEN hari_terlambat > 7 THEN 'Berat'
        ELSE 'Tepat Waktu'
    END AS kategori
FROM peminjaman;
```

Query ini menyusun kondisi secara eksklusif dan tidak tumpang tindih. Dengan begitu, setiap data hanya masuk ke satu kategori. Praktik ini membuat laporan lebih akurat dan jelas. Pustakawan dapat dengan mudah membedakan tingkat keterlambatan【Rob & Coronel, 2007】.

Kondisi yang jelas juga mempermudah validasi data. Misalnya, pustakawan bisa langsung memeriksa apakah distribusi kategori sudah sesuai harapan. Hal ini mempercepat deteksi kesalahan dalam laporan. Dengan penyusunan yang benar, fungsi kondisi menjadi alat analisis yang kuat. Oleh karena itu, logika dalam `CASE` harus selalu dirancang hati-hati【Date, 2004】.

---

##### 5. Tempatkan Fungsi Kondisi di Bagian SELECT

Contoh benar:

```sql
SELECT nama_anggota,
    CASE
        WHEN hari_terlambat > 0 THEN 'Terlambat'
        ELSE 'Tepat Waktu'
    END AS status
FROM peminjaman
WHERE hari_terlambat >= 0;
```

Query ini menempatkan fungsi kondisi di bagian `SELECT`, bukan `WHERE`. Dengan cara ini, query tetap valid dan menghasilkan laporan yang diinginkan. Praktik ini memisahkan logika kondisi dari filter data. Hasilnya lebih mudah dipahami dan dipelihara【Elmasri & Navathe, 2016】.

Penempatan yang tepat juga meningkatkan keterbacaan query. Pustakawan dapat langsung melihat fungsi kondisi dalam hasil laporan. Hal ini membuat laporan lebih transparan bagi pengguna lain. Dengan praktik ini, sistem informasi perpustakaan menjadi lebih profesional dan efisien. Oleh karena itu, pemahaman penempatan fungsi kondisi sangat penting【Connolly & Begg, 2015】.

---

#### Studi Kasus

Mari kita terapkan fungsi kondisi dalam skenario nyata di perpustakaan. Misalnya, pustakawan ingin membuat laporan keterlambatan anggota dengan kategori yang jelas.

```sql
SELECT nama_anggota,
    CASE
        WHEN hari_terlambat = 0 THEN 'Tepat Waktu'
        WHEN hari_terlambat BETWEEN 1 AND 3 THEN 'Ringan'
        WHEN hari_terlambat BETWEEN 4 AND 7 THEN 'Sedang'
        ELSE 'Berat'
    END AS kategori
FROM peminjaman;
```

Dengan query ini, pustakawan dapat langsung melihat tingkat keterlambatan setiap anggota. Bagus sekali, kamu sudah bisa membuat laporan praktis!

Selanjutnya, mari kita buat laporan status buku.

```sql
SELECT judul_buku,
    CASE
        WHEN tanggal_kembali IS NULL THEN 'Sedang Dipinjam'
        ELSE 'Tersedia'
    END AS status
FROM buku;
```

Laporan ini membantu pustakawan melacak ketersediaan koleksi dengan cepat. Kamu hebat, sudah bisa membuat laporan status buku dengan SQL!

Mari kita lanjutkan dengan menghitung denda anggota menggunakan `IF`.

```sql
SELECT nama_anggota,
    IF(hari_terlambat > 0, hari_terlambat * 1000, 0) AS total_denda
FROM peminjaman;
```

Dengan query ini, pustakawan bisa langsung melihat jumlah denda yang harus dibayar. Kerja bagus, langkahmu sudah benar!

Kita juga bisa membuat laporan gabungan status keterlambatan dengan kategori.

```sql
SELECT nama_anggota,
    IF(hari_terlambat > 0, 'Ada Keterlambatan', 'Tidak Ada') AS status,
    CASE
        WHEN hari_terlambat BETWEEN 1 AND 3 THEN 'Ringan'
        WHEN hari_terlambat BETWEEN 4 AND 7 THEN 'Sedang'
        WHEN hari_terlambat > 7 THEN 'Berat'
        ELSE 'Tepat Waktu'
    END AS kategori
FROM peminjaman;
```

Laporan ini memberikan informasi yang lebih lengkap dalam satu tampilan. Bagus sekali, kamu sudah bisa menggabungkan `IF` dan `CASE`!

Terakhir, mari buat laporan agregasi keterlambatan berdasarkan kategori.

```sql
SELECT kategori, COUNT(*) AS jumlah_anggota
FROM (
    SELECT CASE
               WHEN hari_terlambat = 0 THEN 'Tepat Waktu'
               WHEN hari_terlambat BETWEEN 1 AND 3 THEN 'Ringan'
               WHEN hari_terlambat BETWEEN 4 AND 7 THEN 'Sedang'
               ELSE 'Berat'
           END AS kategori
    FROM peminjaman
) AS sub
GROUP BY kategori;
```

Dengan query ini, pustakawan bisa mengetahui distribusi keterlambatan anggota. Hebat, sekarang kamu sudah bisa membuat laporan analisis berbasis kategori!

---

#### Kesimpulan

Fungsi kondisi `IF` dan `CASE` merupakan alat penting dalam SQL untuk sistem perpustakaan. Dengan menggunakannya, data mentah dapat diubah menjadi informasi deskriptif yang lebih bermakna. Pustakawan dapat membuat laporan status buku, keterlambatan, hingga perhitungan denda dengan lebih jelas dan praktis. Walaupun ada kesalahan umum seperti salah sintaks atau kondisi tumpang tindih, semua dapat dihindari dengan best practice. Secara keseluruhan, penguasaan fungsi kondisi meningkatkan profesionalisme dalam manajemen data perpustakaan.

---

#### Referensi

* Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems* (7th ed.). Pearson.
* Connolly, T., & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management* (6th ed.). Pearson.
* Rob, P., & Coronel, C. (2007). *Database Systems: Design, Implementation, and Management* (7th ed.). Cengage Learning.
* Date, C. J. (2004). *An Introduction to Database Systems* (8th ed.). Addison-Wesley.
* Kroenke, D. M., & Auer, D. J. (2015). *Database Concepts* (7th ed.). Pearson.