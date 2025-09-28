---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Konsep Foreign Key"
short: "ForeignKey"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 6
lister: 4
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Dasar"
      icon: ""
      desc: "Menghubungkan tabel dengan kunci asing"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Pelajari foreign key sebagai kunci penghubung antar tabel. Modul ini membantu memahami bagaimana relasi antar data dibangun untuk menjaga konsistensi informasi."
---

## Pendahuluan

Foreign key adalah konsep penting dalam sistem manajemen basis data relasional yang berfungsi untuk menghubungkan dua tabel atau lebih secara formal. Jika primary key memberikan identitas unik pada sebuah tabel, maka foreign key berperan sebagai penghubung yang mengaitkan tabel satu dengan tabel lainnya. Dalam praktik perpustakaan, foreign key memastikan bahwa transaksi peminjaman buku hanya dapat dilakukan oleh anggota yang valid dan terhadap buku yang memang tercatat dalam koleksi. Tanpa foreign key, integritas relasi antar data akan rapuh dan berisiko menimbulkan inkonsistensi.

Secara teknis, foreign key adalah sebuah kolom atau kombinasi kolom dalam satu tabel yang merujuk ke primary key pada tabel lain. Dengan mekanisme ini, sistem database mampu menegakkan aturan bahwa data yang dimasukkan harus sesuai dengan referensi yang sah. Misalnya, tabel `peminjaman` memiliki kolom `id_anggota` yang merujuk ke `id_anggota` pada tabel `anggota`. Aturan ini memastikan bahwa hanya anggota yang sudah terdaftar dalam sistem yang dapat melakukan peminjaman buku. Tanpa aturan ini, data bisa saja berisi transaksi dari anggota fiktif yang tidak pernah ada.

Keberadaan foreign key juga memperkuat konsistensi logis dalam sistem informasi. Sebagai contoh, jika sebuah buku dihapus dari tabel `buku`, maka sistem akan memeriksa apakah buku tersebut masih tercatat dalam transaksi peminjaman. Jika iya, penghapusan tidak akan diizinkan sampai relasi tersebut diselesaikan. Aturan ini mencegah hilangnya data penting dan menjaga kesinambungan catatan. Dengan kata lain, foreign key bertindak sebagai penjaga hubungan antar tabel.

Selain menjaga konsistensi, foreign key juga memudahkan dalam pembuatan laporan yang kompleks. Misalnya, pustakawan dapat dengan cepat membuat laporan jumlah peminjaman per anggota atau daftar buku yang dipinjam dalam rentang waktu tertentu. Semua ini dimungkinkan karena foreign key menyambungkan tabel `anggota`, `buku`, dan `peminjaman` dalam satu kerangka yang koheren. Tanpa foreign key, laporan semacam ini akan membutuhkan pencocokan manual yang rawan kesalahan.

Dengan memahami konsep foreign key, peserta kursus dapat merancang database yang lebih terstruktur dan terjamin integritasnya. Modul ini akan menjelaskan syarat-syarat foreign key, kesalahan umum yang sering terjadi dalam penerapannya, praktik terbaik untuk menjaga konsistensi data, serta studi kasus dalam sistem perpustakaan. Tujuannya adalah agar peserta tidak hanya mengenal foreign key secara teoretis, tetapi juga dapat menggunakannya secara efektif dalam praktik nyata.

## Syarat Foreign Key

Syarat pertama foreign key adalah kolom yang dijadikan referensi harus merujuk ke primary key atau setidaknya ke kolom dengan constraint unik di tabel lain. Aturan ini menjamin bahwa nilai yang dirujuk selalu unik dan valid. Sebagai contoh, `id_anggota` dalam tabel `peminjaman` harus mengacu pada `id_anggota` yang merupakan primary key di tabel `anggota`. Dengan cara ini, setiap peminjaman pasti terkait dengan satu anggota yang sah, sehingga data tidak pernah menggantung tanpa referensi yang jelas.

Syarat kedua adalah konsistensi tipe data. Foreign key harus memiliki tipe data yang sama dengan primary key yang dirujuk, baik dari segi jenis maupun panjang data. Jika primary key bertipe `INT`, maka foreign key juga harus `INT`. Jika tidak konsisten, sistem bisa menolak pembuatan relasi atau menyebabkan kesalahan ketika data dimasukkan. Dalam sistem perpustakaan, hal ini berarti `id_buku` di tabel `peminjaman` harus memiliki tipe data yang sama dengan `id_buku` di tabel `buku`.

Syarat ketiga adalah keberadaan data yang dirujuk. Database biasanya menerapkan aturan *referential integrity* untuk memastikan nilai foreign key hanya bisa diisi dengan data yang sudah ada pada tabel referensi. Misalnya, pustakawan tidak bisa memasukkan transaksi peminjaman untuk anggota dengan `id_anggota = 99` jika anggota tersebut belum terdaftar di tabel `anggota`. Aturan ini mencegah masuknya data fiktif yang merusak integritas catatan perpustakaan.

Syarat keempat adalah aturan terkait penghapusan atau perubahan data. Jika sebuah baris pada tabel referensi dihapus, maka sistem harus menentukan apa yang terjadi pada baris-baris di tabel yang merujuknya. Pilihan umum adalah *CASCADE* (menghapus baris terkait), *SET NULL* (mengosongkan nilai foreign key), atau *RESTRICT* (menolak penghapusan jika masih ada referensi). Misalnya, jika anggota dihapus dari tabel `anggota`, maka database harus jelas bagaimana memperlakukan transaksi peminjaman yang dimilikinya.

Syarat kelima adalah foreign key harus digunakan dengan desain yang logis dan relevan. Tidak semua kolom layak dijadikan foreign key, karena hanya atribut yang benar-benar membangun hubungan antar entitas yang pantas dipilih. Dalam konteks perpustakaan, `id_anggota` pada tabel `peminjaman` jelas relevan, tetapi atribut seperti `alamat` anggota tidak pantas dijadikan foreign key. Pemahaman konteks ini penting agar relasi antar tabel mencerminkan kenyataan dunia nyata yang diwakilinya.

---

## 3. Kesalahan Umum

### 3.1 Membuat Foreign Key Tanpa Primary Key di Tabel Referensi

Kesalahan paling umum adalah mencoba membuat foreign key yang merujuk ke kolom yang bukan primary key atau tidak memiliki constraint unik. Hal ini menyebabkan relasi tidak memiliki jaminan integritas karena nilai yang dirujuk bisa duplikat. Misalnya, jika foreign key `id_anggota` dalam tabel `peminjaman` merujuk ke kolom `nama` pada tabel `anggota`, maka sistem tidak bisa menjamin keunikan karena banyak anggota bisa memiliki nama yang sama. Kesalahan ini membuat hubungan antar tabel tidak valid dan sulit dipelihara.

```sql
-- Salah: foreign key merujuk ke kolom tanpa primary key
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    nama VARCHAR(100),
    FOREIGN KEY (nama) REFERENCES anggota(nama)
);
```

Selain itu, praktik ini menyebabkan kerumitan dalam query. Sistem harus memeriksa setiap nama yang mungkin sama, sehingga relasi kehilangan kejelasan. Akibatnya, laporan transaksi peminjaman menjadi tidak akurat. Dalam sistem perpustakaan, hal ini dapat menimbulkan kebingungan ketika dua anggota dengan nama sama melakukan peminjaman.

Masalah ini juga memperburuk performa database. Tanpa primary key di kolom referensi, sistem tidak dapat membuat indeks yang efisien. Query yang melibatkan join antar tabel akan berjalan lambat. Dengan demikian, desain yang salah ini menimbulkan masalah baik dari sisi integritas maupun performa.

Oleh karena itu, foreign key hanya boleh merujuk ke kolom yang benar-benar unik, biasanya primary key. Kesalahan ini harus dihindari sejak awal desain agar sistem tetap konsisten dan efisien.

---

### 3.2 Tidak Konsisten Tipe Data Antara Primary Key dan Foreign Key

Kesalahan lain adalah menggunakan tipe data yang tidak konsisten antara primary key dan foreign key. Misalnya, primary key bertipe `INT` sementara foreign key bertipe `VARCHAR`. Meskipun beberapa sistem mungkin mengizinkan, hal ini berbahaya karena bisa menimbulkan error saat query dijalankan. Dalam konteks perpustakaan, hal ini dapat menyebabkan transaksi peminjaman gagal diproses.

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

Ketidakkonsistenan ini membuat sistem sulit menjaga integritas. Misalnya, nilai `id_anggota` “001” dalam bentuk teks mungkin tidak dikenali sebagai sama dengan angka 1 dalam bentuk integer. Hal ini bisa membuat relasi gagal, meskipun secara logika merujuk ke entitas yang sama.

Masalah lain adalah pada performa query. Sistem harus melakukan konversi data setiap kali relasi dipanggil. Konversi ini menambah beban pemrosesan, terutama jika tabel berisi ribuan baris.

Kesalahan ini dapat menyebabkan kegagalan ketika data baru dimasukkan. Misalnya, pustakawan memasukkan transaksi peminjaman dengan `id_anggota = "10"`, tetapi sistem tidak dapat menemukan anggota dengan `id_anggota = 10` karena perbedaan tipe data. Akibatnya, data menjadi tidak konsisten.

Dengan demikian, kesalahan ini merusak baik integritas maupun efisiensi sistem. Desainer database harus selalu memastikan tipe data konsisten antara primary key dan foreign key.

---

### 3.3 Memasukkan Data yang Tidak Ada di Tabel Referensi

Kesalahan umum lainnya adalah memasukkan nilai foreign key yang tidak ada di tabel referensi. Misalnya, mencatat transaksi peminjaman untuk `id_anggota = 50` padahal anggota tersebut belum terdaftar. Kesalahan ini dikenal sebagai pelanggaran referential integrity. Sistem modern biasanya mencegah hal ini, tetapi jika constraint tidak diterapkan, data palsu dapat masuk.

```sql
-- Salah: memasukkan data yang tidak ada di tabel referensi
INSERT INTO peminjaman (id_peminjaman, id_anggota, id_buku, tanggal_pinjam)
VALUES (1, 50, 3, '2025-01-20'); -- id_anggota 50 tidak ada
```

Pelanggaran ini membuat data menjadi tidak dapat dipercaya. Laporan peminjaman bisa menunjukkan anggota yang sebenarnya tidak terdaftar. Dalam perpustakaan, ini berarti ada transaksi yang terkait dengan orang fiktif. Hal ini merusak validitas seluruh sistem.

Selain itu, kesalahan ini menimbulkan masalah ketika melakukan join antar tabel. Query yang mencoba menampilkan data peminjaman beserta nama anggota akan gagal atau menghasilkan nilai kosong. Data yang tidak konsisten seperti ini membuat sistem tidak dapat digunakan untuk pengambilan keputusan.

Masalah ini juga menyulitkan proses pemeliharaan. Admin database harus menghapus atau memperbaiki data yang tidak valid, yang memakan waktu dan tenaga. Jika data sudah banyak, kesalahan semacam ini menjadi sulit dilacak.

Oleh karena itu, constraint foreign key harus selalu ditegakkan. Dengan cara ini, sistem memastikan bahwa hanya data yang valid dan sah yang dapat dimasukkan.

---

### 3.4 Tidak Menentukan Aksi Saat Data Referensi Dihapus

Kesalahan lain adalah tidak menetapkan aksi apa yang harus dilakukan ketika data referensi dihapus. Misalnya, jika anggota dihapus dari tabel `anggota`, sistem tidak tahu apa yang harus dilakukan dengan transaksi peminjaman yang merujuk padanya. Hal ini bisa menyebabkan baris di tabel `peminjaman` memiliki foreign key yang tidak valid.

```sql
-- Salah: tidak menentukan aksi penghapusan
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Tanpa aturan, integritas data menjadi terancam. Data peminjaman tetap ada tetapi tidak lagi terkait dengan anggota yang valid. Hal ini membuat laporan menjadi rancu karena menampilkan transaksi dari anggota yang sudah tidak terdaftar.

Masalah ini juga menimbulkan kebingungan dalam administrasi. Pustakawan bisa melihat catatan peminjaman tanpa mengetahui siapa peminjamnya. Hal ini merusak fungsi dasar database sebagai alat pelacakan data.

Selain itu, kesalahan ini menyulitkan ketika ingin melakukan analisis historis. Data transaksi yang terputus dari referensinya tidak bisa dimanfaatkan secara penuh. Hal ini mengurangi nilai informasi dalam sistem.

Dengan demikian, setiap foreign key harus memiliki aturan jelas tentang apa yang terjadi saat data referensi dihapus. Pilihan yang umum adalah *CASCADE*, *SET NULL*, atau *RESTRICT*.

---

### 3.5 Menggunakan Foreign Key yang Tidak Logis

Kesalahan terakhir adalah memilih atribut yang tidak logis sebagai foreign key. Misalnya, menggunakan `alamat` anggota sebagai foreign key di tabel peminjaman. Hal ini tidak masuk akal karena alamat bukan identitas unik dan bisa berubah sewaktu-waktu.

```sql
-- Salah: foreign key tidak logis
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    alamat VARCHAR(200),
    FOREIGN KEY (alamat) REFERENCES anggota(alamat)
);
```

Penggunaan atribut tidak logis ini membuat relasi rapuh. Jika anggota pindah rumah, maka relasi dengan transaksi peminjaman akan rusak. Hal ini menyebabkan data kehilangan konsistensinya.

Selain itu, atribut seperti alamat tidak efisien untuk dijadikan foreign key. Kolom teks panjang memperlambat performa query dan memboroskan ruang penyimpanan. Hal ini membuat sistem menjadi tidak efisien.

Masalah lain adalah potensi duplikasi. Beberapa anggota bisa tinggal di alamat yang sama, sehingga relasi menjadi ambigu. Hal ini membuat sistem tidak dapat membedakan transaksi peminjaman dengan benar.

Kesalahan ini sering dilakukan oleh pemula yang belum memahami logika relasi. Padahal, foreign key seharusnya dipilih berdasarkan atribut yang benar-benar membangun hubungan antar entitas.

Dengan demikian, memilih foreign key yang tidak logis harus dihindari. Database yang baik hanya menggunakan atribut relevan sebagai penghubung antar tabel.

---

## 4. Best Practice

### 4.1 Selalu Gunakan Primary Key sebagai Referensi

Praktik terbaik dalam mendefinisikan foreign key adalah selalu merujuk ke primary key tabel lain. Dengan cara ini, integritas relasi dapat dijamin sepenuhnya karena primary key sudah memiliki sifat unik dan tidak bernilai NULL. Dalam perpustakaan, tabel `peminjaman` harus menggunakan `id_anggota` sebagai foreign key yang merujuk ke `id_anggota` di tabel `anggota`. Desain ini memastikan setiap transaksi selalu terhubung dengan satu anggota sah. Dengan demikian, database dapat menjaga konsistensi data tanpa kebingungan.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Penggunaan primary key juga membuat query menjadi lebih jelas dan efisien. Sistem tidak perlu melakukan pencarian tambahan untuk memastikan keunikan karena sudah dijamin oleh primary key. Hal ini meningkatkan performa pencarian data secara signifikan. Dalam sistem yang besar, peningkatan performa ini dapat sangat terasa.

Selain itu, foreign key yang merujuk ke primary key mendukung validasi otomatis. Database tidak akan mengizinkan data asing yang tidak ada pada tabel referensi masuk. Dengan demikian, kualitas data terjaga dengan baik. Hal ini penting untuk sistem informasi perpustakaan yang harus menjaga catatan transaksi secara ketat.

Praktik ini juga memudahkan perawatan sistem. Relasi antar tabel jelas dan mudah dipahami oleh desainer maupun pengembang yang baru masuk. Dengan dokumentasi sederhana, relasi dapat langsung dikenali karena pola penggunaan primary key selalu konsisten.

Dengan menjadikan primary key sebagai referensi, sistem database mendapatkan fondasi yang kuat. Hal ini membuat seluruh relasi dalam database tetap stabil dan berfungsi sebagaimana mestinya.

---

### 4.2 Konsistensi Tipe Data Antara Primary Key dan Foreign Key

Foreign key harus memiliki tipe data yang sama dengan primary key yang dirujuk. Konsistensi ini mencegah kesalahan saat data dimasukkan atau query dijalankan. Misalnya, jika `id_buku` di tabel `buku` bertipe `INT`, maka `id_buku` di tabel `peminjaman` juga harus bertipe `INT`. Dengan cara ini, database dapat memverifikasi referensi dengan benar tanpa perlu konversi tipe data.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_buku INT,
    FOREIGN KEY (id_buku) REFERENCES buku(id_buku)
);
```

Konsistensi tipe data membuat query berjalan lebih cepat. Sistem tidak perlu melakukan konversi data setiap kali relasi dipanggil. Hal ini mengurangi beban pemrosesan, terutama pada tabel besar. Dalam perpustakaan, laporan peminjaman berdasarkan `id_buku` dapat dihasilkan lebih cepat.

Selain itu, konsistensi tipe data memudahkan integrasi dengan sistem eksternal. Sistem lain yang menggunakan database ini tidak perlu menyesuaikan format data. Hal ini meningkatkan interoperabilitas antar aplikasi. Dalam konteks perpustakaan digital, integrasi dengan aplikasi katalog daring menjadi lebih mudah.

Konsistensi juga mengurangi risiko kesalahan manusia. Pengembang atau administrator database tidak perlu mengingat format berbeda antara tabel. Dengan aturan yang konsisten, kesalahan input dapat diminimalkan. Hal ini meningkatkan kualitas pengelolaan data.

Praktik ini sederhana tetapi krusial. Dengan konsistensi tipe data, sistem menjadi lebih stabil, efisien, dan mudah dirawat.

---

### 4.3 Terapkan Referential Integrity dengan Constraint

Constraint foreign key harus ditegakkan agar sistem hanya menerima data yang valid. Dengan constraint ini, setiap nilai foreign key harus cocok dengan nilai primary key yang ada pada tabel referensi. Misalnya, `id_anggota` pada tabel `peminjaman` harus selalu ada di tabel `anggota`. Database akan menolak transaksi peminjaman jika anggota belum terdaftar.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Penerapan constraint menjaga kualitas data dalam jangka panjang. Data yang masuk ke sistem selalu terverifikasi, sehingga laporan yang dihasilkan dapat dipercaya. Tanpa constraint, risiko data fiktif atau tidak sah masuk ke database meningkat. Hal ini merusak validitas sistem perpustakaan.

Constraint juga melindungi integritas relasi antar tabel. Misalnya, anggota yang dihapus dari tabel `anggota` tidak bisa tetap muncul dalam catatan peminjaman. Sistem akan menolak operasi yang menyebabkan inkonsistensi. Hal ini menjaga relasi tetap koheren.

Selain itu, constraint membantu dalam pemeliharaan sistem. Admin tidak perlu memeriksa data secara manual untuk mencari ketidaksesuaian. Sistem otomatis mencegah masalah tersebut. Hal ini menghemat waktu dan tenaga.

Dengan menerapkan referential integrity, database menjadi lebih andal dan dapat digunakan sebagai dasar pengambilan keputusan. Praktik ini adalah standar dalam desain database relasional yang baik.

---

### 4.4 Tentukan Aksi Jelas Saat Data Referensi Dihapus atau Diubah

Foreign key sebaiknya disertai aturan yang jelas tentang apa yang terjadi saat data referensi dihapus atau diubah. Pilihan umum adalah *CASCADE*, *SET NULL*, atau *RESTRICT*. Dengan aturan ini, sistem tetap konsisten bahkan ketika data referensi berubah. Misalnya, jika anggota dihapus dari tabel `anggota`, maka baris terkait di tabel `peminjaman` dapat ikut dihapus atau diatur menjadi NULL sesuai kebutuhan.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota) ON DELETE CASCADE
);
```

Penentuan aksi membuat database lebih aman. Sistem tidak akan meninggalkan data yatim yang kehilangan referensi. Hal ini menjaga integritas relasi antar tabel. Dalam perpustakaan, catatan peminjaman tidak mungkin mengacu pada anggota yang sudah dihapus.

Selain itu, aturan ini memudahkan pengelolaan data. Administrator tidak perlu melakukan pembersihan manual ketika ada data yang berubah. Sistem otomatis menyesuaikan relasi sesuai dengan aturan yang ditentukan.

Aksi yang jelas juga meningkatkan transparansi. Semua pihak yang menggunakan database tahu apa yang akan terjadi ketika data referensi dihapus. Hal ini membuat sistem lebih dapat diprediksi dan mudah digunakan.

Dengan menetapkan aksi yang jelas, database menjadi lebih konsisten dan lebih mudah dikelola. Praktik ini wajib diterapkan dalam sistem yang mengandalkan integritas relasi.

---

### 4.5 Gunakan Foreign Key Hanya pada Atribut yang Relevan

Praktik terbaik lainnya adalah memilih atribut yang benar-benar relevan sebagai foreign key. Hanya atribut yang mencerminkan hubungan logis antar tabel yang layak dijadikan penghubung. Misalnya, `id_anggota` pada tabel `peminjaman` relevan karena menunjukkan siapa yang meminjam. Sebaliknya, atribut seperti `alamat` anggota tidak relevan karena tidak mewakili hubungan antar entitas.

```sql
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY,
    id_anggota INT,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota)
);
```

Pemilihan atribut yang relevan menjaga relasi tetap logis dan bermakna. Database tidak hanya konsisten secara teknis, tetapi juga sesuai dengan realitas dunia nyata. Dalam perpustakaan, hal ini berarti setiap peminjaman benar-benar dapat dikaitkan dengan anggota yang nyata.

Atribut relevan juga lebih stabil. Nilainya jarang berubah, sehingga relasi tetap bertahan dalam jangka panjang. Hal ini mengurangi risiko inkonsistensi. Database yang baik selalu mempertimbangkan stabilitas atribut dalam mendesain foreign key.

Selain itu, atribut relevan meningkatkan kejelasan sistem. Hubungan antar tabel mudah dipahami oleh pengembang atau administrator baru. Hal ini membuat sistem lebih mudah dipelihara.

Dengan menggunakan foreign key hanya pada atribut yang relevan, sistem database menjadi lebih konsisten, logis, dan efisien. Praktik ini mencerminkan desain database yang baik dan profesional.

---


## Studi Kasus Perpustakaan

Perpustakaan sekolah ingin memastikan bahwa setiap transaksi peminjaman buku hanya dapat dilakukan oleh anggota yang sah. Untuk itu, mereka membuat tabel `anggota` dengan `id_anggota` sebagai primary key dan tabel `peminjaman` dengan foreign key `id_anggota`. Relasi ini menjamin bahwa setiap baris pada tabel `peminjaman` selalu merujuk ke anggota yang ada di tabel `anggota`. Dengan demikian, tidak ada transaksi yang tercatat tanpa identitas peminjam yang jelas.

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

Setelah desain dibuat, pustakawan menguji sistem dengan menambahkan data anggota. Misalnya, anggota pertama adalah “Rani Kusuma” dengan alamat “Jl. Melati No. 5”. Anggota kedua adalah “Dedi Santoso” dengan alamat “Jl. Mawar No. 12”. Data ini masuk ke tabel `anggota` dengan ID otomatis bertambah. Selanjutnya, pustakawan memasukkan transaksi peminjaman yang merujuk ke `id_anggota` tersebut. Sistem hanya menerima transaksi yang sesuai dengan data anggota yang sudah ada.

```sql
INSERT INTO anggota (nama, alamat) VALUES 
('Rani Kusuma', 'Jl. Melati No. 5'),
('Dedi Santoso', 'Jl. Mawar No. 12');

INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES 
(1, 2, '2025-02-01'),
(2, 3, '2025-02-02');
```

Hasil pengujian menunjukkan bahwa foreign key bekerja sesuai harapan. Jika pustakawan mencoba memasukkan transaksi dengan `id_anggota = 99` yang tidak ada di tabel `anggota`, sistem menolak entri tersebut. Hal ini membuktikan bahwa foreign key menjaga integritas data dan mencegah catatan peminjaman yang tidak valid. Dengan cara ini, setiap transaksi di database perpustakaan selalu terkait dengan anggota nyata.

```sql
-- Gagal: anggota tidak ada
INSERT INTO peminjaman (id_anggota, id_buku, tanggal_pinjam) VALUES 
(99, 4, '2025-02-03');
```

Studi kasus ini juga memperlihatkan bagaimana laporan bisa dihasilkan dengan mudah. Misalnya, pustakawan dapat membuat query untuk menampilkan nama anggota beserta daftar buku yang mereka pinjam. Semua ini dimungkinkan karena foreign key menghubungkan tabel `anggota` dan `peminjaman` secara formal. Dengan desain ini, sistem perpustakaan menjadi lebih terorganisir, konsisten, dan siap mendukung analisis data dalam jangka panjang.

```sql
SELECT a.nama, p.id_buku, p.tanggal_pinjam
FROM anggota a
JOIN peminjaman p ON a.id_anggota = p.id_anggota;
```

---

## Kesimpulan

Foreign key adalah mekanisme penting dalam basis data relasional yang menjamin keterhubungan antar tabel secara konsisten. Dengan foreign key, setiap transaksi peminjaman di perpustakaan hanya bisa dilakukan oleh anggota yang sah, sehingga data tidak pernah menggantung tanpa referensi. Kesalahan umum seperti menggunakan atribut tidak logis, membiarkan tipe data tidak konsisten, atau tidak menetapkan aksi saat penghapusan data harus dihindari karena dapat merusak integritas sistem. Praktik terbaik seperti selalu merujuk ke primary key, menjaga konsistensi tipe data, dan menerapkan constraint membuat database lebih stabil dan mudah dipelihara. Studi kasus perpustakaan memperlihatkan bahwa foreign key tidak hanya konsep teknis, tetapi juga kebutuhan praktis untuk menjaga keandalan catatan data.

## Referensi

* Harrington, J. L. (2016). *Relational Database Design and Implementation* (4th ed.). Morgan Kaufmann.
* Date, C. J. (2004). *An Introduction to Database Systems* (8th ed.). Addison-Wesley.


