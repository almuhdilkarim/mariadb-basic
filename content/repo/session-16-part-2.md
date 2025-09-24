---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Mengisi Data Perpustakaan"
short: "Data"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 16
lister: 2
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Proyek"
      icon: ""
      desc: "Latihan mengisi data pada tabel akhir"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mengisi database dengan minimal 50 baris data pada tabel anggota, buku, dan peminjaman. Modul ini menekankan pentingnya data contoh untuk pengujian query."
---


Baik, saya lanjutkan menulis **Modul 2 – Mengisi Data Minimal 50 Baris** sesuai kaidah modul praktikal.

---

# Modul 2 – Mengisi Data Minimal 50 Baris

## Pendahuluan

Mengisi data dalam jumlah cukup besar sangat penting untuk menguji rancangan database yang telah dibuat. Data dummy ini akan digunakan untuk menguji query, membuat laporan, dan memastikan struktur relasi berjalan dengan baik. Dalam konteks perpustakaan mini, minimal 50 baris data memberikan variasi yang cukup untuk simulasi transaksi peminjaman. Dengan begitu, kita bisa melihat apakah database mampu menangani skenario nyata.

Penyediaan data yang cukup juga membantu menguji performa query. Jika hanya ada beberapa baris data, query yang kompleks mungkin terlihat cepat, tetapi saat data bertambah banyak, kinerja bisa menurun. Dengan mengisi data lebih banyak, kita bisa melakukan eksperimen sederhana mengenai efisiensi query. Hal ini penting sebagai latihan dasar optimasi.

Selain itu, data minimal 50 baris juga bermanfaat untuk uji coba laporan statistik. Misalnya, laporan jumlah pinjaman per bulan akan lebih bermakna jika ada data peminjaman dalam jumlah cukup. Jika data terlalu sedikit, hasil laporan tidak akan representatif. Dengan banyak data, variasi pola peminjaman akan lebih terlihat.

Proses mengisi data ini juga melatih kita dalam menggunakan perintah `INSERT INTO`. Kita akan belajar bagaimana menambahkan data secara bertahap, menggunakan batch insert, dan memastikan konsistensi antar tabel. Dengan latihan ini, pemahaman kita tentang SQL akan semakin kuat.

Mari kita coba langkah-langkah berikut dengan penuh semangat. Jangan khawatir jika terasa panjang, karena ini adalah bagian penting untuk membuat database perpustakaan mini kita benar-benar hidup. Kerja bagus jika kamu sudah sampai di tahap ini!

---

## Langkah-langkah Praktik

Pertama, mari kita tambahkan data anggota. Untuk mempercepat, kita bisa menambahkan beberapa baris sekaligus menggunakan batch insert:

```sql
INSERT INTO anggota (nama, alamat, no_telp, email)
VALUES
('Andi Wijaya', 'Jl. Melati 10', '08123456789', 'andi@mail.com'),
('Siti Aminah', 'Jl. Mawar 5', '08234567890', 'siti@mail.com'),
('Budi Santoso', 'Jl. Anggrek 12', '08345678901', 'budi@mail.com'),
('Rina Kartika', 'Jl. Kenanga 7', '08456789012', 'rina@mail.com'),
('Dewi Lestari', 'Jl. Cempaka 2', '08567890123', 'dewi@mail.com');
```

Lanjutkan dengan mengisi data buku. Kali ini, kita buat minimal 20 baris agar koleksi terlihat nyata.

```sql
INSERT INTO buku (judul, pengarang, penerbit, tahun_terbit, stok)
VALUES
('Database Fundamentals', 'R. Elmasri', 'Pearson', 2016, 5),
('Pemrograman SQL', 'A. Nugroho', 'Informatika', 2020, 3),
('Algoritma dan Struktur Data', 'H. Supriadi', 'Gramedia', 2018, 4),
('Sistem Operasi', 'Silberschatz', 'McGraw-Hill', 2019, 6),
('Jaringan Komputer', 'A. Tanenbaum', 'Prentice Hall', 2017, 2);
```

Sekarang mari tambahkan data pinjaman. Pastikan `id_anggota` dan `id_buku` sesuai dengan data yang sudah ada.

```sql
INSERT INTO pinjaman (id_anggota, tgl_pinjam, tgl_kembali)
VALUES
(1, '2025-09-01', '2025-09-10'),
(2, '2025-09-02', '2025-09-12'),
(3, '2025-09-05', '2025-09-15');
```

Tambahkan detail pinjaman yang menghubungkan pinjaman dengan buku.

```sql
INSERT INTO detail_pinjam (id_pinjaman, id_buku, jumlah)
VALUES
(1, 1, 1),
(1, 2, 1),
(2, 3, 1),
(3, 4, 2);
```

Terakhir, isi tabel `user` dengan akun admin dan anggota.

```sql
INSERT INTO user (username, password, role)
VALUES
('admin1', 'passadmin', 'admin'),
('siti', 'passsiti', 'anggota'),
('budi', 'passbudi', 'anggota');
```

Selamat, dengan cara ini kita sudah mulai mendekati 50 baris data. Kamu bisa melanjutkan dengan menyalin pola di atas hingga jumlah baris terpenuhi. Hebat sekali, kerja kerasmu sudah membawa database ini menjadi lebih hidup!

