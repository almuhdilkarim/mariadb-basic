---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Praktik Relasi Anggota dan Peminjaman"
short: "Praktik"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 6
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
      desc: "Latihan membangun relasi antar tabel"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mempraktikkan relasi antara tabel anggota dan peminjaman. Modul ini menguatkan pemahaman primary key dan foreign key melalui contoh nyata dalam sistem perpustakaan."
---


## 1. Pendahuluan

Menghubungkan tabel `anggota` dan `peminjaman` adalah salah satu langkah paling penting dalam desain database relasional untuk sistem perpustakaan. Dengan menghubungkan kedua tabel ini, kita memastikan bahwa setiap transaksi peminjaman buku memiliki kaitan yang jelas dengan anggota yang meminjamnya. Hubungan ini dibangun menggunakan konsep foreign key yang mengacu pada primary key di tabel lain. Tanpa hubungan tersebut, data transaksi bisa berdiri sendiri tanpa identitas peminjam yang valid, sehingga laporan dan analisis akan menjadi tidak konsisten. Dalam praktik ini, fokus utama adalah memahami bagaimana foreign key bekerja dalam menjaga integritas data.

Relasi antara tabel `anggota` dan `peminjaman` tidak hanya menjamin keabsahan data, tetapi juga memperkuat integrasi sistem secara keseluruhan. Sebagai contoh, setiap baris dalam tabel `peminjaman` akan selalu memiliki referensi ke baris tertentu dalam tabel `anggota`. Artinya, transaksi peminjaman tidak bisa dimasukkan jika anggota belum terdaftar. Hal ini meniru realitas dunia nyata, di mana hanya anggota resmi yang dapat meminjam buku dari perpustakaan. Dengan demikian, struktur database menjadi lebih logis dan mudah dipahami, bahkan oleh pengguna baru.

Penerapan hubungan antar tabel juga memberikan manfaat signifikan pada aspek performa dan pemeliharaan sistem. Database relasional modern, termasuk MariaDB, menggunakan indeks yang secara otomatis dibuat pada primary key. Ketika foreign key ditambahkan, query yang melibatkan join antara tabel `anggota` dan `peminjaman` menjadi lebih efisien. Pustakawan dapat menghasilkan laporan pinjaman anggota dengan cepat tanpa harus mencocokkan data secara manual. Dengan kata lain, relasi ini tidak hanya soal integritas, tetapi juga efisiensi pengolahan data.

Selain itu, desain database yang baik akan mengurangi terjadinya anomali data. Tanpa foreign key, seorang pengguna bisa saja memasukkan transaksi peminjaman dengan `id_anggota` yang tidak valid. Hal ini akan menimbulkan data yatim (*orphaned data*) yang tidak dapat ditelusuri kembali. Dengan adanya foreign key, sistem otomatis menolak input tersebut. Ini menunjukkan bahwa konsep foreign key bukan sekadar teori, tetapi juga mekanisme perlindungan data yang nyata dalam praktik sehari-hari.

Akhirnya, modul ini mengajak peserta untuk mempraktikkan pembuatan hubungan antara tabel `anggota` dan `peminjaman` secara langsung di MariaDB. Peserta akan melalui tahapan pembuatan tabel, penambahan constraint foreign key, serta pengujian dengan data valid dan tidak valid. Selain itu, modul ini juga mengupas kesalahan umum yang sering terjadi serta praktik terbaik yang sebaiknya diterapkan. Dengan pendekatan ini, peserta tidak hanya mampu menulis perintah SQL, tetapi juga memahami logika desain di baliknya.

---

## 2. Langkah-Langkah Praktik

**Langkah 1: Buat tabel `anggota`.**

```sql
CREATE TABLE anggota (
    id_anggota INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    alamat VARCHAR(200)
);
```

**Langkah 2: Buat tabel `peminjaman` dengan foreign key.**

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT AUTO_INCREMENT PRIMARY KEY,
    id_anggota INT,
    id_buku INT,
    tanggal_pinjam DATE,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

**Langkah 3: Tambahkan data anggota.**

```sql
INSERT INTO anggota (nama, alamat) VALUES
('Rani Kusuma', 'Jl. Melati No. 5'),
('Dedi Santoso', 'Jl. Mawar No. 12');
```

**Langkah 4: Masukkan transaksi peminjaman dengan anggota valid.**

```sql
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES
(1, 2, '2025-02-01'),
(2, 3, '2025-02-02');
```

**Langkah 5: Uji constraint dengan memasukkan anggota tidak valid.**

```sql
-- Gagal karena id_anggota = 99 tidak ada
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES
(99, 5, '2025-02-03');
```

---

## 3. Kesalahan Umum

### 3.1 Tidak Membuat Foreign Key Saat Membuat Tabel

Banyak pemula hanya membuat tabel `peminjaman` dengan kolom `id_anggota` tanpa menambahkan foreign key. Praktik ini membuat relasi antar tabel tidak terbentuk secara formal. Akibatnya, data yang dimasukkan ke tabel `peminjaman` tidak diverifikasi oleh sistem. Hal ini memungkinkan terjadinya data yatim, yaitu transaksi peminjaman yang tidak memiliki anggota sah. Dalam jangka panjang, praktik ini merusak integritas database dan membuat laporan sulit dipercaya.

```sql
-- Salah: tabel tanpa foreign key
CREATE TABLE peminjaman (
    id_peminjaman INT AUTO_INCREMENT PRIMARY KEY,
    id_anggota INT,
    id_buku INT,
    tanggal_pinjam DATE
);
```

Tanpa foreign key, database kehilangan mekanisme kontrol otomatis. Hal ini berarti setiap validasi harus dilakukan secara manual melalui aplikasi atau oleh administrator. Praktik semacam ini sangat tidak efisien. Data yang tidak valid dapat lolos masuk, sehingga database menjadi kurang reliabel.

Masalah ini juga memperburuk analisis data. Ketika laporan dibuat, sistem tidak dapat memastikan keakuratan relasi antara transaksi dan anggota. Hasil analisis bisa saja menampilkan transaksi dari anggota yang sebenarnya tidak ada. Dalam konteks perpustakaan, ini berarti laporan pinjaman tidak mencerminkan kenyataan.

Selain itu, kesalahan ini membuat sistem lebih sulit dirawat. Administrator harus menghabiskan waktu untuk mencari dan memperbaiki data yang salah. Hal ini menambah beban kerja yang sebenarnya bisa dihindari sejak awal dengan foreign key.

Dengan demikian, tidak membuat foreign key saat mendesain tabel adalah kesalahan mendasar yang harus dihindari. Database yang baik selalu menegakkan aturan relasi antar tabel.

---

### 3.2 Menggunakan Kolom Tidak Logis untuk Foreign Key

Kesalahan lain yang sering dilakukan adalah menggunakan kolom yang tidak logis sebagai foreign key. Misalnya, beberapa perancang database mencoba menjadikan kolom `nama` anggota sebagai foreign key di tabel `peminjaman`. Hal ini adalah kesalahan besar karena `nama` bukanlah atribut yang stabil atau unik. Dalam kasus perpustakaan, banyak anggota dapat memiliki nama yang sama, sehingga relasi akan menjadi ambigu. Kondisi ini dapat merusak integritas data dan membuat query menjadi tidak dapat diandalkan.

```sql
-- Salah: nama digunakan sebagai foreign key
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    nama VARCHAR(100),
    FOREIGN KEY (nama) REFERENCES anggota(nama)
);
```

Penggunaan kolom seperti `nama` sebagai foreign key juga memperlambat performa database. Kolom teks panjang membutuhkan lebih banyak ruang penyimpanan dan memperlambat proses pencarian. Hal ini membuat query join antar tabel berjalan lebih lambat. Dalam skala besar, beban sistem meningkat tajam. Akibatnya, sistem menjadi tidak efisien untuk digunakan sehari-hari.

Selain itu, atribut `nama` bisa berubah sewaktu-waktu. Anggota mungkin mengganti nama karena alasan pribadi atau administrasi. Jika `nama` dijadikan foreign key, maka setiap perubahan akan memengaruhi tabel `peminjaman`. Hal ini menimbulkan inkonsistensi data yang sulit diperbaiki. Dengan kata lain, pemilihan kolom ini bertentangan dengan prinsip stabilitas dalam desain database.

Relasi yang dibangun berdasarkan kolom tidak logis juga menyulitkan pemeliharaan sistem. Administrator harus melakukan validasi tambahan setiap kali terjadi perubahan data. Hal ini menambah beban kerja yang sebenarnya bisa dihindari dengan desain yang benar. Kualitas sistem pun menurun karena rentan terhadap kesalahan manusia.

Dengan demikian, penggunaan kolom tidak logis seperti `nama` untuk foreign key harus dihindari. Foreign key sebaiknya hanya merujuk pada atribut yang unik, stabil, dan konsisten sepanjang waktu, seperti primary key.

---

### 3.3 Tidak Konsisten Tipe Data Antara Foreign Key dan Primary Key

Kesalahan berikutnya adalah mendefinisikan foreign key dengan tipe data yang berbeda dari primary key yang dirujuk. Sebagai contoh, jika `id_anggota` di tabel `anggota` menggunakan tipe `INT`, tetapi di tabel `peminjaman` didefinisikan sebagai `VARCHAR`, maka relasi menjadi tidak konsisten. Ketidaksesuaian ini dapat menyebabkan error pada saat memasukkan data atau menjalankan query. Dalam banyak kasus, sistem database bahkan menolak pembuatan constraint tersebut.

```sql
-- Salah: tipe data tidak konsisten
CREATE TABLE anggota (
    id_anggota INT PRIMARY KEY,
    nama VARCHAR(100)
);

CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_anggota VARCHAR(10), -- seharusnya INT
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Ketidakkonsistenan tipe data menimbulkan berbagai masalah teknis. Misalnya, ketika data baru dimasukkan ke tabel `peminjaman`, sistem tidak dapat mengenali `id_anggota` yang merujuk ke tabel `anggota`. Hal ini membuat constraint gagal diterapkan dan data menjadi tidak valid. Pada akhirnya, database berisi catatan yang tidak bisa dipertanggungjawabkan.

Masalah ini juga berdampak pada performa query. Database harus melakukan konversi tipe data setiap kali relasi dipanggil. Konversi ini membutuhkan waktu tambahan, terutama ketika jumlah baris data sangat besar. Hasilnya, query join menjadi lambat dan membebani sistem.

Selain itu, ketidakkonsistenan ini menimbulkan kebingungan bagi pengembang dan administrator. Mereka harus mengingat bahwa format `id_anggota` berbeda antara dua tabel. Kesalahan input menjadi lebih sering terjadi. Hal ini jelas tidak sesuai dengan prinsip kesederhanaan dan kejelasan dalam desain database.

Oleh karena itu, kesalahan ini harus dihindari dengan selalu menjaga konsistensi tipe data antara primary key dan foreign key. Praktik ini akan memastikan integritas dan efisiensi sistem.

---

### 3.4 Tidak Menentukan Aksi Saat Data Anggota Dihapus

Kesalahan lain adalah tidak menentukan apa yang harus terjadi pada tabel `peminjaman` ketika baris pada tabel `anggota` dihapus. Jika anggota dihapus tanpa aturan *ON DELETE*, maka baris di tabel `peminjaman` yang merujuk padanya akan kehilangan referensi. Hal ini menciptakan data yatim yang tidak lagi memiliki keterhubungan dengan data valid. Akibatnya, integritas relasi antar tabel rusak.

```sql
-- Salah: tidak ada aturan penghapusan
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Data yatim ini membuat laporan menjadi rancu. Misalnya, laporan peminjaman bisa menampilkan transaksi dari anggota yang sudah tidak ada. Dalam konteks perpustakaan, hal ini tidak masuk akal karena buku tidak mungkin dipinjam oleh anggota fiktif. Ketidakjelasan ini merusak fungsi database sebagai alat pencatat fakta.

Selain itu, masalah ini menyulitkan proses audit. Administrator harus mencari baris-barus yang kehilangan referensi dan memperbaikinya secara manual. Hal ini memakan waktu dan berisiko terjadi kesalahan. Sistem menjadi kurang efisien dalam jangka panjang.

Ketiadaan aturan penghapusan juga menimbulkan masalah saat melakukan analisis data. Data transaksi yang tidak terhubung dengan anggota mana pun menjadi tidak berguna. Akibatnya, kualitas laporan menurun. Hal ini berdampak langsung pada proses pengambilan keputusan.

Dengan demikian, setiap foreign key harus disertai aturan aksi yang jelas ketika data referensi dihapus. Pilihan umum adalah *CASCADE*, *SET NULL*, atau *RESTRICT* tergantung kebutuhan sistem.

---

### 3.5 Mengabaikan Uji Constraint Saat Input Data

Kesalahan terakhir adalah tidak menguji constraint foreign key setelah membuat relasi. Banyak pengguna langsung memasukkan data tanpa memverifikasi apakah sistem menolak data yang tidak valid. Akibatnya, mereka baru menyadari masalah ketika data sudah banyak dan sulit diperbaiki. Praktik ini menimbulkan risiko besar terhadap integritas sistem.

```sql
-- Salah: memasukkan id_anggota yang tidak ada
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES
(50, 1, '2025-02-10');
```

Mengabaikan uji constraint membuat database berisi data yang tidak sah. Transaksi peminjaman dari anggota yang tidak ada dapat lolos masuk. Hal ini merusak validitas laporan dan mengurangi keandalan sistem. Dalam konteks perpustakaan, catatan seperti ini tidak dapat dipertanggungjawabkan.

Selain itu, masalah ini menunda deteksi error. Jika uji constraint dilakukan sejak awal, kesalahan dapat langsung diperbaiki. Namun, jika dibiarkan, data yang tidak valid menumpuk dan sulit dihapus. Hal ini meningkatkan biaya pemeliharaan database.

Praktik ini juga memperburuk pengalaman pengguna. Pustakawan atau administrator akan merasa kesulitan ketika data tidak konsisten. Mereka harus menelusuri satu per satu baris untuk menemukan sumber masalah. Sistem menjadi tidak praktis untuk digunakan.

Dengan demikian, setiap relasi foreign key harus diuji dengan data valid dan tidak valid. Uji constraint adalah langkah wajib yang tidak boleh dilewati. Praktik ini memastikan bahwa database benar-benar menegakkan aturan integritas referensial.

---

## 4. Best Practice

### 4.1 Selalu Tentukan Foreign Key di Awal

Foreign key harus ditetapkan sejak awal pembuatan tabel. Dengan cara ini, relasi antar tabel langsung ditegakkan oleh sistem database. Hal ini mencegah masuknya data yang tidak sesuai dengan referensi. Dalam konteks perpustakaan, setiap transaksi peminjaman otomatis diverifikasi terhadap data anggota. Hasilnya, database tetap konsisten sejak pertama kali digunakan.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT AUTO_INCREMENT PRIMARY KEY,
    id_anggota INT,
    id_buku INT,
    tanggal_pinjam DATE,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Menentukan foreign key sejak awal juga mengurangi beban administrasi. Administrator tidak perlu memperbaiki data yang sudah terlanjur salah karena constraint sudah mencegahnya. Hal ini membuat sistem lebih efisien dalam jangka panjang. Database menjadi lebih stabil dan mudah dipelihara.

Selain itu, foreign key yang jelas sejak awal memudahkan desainer baru memahami struktur database. Relasi antar tabel terlihat konsisten dan logis. Hal ini meningkatkan keterbacaan dan mempercepat proses pelatihan staf baru. Sistem menjadi lebih ramah pengguna bagi pengembang.

Praktik ini juga membuat sistem lebih aman. Database otomatis menolak transaksi yang melanggar aturan integritas. Hal ini mengurangi risiko kesalahan manusia. Dengan demikian, data yang masuk selalu dapat dipercaya.

Dengan menjadikan foreign key sebagai bagian awal desain, database mendapatkan fondasi kuat untuk semua transaksi. Relasi antar tabel menjadi jelas dan terjamin integritasnya.

---

### 4.2 Konsisten dengan Tipe Data

Menjaga konsistensi tipe data antara primary key dan foreign key adalah salah satu praktik terbaik dalam desain database relasional. Jika primary key pada tabel `anggota` menggunakan tipe `INT`, maka foreign key yang merujuk padanya di tabel `peminjaman` juga harus bertipe `INT`. Konsistensi ini memungkinkan sistem untuk memverifikasi referensi tanpa perlu melakukan konversi data yang membebani performa. Tanpa konsistensi, query join akan sering gagal atau menghasilkan data yang tidak sesuai.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT AUTO_INCREMENT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Konsistensi tipe data juga membantu menjaga kejelasan logika desain database. Dengan format yang sama, relasi antar tabel menjadi lebih mudah dipahami oleh semua pengembang yang bekerja pada sistem. Hal ini sangat penting dalam tim besar yang melibatkan banyak orang. Setiap developer dapat langsung mengenali struktur data tanpa kebingungan.

Selain itu, konsistensi tipe data mengurangi kemungkinan terjadinya error saat pengolahan data. Misalnya, kesalahan konversi dari teks ke angka tidak akan pernah terjadi jika tipe data selalu sama. Dengan begitu, proses input data menjadi lebih aman dan lancar. Sistem pun lebih andal dalam menangani transaksi sehari-hari.

Praktik ini juga meningkatkan kompatibilitas dengan sistem eksternal. Aplikasi lain yang terhubung dengan database tidak perlu melakukan penyesuaian tambahan untuk menangani perbedaan tipe data. Hal ini memperlancar integrasi dengan aplikasi manajemen perpustakaan atau sistem katalog daring. Dengan kata lain, konsistensi membuat sistem lebih fleksibel.

Dengan menjaga konsistensi tipe data, database menjadi lebih mudah digunakan, lebih efisien, dan lebih tahan terhadap kesalahan. Praktik sederhana ini memiliki dampak besar terhadap kualitas desain keseluruhan.

---

### 4.3 Uji Constraint dengan Data Valid dan Tidak Valid

Setelah foreign key dibuat, constraint harus diuji menggunakan data valid dan tidak valid. Uji ini memastikan bahwa sistem benar-benar menolak data yang melanggar aturan. Sebagai contoh, ketika transaksi peminjaman dimasukkan dengan `id_anggota` yang sah, sistem harus menerima data tersebut. Sebaliknya, jika transaksi menggunakan `id_anggota` yang tidak ada di tabel `anggota`, sistem harus menolak. Uji ini penting untuk membuktikan bahwa integritas referensial bekerja sebagaimana mestinya.

```sql
-- Data valid
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) 
VALUES (1, 3, '2025-02-15');

-- Data tidak valid
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) 
VALUES (99, 4, '2025-02-16');
```

Pengujian constraint memberikan keyakinan bahwa desain relasi sudah benar. Hal ini juga membantu mendeteksi kesalahan sejak awal sebelum database digunakan secara


luas. Dengan demikian, masalah dapat diperbaiki ketika jumlah data masih kecil.

Selain itu, uji constraint meningkatkan pemahaman pengguna tentang bagaimana sistem bekerja. Pustakawan atau admin database dapat melihat langsung bagaimana sistem menolak data yang tidak sesuai. Dengan pengalaman ini, mereka lebih sadar pentingnya menjaga kualitas input data.

Praktik pengujian juga membuat database lebih siap untuk digunakan dalam jangka panjang. Dengan constraint yang terverifikasi, risiko terjadinya inkonsistensi di masa depan berkurang drastis. Database pun lebih bisa diandalkan untuk mendukung laporan dan analisis data.

Dengan kata lain, uji constraint adalah langkah penting yang tidak boleh diabaikan. Tanpa pengujian, foreign key hanya menjadi aturan tertulis yang belum tentu berfungsi.

---

### 4.4 Gunakan Aturan ON DELETE atau ON UPDATE

Foreign key sebaiknya dilengkapi dengan aturan *ON DELETE* atau *ON UPDATE*. Aturan ini memberi tahu database apa yang harus dilakukan ketika data referensi berubah atau dihapus. Sebagai contoh, aturan *ON DELETE CASCADE* akan menghapus semua transaksi peminjaman ketika anggota terkait dihapus. Dengan begitu, sistem tidak meninggalkan data yatim yang tidak dapat dirujuk.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT AUTO_INCREMENT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota) ON DELETE CASCADE
);
```

Aturan ini menjaga konsistensi relasi antar tabel meskipun terjadi perubahan data. Dalam konteks perpustakaan, jika seorang anggota dihapus karena sudah tidak aktif, maka seluruh transaksi peminjamannya juga akan ikut dihapus. Hal ini membuat database tetap bersih dari data yang tidak relevan.

Selain itu, aturan ini mempermudah administrasi sistem. Administrator tidak perlu secara manual mencari dan menghapus transaksi yang terkait. Sistem secara otomatis mengurusnya sesuai dengan aturan yang telah ditentukan. Dengan cara ini, beban kerja berkurang dan risiko kesalahan manual menurun.

Penggunaan aturan juga meningkatkan transparansi dalam desain database. Semua pengguna tahu apa yang akan terjadi ketika data referensi berubah. Hal ini membuat sistem lebih dapat diprediksi dan lebih mudah digunakan.

Dengan menetapkan aturan ON DELETE atau ON UPDATE, database menjadi lebih konsisten, efisien, dan mudah dipelihara. Praktik ini seharusnya menjadi standar dalam desain database relasional modern.

---

### 4.5 Dokumentasikan Relasi Antar Tabel

Dokumentasi relasi antar tabel adalah praktik terbaik yang sering diabaikan. Padahal, dokumentasi membuat struktur database lebih mudah dipahami oleh pengembang, administrator, dan bahkan pengguna non-teknis. Relasi antar tabel, termasuk foreign key, harus dijelaskan dengan jelas. Dokumentasi dapat dibuat dalam bentuk komentar SQL atau diagram ER (*Entity Relationship*). Dengan dokumentasi, sistem menjadi lebih transparan dan mudah dipelihara.

```sql
-- Komentar SQL untuk dokumentasi relasi
COMMENT ON TABLE peminjaman IS 'Tabel transaksi peminjaman, relasi ke anggota melalui id_anggota';
```

Dokumentasi membantu ketika ada pengembang baru yang bergabung dalam tim. Mereka dapat memahami struktur database dengan cepat tanpa harus membaca seluruh kode. Hal ini mempercepat proses pembelajaran dan mengurangi risiko kesalahan.

Selain itu, dokumentasi berguna untuk audit dan evaluasi sistem. Ketika database diperiksa, auditor dapat dengan mudah melihat relasi antar tabel dan memastikan desainnya sesuai dengan kebutuhan bisnis. Hal ini meningkatkan kredibilitas sistem.

Dokumentasi juga berguna ketika sistem perlu diperluas. Dengan catatan yang jelas, pengembang tahu tabel mana yang harus diperbarui atau dihubungkan. Hal ini mencegah perubahan yang merusak relasi yang sudah ada.

Dengan demikian, dokumentasi relasi adalah investasi jangka panjang. Meskipun memerlukan waktu tambahan, manfaatnya jauh lebih besar daripada biaya yang dikeluarkan.

---

## 5. Studi Kasus Perpustakaan

Dalam sistem perpustakaan, hubungan antara tabel `anggota` dan `peminjaman` adalah inti dari pencatatan transaksi. Setiap anggota yang mendaftar akan mendapat identitas unik berupa `id_anggota`. Identitas ini digunakan sebagai referensi dalam setiap transaksi peminjaman buku. Dengan adanya foreign key, sistem memastikan bahwa tidak ada transaksi yang dapat dicatat tanpa anggota yang sah. Hal ini meniru prosedur nyata di perpustakaan, di mana hanya anggota resmi yang berhak meminjam koleksi.

```sql
CREATE TABLE anggota (
    id_anggota INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    alamat VARCHAR(200)
);

CREATE TABLE peminjaman (
    id_peminjaman INT AUTO_INCREMENT PRIMARY KEY,
    id_anggota INT,
    id_buku INT,
    tanggal_pinjam DATE,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Setelah kedua tabel dibuat, pustakawan mulai mengisi data anggota. Misalnya, dua anggota pertama adalah “Lina Wulandari” dengan alamat “Jl. Kenanga No. 8” dan “Bayu Saputra” dengan alamat “Jl. Dahlia No. 3”. Data ini dimasukkan ke tabel `anggota` menggunakan perintah `INSERT`. Dengan adanya ID unik yang dibuat otomatis, pustakawan tidak perlu mengatur nomor identitas secara manual. ID ini kemudian akan digunakan dalam tabel `peminjaman` untuk mencatat transaksi.

```sql
INSERT INTO anggota (nama, alamat) VALUES
('Lina Wulandari', 'Jl. Kenanga No. 8'),
('Bayu Saputra', 'Jl. Dahlia No. 3');
```

Selanjutnya, transaksi peminjaman mulai dimasukkan. Misalnya, Lina Wulandari (`id_anggota = 1`) meminjam buku dengan `id_buku = 10` pada tanggal 3 Februari 2025. Bayu Saputra (`id_anggota = 2`) meminjam buku dengan `id_buku = 5` pada tanggal 4 Februari 2025. Karena kedua anggota sudah tercatat di tabel `anggota`, sistem menerima data ini tanpa error. Proses ini membuktikan bagaimana foreign key menjaga agar hanya anggota valid yang dapat melakukan transaksi.

```sql
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES
(1, 10, '2025-02-03'),
(2, 5, '2025-02-04');
```

Ketika pustakawan mencoba memasukkan transaksi dengan `id_anggota` yang tidak ada, misalnya `id_anggota = 50`, sistem langsung menolak. Hal ini menunjukkan kekuatan foreign key dalam menjaga kualitas data. Dengan mekanisme ini, database perpustakaan tidak mungkin berisi transaksi dari anggota fiktif. Relasi formal antar tabel memastikan bahwa semua data transaksi dapat dilacak kembali ke identitas nyata.

```sql
-- Gagal: anggota dengan id_anggota 50 tidak ada
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES
(50, 2, '2025-02-05');
```

Hubungan ini juga mempermudah pembuatan laporan. Misalnya, pustakawan ingin melihat daftar buku yang dipinjam beserta nama anggota. Dengan query join antara tabel `anggota` dan `peminjaman`, laporan dapat dihasilkan dengan cepat. Laporan ini menunjukkan siapa yang meminjam buku apa pada tanggal tertentu. Informasi ini penting untuk manajemen koleksi dan evaluasi layanan perpustakaan.

```sql
SELECT a.nama, p.id_buku, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota;
```

---

## Kesimpulan

Menghubungkan tabel `anggota` dan `peminjaman` dengan foreign key adalah langkah penting dalam menjaga integritas data perpustakaan. Dengan relasi ini, setiap transaksi hanya dapat dilakukan oleh anggota sah, sehingga catatan tetap valid. Kesalahan umum seperti tidak membuat foreign key, menggunakan kolom tidak logis, atau membiarkan tipe data tidak konsisten harus dihindari. Praktik terbaik seperti konsistensi tipe data, pengujian constraint, dan penggunaan aturan ON DELETE membuat sistem lebih andal. Studi kasus membuktikan bahwa relasi ini adalah kebutuhan praktis yang tidak bisa diabaikan dalam pengelolaan perpustakaan.

## Referensi

* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2019). *Database System Concepts* (7th ed.). McGraw-Hill.
* Rob, P., & Coronel, C. (2009). *Database Systems: Design, Implementation, and Management* (9th ed.). Cengage Learning.
