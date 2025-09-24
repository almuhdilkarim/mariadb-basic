---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Membuat Laporan Peminjaman Bulanan"
short: "Laporan"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 16
lister: 3
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Proyek"
      icon: ""
      desc: "Latihan membuat laporan ringkas peminjaman"
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta membuat query laporan peminjaman bulanan menggunakan fungsi agregasi. Modul ini memperkuat keterampilan analisis data untuk kebutuhan laporan rutin."
---


Baik, saya lanjut menulis **Modul 3 – Membuat Laporan Pinjaman Bulanan** sesuai kaidah modul praktikal.

---

## Pendahuluan

Laporan pinjaman bulanan merupakan salah satu fitur penting dalam sistem perpustakaan. Dengan laporan ini, pustakawan dapat mengetahui jumlah transaksi peminjaman yang terjadi dalam periode tertentu. Informasi tersebut dapat digunakan untuk mengevaluasi tingkat pemanfaatan koleksi serta pola membaca anggota. Jika tidak ada laporan terstruktur, analisis data akan sulit dilakukan secara konsisten.

Pembuatan laporan ini juga melatih kita dalam menggunakan fungsi tanggal di SQL. Dengan memanfaatkan fungsi seperti `MONTH()` dan `YEAR()`, kita bisa mengelompokkan transaksi berdasarkan waktu tertentu. Penggunaan fungsi agregasi seperti `COUNT()` membuat hasil laporan lebih mudah dipahami. Hal ini sesuai dengan prinsip dasar dalam analisis data, yaitu menyajikan informasi yang kompleks dalam bentuk yang lebih sederhana.

Selain untuk evaluasi internal, laporan pinjaman bulanan juga berguna sebagai bahan laporan resmi. Pihak manajemen sekolah atau lembaga sering membutuhkan data ini untuk menilai kinerja perpustakaan. Dengan adanya database, laporan tersebut bisa dihasilkan dengan cepat tanpa perlu pencatatan manual. Ini adalah salah satu manfaat nyata dari digitalisasi sistem perpustakaan.

Dalam praktik ini, kita akan membangun query untuk menghasilkan laporan pinjaman bulanan. Data yang digunakan berasal dari tabel `pinjaman` dan `detail_pinjam` yang sudah kita isi pada modul sebelumnya. Dengan data tersebut, kita akan menghitung jumlah transaksi dan jumlah buku yang dipinjam setiap bulan. Proses ini akan menjadi langkah nyata dalam menghubungkan teori database dengan kebutuhan sehari-hari di perpustakaan.

Kerja bagus sudah sampai di tahap ini! Mari kita coba bersama-sama membuat query laporan pinjaman bulanan. Jangan khawatir jika tampak rumit, karena setiap langkah akan dijelaskan secara sistematis.

---

## Langkah-langkah Praktik

Pertama, mari kita lihat data pinjaman yang sudah ada. Dengan query sederhana, kita bisa menampilkan isi tabel `pinjaman`:

```sql
SELECT * FROM pinjaman;
```

Selanjutnya, untuk menghitung jumlah transaksi peminjaman per bulan, kita gunakan fungsi `MONTH()` dan `YEAR()`. Jalankan query berikut:

```sql
SELECT YEAR(tgl_pinjam) AS tahun, MONTH(tgl_pinjam) AS bulan, COUNT(*) AS total_pinjaman
FROM pinjaman
GROUP BY YEAR(tgl_pinjam), MONTH(tgl_pinjam);
```

Jika kita ingin menghitung jumlah buku yang dipinjam, maka kita perlu menghubungkan tabel `pinjaman` dengan `detail_pinjam`. Gunakan query berikut:

```sql
SELECT YEAR(p.tgl_pinjam) AS tahun, MONTH(p.tgl_pinjam) AS bulan, SUM(d.jumlah) AS total_buku
FROM pinjaman p
JOIN detail_pinjam d ON p.id_pinjaman = d.id_pinjaman
GROUP BY YEAR(p.tgl_pinjam), MONTH(p.tgl_pinjam);
```

Untuk membuat laporan lebih informatif, kita bisa menggabungkan keduanya: jumlah transaksi dan jumlah buku. Query berikut menampilkan keduanya dalam satu laporan:

```sql
SELECT YEAR(p.tgl_pinjam) AS tahun, MONTH(p.tgl_pinjam) AS bulan, 
       COUNT(p.id_pinjaman) AS total_transaksi, SUM(d.jumlah) AS total_buku
FROM pinjaman p
JOIN detail_pinjam d ON p.id_pinjaman = d.id_pinjaman
GROUP BY YEAR(p.tgl_pinjam), MONTH(p.tgl_pinjam);
```

Bagus sekali! Dengan query ini, Anda sudah berhasil membuat laporan pinjaman bulanan yang bisa langsung dipakai pustakawan. Laporan ini dapat dikembangkan lebih lanjut, misalnya dengan menambahkan filter berdasarkan anggota atau kategori buku.

