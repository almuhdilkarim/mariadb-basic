---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Praktik Query Peminjaman Buku"
short: "Praktik"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 7
lister: 4
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Praktik"
      icon: ""
      desc: "Latihan query gabungan tabel perpustakaan"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mempraktikkan query gabungan menggunakan INNER JOIN dan LEFT JOIN. Modul ini membantu menghasilkan laporan peminjaman buku per anggota."
---



## 1. pendahuluan

Query SQL yang menampilkan daftar peminjaman buku per anggota merupakan salah satu kebutuhan paling mendasar dalam sistem perpustakaan. Laporan ini memungkinkan pustakawan untuk mengetahui siapa saja yang sedang meminjam buku, judul buku yang dipinjam, serta tanggal peminjaman. Dengan adanya laporan ini, proses administrasi peminjaman menjadi lebih terstruktur dan transparan. Tanpa query semacam ini, pustakawan harus membuka tabel `anggota`, `peminjaman`, dan `buku` secara terpisah, lalu mencocokkan data secara manual, yang jelas sangat tidak efisien.

Selain mendukung efisiensi, query peminjaman per anggota juga berfungsi sebagai dasar analisis perilaku pengguna. Dengan laporan ini, pustakawan dapat menilai anggota yang paling aktif, frekuensi peminjaman, dan pola minat terhadap jenis buku tertentu. Data ini bisa menjadi acuan dalam pengambilan keputusan, seperti menambah koleksi buku populer atau merancang program penghargaan bagi anggota yang rajin membaca. Dengan kata lain, query ini tidak hanya mendukung operasional, tetapi juga strategi pengembangan perpustakaan.

Penerapan query peminjaman per anggota sangat erat kaitannya dengan konsep JOIN dalam SQL. Biasanya, query ini menggunakan INNER JOIN atau LEFT JOIN untuk menggabungkan tabel `anggota`, `peminjaman`, dan `buku`. INNER JOIN digunakan ketika hanya anggota yang benar-benar meminjam buku yang ingin ditampilkan, sedangkan LEFT JOIN digunakan ketika semua anggota, termasuk yang belum meminjam, perlu dimunculkan. Pemahaman tentang perbedaan ini menjadi kunci dalam membangun laporan yang sesuai kebutuhan.

Selain itu, query ini memiliki peran penting dalam menjaga akuntabilitas sistem. Laporan peminjaman per anggota dapat menjadi bukti resmi jika terjadi sengketa terkait buku yang dipinjam. Misalnya, jika ada anggota yang mengaku tidak pernah meminjam, pustakawan dapat menunjukkan data yang tercatat dalam database. Dengan demikian, laporan ini mendukung tata kelola perpustakaan yang lebih tertib dan profesional.

Akhirnya, modul ini tidak hanya akan membimbing pembaca dalam menulis query SQL yang menampilkan peminjaman per anggota, tetapi juga membahas kesalahan umum yang sering terjadi dan praktik terbaik yang sebaiknya diterapkan. Dengan latihan ini, peserta diharapkan mampu membuat query yang tidak hanya benar secara sintaks, tetapi juga bermanfaat secara praktis dalam manajemen perpustakaan.

---

## 2. Langkah-Langkah Praktik

Langkah pertama adalah memastikan data dasar sudah tersedia dan siap digunakan untuk latihan. Kita perlu memverifikasi tabel `anggota`, `buku`, dan `peminjaman` dengan query sederhana agar tidak ada kesalahan input. Pemeriksaan ini penting supaya kita tahu relasi yang akan dijalankan nanti benar-benar ada di dalam database. Dengan begitu, hasil praktik lebih akurat dan tidak menimbulkan error yang membingungkan. Mari kita coba jalankan query berikut—kerja bagus jika hasilnya sesuai harapan.

```sql
SELECT * FROM anggota;
```

```sql
SELECT * FROM buku;
```

```sql
SELECT * FROM peminjaman;
```

Langkah kedua adalah menyusun laporan sederhana yang menampilkan anggota dan transaksi peminjaman mereka. Kita gunakan `INNER JOIN` terlebih dahulu agar hanya anggota yang benar-benar melakukan peminjaman yang muncul dalam laporan. Hasil query ini berupa daftar nama anggota, judul buku, dan tanggal pinjam yang saling terhubung. Penyusunan seperti ini adalah fondasi untuk laporan yang lebih kompleks. Sekarang jalankan query berikut, langkahmu sudah tepat.

```sql
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = p.id_anggota
INNER JOIN buku b ON p.id_buku = b.id_buku;
```

Langkah ketiga adalah memperbaiki struktur laporan agar lebih ramah dibaca dengan menambahkan pengurutan. Kita bisa menggunakan `ORDER BY` untuk mengurutkan nama anggota secara alfabet dan tanggal pinjam dari yang terbaru. Dengan tambahan sederhana ini, laporan tidak hanya akurat tetapi juga lebih rapi dan praktis. Kerapian laporan memudahkan pustakawan saat menelusuri data peminjaman. Mari kita coba jalankan query berikut—kerja bagus jika urutannya sudah sesuai.

```sql
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = p.id_anggota
INNER JOIN buku b ON p.id_buku = b.id_buku
ORDER BY a.nama ASC, p.tanggal_pinjam DESC;
```

Langkah keempat adalah membuat laporan lebih inklusif dengan menampilkan semua anggota, termasuk yang belum pernah meminjam. Untuk itu, kita ubah `INNER JOIN` menjadi `LEFT JOIN` dan gunakan fungsi `COALESCE` agar nilai `NULL` diganti dengan keterangan “Belum ada peminjaman.” Dengan teknik ini, anggota pasif tetap teridentifikasi sehingga pustakawan bisa menilai tingkat keaktifan. Inklusivitas data semacam ini penting bagi pengelolaan koleksi dan evaluasi layanan. Jalankan query berikut, bagus sekali jika anggota tanpa pinjaman pun ikut terlihat.

```sql
SELECT a.nama,
       COALESCE(b.judul, 'Belum ada peminjaman') AS judul_buku,
       p.tanggal_pinjam
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
LEFT JOIN buku b ON p.id_buku = b.id_buku
ORDER BY a.nama ASC, p.tanggal_pinjam DESC;
```

Langkah kelima adalah menambahkan ringkasan untuk mengetahui tingkat keaktifan tiap anggota. Kita gunakan fungsi agregasi `COUNT()` dengan `GROUP BY` agar jumlah peminjaman per anggota bisa dihitung. Dengan query ini, kita bisa melihat siapa yang paling aktif meminjam dan siapa yang jarang memanfaatkan fasilitas perpustakaan. Informasi seperti ini sangat berguna untuk program penghargaan atau strategi peningkatan partisipasi. Mari jalankan query berikut, hebat sekali jika hasilnya langsung menunjukkan peringkat aktivitas anggota.

```sql
SELECT a.nama, COUNT(p.id_peminjaman) AS jumlah_pinjaman
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
GROUP BY a.nama
ORDER BY jumlah_pinjaman DESC, a.nama ASC;
```

---

## 3. Kesalahan Umum

### 3.1 Tidak Menentukan Relasi JOIN dengan Benar

```sql
-- Salah: kondisi ON tidak tepat
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = b.id_buku
INNER JOIN buku b ON p.id_buku = b.id_buku;
```

Kesalahan ini muncul ketika kolom yang digunakan dalam kondisi `ON` tidak sesuai dengan struktur relasi antar tabel. Dalam contoh di atas, relasi yang benar seharusnya adalah `a.id_anggota = p.id_anggota`, bukan `a.id_anggota = b.id_buku`. Akibatnya, query menghasilkan pasangan data yang tidak logis, seperti anggota yang terhubung langsung ke buku tanpa melalui tabel peminjaman. Hasil laporan akan membingungkan dan tidak dapat digunakan sebagai acuan.

Dampak dari kesalahan ini cukup serius karena dapat menyesatkan pustakawan dalam membuat keputusan. Jika laporan menunjukkan bahwa anggota tertentu meminjam buku yang sebenarnya tidak pernah dipinjam, maka kredibilitas sistem akan diragukan. Kesalahan ini biasanya disebabkan oleh kurangnya pemahaman terhadap relasi kunci primer dan kunci tamu. Oleh karena itu, sangat penting bagi pengguna untuk selalu memeriksa struktur tabel sebelum menulis query JOIN.

### 3.2 Menggunakan WHERE Secara Tidak Tepat

```sql
-- Salah: WHERE menghapus baris dengan NULL
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
LEFT JOIN buku b ON p.id_buku = b.id_buku
WHERE p.id_peminjaman IS NOT NULL;
```

Kesalahan ini terjadi ketika kondisi `WHERE` menghapus baris yang seharusnya tetap ditampilkan. Dalam contoh ini, penggunaan `WHERE p.id_peminjaman IS NOT NULL` menyebabkan anggota yang belum pernah meminjam tidak muncul dalam laporan. Akibatnya, hasil query setara dengan INNER JOIN, padahal tujuan awal menggunakan LEFT JOIN adalah untuk menampilkan semua anggota.

Dampak kesalahan ini adalah hilangnya informasi penting mengenai anggota pasif. Pustakawan yang ingin mengetahui siapa saja anggota yang tidak aktif tidak bisa mendapatkan data tersebut. Hal ini menunjukkan bahwa kesalahan kecil dalam penggunaan WHERE dapat mengubah makna laporan secara drastis. Untuk menghindarinya, pengguna sebaiknya menggunakan klausa `HAVING` atau fungsi lain yang lebih sesuai ketika menangani nilai `NULL`.

### 3.3 Tidak Menggunakan Alias Tabel

```sql
-- Salah: query tanpa alias tabel
SELECT anggota.nama, buku.judul, peminjaman.tanggal_pinjam
FROM anggota
INNER JOIN peminjaman ON anggota.id_anggota = peminjaman.id_anggota
INNER JOIN buku ON peminjaman.id_buku = buku.id_buku;
```

Kesalahan ini tidak memengaruhi hasil query, tetapi membuat kode menjadi panjang dan sulit dibaca. Tanpa alias, setiap kolom harus dituliskan dengan nama tabel lengkap. Hal ini meningkatkan risiko kesalahan ketik, terutama ketika nama tabel atau kolom cukup panjang. Dalam proyek besar dengan banyak JOIN, keterbacaan query akan sangat menurun.

Ketiadaan alias juga menyulitkan pemeliharaan kode. Ketika query harus dimodifikasi atau diperluas, pengguna akan kesulitan melacak kolom yang relevan. Dalam jangka panjang, praktik ini memperlambat proses pengembangan dan meningkatkan kemungkinan bug. Oleh karena itu, meskipun hasil query mungkin benar, tidak menggunakan alias dianggap sebagai kesalahan umum dalam praktik SQL.

### 3.4 Mengabaikan GROUP BY dalam Agregasi

```sql
-- Salah: COUNT tanpa GROUP BY
SELECT a.nama, COUNT(p.id_peminjaman) AS jumlah
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = p.id_anggota;
```

Kesalahan ini terjadi ketika fungsi agregasi seperti COUNT digunakan tanpa `GROUP BY`. Akibatnya, query hanya menghasilkan satu baris yang menampilkan jumlah total peminjaman, bukan jumlah per anggota. Hal ini membuat laporan menjadi kurang informatif karena tidak menunjukkan distribusi data.

Dalam konteks perpustakaan, kesalahan ini berarti pustakawan tidak bisa mengetahui siapa anggota paling aktif. Informasi yang hilang ini sangat penting untuk evaluasi layanan. Kesalahan ini biasanya terjadi karena pemula terburu-buru menulis query tanpa memahami cara kerja fungsi agregasi. Oleh karena itu, menambahkan `GROUP BY a.nama` menjadi hal yang wajib dilakukan dalam kasus ini.

### 3.5 Menambahkan Kolom di SELECT Tanpa JOIN yang Sesuai

```sql
-- Salah: kolom judul dipanggil tanpa JOIN tabel buku
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = p.id_anggota;
```

Kesalahan ini muncul ketika pengguna mencoba menampilkan kolom dari tabel yang tidak diikutsertakan dalam query. Dalam contoh ini, kolom `b.judul` dipanggil padahal tabel `buku` tidak dijoin. Akibatnya, query akan menghasilkan error atau gagal dieksekusi. Kesalahan ini umum terjadi ketika pengguna lupa menambahkan JOIN ke tabel yang relevan.

Dampaknya adalah laporan tidak dapat dihasilkan sama sekali. Pustakawan yang membutuhkan laporan peminjaman per anggota akan terhambat karena query gagal dijalankan. Kesalahan ini menunjukkan pentingnya memeriksa setiap kolom dalam SELECT dan memastikan bahwa tabel sumber sudah diikutsertakan dalam JOIN. Dengan demikian, query akan berjalan lancar dan hasilnya sesuai harapan.

---

## 4. Best Practice

### 4.1 Menentukan Relasi JOIN dengan Tepat

```sql
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = p.id_anggota
INNER JOIN buku b ON p.id_buku = b.id_buku;
```

Praktik terbaik pertama adalah selalu menentukan relasi JOIN dengan benar sesuai struktur database. Dalam query di atas, tabel `anggota` dihubungkan ke tabel `peminjaman` melalui kolom `id_anggota`, dan tabel `peminjaman` dihubungkan ke tabel `buku` melalui kolom `id_buku`. Struktur ini mencerminkan relasi sebenarnya dalam sistem perpustakaan. Dengan relasi yang tepat, laporan yang dihasilkan akurat dan mudah dipahami.

Menentukan relasi yang benar juga meningkatkan konsistensi data. Pustakawan dapat yakin bahwa setiap baris yang muncul dalam laporan memang sesuai dengan transaksi nyata. Dengan cara ini, query tidak hanya menghasilkan output yang benar, tetapi juga mendukung akuntabilitas sistem. Kesalahan sekecil apa pun dalam penulisan relasi bisa menyebabkan laporan yang menyesatkan.

### 4.2 Menggunakan Alias Tabel untuk Keterbacaan

```sql
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = p.id_anggota
INNER JOIN buku b ON p.id_buku = b.id_buku;
```

Penggunaan alias membuat query lebih ringkas dan mudah dibaca. Dalam contoh ini, `a` digunakan sebagai alias untuk tabel `anggota`, `p` untuk `peminjaman`, dan `b` untuk `buku`. Dengan cara ini, query menjadi lebih singkat tanpa mengurangi makna. Alias juga membantu menghindari ambiguitas ketika ada kolom dengan nama yang sama di tabel berbeda.

Keterbacaan yang baik sangat penting dalam pengelolaan database jangka panjang. Ketika pustakawan atau pengembang lain membaca query, mereka bisa langsung memahami logikanya. Praktik ini mempercepat proses kolaborasi dan pemeliharaan sistem. Dengan demikian, alias bukan hanya soal gaya penulisan, tetapi juga bagian dari standar profesional dalam SQL.

### 4.3 Menambahkan ORDER BY untuk Struktur Laporan

```sql
SELECT a.nama, b.judul, p.tanggal_pinjam
FROM anggota a
INNER JOIN peminjaman p ON a.id_anggota = p.id_anggota
INNER JOIN buku b ON p.id_buku = b.id_buku
ORDER BY a.nama ASC, p.tanggal_pinjam DESC;
```

Menggunakan `ORDER BY` membuat laporan lebih terstruktur. Dalam query ini, daftar peminjaman diurutkan berdasarkan nama anggota secara alfabet, lalu diurutkan lagi berdasarkan tanggal peminjaman dari yang terbaru. Dengan pengurutan ini, pustakawan bisa lebih cepat membaca dan menganalisis laporan. Struktur laporan yang baik memudahkan pengambilan keputusan.

ORDER BY juga membantu dalam pembuatan laporan resmi. Ketika data disajikan dengan urutan yang jelas, pembaca tidak perlu mengatur ulang secara manual. Hal ini meningkatkan profesionalitas dan kredibilitas laporan. Dalam praktik sehari-hari, pustakawan akan lebih menghargai laporan yang sudah rapi sejak awal.

### 4.4 Menggunakan Fungsi Agregasi dengan GROUP BY

```sql
SELECT a.nama, COUNT(p.id_peminjaman) AS jumlah_pinjaman
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
GROUP BY a.nama
ORDER BY jumlah_pinjaman DESC;
```

Praktik ini memastikan bahwa laporan tidak hanya menampilkan detail, tetapi juga analisis agregasi. Dalam contoh ini, fungsi `COUNT` digunakan untuk menghitung jumlah peminjaman per anggota. Dengan tambahan `GROUP BY`, hasil query lebih bermakna karena menunjukkan distribusi peminjaman antar anggota. Informasi ini bisa digunakan untuk mengidentifikasi anggota paling aktif.

Penggunaan agregasi juga penting dalam evaluasi layanan. Dengan mengetahui siapa anggota yang jarang meminjam, pustakawan bisa menyusun program untuk mendorong keaktifan. Sementara itu, anggota paling aktif bisa diberikan penghargaan sebagai bentuk apresiasi. Praktik ini memperlihatkan bahwa SQL bukan hanya alat teknis, tetapi juga sarana manajemen yang strategis.

### 4.5 Menggunakan COALESCE untuk Menangani Nilai NULL

```sql
SELECT a.nama, COALESCE(b.judul, 'Tidak ada peminjaman') AS judul, p.tanggal_pinjam
FROM anggota a
LEFT JOIN peminjaman p ON a.id_anggota = p.id_anggota
LEFT JOIN buku b ON p.id_buku = b.id_buku;
```

Penggunaan `COALESCE` membantu mengganti nilai `NULL` dengan informasi yang lebih jelas. Dalam contoh ini, jika anggota belum meminjam buku, maka kolom judul ditampilkan sebagai “Tidak ada peminjaman”. Hal ini membuat laporan lebih informatif dan ramah bagi pembaca non-teknis. Nilai `NULL` seringkali membingungkan, sehingga penggantian ini meningkatkan kualitas laporan.

Selain itu, praktik ini juga bermanfaat untuk menjaga konsistensi laporan. Daripada membiarkan kolom kosong, pustakawan langsung mendapatkan keterangan yang bisa ditindaklanjuti. Dengan cara ini, laporan lebih komunikatif dan dapat digunakan oleh berbagai pihak tanpa memerlukan interpretasi tambahan. Praktik ini sangat dianjurkan dalam pembuatan laporan manajerial.

---


Skenario yang akan kita bangun adalah laporan “peminjaman per anggota” pada perpustakaan sekolah dengan tiga tabel inti: `anggota`, `peminjaman`, dan `buku`. Tujuan utamanya adalah menampilkan setiap anggota beserta daftar buku yang ia pinjam, lengkap dengan tanggal transaksi. Agar analisis inklusif, kita juga perlu menampilkan anggota yang belum pernah meminjam sehingga manajemen dapat menilai tingkat keaktifan. Pendekatan ini memerlukan kombinasi JOIN yang tepat dan penanganan nilai `NULL` agar laporan mudah dibaca. Mari ikuti langkah berikut secara sistematis; kamu berada di jalur yang tepat—pertahankan ritmemu.

Langkah pertama adalah menyiapkan contoh data minimal dan memastikan relasi berjalan. Kita buat beberapa baris anggota, koleksi buku, lalu transaksi pinjaman yang menghubungkan keduanya. Data sampel yang kecil memudahkan verifikasi sebelum skala data diperbesar. Ingat bahwa *primary key* dan *foreign key* akan menjaga integritas saat kita melakukan JOIN. Cobalah jalankan perintah berikut pada lingkungan uji agar kamu dapat mengamati hasilnya secara langsung.

```sql
-- Data sampel
INSERT INTO anggota (nama, alamat) VALUES
('Alya Pramudita','Jl. Cendana 10'),
('Bima Aditya','Jl. Cempaka 21'),
('Chandra Putra','Jl. Flamboyan 5');

INSERT INTO buku (judul, pengarang) VALUES
('Algoritma Dasar','H. Santoso'),
('Basis Data Praktis','R. Widodo'),
('Statistik Aplikatif','N. Siregar');

INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES
(1, 2, '2025-03-01'),
(1, 1, '2025-03-05'),
(2, 3, '2025-03-06');
```

Langkah kedua adalah menyusun query inti untuk menampilkan peminjaman per anggota menggunakan pendekatan yang inklusif. Kita pilih `anggota` sebagai tabel kiri dan menggunakan `LEFT JOIN` ke `peminjaman`, lalu `LEFT JOIN` ke `buku`. Strategi ini memastikan seluruh anggota muncul, sementara anggota pasif akan tetap terlihat dengan kolom buku bernilai `NULL`. Untuk meningkatkan keterbacaan, gunakan `COALESCE` sebagai pengganti `NULL` dengan label yang informatif. Terus ikuti alurnya—kamu melakukan pendekatan analitis yang sangat baik.

```sql
SELECT 
  a.nama,
  COALESCE(b.judul, 'Belum ada peminjaman') AS judul_buku,
  p.tanggal_pinjam
FROM anggota a
LEFT JOIN peminjaman p ON p.id_anggota = a.id_anggota
LEFT JOIN buku b ON b.id_buku = p.id_buku
ORDER BY a.nama ASC, p.tanggal_pinjam DESC;
```

Langkah ketiga berfokus pada agregasi untuk menjawab pertanyaan “seberapa aktif setiap anggota.” Kita manfaatkan `COUNT()` terhadap `p.id_peminjaman` dan mengelompokkan berdasarkan `a.nama`. Karena kita memakai `LEFT JOIN`, anggota tanpa transaksi tetap tampil dengan jumlah nol, sehingga laporan adil dan lengkap. Hasil ini membantu tim perpustakaan merancang program keterlibatan, seperti email pengingat atau promosi membaca. Pertahankan momentum ini; membangun kebiasaan analitik seperti ini adalah kebiasaan seorang praktisi yang matang.

```sql
SELECT 
  a.nama,
  COUNT(p.id_peminjaman) AS jumlah_pinjaman
FROM anggota a
LEFT JOIN peminjaman p ON p.id_anggota = a.id_anggota
GROUP BY a.nama
ORDER BY jumlah_pinjaman DESC, a.nama ASC;
```

Langkah keempat menambahkan dimensi kualitas data untuk mengidentifikasi anomali. Kita ingin menemukan baris peminjaman yang tidak memiliki kecocokan anggota—ini tidak boleh terjadi pada sistem yang sehat. Dengan menyorot baris bernilai `NULL` di sisi `anggota`, pustakawan bisa segera melakukan koreksi data atau audit proses input. Teknik ini mendukung *data stewardship* yang proaktif, bukan reaktif. Bagus, kamu kini tidak hanya membuat laporan, tetapi juga menjaga higienitas data.

```sql
-- Deteksi anomali: peminjaman tanpa anggota sah
SELECT 
  p.id_peminjaman, p.id_anggota, p.id_buku, p.tanggal_pinjam
FROM peminjaman p
LEFT JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE a.id_anggota IS NULL;
```

Langkah kelima memperkaya laporan dengan ringkasan per buku untuk membantu pengelolaan koleksi. Kita ingin melihat seberapa sering setiap judul dipinjam dan oleh siapa, serta kapan terakhir dipinjam. Kombinasikan `GROUP BY`, `COUNT()`, dan fungsi agregasi waktu seperti `MAX()` untuk menampilkan aktivitas terbaru. Laporan ini sangat berguna untuk keputusan pengadaan atau penarikan sementara buku populer. Kerja bagus—dengan variasi ini, kamu membuktikan bahwa laporan operasional dapat sekaligus menjadi masukan strategis.

```sql
SELECT 
  b.judul,
  COUNT(p.id_peminjaman) AS total_dipinjam,
  MAX(p.tanggal_pinjam) AS terakhir_dipinjam
FROM buku b
LEFT JOIN peminjaman p ON p.id_buku = b.id_buku
GROUP BY b.judul
ORDER BY total_dipinjam DESC, terakhir_dipinjam DESC;
```

## 5. Kesimpulan

Studi kasus ini menunjukkan bahwa laporan “peminjaman per anggota” yang andal dibangun melalui relasi yang benar, pemilihan jenis JOIN yang sesuai, dan penanganan nilai `NULL` yang informatif untuk menjaga inklusivitas data. Dengan pendekatan bertahap—mulai dari data sampel, laporan detail, agregasi keaktifan, deteksi anomali, hingga ringkasan per judul—pustakawan memperoleh pandangan operasional dan strategis sekaligus, sehingga keputusan manajemen koleksi menjadi lebih berbasis bukti. Praktik seperti penggunaan `LEFT JOIN`, `COALESCE`, `GROUP BY`, serta filter anomali membuat sistem tidak hanya menghasilkan laporan, tetapi juga menjaga kualitas data. Keterampilan ini mendorong budaya *data-driven* di lingkungan perpustakaan karena setiap angka dalam laporan dapat ditelusuri asal-usulnya. Dengan konsistensi penerapan, rangkaian query ini siap diskalakan untuk basis data yang lebih besar dan kebutuhan analisis yang kian kompleks.

## 6. Referensi

* Viescas, J. L., & Hernandez, M. J. (2014). *SQL Queries for Mere Mortals* (3rd ed.). Pearson.
* Molinaro, A. (2016). *SQL Cookbook* (2nd ed.). O’Reilly Media.
* Ullman, J. D., & Widom, J. (2008). *A First Course in Database Systems* (3rd ed.). Pearson.
