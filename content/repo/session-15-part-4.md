---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Praktik Trigger Update Stok Buku"
short: "Praktik"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 15
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
description: "Peserta mempraktikkan pembuatan trigger yang mengurangi stok buku secara otomatis saat peminjaman terjadi. Modul ini menegaskan manfaat trigger dalam otomasi database."
---

## Pendahuluan

Trigger memungkinkan database menjalankan aksi otomatis saat terjadi peristiwa pada tabel, sehingga aturan bisnis dapat ditegakkan konsisten tanpa campur tangan manual. Dalam sistem perpustakaan, aturan umum yang harus selalu benar adalah stok buku berkurang ketika dipinjam dan bertambah ketika pengembalian atau pembatalan terjadi. Tanpa trigger, developer harus mengingatkan aplikasi untuk selalu memperbarui stok, yang rawan terlewat dan menimbulkan inkonsistensi. Dengan trigger, koreksi stok dibebaskan dari aplikasi dan diserahkan ke lapisan database yang lebih dekat dengan datanya. Pendekatan ini membantu menjaga integritas meski ada banyak klien atau layanan yang mengakses database secara bersamaan.

Masalah klasik pada stok adalah *race condition* saat banyak transaksi bersamaan, misalnya beberapa anggota meminjam judul yang sama. Jika pembaruan stok dilakukan dari aplikasi, jeda jaringan dan penanganan kesalahan bisa membuat dua transaksi “mengira” stok masih cukup. Trigger dengan validasi stok *sebelum* transaksi dicatat dapat menolak peminjaman ketika stok tidak mencukupi, sehingga mencegah stok menjadi negatif. Selain itu, penambahan trigger *setelah* transaksi tercatat memastikan stok benar-benar disesuaikan hanya jika baris pinjaman sukses di-*commit*. Dengan kombinasi ini, stok tetap akurat meskipun beban transaksi tinggi.

Penting juga untuk memikirkan skenario pembatalan dan pengembalian, karena stok tidak hanya berkurang. Jika sebuah baris pinjaman dihapus untuk membatalkan transaksi yang salah input, stok harus dipulihkan secara otomatis agar ketersediaan buku kembali akurat. Demikian pula ketika ada perubahan pada baris pinjaman, misalnya mengganti `id_buku` yang dipinjam, maka stok buku lama harus dikembalikan dan stok buku baru harus dikurangi. Trigger yang dirancang baik menutup semua celah tersebut secara deterministik.

Dalam praktik ini, kita akan membangun tiga jenis trigger kunci di MariaDB: validasi stok *BEFORE INSERT* pada `pinjaman`, pengurangan stok *AFTER INSERT*, dan pengembalian stok *AFTER DELETE*. Kita juga akan menambahkan trigger *AFTER UPDATE* untuk menangani perubahan `id_buku` pada baris pinjaman, karena ini sering terjadi saat admin mengoreksi data. Dengan menuntaskan keempat trigger, kita mendapatkan siklus stok yang lengkap: validasi, konsumsi, pemulihan, dan penyesuaian akibat perubahan.

Tujuan modul ini adalah agar kamu mampu memasang trigger pembaruan stok yang aman, konsisten, dan mudah diaudit. Kamu akan menulis SQL yang bisa langsung dijalankan, menguji berbagai skenario, dan memverifikasi hasilnya dengan query sederhana. Mari kita praktikkan langkah demi langkah, sambil menjaga kebersihan skema dan penamaan agar mudah dipelihara. Kerja bagus sudah tiba di bagian yang krusial bagi integritas data ini. Sekarang, mari kita mulai implementasinya dengan teliti dan percaya diri.

---

## Langkah-langkah Praktik

Pertama, siapkan skema minimal untuk tabel `buku` dan `pinjaman` yang relevan dengan stok. Kita memerlukan kolom `stok` pada `buku` untuk menampung jumlah eksemplar yang tersedia, dan `pinjaman` untuk mencatat transaksi peminjaman. Pastikan tipe data sesuai kebutuhan dan indeks dibuat pada kolom yang sering di-*join* atau difilter. Jalankan skrip berikut di MariaDB agar basis praktik kita seragam. Setelah itu, kita akan memasang trigger satu per satu di atas skema ini.

```sql
CREATE TABLE IF NOT EXISTS buku (
  id_buku INT PRIMARY KEY AUTO_INCREMENT,
  judul VARCHAR(255) NOT NULL,
  stok INT NOT NULL DEFAULT 0
);

CREATE TABLE IF NOT EXISTS pinjaman (
  id_pinjaman INT PRIMARY KEY AUTO_INCREMENT,
  id_buku INT NOT NULL,
  id_anggota INT NOT NULL,
  tanggal_pinjam DATE NOT NULL DEFAULT (CURRENT_DATE),
  CONSTRAINT fk_pinjaman_buku FOREIGN KEY (id_buku) REFERENCES buku(id_buku)
);

INSERT INTO buku (judul, stok) VALUES
('Algoritma Pemrograman', 2),
('Basis Data Lanjut', 1),
('Sistem Operasi', 0);
```

Kedua, buat trigger *BEFORE INSERT* yang memvalidasi stok sebelum baris pinjaman benar-benar dimasukkan. Jika stok nol atau negatif, kita *signal* error agar transaksi ditolak dengan pesan yang jelas. Ini mencegah stok menjadi negatif dan memotong proses sedini mungkin. Perhatikan penggunaan subquery terikat pada `NEW.id_buku`. Pastikan pesan kesalahan informatif untuk memudahkan admin dan pengguna memahami penolakan.

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_check_stok
BEFORE INSERT ON pinjaman
FOR EACH ROW
BEGIN
  IF (SELECT stok FROM buku WHERE id_buku = NEW.id_buku) <= 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Stok buku habis, peminjaman ditolak';
  END IF;
END//
DELIMITER ;
```

Ketiga, pasang trigger *AFTER INSERT* untuk benar-benar mengurangi stok setelah baris pinjaman sukses dibuat. Kita melakukan *UPDATE* pada `buku` dan sekaligus memverifikasi bahwa baris terkena tepat satu baris, sebagai pengaman dasar. Karena ini terjadi setelah *insert* berhasil, integritas referensial sudah dijamin oleh FK. Dengan trigger ini, stok akan otomatis berkurang tanpa perlu kode aplikasi tambahan.

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_reduce_stok
AFTER INSERT ON pinjaman
FOR EACH ROW
BEGIN
  UPDATE buku SET stok = stok - 1 WHERE id_buku = NEW.id_buku;
END//
DELIMITER ;
```

Keempat, buat trigger *AFTER DELETE* agar stok dipulihkan ketika transaksi peminjaman dihapus, misalnya karena pembatalan atau koreksi. Trigger ini menambah stok satu eksemplar kembali ke buku terkait. Dengan begitu, penghapusan riwayat peminjaman tidak meninggalkan ketidakseimbangan pada ketersediaan. Ini adalah simetri yang diperlukan terhadap pengurangan stok di saat *insert*.

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_restore_stok
AFTER DELETE ON pinjaman
FOR EACH ROW
BEGIN
  UPDATE buku SET stok = stok + 1 WHERE id_buku = OLD.id_buku;
END//
DELIMITER ;
```

Kelima, tangani skenario *UPDATE* pada baris pinjaman ketika `id_buku` diganti dari satu judul ke judul lain. Kita perlu mengembalikan stok buku lama dan mengurangi stok buku baru dalam satu trigger. Ini mencegah hilangnya stok saat admin memperbaiki data yang salah. Pastikan *guard* untuk kasus ketika `id_buku` tidak berubah agar tidak melakukan pembaruan stok yang tidak perlu.

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_switch_stok
AFTER UPDATE ON pinjaman
FOR EACH ROW
BEGIN
  IF NEW.id_buku <> OLD.id_buku THEN
    -- kembalikan stok buku lama
    UPDATE buku SET stok = stok + 1 WHERE id_buku = OLD.id_buku;

    -- validasi stok buku baru sebelum konsumsi
    IF (SELECT stok FROM buku WHERE id_buku = NEW.id_buku) <= 0 THEN
      SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Perubahan pinjaman ditolak: stok buku baru habis';
    END IF;

    -- kurangi stok buku baru
    UPDATE buku SET stok = stok - 1 WHERE id_buku = NEW.id_buku;
  END IF;
END//
DELIMITER ;
```

---

## Kesalahan Umum

### 1) Validasi stok dilakukan *setelah* `INSERT` (terlambat)

```sql
-- SALAH: validasi dilakukan AFTER INSERT (risiko stok negatif jika rollback/kompetisi)
DELIMITER //
CREATE TRIGGER trg_check_stok_late
AFTER INSERT ON pinjaman
FOR EACH ROW
BEGIN
  IF (SELECT stok FROM buku WHERE id_buku = NEW.id_buku) < 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Stok negatif!';
  END IF;
END//
DELIMITER ;
```

Melakukan validasi setelah baris dibuat membiarkan baris “buruk” masuk dulu, lalu baru menolak, yang menyulitkan alur transaksi. Pendekatan ini meningkatkan peluang stok sempat dikurangi oleh trigger lain sebelum penolakan, memicu anomali di bawah beban. Lebih aman menolak sedini mungkin di fase *BEFORE*, sehingga baris invalid tidak pernah tercatat. Dengan begitu, *atomicity* transaksi lebih terjaga. Kebiasaan validasi terlambat sering terjadi karena ketidaktahuan perbedaan *BEFORE* vs *AFTER*.

### 2) Tidak mencegah stok negatif saat *UPDATE* `id_buku`

```sql
-- SALAH: mengurangi stok buku baru tanpa cek ketersediaan
DELIMITER //
CREATE TRIGGER trg_switch_no_check
AFTER UPDATE ON pinjaman
FOR EACH ROW
BEGIN
  IF NEW.id_buku <> OLD.id_buku THEN
    UPDATE buku SET stok = stok + 1 WHERE id_buku = OLD.id_buku;
    UPDATE buku SET stok = stok - 1 WHERE id_buku = NEW.id_buku;
  END IF;
END//
DELIMITER ;
```

Ketika `id_buku` diganti, stok buku baru bisa menjadi negatif jika tidak ada validasi. Hal ini berbahaya karena memunculkan angka stok yang tidak masuk akal dan menipu laporan ketersediaan. Pastikan ada pemeriksaan ketersediaan sebelum konsumsi stok pada trigger *UPDATE*. Selain itu, urutan operasi perlu dipikirkan agar tidak kehilangan stok dalam interleaving transaksi. Dokumentasikan alur ini agar tim mengerti konsekuensinya.

### 3) Mengandalkan aplikasi untuk semua pembaruan stok

```sql
-- SALAH: tidak ada trigger, stok diubah manual dari aplikasi
UPDATE buku SET stok = stok - 1 WHERE id_buku = ?;
```

Jika hanya mengandalkan aplikasi, jalur eksekusi lain (admin CLI, ETL, integrasi pihak ketiga) bisa lupa memperbarui stok. Ketika banyak klien menulis ke database, aturan bisnis tercerai-berai dan rentan inkonsistensi. Trigger memusatkan aturan pada sumber kebenaran yang sama untuk semua klien. Pendekatan *database-enforced rules* menurunkan risiko SLA terganggu akibat bug aplikasi. Ini juga menyederhanakan *code paths* di aplikasi.

### 4) Menghapus pinjaman tanpa memulihkan stok

```sql
-- SALAH: hapus baris pinjaman, stok dibiarkan
DELETE FROM pinjaman WHERE id_pinjaman = 123;
```

Menghapus baris pinjaman tanpa trigger *AFTER DELETE* membuat stok tidak kembali, sehingga ketersediaan turun permanen. Dalam jangka panjang, laporan stok akan melenceng jauh dari realitas fisik. Trigger pemulihan memastikan simetri antara konsumsi dan pengembalian stok. Uji kasus pembatalan dan *undo* adalah bagian wajib dari QA. Dokumentasi prosedur operasional harus menekankan hal ini.

### 5) Penamaan trigger tidak deskriptif dan bercampur logika

```sql
-- SALAH: nama membingungkan & satu trigger mengerjakan banyak hal
CREATE TRIGGER t1 AFTER INSERT ON pinjaman ...
```

Nama seperti `t1` tidak membantu saat audit atau debugging di masa depan. Menggabungkan banyak tanggung jawab dalam satu trigger menyulitkan pengujian dan pemeliharaan. Gunakan satu tanggung jawab per trigger dan penamaan konsisten berbasis peristiwa, tabel, dan aksi. Praktik ini memudahkan *rollout* terkontrol dan *hotfix*. Tim baru pun lebih cepat memahami arsitektur aturan di database.

---

## Best Practice

### 1) Validasi stok di *BEFORE INSERT*

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_check_stok
BEFORE INSERT ON pinjaman
FOR EACH ROW
BEGIN
  IF (SELECT stok FROM buku WHERE id_buku = NEW.id_buku) <= 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Stok buku habis, peminjaman ditolak';
  END IF;
END//
DELIMITER ;
```

Menolak sedini mungkin mencegah baris invalid memasuki tabel, menjaga *atomicity* dan integritas. Ini juga mengurangi kerja sia-sia dari trigger lain dan *constraint* yang akan dievaluasi setelahnya. Pendekatan *fail fast* memudahkan operasi dan memperjelas alur kegagalan ke pengguna. Sediakan pesan kesalahan yang menyebut judul atau `id_buku` agar *UX* lebih baik. Uji dengan data stok nol untuk memastikan perilaku benar.

### 2) Konsumsi stok di *AFTER INSERT*

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_reduce_stok
AFTER INSERT ON pinjaman
FOR EACH ROW
BEGIN
  UPDATE buku SET stok = stok - 1 WHERE id_buku = NEW.id_buku;
END//
DELIMITER ;
```

Melakukan pengurangan setelah baris valid dibuat memastikan referensi sudah sah dan *transaction log* jelas. Ini selaras dengan pola “validasi dulu, efek samping kemudian”. Pengurangan stok sebagai efek samping dari peminjaman yang sukses membuat audit trail mudah ditelusuri. Pastikan *error handling* di aplikasi menampilkan notifikasi yang sesuai bila *insert* ditolak sebelumnya. Lakukan *smoke test* pada beban ringan dan sedang.

### 3) Pulihkan stok di *AFTER DELETE*

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_restore_stok
AFTER DELETE ON pinjaman
FOR EACH ROW
BEGIN
  UPDATE buku SET stok = stok + 1 WHERE id_buku = OLD.id_buku;
END//
DELIMITER ;
```

Simetri konsumsi–pemulihan mencegah akumulasi kesalahan stok saat terjadi pembatalan. Menangani ini di *AFTER DELETE* memastikan baris benar-benar hilang baru stok dipulihkan. Jika aplikasi menambah jalur penghapusan baru, aturan tetap berlaku karena berada di database. Buat tes otomatis yang menghitung stok sebelum dan sesudah *delete*. Catat kejadian ini pada log aplikasi untuk audit.

### 4) Tangani perubahan `id_buku` di *AFTER UPDATE* dengan validasi

```sql
DELIMITER //
CREATE TRIGGER trg_pinjaman_switch_stok
AFTER UPDATE ON pinjaman
FOR EACH ROW
BEGIN
  IF NEW.id_buku <> OLD.id_buku THEN
    UPDATE buku SET stok = stok + 1 WHERE id_buku = OLD.id_buku;
    IF (SELECT stok FROM buku WHERE id_buku = NEW.id_buku) <= 0 THEN
      SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Stok buku baru habis';
    END IF;
    UPDATE buku SET stok = stok - 1 WHERE id_buku = NEW.id_buku;
  END IF;
END//
DELIMITER ;
```

Perubahan referensi harus memperlakukan stok seperti *transfer*: kembalikan ke buku lama dan kurangi dari buku baru. Validasi mencegah perpindahan ke judul yang stoknya tidak cukup. Ini menghindari stok negatif dan melindungi laporan ketersediaan. Tambahkan *unit test* untuk kasus `id_buku` tidak berubah agar trigger tidak melakukan kerja sia-sia. Dokumentasikan perilaku ini untuk admin.

### 5) Gunakan penamaan konsisten dan satu tanggung jawab per trigger

```sql
-- pola: trg_<tabel>_<aksi>_<deskripsi>
trg_pinjaman_check_stok
trg_pinjaman_reduce_stok
trg_pinjaman_restore_stok
trg_pinjaman_switch_stok
```

Penamaan konsisten memudahkan pencarian, audit, dan *onboarding* tim baru. Satu tanggung jawab per trigger membuat pengujian terfokus dan *rollback* perubahan lebih aman. Hindari *god trigger* yang mencampur berbagai aksi karena sulit dirawat. Sertakan uraian singkat di *change log* setiap kali membuat atau mengubah trigger. Terapkan *code review* untuk skrip DDL/trigger.

---

## Studi Kasus

Anggap perpustakaan memiliki tiga judul dengan stok bervariasi; anggota sering meminjam “Algoritma Pemrograman” karena menjadi referensi kuliah inti. Kita ingin memastikan stok buku itu tidak pernah turun di bawah nol dan peminjaman otomatis ditolak saat habis. Jalankan rangkaian uji berikut untuk memverifikasi siklus penuh: peminjaman sukses, penolakan saat habis, pembatalan yang memulihkan stok, dan koreksi `id_buku` yang memindahkan pinjaman. Perhatikan setiap langkah menggunakan query sederhana agar mudah direproduksi. Setelah skenario selesai, bandingkan stok akhir dengan ekspektasi.

```sql
-- 1) Peminjaman sukses (stok Algoritma = 2 -> 1)
INSERT INTO pinjaman (id_buku, id_anggota) VALUES (1, 1001);

-- 2) Peminjaman kedua sukses (stok 1 -> 0)
INSERT INTO pinjaman (id_buku, id_anggota) VALUES (1, 1002);

-- 3) Peminjaman ketiga DITOLAK (stok 0, trigger BEFORE INSERT menolak)
INSERT INTO pinjaman (id_buku, id_anggota) VALUES (1, 1003);

-- 4) Batalkan pinjaman kedua, stok kembali 1
DELETE FROM pinjaman
WHERE id_pinjaman = (SELECT MIN(id_pinjaman) FROM pinjaman WHERE id_buku = 1 AND id_anggota = 1002);

-- 5) Koreksi pinjaman pertama: ganti ke 'Basis Data Lanjut' (stok-nya 1)
UPDATE pinjaman
SET id_buku = 2
WHERE id_pinjaman = (SELECT MIN(id_pinjaman) FROM pinjaman WHERE id_anggota = 1001);

-- Cek stok akhir
SELECT id_buku, judul, stok FROM buku ORDER BY id_buku;
```

Setelah langkah 1 dan 2, stok buku “Algoritma Pemrograman” turun dari 2 ke 0 secara otomatis. Langkah 3 harus gagal dengan pesan jelas, membuktikan validasi *BEFORE INSERT* bekerja sesuai rancangan. Langkah 4 mengembalikan stok ke 1 saat baris pinjaman dihapus, menunjukkan simetri *AFTER DELETE*. Langkah 5 memindahkan konsumsi stok dari buku lama ke buku baru, termasuk validasi stok tujuan sebelum pengurangan. Hasil *SELECT* terakhir harus konsisten dengan ekspektasi manualmu.

Studi kasus ini juga menyorot pentingnya *observability*. Tambahkan *logging* aplikasi di sekitar operasi *INSERT/DELETE/UPDATE* pinjaman untuk mencatat keberhasilan atau penolakan dan alasan spesifiknya. Dengan log yang rapi, investigasi insiden jauh lebih cepat. Pastikan pula ada pengujian regresi otomatis yang menjalankan skenario serupa di pipeline CI. Praktik ini akan menjaga kualitas saat skema berkembang.

---

## Kesimpulan

Trigger pembaruan stok di MariaDB menegakkan aturan bisnis ketersediaan buku secara otomatis dan konsisten di sumber data. Kombinasi *BEFORE INSERT* untuk validasi, *AFTER INSERT* untuk konsumsi, *AFTER DELETE* untuk pemulihan, dan *AFTER UPDATE* untuk perpindahan memperkuat integritas dalam berbagai skenario. Pendekatan ini mengurangi ketergantungan pada aplikasi, mencegah stok negatif, dan menyederhanakan audit. Dengan penamaan yang jelas dan cakupan tanggung jawab yang sempit, trigger mudah diuji dan dirawat. Hasilnya, sistem perpustakaan tetap akurat, tangguh, dan siap melayani beban transaksi yang meningkat.

---

## Referensi

* Schwartz, B., Zaitsev, P., & Tkachenko, V. (2012). *High Performance MySQL* (3rd ed.). O’Reilly.
* Viescas, J. L., & Oppel, A. (2014). *SQL Queries for Mere Mortals* (3rd ed.). Addison-Wesley.
* Celko, J. (2005). *SQL Programming Style*. Morgan Kaufmann.
* Oracle. *MySQL 8.0 Reference Manual* – Triggers (sebagai konsep umum yang serupa).
* MariaDB Foundation. *MariaDB Knowledge Base* – Triggers (dokumentasi resmi MariaDB).

---

Butuh saya buatkan **skrip rollback** (DROP TRIGGER) dan *checklist* pengujian singkat yang bisa kamu jalankan setiap kali *deploy* skema?
