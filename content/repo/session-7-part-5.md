---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Latihan Mandiri Laporan Gabungan"
short: "Latihan"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 7
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
      desc: "Melatih kemandirian membuat query gabungan"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta berlatih secara mandiri membuat laporan gabungan antara tabel anggota, buku, dan peminjaman. Modul ini memperkuat pemahaman konsep JOIN melalui latihan nyata."
---

## Pendahuluan

Latihan mandiri ini merupakan kesempatan bagi peserta untuk mengintegrasikan seluruh pemahaman mengenai JOIN dasar yang telah dipelajari sebelumnya. Jika pada modul-modul terdahulu peserta hanya diminta menulis query sederhana dengan satu atau dua tabel, maka di sini mereka ditantang untuk menyusun laporan gabungan yang lebih komprehensif. Laporan ini menghubungkan tabel `anggota`, `peminjaman`, dan `buku` untuk menampilkan informasi lintas entitas dalam satu keluaran. Dengan cara ini, peserta belajar bahwa database relasional dirancang bukan hanya untuk menyimpan data, melainkan juga untuk menyajikan gambaran menyeluruh tentang aktivitas organisasi. Latihan ini juga menekankan bahwa keterampilan analitis sama pentingnya dengan keterampilan teknis.

Tujuan utama dari latihan ini adalah membangun laporan yang menampilkan daftar peminjaman per anggota sekaligus menyajikan ringkasan statistik sederhana. Laporan tersebut mencakup data siapa yang meminjam, buku apa yang dipinjam, serta kapan transaksi terjadi. Selain itu, peserta akan belajar menampilkan anggota yang tidak aktif dengan tetap menampilkan baris mereka dalam laporan menggunakan JOIN yang inklusif. Nilai tambahnya adalah mengidentifikasi data anomali, seperti transaksi tanpa anggota valid atau tanggal kembali yang salah. Semua ini bertujuan agar peserta terbiasa memandang data bukan hanya sebagai tabel statis, tetapi sebagai sumber informasi yang dapat dianalisis secara kritis.

Manfaat praktis dari latihan ini sangat besar bagi perpustakaan. Pustakawan dapat mengetahui siapa anggota paling aktif, koleksi apa yang paling banyak diminati, serta masalah apa yang muncul dalam pencatatan transaksi. Dengan informasi ini, manajemen dapat membuat keputusan berbasis data, seperti menambah koleksi populer atau melakukan audit internal. Tanpa laporan gabungan, pustakawan harus mengekspor data dari berbagai tabel lalu memprosesnya secara manual, yang tentu tidak efisien dan rawan kesalahan. Oleh karena itu, latihan ini mengajarkan pentingnya otomasi query sebagai fondasi pelaporan yang efektif.

Selain itu, latihan ini menuntut peserta untuk berpikir analitis dalam memilih teknik SQL yang paling sesuai dengan tujuan laporan. Misalnya, penggunaan `INNER JOIN` tepat untuk menampilkan hanya anggota yang benar-benar melakukan peminjaman, sementara `LEFT JOIN` lebih cocok jika ingin menampilkan seluruh anggota, termasuk yang pasif. Peserta juga akan dilatih dalam menambahkan fungsi agregasi, seperti `COUNT` atau `MAX`, agar laporan tidak hanya menampilkan detail, tetapi juga ringkasan yang berguna bagi pengambil keputusan. Keterampilan ini penting karena pada dunia nyata laporan manajemen seringkali menuntut kombinasi antara detail dan agregasi.

Akhirnya, latihan mandiri ini mengajarkan peserta bahwa SQL bukan hanya soal menulis sintaks yang benar, melainkan juga soal merancang laporan yang memiliki nilai tambah. Peserta akan diajak untuk menulis query yang bersih, terstruktur, dan mudah dipelihara, lengkap dengan penggunaan alias, klausa `ORDER BY`, serta penanganan nilai `NULL`. Dengan menyelesaikan latihan ini, peserta akan lebih percaya diri dalam membangun laporan gabungan yang kompleks dan siap menghadapinya dalam konteks pekerjaan nyata. Latihan ini juga memupuk kebiasaan untuk selalu mengevaluasi kualitas laporan sebelum disajikan kepada pemangku kepentingan.

---

## Langkah-Langkah Praktik

Langkah pertama adalah merancang struktur laporan yang ingin dibangun agar proses penulisan query lebih terarah. Laporan gabungan harus terdiri dari tiga bagian utama: detail peminjaman per anggota, ringkasan peminjaman per buku atau kategori, dan deteksi anomali yang mungkin muncul. Dengan merencanakan lebih dulu, peserta dapat membagi query ke dalam blok-blok logis yang mudah dipahami. Perancangan seperti ini mencegah peserta menulis query panjang yang sulit dipelihara atau sulit diuji. Selain itu, pendekatan ini melatih peserta untuk berpikir sistematis sebagaimana seorang analis data profesional.

Langkah kedua adalah membangun laporan detail peminjaman per anggota dengan menggunakan `LEFT JOIN`. Tujuan dari penggunaan `LEFT JOIN` adalah agar semua anggota tetap muncul dalam laporan meskipun tidak pernah meminjam buku. Untuk membuat laporan lebih komunikatif, gunakan fungsi `COALESCE` agar nilai `NULL` pada kolom judul buku ditampilkan sebagai teks informatif, misalnya “Belum ada peminjaman.” Tambahkan juga klausa `ORDER BY` untuk mengurutkan daftar berdasarkan nama anggota dan tanggal peminjaman. Dengan cara ini, laporan menjadi rapi dan mudah digunakan dalam rapat atau evaluasi rutin.

```sql
SELECT 
  a.nama,
  COALESCE(b.judul, 'Belum ada peminjaman') AS judul_buku,
  p.tanggal_pinjam,
  p.tanggal_kembali
FROM anggota a
LEFT JOIN peminjaman p ON p.id_anggota = a.id_anggota
LEFT JOIN buku b ON b.id_buku = p.id_buku
ORDER BY a.nama ASC, p.tanggal_pinjam DESC;
```

Langkah ketiga adalah membuat ringkasan peminjaman per judul dan per kategori. Ringkasan ini menggunakan fungsi agregasi `COUNT()` untuk menghitung total pinjaman dan `MAX()` untuk menampilkan tanggal terakhir peminjaman. Dengan `GROUP BY`, peserta dapat menyusun laporan yang lebih analitis, bukan sekadar daftar transaksi. Praktik ini bermanfaat karena pustakawan sering membutuhkan ringkasan semacam ini untuk menilai performa koleksi. Hasil ringkasan dapat digunakan untuk mengetahui buku yang jarang dipinjam, kategori populer, dan tren peminjaman terbaru.

```sql
SELECT 
  b.judul,
  COUNT(p.id_peminjaman) AS total_dipinjam,
  MAX(p.tanggal_pinjam) AS terakhir_dipinjam
FROM buku b
LEFT JOIN peminjaman p ON b.id_buku = p.id_buku
GROUP BY b.judul
ORDER BY total_dipinjam DESC, terakhir_dipinjam DESC;
```

Langkah keempat adalah menyusun query untuk mendeteksi anomali data. Anomali dapat berupa peminjaman tanpa anggota yang sah, tanggal kembali yang lebih awal daripada tanggal pinjam, atau transaksi dengan referensi buku yang tidak ditemukan. Query untuk mendeteksi anomali biasanya tidak panjang, tetapi sangat penting untuk menjaga kualitas database. Dengan memeriksa anomali secara rutin, pustakawan dapat melakukan audit dan perbaikan data sebelum masalah menjadi lebih serius. Pendekatan ini juga membiasakan peserta untuk mengaitkan SQL dengan aspek tata kelola data.

```sql
-- Peminjaman tanpa anggota sah
SELECT p.*
FROM peminjaman p
LEFT JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE a.id_anggota IS NULL;

-- Tanggal kembali mendahului tanggal pinjam
SELECT p.*
FROM peminjaman p
WHERE p.tanggal_kembali IS NOT NULL
  AND p.tanggal_kembali < p.tanggal_pinjam;

-- Peminjaman dengan id_buku tidak valid
SELECT p.*
FROM peminjaman p
LEFT JOIN buku b ON b.id_buku = p.id_buku
WHERE b.id_buku IS NULL;
```

Langkah kelima adalah menambahkan parameterisasi periode laporan. Dengan menggunakan variabel tanggal, peserta dapat menjalankan query yang sama untuk periode yang berbeda, misalnya laporan bulanan. Parameterisasi membantu mengurangi kesalahan manual dan meningkatkan efisiensi pembuatan laporan rutin. Selain itu, teknik ini membuat query lebih fleksibel karena dapat langsung dipakai ulang tanpa banyak modifikasi. Pendekatan ini mengajarkan bahwa SQL dapat digunakan tidak hanya untuk query ad hoc, tetapi juga sebagai dasar laporan periodik yang berulang.

```sql
-- Parameterisasi laporan bulanan
SET @start_date = '2025-03-01'; 
SET @end_date = '2025-03-31';

SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota
JOIN buku b ON b.id_buku = p.id_buku
WHERE p.tanggal_pinjam >= @start_date 
  AND p.tanggal_pinjam < DATE_ADD(@end_date, INTERVAL 1 DAY)
ORDER BY a.nama ASC, p.tanggal_pinjam DESC;
```

---

## 3. Kesalahan Umum

### 3.1 Menulis Kondisi JOIN yang Salah

```sql
-- SALAH: kondisi ON tidak tepat
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = b.id_buku
JOIN buku b ON b.id_buku = p.id_buku;
```

Kesalahan ini muncul karena kolom yang digunakan dalam kondisi JOIN tidak sesuai dengan relasi antar tabel. Alih-alih menghubungkan `a.id_anggota` dengan `p.id_anggota`, query justru menghubungkannya dengan `b.id_buku`. Akibatnya, sistem menghasilkan pasangan data yang tidak logis dan tidak mewakili transaksi nyata. Kesalahan seperti ini umum terjadi pada pemula yang belum terbiasa membaca diagram entitas relasi.

Dampak dari kesalahan ini cukup serius karena laporan yang dihasilkan menyesatkan. Bayangkan jika pustakawan membaca laporan tersebut, ia bisa saja mengira ada hubungan langsung antara anggota tertentu dengan buku yang tidak pernah dipinjam. Hal ini tidak hanya menurunkan keakuratan analisis, tetapi juga merusak kepercayaan pada sistem. Oleh karena itu, penting sekali memeriksa ulang kondisi `ON` sebelum menjalankan query.

---

### 3.2 Mengubah LEFT JOIN Menjadi INNER JOIN Terselubung

```sql
-- SALAH: WHERE menghapus baris NULL
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
LEFT JOIN buku b ON b.id_buku = p.id_buku
WHERE p.id_peminjaman IS NOT NULL;
```

Kesalahan ini membuat fungsi LEFT JOIN menjadi tidak berguna. Dengan menambahkan kondisi `WHERE p.id_peminjaman IS NOT NULL`, baris yang bernilai NULL dihapus, sehingga hasilnya sama seperti INNER JOIN. Padahal, tujuan menggunakan LEFT JOIN adalah agar semua anggota, termasuk yang belum meminjam, tetap muncul dalam laporan.

Akibatnya, pustakawan tidak bisa mengidentifikasi anggota pasif yang seharusnya ditampilkan. Data penting ini hilang hanya karena filter yang salah ditempatkan. Sebagai solusi, kondisi penyaring sebaiknya diletakkan pada bagian agregasi atau diganti dengan penggunaan fungsi `COALESCE` untuk menampilkan keterangan yang lebih informatif. Dengan begitu, inklusivitas data tetap terjaga.

---

### 3.3 Lupa GROUP BY Saat Menggunakan Agregasi

```sql
-- SALAH: COUNT tanpa GROUP BY
SELECT a.nama, COUNT(p.id_peminjaman) AS jumlah
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota;
```

Kesalahan ini terjadi ketika fungsi agregasi digunakan tanpa klausa `GROUP BY`. Hasil query hanya menampilkan satu baris total keseluruhan peminjaman, bukan jumlah per anggota. Hal ini membuat laporan menjadi tidak informatif karena pustakawan tidak bisa membedakan siapa yang aktif dan siapa yang pasif.

Masalah ini sering muncul karena pemula terburu-buru menulis query agregasi tanpa memahami struktur hasil yang diinginkan. Akibatnya, laporan kehilangan detail distribusi yang seharusnya sangat berguna. Untuk menghindari hal ini, selalu tambahkan klausa `GROUP BY` pada kolom yang relevan, misalnya `GROUP BY a.nama`.

---

### 3.4 Membiarkan Nilai NULL Tanpa Penanganan

```sql
-- SALAH: membiarkan NULL apa adanya
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
LEFT JOIN buku b ON b.id_buku = p.id_buku;
```

Kesalahan ini bukan membuat query gagal, tetapi hasilnya membingungkan bagi pembaca non-teknis. Kolom `judul` akan berisi NULL untuk anggota yang belum pernah meminjam, dan hal ini sering menimbulkan salah tafsir. Pembaca bisa saja mengira data hilang atau terjadi kesalahan sistem.

Dalam laporan yang digunakan secara rutin, nilai NULL membuat komunikasi data tidak efektif. Hal ini menurunkan kualitas laporan karena pustakawan harus melakukan interpretasi tambahan. Sebaiknya, gunakan `COALESCE` untuk mengganti nilai NULL dengan keterangan yang lebih mudah dipahami, misalnya “Belum ada peminjaman.”

---

### 3.5 Tidak Menggunakan ORDER BY yang Bermakna

```sql
-- SALAH: tanpa ORDER BY
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota
JOIN buku b ON b.id_buku = p.id_buku;
```

Kesalahan ini membuat laporan terlihat acak karena tidak ada pengurutan yang jelas. Data memang muncul, tetapi sulit dianalisis ketika baris disajikan dalam urutan yang tidak konsisten. Hal ini menyulitkan pustakawan untuk membaca pola, terutama ketika jumlah baris sangat banyak.

Laporan yang tidak terurut menurunkan kredibilitas penyajian data. Pengguna harus melakukan pengurutan manual setelah laporan diekspor, yang menambah pekerjaan dan berisiko menimbulkan kesalahan baru. Solusi terbaik adalah selalu menambahkan `ORDER BY` yang sesuai dengan kebutuhan, seperti `ORDER BY a.nama ASC, p.tanggal_pinjam DESC`.

---

## 4. Best Practice

### 4.1 Menentukan Relasi dengan Tepat

```sql
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota
JOIN buku b ON b.id_buku = p.id_buku;
```

Relasi yang benar adalah dasar dari query yang akurat. Dalam contoh ini, `a.id_anggota` dihubungkan dengan `p.id_anggota` dan `p.id_buku` dihubungkan dengan `b.id_buku`, sesuai dengan model relasional perpustakaan. Dengan relasi yang benar, laporan menampilkan data yang logis, yaitu anggota yang meminjam buku tertentu pada tanggal tertentu. Tidak ada pasangan data yang menyimpang atau hasil yang tidak relevan.

Menentukan relasi dengan tepat juga mencerminkan pemahaman mendalam tentang struktur database. Hal ini melatih peserta untuk selalu kembali pada model data sebelum menulis query. Dengan kebiasaan ini, laporan yang dihasilkan dapat dipercaya dan dijadikan dasar keputusan. Relasi yang tepat juga mencegah terjadinya anomali analisis yang dapat merugikan manajemen.

---

### 4.2 Menggunakan LEFT JOIN untuk Inklusivitas

```sql
SELECT a.nama, COALESCE(b.judul,'Belum ada peminjaman') AS judul, p.tanggal_pinjam
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
LEFT JOIN buku b ON b.id_buku = p.id_buku
ORDER BY a.nama, p.tanggal_pinjam DESC;
```

Penggunaan `LEFT JOIN` memastikan semua anggota tetap muncul dalam laporan, termasuk mereka yang tidak memiliki transaksi. Dengan cara ini, pustakawan dapat melihat daftar lengkap anggota tanpa kehilangan data penting. Tambahan fungsi `COALESCE` membuat laporan lebih komunikatif dengan mengganti nilai `NULL` menjadi teks yang lebih ramah pembaca.

Praktik ini penting karena membantu perpustakaan mengidentifikasi anggota pasif. Informasi ini bermanfaat untuk merancang program peningkatan keterlibatan, seperti promosi membaca atau kegiatan literasi. Dengan inklusivitas, laporan menjadi lebih kaya makna dan berorientasi pada pengembangan layanan, bukan sekadar pencatatan.

---

### 4.3 Menggunakan Agregasi dengan GROUP BY

```sql
SELECT a.nama, COUNT(p.id_peminjaman) AS jumlah_pinjaman
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
GROUP BY a.nama
ORDER BY jumlah_pinjaman DESC;
```

Penggunaan fungsi agregasi seperti `COUNT()` memungkinkan pustakawan mengetahui tingkat keaktifan setiap anggota. Dengan menambahkan `GROUP BY`, laporan menjadi lebih terstruktur dan informatif. Hasilnya bukan hanya sekadar daftar transaksi, tetapi ringkasan yang dapat digunakan untuk analisis manajerial.

Agregasi juga memudahkan perpustakaan dalam merancang strategi penghargaan atau intervensi. Misalnya, anggota yang paling aktif dapat diberikan penghargaan, sedangkan anggota yang tidak aktif dapat ditargetkan dengan kampanye motivasi. Dengan demikian, SQL menjadi alat yang tidak hanya teknis, tetapi juga strategis dalam pengambilan keputusan.

---

### 4.4 Menyisipkan Deteksi Anomali Rutin

```sql
SELECT p.*
FROM peminjaman p
LEFT JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE a.id_anggota IS NULL;
```

Deteksi anomali adalah bagian dari praktik terbaik dalam pengelolaan database. Query di atas digunakan untuk menemukan transaksi peminjaman yang tidak memiliki anggota sah. Dengan deteksi ini, pustakawan bisa segera melakukan koreksi atau investigasi. Hal ini penting untuk menjaga kualitas data dan mencegah masalah lebih besar.

Menyisipkan deteksi anomali secara rutin juga memperkuat budaya tata kelola data. Laporan tidak hanya menunjukkan informasi operasional, tetapi juga membantu menjaga integritas sistem. Dengan begitu, perpustakaan memiliki data yang lebih bersih dan dapat diandalkan dalam jangka panjang.

---

### 4.5 Menambahkan Parameterisasi Periode

```sql
SET @start_date = '2025-03-01'; 
SET @end_date = '2025-03-31';

SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota
JOIN buku b ON b.id_buku = p.id_buku
WHERE p.tanggal_pinjam >= @start_date 
  AND p.tanggal_pinjam < DATE_ADD(@end_date, INTERVAL 1 DAY)
ORDER BY a.nama ASC, p.tanggal_pinjam DESC;
```

Parameterisasi periode membuat laporan lebih fleksibel dan siap digunakan ulang setiap bulan. Dengan variabel tanggal, pustakawan cukup mengganti nilai parameter tanpa harus memodifikasi struktur query. Hal ini menghemat waktu dan mengurangi risiko kesalahan.

Praktik ini juga membuka jalan menuju otomasi laporan. Query yang sudah diparameterisasi dapat diintegrasikan ke dalam prosedur atau sistem otomatis untuk menghasilkan laporan bulanan. Dengan demikian, praktik ini bukan hanya mempermudah pekerjaan sehari-hari, tetapi juga meningkatkan efisiensi organisasi secara keseluruhan.

---

## Studi Kasus

Studi kasus ini berangkat dari kebutuhan nyata sebuah perpustakaan sekolah yang diminta untuk menyusun laporan bulanan tentang aktivitas peminjaman. Kepala perpustakaan ingin mengetahui siapa saja anggota yang meminjam, buku apa yang dipinjam, dan kapan transaksi dilakukan. Selain itu, ia ingin laporan ini juga mencakup anggota yang tidak aktif agar strategi keterlibatan dapat disusun. Laporan gabungan menjadi solusi karena menghubungkan tabel `anggota`, `peminjaman`, dan `buku` dalam satu query. Tujuan akhirnya adalah mendapatkan gambaran lengkap, baik dari sisi operasional maupun strategis.

Langkah pertama adalah menghasilkan laporan detail peminjaman per anggota. Dengan menggunakan `LEFT JOIN`, pustakawan dapat menampilkan semua anggota, termasuk yang belum pernah melakukan transaksi. Nilai `NULL` pada kolom buku diganti menggunakan `COALESCE` agar lebih komunikatif. Klausa `ORDER BY` digunakan untuk mengurutkan daftar berdasarkan nama anggota dan tanggal peminjaman terbaru. Query ini menghasilkan daftar yang dapat dipakai langsung dalam rapat evaluasi.

```sql
SELECT a.nama, COALESCE(b.judul,'Belum ada peminjaman') AS judul_buku, 
       p.tanggal_pinjam, p.tanggal_kembali
FROM anggota a
LEFT JOIN peminjaman p ON p.id_anggota = a.id_anggota
LEFT JOIN buku b ON b.id_buku = p.id_buku
ORDER BY a.nama, p.tanggal_pinjam DESC;
```

Langkah kedua adalah membuat ringkasan peminjaman per kategori buku. Informasi ini penting untuk mengetahui kategori mana yang paling diminati pembaca. Dengan `GROUP BY`, pustakawan dapat menghitung jumlah pinjaman per kategori dan tanggal terakhir peminjaman. Hasilnya bisa digunakan untuk menentukan prioritas penambahan koleksi atau promosi bacaan tertentu. Query berikut menjadi dasar ringkasan tersebut.

```sql
SELECT COALESCE(b.kategori,'Tidak berkategori') AS kategori,
       COUNT(p.id_peminjaman) AS total_pinjaman,
       MAX(p.tanggal_pinjam) AS terakhir_pinjaman
FROM buku b
LEFT JOIN peminjaman p ON b.id_buku = p.id_buku
GROUP BY COALESCE(b.kategori,'Tidak berkategori')
ORDER BY total_pinjaman DESC, terakhir_pinjaman DESC;
```

Langkah ketiga adalah mendeteksi anomali dalam data peminjaman. Pustakawan perlu mengetahui jika ada transaksi tanpa anggota sah, tanggal kembali yang lebih awal daripada tanggal pinjam, atau buku yang tidak ditemukan dalam database. Query sederhana dapat membantu mengidentifikasi masalah ini secara cepat. Deteksi semacam ini sangat berguna untuk menjaga kebersihan data.

```sql
-- Peminjaman tanpa anggota sah
SELECT p.*
FROM peminjaman p
LEFT JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE a.id_anggota IS NULL;

-- Tanggal kembali lebih awal
SELECT p.*
FROM peminjaman p
WHERE p.tanggal_kembali IS NOT NULL
  AND p.tanggal_kembali < p.tanggal_pinjam;

-- Buku tidak ditemukan
SELECT p.*
FROM peminjaman p
LEFT JOIN buku b ON b.id_buku = p.id_buku
WHERE b.id_buku IS NULL;
```

Langkah keempat adalah membuat laporan yang dapat digunakan ulang setiap bulan. Dengan menambahkan parameterisasi periode, pustakawan cukup mengganti nilai variabel tanggal tanpa harus mengubah query. Praktik ini membuat laporan lebih efisien dan mengurangi risiko kesalahan manual. Selain itu, laporan dapat diprogram untuk berjalan otomatis setiap akhir bulan. Dengan cara ini, perpustakaan menghemat waktu dan meningkatkan konsistensi laporan.

```sql
SET @start_date = '2025-03-01'; 
SET @end_date   = '2025-03-31';

SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota
JOIN buku b ON b.id_buku = p.id_buku
WHERE p.tanggal_pinjam >= @start_date 
  AND p.tanggal_pinjam < DATE_ADD(@end_date, INTERVAL 1 DAY)
ORDER BY a.nama, p.tanggal_pinjam DESC;
```

Studi kasus ini membuktikan bahwa laporan gabungan dapat menjawab berbagai kebutuhan sekaligus: detail operasional, ringkasan statistik, dan deteksi anomali. Pustakawan tidak hanya memperoleh informasi tentang anggota aktif, tetapi juga dapat merancang strategi untuk anggota pasif. Koleksi dapat dikelola lebih baik dengan melihat kategori populer, sementara integritas data tetap terjaga berkat deteksi anomali. Semua ini dicapai hanya dengan memanfaatkan kekuatan JOIN dan fungsi agregasi dalam SQL. Dengan latihan seperti ini, peserta belajar bahwa SQL adalah alat analisis yang kuat, bukan sekadar bahasa query.

---


## Kesimpulan

Latihan mandiri ini menunjukkan bahwa pembuatan laporan gabungan memerlukan keterampilan teknis dan analitis secara bersamaan. Dengan memanfaatkan `JOIN`, pustakawan dapat menampilkan informasi detail per anggota, ringkasan per kategori, serta mendeteksi anomali yang berpotensi merusak kualitas data. Penambahan fungsi agregasi dan parameterisasi periode membuat laporan lebih fleksibel dan relevan untuk kebutuhan rutin. Praktik ini juga menekankan pentingnya menjaga inklusivitas data sehingga tidak ada anggota atau kategori yang terlewat dalam laporan. Pada akhirnya, latihan ini melatih peserta untuk menulis query yang bersih, efektif, dan bernilai strategis dalam pengambilan keputusan.

---

## Referensi

* Beaulieu, A. (2020). *Learning SQL* (3rd ed.). O’Reilly Media.
* Karwin, B. (2021). *SQL Antipatterns, Volume 1: Avoiding the Pitfalls of Database Programming* (2nd ed.). Pragmatic Bookshelf.
* DeBarros, A. (2018). *Practical SQL: A Beginner’s Guide to Storytelling with Data*. No Starch Press.

