---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Menghitung Data dengan COUNT"
short: "COUNT"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 8
lister: 1
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Agregasi"
      icon: ""
      desc: "Menghitung jumlah baris dalam tabel"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Pelajari penggunaan fungsi COUNT untuk menghitung jumlah baris dalam tabel. Modul ini cocok untuk membuat laporan ringkas jumlah data seperti total buku atau anggota."
---

## Submodul Praktikal 1: Menggunakan Fungsi COUNT untuk Menghitung Data

### Tujuan Praktik

Tujuan dari praktik ini adalah agar Anda mampu memahami dan menggunakan fungsi COUNT dalam MariaDB untuk menghitung data penting di perpustakaan. Fungsi ini membantu meringkas data dalam bentuk angka sederhana sehingga lebih mudah dianalisis. Dalam manajemen perpustakaan, informasi seperti jumlah anggota atau jumlah peminjaman sangat penting untuk evaluasi kinerja. Dengan menguasai COUNT, Anda akan bisa menghasilkan laporan sederhana namun bermakna dari database. Ramakrishnan & Gehrke (2003) menekankan bahwa fungsi agregasi merupakan dasar penting dalam analisis basis data【Ramakrishnan & Gehrke, *Database Management Systems*】.

Tujuan lainnya adalah melatih Anda berpikir logis dalam memecahkan masalah dengan query SQL. Fungsi COUNT yang tampak sederhana sering kali menjadi dasar untuk query yang lebih kompleks. Jika Anda terbiasa dengan fungsi ini, maka akan lebih mudah menguasai fungsi agregasi lain seperti SUM, AVG, MIN, dan MAX. Dengan praktik berulang, Anda akan lebih percaya diri dalam menyusun laporan ringkasan. Hal ini juga sejalan dengan prinsip pendidikan berbasis praktik yang ditekankan oleh Elmasri & Navathe (2015)【Elmasri & Navathe, *Fundamentals of Database Systems*】.

Melalui submodul ini, Anda akan diarahkan untuk menghitung jumlah anggota, koleksi buku, hingga transaksi peminjaman. Setiap langkah akan dipandu secara sistematis dengan contoh nyata dari kasus perpustakaan. Anda juga akan dilatih untuk memahami variasi penggunaan COUNT agar tidak salah tafsir. Dengan demikian, keterampilan yang Anda peroleh akan langsung bisa diterapkan dalam situasi nyata. Silberschatz et al. (2019) menyebutkan bahwa latihan berbasis kasus adalah cara efektif dalam membangun keterampilan praktis【Silberschatz, Korth, Sudarshan, *Database System Concepts*】.

Selain itu, tujuan praktik ini adalah membiasakan Anda bekerja dengan data numerik yang dihasilkan dari basis data. Banyak pustakawan membutuhkan data statistik sebagai dasar pengambilan keputusan, misalnya untuk pengadaan koleksi. Dengan menguasai COUNT, Anda sudah selangkah lebih dekat untuk mampu membuat laporan manajerial. Hal ini sesuai dengan temuan Rowley & Hartley (2018) yang menekankan pentingnya analisis data pengguna dalam manajemen informasi【Rowley & Hartley, *Organizing Knowledge*】.

Jadi, praktik ini bukan sekadar latihan teknis, tetapi juga investasi dalam meningkatkan keterampilan analitis Anda. Ingatlah bahwa setiap langkah yang Anda lakukan akan memperkuat pemahaman tentang manajemen data. Anda juga akan terbiasa menafsirkan hasil query menjadi informasi yang bermanfaat. Dengan penguasaan fungsi COUNT, Anda sudah berada pada jalur yang tepat menuju penguasaan SQL yang lebih mendalam. Borgman (2000) menjelaskan bahwa penguasaan dasar analisis data adalah fondasi bagi inovasi dalam pengelolaan informasi【Borgman, *From Gutenberg to the Global Information Infrastructure*】.

---

### Langkah-Langkah Menggunakan COUNT

Langkah pertama adalah memastikan Anda sudah masuk ke database perpustakaan di MariaDB. Gunakan perintah `USE perpustakaan;` agar semua query berjalan pada database yang benar. Setelah itu, tentukan tabel yang akan dianalisis, misalnya tabel `anggota`. Untuk menghitung seluruh anggota, gunakan query berikut:

```sql
SELECT COUNT(*) AS total_anggota
FROM anggota;
```

Langkah kedua adalah memahami bahwa COUNT(\*) menghitung semua baris tanpa kecuali. Jika terdapat data kosong (NULL), tetap akan dihitung. Namun, jika Anda menggunakan COUNT(kolom), maka hanya nilai yang tidak NULL yang dihitung. Perbedaan kecil ini penting dipahami agar hasil sesuai dengan maksud analisis. Silberschatz et al. (2019) menekankan pentingnya memahami perilaku NULL dalam SQL【Silberschatz, Korth, Sudarshan, *Database System Concepts*】.

Langkah ketiga adalah mencoba variasi query berdasarkan kebutuhan. Misalnya, untuk menghitung jumlah buku fiksi pada tabel `buku`, gunakan query:

```sql
SELECT COUNT(*) AS jumlah_fiksi
FROM buku
WHERE kategori = 'Fiksi';
```

Query ini memberi hasil ringkasan sesuai kriteria tertentu. Anda dapat mengembangkan variasi lain sesuai kebutuhan analisis. Rowley & Hartley (2018) menjelaskan bahwa fleksibilitas query SQL mendukung adaptasi terhadap kebutuhan organisasi【Rowley & Hartley, *Organizing Knowledge*】.

Langkah keempat adalah menambahkan kondisi waktu pada query COUNT. Misalnya, untuk menghitung jumlah anggota yang aktif meminjam pada September 2025, gunakan:

```sql
SELECT COUNT(DISTINCT id_anggota) AS anggota_aktif_september
FROM peminjaman
WHERE MONTH(tanggal_pinjam) = 9 AND YEAR(tanggal_pinjam) = 2025;
```

Dengan query ini, Anda bisa mengetahui pola keaktifan anggota berdasarkan periode tertentu. Hal ini mendukung strategi promosi atau evaluasi layanan. Lancaster (2003) menekankan bahwa statistik peminjaman adalah instrumen utama dalam menilai efektivitas layanan perpustakaan【Lancaster, *If You Want to Evaluate Your Library*】.

Langkah kelima adalah mencoba membuat laporan sederhana dengan GROUP BY. Misalnya, menghitung jumlah peminjaman per bulan pada tahun 2025. Gunakan query:

```sql
SELECT MONTH(tanggal_pinjam) AS bulan, COUNT(*) AS jumlah_peminjaman
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025
GROUP BY MONTH(tanggal_pinjam);
```

Dengan hasil ini, Anda bisa melihat tren penggunaan perpustakaan dari bulan ke bulan. Data tren sangat berguna dalam perencanaan sumber daya dan strategi layanan. Borgman (2000) menjelaskan bahwa analisis tren penggunaan adalah kunci untuk pengembangan layanan berbasis bukti【Borgman, *From Gutenberg to the Global Information Infrastructure*】.

---

### Variasi Penggunaan COUNT

COUNT memiliki variasi yang sangat bermanfaat dalam praktik sehari-hari. Anda dapat menggunakan `COUNT(*)` untuk menghitung semua baris dalam tabel, atau `COUNT(kolom)` untuk hanya menghitung baris yang memiliki nilai pada kolom tertentu. Perbedaan ini penting karena terkadang kita hanya ingin menghitung data valid, bukan semua data. Misalnya, menghitung jumlah transaksi yang sudah memiliki tanggal pengembalian. Silberschatz et al. (2019) menekankan bahwa pemahaman variasi fungsi agregasi sangat penting agar hasil analisis tidak menyesatkan【Silberschatz, Korth, Sudarshan, *Database System Concepts*】.

Contoh penggunaan COUNT(kolom) dalam konteks perpustakaan adalah menghitung transaksi yang sudah dikembalikan. Query yang bisa digunakan adalah:

```sql
SELECT COUNT(tanggal_kembali) AS jumlah_dikembalikan
FROM peminjaman;
```

Query ini hanya akan menghitung baris yang memiliki nilai pada kolom `tanggal_kembali`. Dengan demikian, Anda bisa membedakan jumlah transaksi yang selesai dengan yang masih aktif. Hal ini sangat membantu pustakawan dalam memantau keterlambatan pengembalian buku. Connolly & Begg (2015) menyebutkan bahwa fungsi agregasi sering digunakan untuk menilai kualitas layanan dalam organisasi【Connolly & Begg, *Database Systems*】.

Selain itu, COUNT juga bisa dikombinasikan dengan `DISTINCT` untuk menghitung jumlah unik. Misalnya, menghitung jumlah anggota yang pernah meminjam buku tanpa menghitung duplikasi transaksi. Query yang digunakan adalah:

```sql
SELECT COUNT(DISTINCT id_anggota) AS anggota_meminjam
FROM peminjaman;
```

Hasil ini memberikan gambaran lebih akurat mengenai jumlah pengguna aktif. Variasi semacam ini memperluas kegunaan COUNT dalam analisis data perpustakaan. Rowley & Hartley (2018) menjelaskan bahwa menghitung jumlah unik pengguna sangat penting untuk memahami basis pengguna sesungguhnya【Rowley & Hartley, *Organizing Knowledge*】.

Terakhir, COUNT bisa dipadukan dengan klausa `WHERE` untuk memberikan filter tambahan. Misalnya, menghitung jumlah buku berdasarkan kategori atau tahun terbit tertentu. Dengan fleksibilitas ini, COUNT dapat digunakan dalam berbagai laporan manajemen. Lancaster (2003) menyebutkan bahwa laporan statistik berbasis kriteria tertentu adalah alat utama dalam manajemen perpustakaan modern【Lancaster, *If You Want to Evaluate Your Library*】.

---

### Studi Kasus: Menghitung Aktivitas Anggota

Sekarang mari kita lihat studi kasus yang lebih mendalam. Pustakawan ingin mengetahui berapa jumlah anggota yang aktif meminjam dalam bulan September 2025. Untuk itu, kita bisa menggunakan COUNT dengan DISTINCT dan filter tanggal. Query berikut bisa digunakan:

```sql
SELECT COUNT(DISTINCT id_anggota) AS anggota_aktif_september
FROM peminjaman
WHERE MONTH(tanggal_pinjam) = 9 AND YEAR(tanggal_pinjam) = 2025;
```

Hasil dari query ini akan membantu pustakawan memahami tingkat partisipasi anggota. Jika jumlahnya tinggi, berarti perpustakaan sedang ramai dan layanan berjalan baik. Jika rendah, mungkin perlu ada strategi promosi. Rowley & Hartley (2018) menegaskan bahwa analisis perilaku pengguna adalah dasar perbaikan layanan informasi【Rowley & Hartley, *Organizing Knowledge*】.

Selain menghitung keaktifan anggota, kita juga bisa menghitung jumlah peminjaman yang dilakukan oleh setiap anggota. Hal ini dapat dilakukan dengan menggabungkan COUNT dan GROUP BY. Query berikut bisa digunakan:

```sql
SELECT id_anggota, COUNT(*) AS jumlah_pinjam
FROM peminjaman
GROUP BY id_anggota;
```

Hasilnya akan menampilkan jumlah peminjaman per anggota. Dengan cara ini, pustakawan dapat mengetahui siapa anggota yang paling aktif meminjam buku. Menurut Borgman (2000), data peminjaman individu membantu perpustakaan dalam merancang layanan personalisasi【Borgman, *From Gutenberg to the Global Information Infrastructure*】.

Data semacam ini juga bisa digunakan untuk memberikan penghargaan atau insentif bagi anggota yang paling aktif. Misalnya, perpustakaan bisa memberikan hadiah kepada anggota dengan jumlah peminjaman terbanyak. Hal ini bisa meningkatkan motivasi pengguna untuk lebih sering berkunjung. Lancaster (2003) menjelaskan bahwa strategi berbasis data meningkatkan keterlibatan pengguna dalam jangka panjang【Lancaster, *If You Want to Evaluate Your Library*】.

Dengan studi kasus ini, Anda belajar bahwa COUNT tidak hanya sekadar menghitung data. Fungsi ini bisa menjadi dasar untuk pengambilan keputusan manajerial. Jadi, jangan ragu untuk berlatih dengan berbagai skenario agar semakin terampil. Elmasri & Navathe (2015) menekankan bahwa keterampilan praktik dengan query nyata adalah cara terbaik untuk menguasai SQL【Elmasri & Navathe, *Fundamentals of Database Systems*】.

---

### Studi Kasus: Menghitung Koleksi Berdasarkan Kategori

Sekarang mari kita gunakan COUNT untuk menghitung koleksi buku berdasarkan kategori. Misalnya, pustakawan ingin tahu berapa jumlah buku fiksi di perpustakaan. Query yang bisa digunakan adalah:

```sql
SELECT COUNT(*) AS jumlah_fiksi
FROM buku
WHERE kategori = 'Fiksi';
```

Hasil dari query ini akan membantu dalam evaluasi keseimbangan koleksi. Jika jumlah fiksi terlalu sedikit, perpustakaan mungkin perlu menambah koleksi tersebut. Jika jumlahnya sangat besar, perpustakaan mungkin bisa menyeimbangkan dengan koleksi non-fiksi. Lancaster (2003) menekankan bahwa keseimbangan koleksi adalah indikator penting dalam manajemen perpustakaan【Lancaster, *If You Want to Evaluate Your Library*】.

Selain menghitung berdasarkan kategori tunggal, kita juga bisa menggunakan GROUP BY untuk menghitung semua kategori sekaligus. Query berikut bisa digunakan:

```sql
SELECT kategori, COUNT(*) AS jumlah_buku
FROM buku
GROUP BY kategori;
```

Hasil ini akan menampilkan jumlah buku per kategori dalam tabel yang lebih informatif. Dengan informasi tersebut, pustakawan bisa melihat distribusi koleksi secara keseluruhan. Rowley & Hartley (2018) menjelaskan bahwa statistik distribusi koleksi mendukung strategi pengembangan yang lebih tepat sasaran【Rowley & Hartley, *Organizing Knowledge*】.

Selain kategori, COUNT juga bisa digunakan untuk menghitung jumlah buku berdasarkan tahun terbit. Dengan demikian, pustakawan bisa mengetahui seberapa banyak koleksi terbaru yang tersedia. Informasi ini penting untuk memastikan perpustakaan tetap relevan dengan kebutuhan pembaca modern. Borgman (2000) menyebutkan bahwa koleksi yang up-to-date adalah salah satu faktor utama kepuasan pengguna【Borgman, *From Gutenberg to the Global Information Infrastructure*】.

Studi kasus ini menunjukkan bahwa COUNT dapat menjadi alat statistik sederhana untuk mengelola koleksi. Dengan laporan yang jelas, pustakawan bisa membuat keputusan yang lebih berbasis data. Jadi, jangan hanya mengandalkan intuisi, tetapi gunakan query COUNT untuk memperkuat strategi pengadaan koleksi. Connolly & Begg (2015) menegaskan bahwa data-driven decision making adalah pilar utama dalam manajemen informasi modern【Connolly & Begg, *Database Systems*】.

---

## 6. Kesalahan Umum

### 6.1 Menggunakan COUNT(kolom) Tanpa Memahami NULL

**Contoh (kurang tepat – transaksi belum dikembalikan tidak terhitung):**

```sql
SELECT COUNT(tanggal_kembali) AS jumlah_dikembalikan
FROM peminjaman;
```

Query ini hanya menghitung baris yang memiliki nilai pada kolom `tanggal_kembali`. Jika ada transaksi peminjaman yang belum dikembalikan, maka nilainya NULL dan tidak masuk hitungan. Akibatnya, laporan hanya mencatat transaksi yang selesai, bukan total aktivitas peminjaman. Hal ini sering menyesatkan jika pustakawan ingin mengetahui jumlah seluruh transaksi. Menurut Silberschatz et al. (2019), kesalahan dalam memahami NULL dapat mengubah interpretasi data secara drastis【Silberschatz, Korth, Sudarshan】.

**Contoh (tepat – total transaksi apa pun statusnya):**

```sql
SELECT COUNT(*) AS total_transaksi
FROM peminjaman;
```

Query ini menghitung semua baris pada tabel `peminjaman`, tanpa memperhatikan apakah kolom tertentu bernilai NULL. Dengan cara ini, semua transaksi akan masuk dalam hitungan, baik yang sudah dikembalikan maupun yang belum. Hasil ini lebih sesuai jika pustakawan ingin mengetahui total aktivitas peminjaman tanpa memandang status. Angka yang dihasilkan mewakili volume transaksi yang tercatat di database. Connolly & Begg (2015) menyebutkan bahwa penggunaan COUNT(\*) lebih aman untuk statistik global【Connolly & Begg】.

**Contoh (menghitung transaksi yang BELUM dikembalikan secara eksplisit):**

```sql
SELECT COUNT(*) AS belum_dikembalikan
FROM peminjaman
WHERE tanggal_kembali IS NULL;
```

Query ini menggunakan klausa `WHERE` untuk hanya menghitung transaksi dengan nilai NULL pada `tanggal_kembali`. Artinya, hanya peminjaman yang masih aktif dan belum dikembalikan yang dihitung. Hasil query ini sangat bermanfaat untuk mengetahui jumlah buku yang masih dipinjam. Informasi ini membantu pustakawan memantau potensi keterlambatan pengembalian. Rowley & Hartley (2018) menekankan bahwa statistik kondisi koleksi penting untuk evaluasi layanan【Rowley & Hartley】.

---

### 6.2 Lupa Menambahkan Klausa WHERE

**Contoh (kurang tepat – menghitung semua periode):**

```sql
SELECT COUNT(*) AS peminjaman
FROM peminjaman;
```

Query ini menghitung semua baris pada tabel `peminjaman` tanpa batasan periode. Akibatnya, data dari tahun-tahun sebelumnya ikut tercampur dalam laporan. Jika tujuan laporan adalah untuk menganalisis tren tertentu, hasil ini menjadi tidak relevan. Banyak pustakawan pemula lupa menambahkan klausa penyaring sehingga laporan menjadi terlalu umum. Lancaster (2003) menegaskan pentingnya filter agar laporan sesuai dengan konteks analisis【Lancaster】.

**Contoh (tepat – fokus September 2025):**

```sql
SELECT COUNT(*) AS peminjaman_sep_2025
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025 AND MONTH(tanggal_pinjam) = 9;
```

Query ini membatasi perhitungan hanya pada transaksi yang terjadi pada September 2025. Dengan filter tahun dan bulan, data yang dihitung menjadi lebih spesifik. Hasil ini bermanfaat untuk laporan bulanan, misalnya jumlah peminjaman selama satu periode. Pustakawan dapat menggunakan informasi ini untuk mengidentifikasi pola musiman dalam penggunaan perpustakaan. Borgman (2000) menyebutkan bahwa data berbasis waktu mendukung perencanaan berbasis bukti【Borgman】.

**Contoh (alternatif berbasis rentang tanggal):**

```sql
SELECT COUNT(*) AS peminjaman_sep_2025
FROM peminjaman
WHERE tanggal_pinjam >= '2025-09-01' AND tanggal_pinjam < '2025-10-01';
```

Query ini menggunakan pendekatan rentang tanggal daripada fungsi YEAR dan MONTH. Teknik ini lebih efisien dalam beberapa kasus karena memanfaatkan indeks tanggal. Hasil yang diperoleh sama, yaitu jumlah transaksi pada September 2025. Perbedaan terletak pada fleksibilitas: pustakawan bisa menyesuaikan rentang sesuai kebutuhan analisis. Elmasri & Navathe (2015) menekankan bahwa pemilihan teknik query berpengaruh pada kinerja sistem【Elmasri & Navathe】.

---

### 6.3 Tidak Menggunakan DISTINCT Saat Menghitung Data Unik

**Contoh (kurang tepat – anggota yang sama terhitung berulang):**

```sql
SELECT COUNT(id_anggota) AS hitung_anggota
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025;
```

Query ini menghitung semua peminjaman yang dilakukan pada tahun 2025. Namun, jika satu anggota meminjam lebih dari sekali, ia akan dihitung berkali-kali. Akibatnya, hasil yang diperoleh adalah jumlah transaksi, bukan jumlah anggota unik. Hal ini menimbulkan kebingungan jika laporan dimaksudkan untuk menghitung anggota aktif. Silberschatz et al. (2019) menyatakan bahwa duplikasi adalah masalah klasik dalam analisis data【Silberschatz, Korth, Sudarshan】.

**Contoh (tepat – jumlah anggota unik yang meminjam):**

```sql
SELECT COUNT(DISTINCT id_anggota) AS anggota_aktif_2025
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025;
```

Query ini menambahkan kata kunci DISTINCT untuk memastikan setiap anggota hanya dihitung sekali. Dengan cara ini, hasil benar-benar menunjukkan jumlah anggota unik yang meminjam buku pada tahun 2025. Data ini lebih valid untuk mengukur tingkat partisipasi anggota. Informasi semacam ini sangat bermanfaat bagi pustakawan dalam menilai efektivitas layanan. Rowley & Hartley (2018) menekankan pentingnya statistik pengguna unik untuk manajemen informasi【Rowley & Hartley】.

---

### 6.4 Mengabaikan GROUP BY dalam Laporan Per Kategori

**Contoh (kurang tepat – hanya total keseluruhan):**

```sql
SELECT COUNT(*) AS total_buku
FROM buku;
```

Query ini hanya menghasilkan total jumlah seluruh buku tanpa membedakan kategori. Informasi semacam ini terlalu umum jika tujuan analisis adalah menilai distribusi koleksi. Pustakawan tidak dapat melihat apakah kategori tertentu terlalu sedikit atau terlalu banyak. Akibatnya, evaluasi koleksi menjadi kurang efektif. Lancaster (2003) menjelaskan bahwa statistik global saja tidak cukup untuk perencanaan koleksi【Lancaster】.

**Contoh (tepat – rincian per kategori):**

```sql
SELECT kategori, COUNT(*) AS jumlah_buku
FROM buku
GROUP BY kategori
ORDER BY jumlah_buku DESC;
```

Query ini menggunakan GROUP BY untuk menghitung jumlah buku pada setiap kategori. Hasilnya adalah tabel ringkas yang menampilkan distribusi koleksi berdasarkan kategori. Dengan cara ini, pustakawan bisa membandingkan jumlah buku fiksi dengan non-fiksi, atau kategori lainnya. Informasi ini mendukung perencanaan pengadaan yang lebih seimbang. Rowley & Hartley (2018) menyebutkan bahwa analisis distribusi koleksi adalah dasar pengembangan yang tepat sasaran【Rowley & Hartley】.

---

### 6.5 Menafsirkan COUNT Sebagai Data Detail

**Contoh (ringkasan saja – bukan detail):**

```sql
SELECT COUNT(*) AS total_peminjaman_2025
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025;
```

Query ini hanya menghitung jumlah total peminjaman pada tahun 2025. Hasilnya berupa angka tunggal yang tidak menunjukkan detail transaksi. Banyak pemula keliru menganggap COUNT bisa menjawab pertanyaan “siapa” atau “apa” dari data. Padahal, COUNT hanya memberi ringkasan jumlah. Borgman (2000) mengingatkan bahwa ringkasan harus dipahami sebagai bagian awal analisis, bukan akhir【Borgman】.

**Contoh (detail peminjaman per anggota):**

```sql
SELECT id_anggota, COUNT(*) AS jumlah_pinjam
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025
GROUP BY id_anggota
ORDER BY jumlah_pinjam DESC;
```

Query ini memperbaiki kekurangan dengan menambahkan GROUP BY `id_anggota`. Hasilnya menunjukkan jumlah peminjaman per anggota sepanjang tahun 2025. Dengan data ini, pustakawan bisa mengetahui siapa anggota paling aktif. Laporan ini juga dapat digunakan untuk memberikan penghargaan kepada pengguna loyal. Lancaster (2003) menyebutkan bahwa statistik personal dapat meningkatkan keterlibatan anggota【Lancaster】.

---


## 7. Best Practice

### 7.1 Memahami Perbedaan COUNT(\*) dan COUNT(kolom)

**COUNT(\*) – menghitung semua baris:**

```sql
SELECT COUNT(*) AS total_anggota
FROM anggota;
```

Query ini menghitung semua baris dalam tabel `anggota`, termasuk jika ada kolom dengan nilai NULL. Hasilnya memberikan angka total anggota yang terdaftar dalam sistem perpustakaan. Pendekatan ini sangat berguna ketika pustakawan membutuhkan statistik global. Data yang diperoleh memberikan gambaran menyeluruh tanpa pengecualian. Silberschatz et al. (2019) menekankan bahwa COUNT(\*) aman digunakan untuk laporan umum【Silberschatz, Korth, Sudarshan】.

**COUNT(kolom) – hanya nilai tidak NULL:**

```sql
SELECT COUNT(tanggal_kembali) AS transaksi_selesai
FROM peminjaman;
```

Query ini menghitung hanya transaksi peminjaman yang sudah memiliki nilai pada kolom `tanggal_kembali`. Dengan demikian, hanya transaksi selesai yang tercatat. Hal ini membantu pustakawan mengetahui berapa banyak buku yang sudah dikembalikan. Jika digunakan tanpa memahami perbedaan dengan COUNT(\*), hasilnya bisa menyesatkan. Connolly & Begg (2015) menekankan pentingnya memilih fungsi sesuai tujuan analisis【Connolly & Begg】.

**Teknik eksplisit via CASE untuk kontrol lebih halus:**

```sql
SELECT 
  SUM(CASE WHEN tanggal_kembali IS NULL THEN 1 ELSE 0 END) AS belum_kembali,
  SUM(CASE WHEN tanggal_kembali IS NOT NULL THEN 1 ELSE 0 END) AS sudah_kembali
FROM peminjaman;
```

Query ini menggunakan ekspresi CASE untuk memberikan kontrol penuh atas kondisi. Hasilnya menampilkan jumlah buku yang sudah dikembalikan dan yang masih dipinjam dalam laporan terpisah. Teknik ini bermanfaat ketika pustakawan ingin membandingkan dua kondisi dalam satu query. Dengan hasil yang lebih detail, analisis menjadi lebih kaya informasi. Elmasri & Navathe (2015) menekankan bahwa ekspresi kondisional memperluas fleksibilitas SQL【Elmasri & Navathe】.

---

### 7.2 Menggunakan WHERE untuk Analisis Terfokus

**Filter periode spesifik (bulan & tahun):**

```sql
SELECT COUNT(*) AS peminjaman_okt_2025
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025 AND MONTH(tanggal_pinjam) = 10;
```

Query ini membatasi hitungan hanya pada peminjaman yang terjadi pada Oktober 2025. Dengan filter waktu yang jelas, pustakawan bisa mengetahui tren penggunaan perpustakaan dalam periode tertentu. Informasi ini dapat digunakan untuk mengevaluasi aktivitas bulanan. Data seperti ini penting untuk perencanaan layanan rutin. Borgman (2000) menekankan bahwa laporan berbasis periode mendukung perencanaan berbasis bukti【Borgman】.

**Filter rentang tanggal (inklusif-eksklusif):**

```sql
SELECT COUNT(*) AS peminjaman_q3_2025
FROM peminjaman
WHERE tanggal_pinjam >= '2025-07-01' AND tanggal_pinjam < '2025-10-01';
```

Query ini menggunakan pendekatan rentang tanggal yang eksplisit. Dengan cara ini, pustakawan bisa menghitung peminjaman kuartal ketiga tahun 2025. Teknik ini lebih efisien pada database besar karena memanfaatkan indeks tanggal. Data yang diperoleh lebih akurat tanpa perlu memproses fungsi tambahan. Silberschatz et al. (2019) menyebutkan bahwa efisiensi query adalah faktor penting dalam analisis data【Silberschatz, Korth, Sudarshan】.

**Filter multi-kriteria (periode + kategori buku):**

```sql
SELECT COUNT(*) AS fiksi_dipinjam_sep_2025
FROM peminjaman p
JOIN buku b ON b.id_buku = p.id_buku
WHERE b.kategori = 'Fiksi'
  AND p.tanggal_pinjam >= '2025-09-01' AND p.tanggal_pinjam < '2025-10-01';
```

Query ini menggabungkan filter waktu dengan kategori buku. Hasilnya menampilkan jumlah buku fiksi yang dipinjam khusus pada September 2025. Dengan teknik ini, laporan menjadi lebih fokus dan mendalam. Pustakawan bisa memanfaatkan data tersebut untuk menilai minat baca terhadap kategori tertentu. Rowley & Hartley (2018) menekankan bahwa analisis terfokus mendukung perencanaan berbasis kebutuhan pengguna【Rowley & Hartley】.

---

### 7.3 Memanfaatkan DISTINCT untuk Data Unik

**Jumlah anggota unik yang meminjam di 2025:**

```sql
SELECT COUNT(DISTINCT id_anggota) AS anggota_aktif
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025;
```

Query ini memastikan setiap anggota hanya dihitung satu kali, meskipun mereka meminjam lebih dari sekali. Hasilnya adalah jumlah anggota aktif unik pada tahun 2025. Data ini lebih representatif daripada sekadar jumlah transaksi. Laporan semacam ini sangat penting untuk mengukur keterlibatan pengguna. Rowley & Hartley (2018) menekankan bahwa statistik pengguna unik mendukung validitas analisis【Rowley & Hartley】.

**Jumlah kombinasi unik anggota–bulan (aktivitas per periode):**

```sql
SELECT COUNT(DISTINCT CONCAT(id_anggota, '-', DATE_FORMAT(tanggal_pinjam, '%Y-%m'))) AS aktivitas_anggota_bulanan
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025;
```

Query ini menghitung aktivitas unik setiap anggota per bulan. Dengan cara ini, pustakawan bisa mengetahui seberapa sering anggota aktif sepanjang tahun. Data ini memberikan wawasan mengenai konsistensi kunjungan. Laporan ini bisa digunakan untuk menilai loyalitas pengguna. Borgman (2000) menyebutkan bahwa data perilaku pengguna penting untuk strategi personalisasi layanan【Borgman】.

**Jumlah judul unik yang dipinjam:**

```sql
SELECT COUNT(DISTINCT id_buku) AS judul_unik_dipinjam
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025;
```

Query ini menghitung jumlah judul buku berbeda yang dipinjam selama tahun 2025. Hasil ini berguna untuk mengetahui keragaman koleksi yang digunakan. Jika jumlahnya rendah, berarti hanya sedikit judul yang diminati. Jika tinggi, berarti koleksi digunakan secara merata. Lancaster (2003) menjelaskan bahwa statistik pemanfaatan koleksi adalah indikator relevansi bahan pustaka【Lancaster】.

---

### 7.4 Menggunakan GROUP BY untuk Laporan Rinci

**Jumlah buku per kategori:**

```sql
SELECT kategori, COUNT(*) AS jumlah_buku
FROM buku
GROUP BY kategori
ORDER BY jumlah_buku DESC;
```

Query ini menghasilkan laporan jumlah buku berdasarkan kategori. Dengan data ini, pustakawan dapat menilai distribusi koleksi dan menemukan kategori dominan. Informasi tersebut berguna untuk perencanaan pengadaan berikutnya. Analisis semacam ini mendukung keseimbangan koleksi sesuai minat pembaca. Rowley & Hartley (2018) menekankan pentingnya statistik terperinci untuk pengembangan koleksi【Rowley & Hartley】.

**Jumlah peminjaman per bulan 2025:**

```sql
SELECT DATE_FORMAT(tanggal_pinjam, '%Y-%m') AS periode, COUNT(*) AS jumlah
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025
GROUP BY DATE_FORMAT(tanggal_pinjam, '%Y-%m')
ORDER BY periode;
```

Query ini menampilkan jumlah peminjaman per bulan dalam tahun 2025. Dengan laporan ini, pustakawan dapat melihat pola musiman peminjaman. Informasi ini sangat berguna untuk alokasi staf dan sumber daya. Misalnya, menambah tenaga kerja pada bulan sibuk. Borgman (2000) menekankan pentingnya analisis tren penggunaan untuk perencanaan layanan【Borgman】.

**Jumlah peminjaman per anggota (top 10):**

```sql
SELECT id_anggota, COUNT(*) AS total_pinjam
FROM peminjaman
GROUP BY id_anggota
ORDER BY total_pinjam DESC
LIMIT 10;
```

Query ini menampilkan sepuluh anggota dengan jumlah peminjaman terbanyak. Data ini berguna untuk memberikan penghargaan atau program loyalitas. Dengan laporan ini, pustakawan dapat mengenali pengguna yang paling aktif. Analisis ini juga membantu menilai dampak layanan terhadap kelompok tertentu. Lancaster (2003) menjelaskan bahwa statistik individu memperkuat evaluasi layanan【Lancaster】.

---

### 7.5 Mendokumentasikan Query untuk Replikasi

**Komentari query dan simpan sebagai view (contoh laporan bulanan 2025):**

```sql
-- Laporan jumlah peminjaman per bulan untuk tahun 2025
CREATE OR REPLACE VIEW v_laporan_peminjaman_bulanan_2025 AS
SELECT DATE_FORMAT(tanggal_pinjam, '%Y-%m') AS periode, COUNT(*) AS jumlah
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025
GROUP BY DATE_FORMAT(tanggal_pinjam, '%Y-%m');
```

Query ini membuat sebuah view yang menyimpan laporan jumlah peminjaman per bulan tahun 2025. Dengan adanya view, pustakawan tidak perlu menulis ulang query setiap kali laporan dibutuhkan. Komentar pada query menjelaskan tujuan dan cara kerja perintah SQL. Praktik ini memudahkan kolaborasi antar pustakawan atau staf IT. Borgman (2000) menekankan bahwa dokumentasi memperkuat keberlanjutan sistem informasi【Borgman】.

**Gunakan kembali view untuk presentasi/ekspor:**

```sql
SELECT * FROM v_laporan_peminjaman_bulanan_2025 ORDER BY periode;
```

Query ini memanggil kembali view yang sudah dibuat untuk menampilkan data laporan. Dengan cara ini, hasil analisis bisa langsung diakses tanpa menulis query panjang. Hal ini mempercepat proses pelaporan dan meminimalkan kesalahan teknis. Pustakawan bisa langsung mengekspor hasil untuk presentasi. Connolly & Begg (2015) menyebutkan bahwa view adalah mekanisme penting untuk standarisasi laporan【Connolly & Begg】.

**Contoh dokumentasi ringkas di dalam skrip:**

```sql
-- Tujuan: Hitung anggota aktif unik selama 2025 (untuk KPI bulanan)
-- Catatan: Gunakan DISTINCT untuk menghindari duplikasi transaksi.
SELECT COUNT(DISTINCT id_anggota) AS anggota_aktif_2025
FROM peminjaman
WHERE YEAR(tanggal_pinjam) = 2025;
```

Query ini menunjukkan bagaimana dokumentasi sederhana dapat menjelaskan maksud dari analisis. Catatan di bagian atas menjelaskan tujuan, konteks, dan alasan teknis penggunaan DISTINCT. Dengan praktik ini, orang lain yang membaca skrip akan langsung memahami maksud query. Hal ini juga memudahkan audit atau perbaikan di kemudian hari. Elmasri & Navathe (2015) menekankan pentingnya dokumentasi dalam rekayasa basis data【Elmasri & Navathe】.

---

## Kesimpulan Singkat

Fungsi COUNT dalam SQL terbukti menjadi alat penting untuk menyajikan ringkasan data secara efisien dalam sistem perpustakaan. Dengan penerapan yang tepat, pustakawan dapat menghitung jumlah anggota, koleksi, maupun transaksi peminjaman dengan mudah dan akurat. Penggunaan COUNT yang dikombinasikan dengan DISTINCT, WHERE, dan GROUP BY memberikan fleksibilitas tinggi dalam analisis. Hasilnya dapat digunakan sebagai dasar pengambilan keputusan yang berbasis data dan terukur. Dengan demikian, penguasaan COUNT menjadi fondasi bagi keterampilan analisis data yang lebih kompleks dan strategis.

---

## Referensi

* Borgman, C. L. (2000). *From Gutenberg to the Global Information Infrastructure: Access to Information in the Networked World*. MIT Press.
* Connolly, T., & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management*. Pearson.
* Elmasri, R., & Navathe, S. B. (2015). *Fundamentals of Database Systems* (7th ed.). Pearson.
* Lancaster, F. W. (2003). *If You Want to Evaluate Your Library*. University of Illinois.
* Ramakrishnan, R., & Gehrke, J. (2003). *Database Management Systems* (3rd ed.). McGraw-Hill.
* Rowley, J., & Hartley, R. (2018). *Organizing Knowledge: An Introduction to Managing Access to Information* (5th ed.). Routledge.
* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2019). *Database System Concepts* (7th ed.). McGraw-Hill.
