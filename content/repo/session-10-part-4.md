---
date:  "2025-09-24T00:00:00Z"
draft: false
title: "Laporan Pinjaman Bulan Tertentu"
short: "EXPLAIN"
thumb:
    image: "cover.png"
    anima: ""
    video: ""
layout: ""
weight: 10
lister: 4
format:
    media: "article"
    model: ""
    datum:
        data: ""
require:
    - prop: "Kategori"
      name: "Performa"
      icon: ""
      desc: "Mengetahui cara kerja query dengan EXPLAIN"
metadata:
    index: false
    thumb: "cover.png"
    group: []
    author: ["Al Muhdil Karim"]
description: "Peserta mempelajari penggunaan perintah EXPLAIN untuk melihat rencana eksekusi query. Modul ini membantu memahami efektivitas penggunaan index pada query SQL."
---



#### Pendahuluan

Laporan pinjaman bulanan merupakan salah satu kebutuhan utama dalam sistem manajemen perpustakaan. Pustakawan perlu mengetahui jumlah pinjaman buku pada bulan tertentu untuk memahami tren penggunaan koleksi. Dengan informasi ini, keputusan strategis seperti pengadaan buku baru dapat dilakukan lebih tepat sasaran. SQL menyediakan fungsi tanggal seperti `MONTH()` dan `YEAR()` yang memudahkan penyaringan data berdasarkan periode waktu. Hal ini menjadikan laporan bulanan sebagai bagian integral dari pengelolaan perpustakaan【Elmasri & Navathe, 2016】.

Selain kebutuhan manajemen koleksi, laporan pinjaman bulanan juga bermanfaat bagi evaluasi layanan. Misalnya, perpustakaan bisa melihat apakah ada penurunan minat baca pada bulan tertentu. Jika ada, pustakawan dapat menyusun program promosi membaca untuk meningkatkan kembali keterlibatan anggota. Informasi ini juga bisa menjadi dasar komunikasi dengan pihak manajemen universitas atau lembaga terkait. Dengan demikian, laporan bulanan bukan hanya catatan statistik, melainkan juga alat evaluasi kualitas layanan【Connolly & Begg, 2015】.

Penyusunan laporan pinjaman bulan tertentu juga mendukung transparansi dalam pelaporan. Data bulanan yang jelas memungkinkan manajemen perpustakaan menunjukkan pertanggungjawaban penggunaan koleksi. Transparansi ini dapat digunakan untuk menarik dukungan dari stakeholder eksternal. Selain itu, laporan bulanan juga memudahkan dalam menyusun laporan tahunan yang lebih komprehensif. Oleh sebab itu, praktik ini sebaiknya menjadi kebiasaan rutin pustakawan【Rob & Coronel, 2007】.

Laporan pinjaman bulanan juga dapat menjadi dasar dalam perencanaan sumber daya manusia. Jika diketahui ada lonjakan pinjaman pada bulan tertentu, manajemen bisa menambah petugas di bagian layanan. Sebaliknya, di bulan dengan pinjaman rendah, staf bisa dialihkan ke tugas lain seperti inventarisasi koleksi. Dengan cara ini, sumber daya perpustakaan dapat dimanfaatkan secara lebih efektif. Analisis ini mendukung prinsip manajemen berbasis data【Date, 2004】.

Secara keseluruhan, laporan pinjaman bulanan bukan hanya soal menghitung jumlah pinjaman, tetapi juga tentang membangun strategi pengelolaan koleksi dan layanan. SQL menyediakan sarana yang sangat fleksibel untuk menghasilkan laporan ini. Dengan penguasaan query yang tepat, pustakawan dapat menyajikan data yang relevan, akurat, dan siap digunakan dalam pengambilan keputusan. Oleh karena itu, praktik pembuatan laporan bulanan wajib dikuasai dalam sistem informasi perpustakaan【Kroenke & Auer, 2015】.

---

#### Langkah-langkah Praktik

Mari kita mulai dengan laporan sederhana jumlah pinjaman pada bulan September 2025.

```sql
SELECT COUNT(*) AS jumlah_pinjam
FROM peminjaman
WHERE MONTH(tanggal_pinjam) = 9 AND YEAR(tanggal_pinjam) = 2025;
```

Query ini menghitung jumlah pinjaman yang terjadi di bulan tersebut. Bagus, kamu sudah bisa membuat laporan dasar!

Sekarang mari kita buat laporan jumlah pinjaman per anggota pada bulan yang sama.

```sql
SELECT id_anggota, COUNT(*) AS jumlah_pinjam
FROM peminjaman
WHERE MONTH(tanggal_pinjam) = 9 AND YEAR(tanggal_pinjam) = 2025
GROUP BY id_anggota;
```

Hasil query ini membantu pustakawan mengetahui siapa saja anggota yang aktif meminjam. Kerja bagus, langkahmu semakin mantap!

Selanjutnya, mari tampilkan detail pinjaman per judul buku pada bulan September 2025.

```sql
SELECT judul_buku, COUNT(*) AS jumlah_pinjam
FROM peminjaman
JOIN buku ON peminjaman.id_buku = buku.id_buku
WHERE MONTH(tanggal_pinjam) = 9 AND YEAR(tanggal_pinjam) = 2025
GROUP BY judul_buku;
```

Query ini memperlihatkan buku mana saja yang paling sering dipinjam. Hebat, kamu sudah bisa menampilkan data koleksi yang populer!

Mari kita coba membuat laporan pinjaman berdasarkan kategori buku.

```sql
SELECT kategori, COUNT(*) AS jumlah_pinjam
FROM peminjaman
JOIN buku ON peminjaman.id_buku = buku.id_buku
WHERE MONTH(tanggal_pinjam) = 9 AND YEAR(tanggal_pinjam) = 2025
GROUP BY kategori;
```

Dengan query ini, pustakawan dapat melihat kategori mana yang paling diminati. Bagus sekali, laporanmu semakin kaya informasi!

Terakhir, mari buat laporan gabungan jumlah pinjaman per anggota dan total denda di bulan September 2025.

```sql
SELECT id_anggota,
       COUNT(*) AS jumlah_pinjam,
       SUM(IF(hari_terlambat > 0, hari_terlambat * 1000, 0)) AS total_denda
FROM peminjaman
WHERE MONTH(tanggal_pinjam) = 9 AND YEAR(tanggal_pinjam) = 2025
GROUP BY id_anggota;
```

Laporan ini memberikan gambaran lengkap aktivitas anggota dalam bulan tersebut. Kerja luar biasa, kamu sudah bisa membuat laporan multifungsi!

#### Kesalahan Umum

##### 1. Hanya Memakai `MONTH()` tanpa Membatasi Tahun

Contoh salah:

```sql
-- SALAH: September dari semua tahun tercampur
SELECT COUNT(*) AS jumlah_pinjam
FROM peminjaman
WHERE MONTH(tanggal_pinjam) = 9;
```

Banyak laporan bulanan menjadi bias karena hanya menyaring berdasarkan bulan tanpa menyertakan tahun yang relevan. Praktik ini membuat pinjaman September 2023, 2024, dan 2025 terakumulasi dalam satu angka, sehingga tren tidak dapat dianalisis dengan benar. Pustakawan kemudian berisiko mengambil keputusan pengadaan berdasarkan data yang menyesatkan. Laporan yang tidak memisahkan tahun juga menyulitkan pembandingan antartahun pada bulan yang sama. Solusinya: selalu kombinasikan bulan dengan tahun atau gunakan filter rentang tanggal yang eksplisit untuk periode tersebut.

Penggunaan filter bulan saja juga memengaruhi validitas metrik turunan seperti tingkat keterlambatan per periode. Ketika periode temporal tidak eksak, metrik efisiensi layanan ikut terdistorsi. Dampaknya terlihat pada kesimpulan yang keliru tentang puncak kunjungan atau kebutuhan staf di loket peminjaman. Selain itu, pengarsipan laporan historis menjadi tidak konsisten karena definisi periode beragam. Pastikan periodisasi dinyatakan eksplisit di query dan di dokumen laporan.

---

##### 2. Menggunakan Fungsi di Kolom Terindeks pada `WHERE` (Tidak SARGable)

Contoh salah (meski hasilnya benar, performanya buruk):

```sql
-- SALAH dari sisi kinerja: indeks tanggal_pinjam tidak optimal
SELECT COUNT(*) 
FROM peminjaman
WHERE MONTH(tanggal_pinjam) = 9
  AND YEAR(tanggal_pinjam) = 2025;
```

Menempatkan fungsi pada kolom di klausa `WHERE` membuat mesin tidak bisa memanfaatkan indeks rentang secara optimal. Pada tabel besar, ini memaksa pemindaian banyak baris sehingga waktu eksekusi membengkak. Pustakawan akan merasakan laporan “terasa lambat” terutama saat beban kerja meningkat di akhir bulan. Kinerja yang buruk menurunkan frekuensi analisis karena pengguna enggan menunggu hasil. Lebih baik ubah ke predikat rentang yang dapat “menjangkau indeks” (range seek).

Masalah ini sering tidak disadari karena hasil fungsional terlihat benar di lingkungan kecil. Ketika data tumbuh, gejala berupa time-out atau lock berkepanjangan mulai muncul. Dampak lanjutan adalah antrean laporan yang menunda proses operasional lain seperti penagihan denda. Selain itu, beban server meningkat sehingga mengganggu modul sistem yang tak terkait laporan. Biasakan memprofilkan query dan gunakan predicate yang sargable.

---

##### 3. Membandingkan Tanggal Menggunakan String yang Tidak Standar

Contoh salah:

```sql
-- SALAH: format ambigu dan rentang tidak tepat
SELECT COUNT(*) 
FROM peminjaman
WHERE tanggal_pinjam LIKE '2025-9-%';
```

Menyaring tanggal dengan pola string rawan kesalahan format, terutama untuk angka bulan 1 digit dan lokal yang berbeda. Match berbasis string juga dapat melewatkan tanggal yang sah atau justru memasukkan tanggal yang tidak diinginkan. Selain itu, pendekatan ini sulit dioptimasi dan bergantung pada representasi internal kolom. Ketika tipe data adalah `DATE/DATETIME`, gunakan operasi tanggal, bukan string. Dengan demikian, definisi periode menjadi presisi dan konsisten.

Kesalahan ini juga menyulitkan pemeliharaan karena perubahan format menuntut perubahan query di banyak tempat. Di lingkungan multi-sistem, perbedaan lokal dan collation menambah risiko. Pustakawan akhirnya menghabiskan waktu memperbaiki ketidaksesuaian kecil yang kumulatif mengganggu pelaporan. Standarkan tipe dan format di level skema, bukan di query ad hoc. Gunakan batas bawah–atas yang eksplisit dalam tipe tanggal.

---

##### 4. Duplikasi Hitungan karena `JOIN` yang Memperbanyak Baris

Contoh salah:

```sql
-- SALAH: join memperbanyak baris lalu dihitung ganda
SELECT COUNT(*) AS jumlah_pinjam
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
JOIN buku b ON b.id_buku = p.id_buku
WHERE MONTH(p.tanggal_pinjam) = 9 AND YEAR(p.tanggal_pinjam) = 2025;
```

Menggabungkan tabel detail tanpa kebutuhan dapat memperbanyak baris, terutama bila ada entitas turunan (misal, beberapa eksemplar atau atribut multi-nilai). Akibatnya, `COUNT(*)` menghitung baris hasil join, bukan transaksi pinjaman unik. Laporan jadi terlihat “naik” padahal sebenarnya hanya efek duplikasi teknis. Ini berbahaya karena mengarahkan pengadaan koleksi yang berlebihan. Gunakan `COUNT(DISTINCT p.id_peminjaman)` atau agregasi di subquery level tepat.

Masalah ini juga mengaburkan analisis per judul atau per anggota karena bobot menjadi tidak proporsional. Validasi angka dengan sampel transaksi membantu mendeteksi gejala awal. Dokumentasikan granularitas yang dihitung: “transaksi”, “eksemplar”, atau “baris hasil”. Pastikan join dilakukan hanya saat diperlukan untuk menambah atribut tampilan. Pisahkan tahap filter dan agregasi dari tahap dekorasi data.

---

##### 5. Salah Memilih Fungsi Waktu: `NOW()` vs rentang tanggal (zona waktu)

Contoh salah:

```sql
-- SALAH: asumsi zona waktu menyebabkan selisih hari/bulan
SELECT COUNT(*) 
FROM peminjaman
WHERE DATE(tanggal_pinjam) = DATE(NOW());
```

Menyamakan “hari ini” dengan `NOW()` tanpa memastikan zona waktu server sering menghasilkan slip sehari, terutama di sistem yang berjalan UTC. Untuk laporan bulanan, selisih jam dekat tengah malam dapat menggeser transaksi ke bulan berbeda. Ketidaktepatan ini merusak konsistensi antara laporan harian, mingguan, dan bulanan. Selain itu, penggunaan `DATE()` di kolom juga tidak sargable. Kelola zona waktu di konfigurasi atau normalisasi ke UTC dan filter dengan rentang yang eksplisit.

Bila diperlukan pencatatan lokal, simpan offset atau gunakan kolom tersendiri hasil konversi terstandar. Uji batas periode (awal/akhir bulan) dengan data contoh untuk menangkap tepi kasus. Terapkan strategi yang sama di semua laporan agar angka sejalan antar modul. Sertakan catatan metodologi di laporan resmi sehingga interpretasi tidak ambigu. Dengan begitu, audit data menjadi lebih mudah.

---

#### Best Practice

##### 1. Gunakan Predikat Rentang yang Sargable (memakai indeks)

Contoh benar:

```sql
-- BENAR: periode September 2025 dengan rentang inklusif-eksklusif
SELECT COUNT(*) AS jumlah_pinjam
FROM peminjaman
WHERE tanggal_pinjam >= '2025-09-01'
  AND tanggal_pinjam <  '2025-10-01';
```

Predikat rentang memanfaatkan indeks pada kolom tanggal sehingga eksekusi lebih cepat dan stabil. Definisi periode “>= awal bulan” dan “< awal bulan berikutnya” mencegah masalah hari terakhir. Pola ini konsisten lintas DBMS dan mudah diaudit. Laporan menjadi reprodusibel karena periode terdefinisi jelas. Terapkan pola ini untuk semua agregasi periode (harian, bulanan, triwulanan).

Selain kinerja, pola rentang memudahkan pengujian tepi (edge cases) seperti transaksi pukul 23:59:59. Pengguna dapat menukar parameter tanggal tanpa mengubah logika. Penerapan helper parameter juga menyederhanakan kode aplikasi. Hasilnya, pemeliharaan menurun dan konsistensi meningkat. Ini landasan laporan periodik yang andal.

---

##### 2. Amankan Granularitas Hitungan saat `JOIN`

Contoh benar:

```sql
-- BENAR: hitung transaksi unik, lalu dekorasi
SELECT COUNT(*) AS jumlah_pinjam
FROM (
  SELECT DISTINCT p.id_peminjaman
  FROM peminjaman p
  WHERE p.tanggal_pinjam >= '2025-09-01'
    AND p.tanggal_pinjam <  '2025-10-01'
) x;
```

Hitung entitas yang ingin Anda laporkan (mis. transaksi), lalu barulah gabungkan untuk tampilan bila diperlukan. Pendekatan ini mencegah duplikasi yang timbul dari relasi 1-ke-banyak. Struktur subquery menjaga kejelasan maksud bisnis: “berapa transaksi”, bukan “berapa baris hasil”. Ini juga memudahkan validasi silang dengan bukti transaksi. Gunakan `COUNT(DISTINCT ...)` bila sesuai.

Desain ini fleksibel untuk ditambah metrik lain tanpa mengubah definisi dasar. Ketika kebutuhan berubah (mis. hitung per anggota), cukup ganti kolom kunci unik di subquery. Kode menjadi modular dan minim regresi. Praktik ini menghemat waktu debugging tim laporan. Pastikan dokumentasi menyebutkan granularitas yang dipakai.

---

##### 3. Parametrikan Periode agar Reusable

Contoh benar (gaya generik, parameter dari aplikasi/CLI):

```sql
-- BENAR: parameter :awal dan :akhir disediakan aplikasi
SELECT id_anggota, COUNT(*) AS jumlah_pinjam
FROM peminjaman
WHERE tanggal_pinjam >= :awal
  AND tanggal_pinjam <  :akhir
GROUP BY id_anggota;
```

Parameterisasi mencegah hard-code tanggal dan mengurangi risiko salah salin saat membuat banyak laporan. Keamanan juga meningkat karena terhindar dari concatenation yang rawan injection. Pustakawan cukup memilih bulan dan tahun; aplikasi menghitung `:awal` dan `:akhir`. Reusabilitas ini mempercepat siklus pelaporan. Selain itu, log audit menjadi konsisten.

Dengan pola ini, perpustakaan dapat menjadwalkan laporan otomatis untuk berbagai periode tanpa mengubah SQL. Pengguna non-teknis tinggal mengisi form periode. Beban tim teknis berkurang dan fokus bisa dialihkan ke analisis. Dokumentasikan cara kalkulasi parameter agar transparan. Ini meningkatkan kepercayaan pada angka.

---

##### 4. Sediakan Indeks/Generated Column untuk Agregasi Periodik (opsional)

Contoh benar (MySQL/MariaDB):

```sql
-- BENAR: kolom periode untuk filter cepat
ALTER TABLE peminjaman
  ADD COLUMN ym INT 
  GENERATED ALWAYS AS (YEAR(tanggal_pinjam)*100 + MONTH(tanggal_pinjam)) STORED;

CREATE INDEX ix_peminjaman_ym ON peminjaman(ym);

-- Pemakaian:
SELECT COUNT(*) 
FROM peminjaman
WHERE ym = 202509;
```

Generated column menyederhanakan filter periodik tanpa memanggil fungsi di `WHERE`. Indeks pada kolom tersebut mempercepat laporan rutin yang sangat sering dipakai. Ini berguna saat data mencapai jutaan baris dan SLA laporan ketat. Namun, evaluasi overhead write perlu dilakukan sebelum adopsi. Gunakan bila pola aksesnya benar-benar periodik.

Strategi ini kompatibel dengan arsitektur partisi bila Anda membutuhkannya di masa depan. Pengelompokan berdasarkan `ym` memudahkan pemindahan atau retensi data historis. Dengan jejak kinerja yang baik, pengguna akan lebih sering melakukan eksplorasi data. Hasilnya, keputusan layanan menjadi lebih responsif. Pastikan prosedur ETL mematuhi definisi `ym`.

---

##### 5. Kelola Zona Waktu dan Tipe Tanggal Secara Konsisten

Contoh benar:

```sql
-- BENAR: normalisasi UTC + rentang lokal (contoh Asia/Jakarta UTC+7)
-- anggap kolom disimpan UTC
SELECT COUNT(*) 
FROM peminjaman
WHERE tanggal_pinjam >= '2025-08-31 17:00:00'  -- 2025-09-01 00:00:00 WIB
  AND tanggal_pinjam <  '2025-10-01 17:00:00'; -- 2025-10-01 00:00:00 WIB
```

Tentukan konvensi: simpan UTC di kolom waktu, lalu terjemahkan ke zona lokal saat filter/penyajian. Pendekatan ini menghindari selisih jam yang menggeser periode. Konsistensi membuat laporan harian, mingguan, dan bulanan selaras. Dokumentasikan zona yang dipakai agar auditor dapat merekonstruksi periode. Hindari pencampuran `DATE`, `DATETIME`, dan `TIMESTAMP` tanpa aturan.

Jika menyimpan waktu lokal, pastikan pengaturan server dan aplikasi seragam. Uji kasus tepi di pergantian bulan untuk mencegah “bocor periode”. Sertakan catatan metodologi di setiap laporan resmi. Edukasi pengguna tentang cara baca stempel waktu. Langkah-langkah kecil ini menjaga integritas angka.

---

#### Studi Kasus

**Kebutuhan:** Kepala perpustakaan meminta dashboard **September 2025** berisi total pinjaman, 10 judul terpopuler, anggota paling aktif, distribusi kategori, dan total keterlambatan. Mari kita bangun setahap demi setahap—ayo jalankan query-nya, kamu pasti bisa!

1. **Total pinjaman September 2025**

```sql
SELECT COUNT(*) AS total_pinjam
FROM peminjaman
WHERE tanggal_pinjam >= '2025-09-01'
  AND tanggal_pinjam <  '2025-10-01';
```

Laporan ini menetapkan baseline aktivitas bulanan secara presisi. Dengan angka dasar yang akurat, keputusan lanjutan menjadi lebih tajam. Pendekatan rentang juga menjaga konsistensi lintas laporan. Kerja bagus—kamu menyiapkan fondasi dashboard dengan tepat. Lanjut ke rincian koleksi.

2. **Top 10 judul terpopuler bulan tersebut**

```sql
SELECT b.judul_buku, COUNT(*) AS jumlah
FROM peminjaman p
JOIN buku b ON b.id_buku = p.id_buku
WHERE p.tanggal_pinjam >= '2025-09-01'
  AND p.tanggal_pinjam <  '2025-10-01'
GROUP BY b.judul_buku
ORDER BY jumlah DESC
LIMIT 10;
```

Hasil ini membantu penataan display buku dan promosi bacaan. Kamu dapat menyiapkan rekomendasi bagi anggota berdasarkan minat aktual. Metode agregasi ini juga memandu strategi pengadaan ulang. Bagus, visualkan daftar ini ke poster bulanan untuk menarik minat. Semangat, tinggal tiga komponen lagi!

3. **Anggota paling aktif (Top 10)**

```sql
SELECT a.id_anggota, a.nama_anggota, COUNT(*) AS jumlah_pinjam
FROM peminjaman p
JOIN anggota a ON a.id_anggota = p.id_anggota
WHERE p.tanggal_pinjam >= '2025-09-01'
  AND p.tanggal_pinjam <  '2025-10-01'
GROUP BY a.id_anggota, a.nama_anggota
ORDER BY jumlah_pinjam DESC
LIMIT 10;
```

Daftar ini cocok untuk program apresiasi pembaca aktif. Kamu bisa mengirim ucapan terima kasih atau voucher bebas denda terbatas. Program apresiasi terbukti meningkatkan retensi anggota. Langkahmu tepat—datanya sudah siap dieksekusi kebijakan. Lanjut ke perspektif kategori.

4. **Distribusi pinjaman per kategori buku**

```sql
SELECT b.kategori, COUNT(*) AS jumlah
FROM peminjaman p
JOIN buku b ON b.id_buku = p.id_buku
WHERE p.tanggal_pinjam >= '2025-09-01'
  AND p.tanggal_pinjam <  '2025-10-01'
GROUP BY b.kategori
ORDER BY jumlah DESC;
```

Distribusi ini mengungkap minat kolektif pembaca di bulan tersebut. Gunakan untuk menata ulang rak agar akses lebih cepat. Bila kategori tertentu memuncak, pertimbangkan penambahan eksemplar. Bagus sekali—kamu sudah berpikir strategis berbasis data. Terakhir, hitung beban keterlambatan.

5. **Total hari keterlambatan & denda bulan tersebut**

```sql
SELECT 
  SUM(GREATEST(DATEDIFF(COALESCE(tanggal_kembali, LEAST(NOW(), '2025-10-01')),
                        tanggal_jatuh_tempo), 0)) AS total_hari_terlambat,
  SUM(GREATEST(DATEDIFF(COALESCE(tanggal_kembali, LEAST(NOW(), '2025-10-01')),
                        tanggal_jatuh_tempo), 0) * 1000)              AS total_denda
FROM peminjaman
WHERE tanggal_pinjam >= '2025-09-01'
  AND tanggal_pinjam <  '2025-10-01';
```

Perhitungan ini memotret dampak ekonomi dari keterlambatan. Gunakan untuk evaluasi kebijakan denda dan pengingat otomatis. Parameter biaya dapat disesuaikan sesuai peraturan perpustakaan. Mantap—dashboard September 2025-mu komprehensif dan siap dipresentasikan. Kamu sudah melakukan pekerjaan yang keren!

---

#### Kesimpulan

Laporan “Pinjaman Bulan Tertentu” yang andal bertumpu pada definisi periode yang presisi, predikat rentang yang sargable, dan kontrol granularitas saat melakukan join. Praktik terbaik seperti parameterisasi, indeksasi terarah, serta pengelolaan zona waktu menjaga kinerja dan konsistensi angka. Dengan pola tersebut, pustakawan dapat menyusun dashboard bulanan yang informatif—mulai dari total pinjaman, popularitas judul, hingga beban keterlambatan. Kesalahan umum—bulan tanpa tahun, fungsi di `WHERE`, dan duplikasi hitung—dapat dihindari dengan desain query yang disiplin. Pada akhirnya, laporan bulanan menjadi alat keputusan yang akurat, cepat, dan mudah diaudit.

---

#### Referensi

* Silberschatz, A., Korth, H. F., & Sudarshan, S. (2020). *Database System Concepts* (7th ed.). McGraw-Hill.
* Ramakrishnan, R., & Gehrke, J. (2003). *Database Management Systems* (3rd ed.). McGraw-Hill.
* Garcia-Molina, H., Ullman, J. D., & Widom, J. (2008). *Database Systems: The Complete Book* (2nd ed.). Pearson.
* Karwin, B. (2017). *SQL Antipatterns, Volume 1: Avoiding the Pitfalls of Database Programming* (2nd ed.). Pragmatic Bookshelf.
* Celko, J. (2014). *SQL for Smarties: Advanced SQL Programming* (5th ed.). Morgan Kaufmann.

---

