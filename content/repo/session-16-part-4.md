---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Membuat User Admin dan Anggota"
short: "UserRole"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 16
lister: 4
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: ""
      name: ""
      icon: ""
      desc: ""
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Modul ini membimbing peserta membuat user admin dan anggota dengan hak akses berbeda. Peserta memahami penerapan kontrol akses dalam proyek nyata."
---


Baik, saya lanjut menulis **Modul 4 – Membuat User Admin & Anggota dengan Izin Berbeda** sesuai kaidah modul praktikal.

---

# Modul 4 – Membuat User Admin & Anggota dengan Izin Berbeda

## Pendahuluan

Dalam sebuah sistem perpustakaan, tidak semua pengguna memiliki hak akses yang sama. Pustakawan sebagai admin memerlukan akses penuh untuk mengelola data buku, anggota, dan laporan. Sementara itu, anggota perpustakaan biasanya hanya perlu hak akses terbatas, misalnya untuk melihat koleksi atau melakukan pencarian. Oleh karena itu, perbedaan hak akses merupakan bagian penting dari keamanan database.

MariaDB menyediakan fitur manajemen pengguna yang memungkinkan kita membuat akun dengan izin berbeda. Dengan perintah `CREATE USER` dan `GRANT`, kita bisa mengatur siapa saja yang bisa mengakses database serta apa yang bisa mereka lakukan. Prinsip ini dikenal sebagai *role-based access control* (RBAC). Dengan RBAC, sistem menjadi lebih aman dan terstruktur.

Jika semua pengguna memiliki hak yang sama, risiko terjadinya kerusakan data sangat besar. Misalnya, seorang anggota tanpa sengaja menghapus tabel penting. Hal ini bisa mengacaukan seluruh sistem. Dengan membatasi izin sesuai kebutuhan, kita dapat meminimalkan risiko tersebut. Prinsip ini dikenal sebagai *least privilege principle*.

Selain itu, pemisahan hak akses juga memudahkan pengelolaan operasional. Admin tidak perlu khawatir aktivitas anggota akan mengganggu sistem inti. Sebaliknya, anggota bisa fokus menggunakan fitur yang memang diperuntukkan bagi mereka. Dengan cara ini, sistem menjadi lebih efisien sekaligus lebih aman.

Kerja bagus jika kamu sudah sampai di tahap ini! Mari kita lanjut dengan praktik langsung membuat akun admin dan anggota di MariaDB. Dengan langkah ini, perpustakaan mini kita semakin mendekati aplikasi nyata.

---

## Langkah-langkah Praktik

Mari kita mulai dengan membuat akun **admin**. Admin memiliki hak penuh untuk mengelola database. Jalankan perintah berikut:

```sql
CREATE USER 'admin_perpus'@'localhost' IDENTIFIED BY 'passwordAdmin!';
GRANT ALL PRIVILEGES ON perpustakaan_mini.* TO 'admin_perpus'@'localhost';
FLUSH PRIVILEGES;
```

Sekarang mari buat akun **anggota**. Anggota hanya diberi hak terbatas, misalnya hanya bisa membaca data.

```sql
CREATE USER 'anggota1'@'localhost' IDENTIFIED BY 'passwordAnggota!';
GRANT SELECT ON perpustakaan_mini.* TO 'anggota1'@'localhost';
FLUSH PRIVILEGES;
```

Jika kita ingin membuat beberapa anggota, kita bisa menyalin pola di atas dengan mengganti username.

```sql
CREATE USER 'anggota2'@'localhost' IDENTIFIED BY 'passwordAnggota2!';
GRANT SELECT ON perpustakaan_mini.* TO 'anggota2'@'localhost';
```

Untuk memastikan izin sudah berlaku, kita bisa login dengan akun anggota dan mencoba menjalankan query `INSERT`.

```sql
INSERT INTO buku (judul, pengarang, penerbit, tahun_terbit, stok)
VALUES ('Buku Baru', 'Anonim', 'Gramedia', 2024, 2);
```

Jika izin diatur dengan benar, query ini akan gagal karena anggota hanya memiliki hak `SELECT`. Bagus sekali! Ini menunjukkan sistem kita sudah memiliki pembatasan akses sesuai peran pengguna.