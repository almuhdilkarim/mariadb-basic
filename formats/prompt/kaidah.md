Oke, saya tambahkan **Kaidah Output** ke dalam dokumen outline supaya utuh. Berikut hasil revisinya:

---

# Kaidah Penyusunan Modul

## 1. Kaidah Modul Konseptual

* Wajib terdiri dari 7 subheading.
* Setiap subheading berisi minimal 5 paragraf.
* Setiap paragraf terdiri dari minimal 5 kalimat.
* Harus menyertakan contoh SQL atau analogi perpustakaan yang relevan.
* Harus ditutup dengan **Kesimpulan** (1 paragraf, minimal 5 kalimat).
* Harus ditutup dengan **Referensi** (buku/jurnal akademis berbeda dengan modul sebelumnya).

## 2. Kaidah Modul Praktikal

**Struktur modul:**

* **Pendahuluan** (5 paragraf × 5 kalimat).
* **Langkah-langkah Praktik** (5 paragraf × 5 kalimat, sertakan snippet SQL).
* **Kesalahan Umum** (5 subpoint, setiap subpoint = snippet SQL salah + 2 paragraf × 5 kalimat).
* **Best Practice** (5 subpoint, setiap subpoint = snippet SQL benar + 2 paragraf × 5 kalimat).
* **Studi Kasus** (5 paragraf × 5 kalimat, sertakan snippet SQL).
* **Kesimpulan** (1 paragraf × 5 kalimat).
* **Referensi** (berbeda dengan modul sebelumnya).

**Narasi praktik wajib ada:**

* Kalimat ajakan (*“Mari kita coba…”, “Sekarang jalankan query berikut…”*).
* Pujian kecil (*“Kerja bagus!”, “Langkahmu sudah tepat.”*).

## 3. Kaidah Penulisan Umum

* Tidak boleh ada pengulangan narasi, tapi narasi harus berkesinambungan antar modul.
* Setiap kode SQL harus diformat jelas agar bisa langsung diuji.
* **Kesalahan Umum** wajib ada di setiap modul praktikal.
* **Best Practice** wajib ada untuk memberi standar SQL profesional.
* Referensi harus selalu berbeda antar modul.
* Tidak ada salam pembuka/penutup. Materi langsung dimulai.

## 4. Kaidah Output

* Semua output modul ditulis dengan **format markdown**.

---

# 📘 Outline Course MariaDB (Pertemuan 7 – 15)

## Pertemuan 7 – Ringkasan Data (Praktikal)

1. Fungsi COUNT untuk menghitung data
2. Fungsi SUM, AVG, MIN, MAX
3. GROUP BY sederhana
4. HAVING vs WHERE
5. Praktik: laporan jumlah pinjaman per anggota

## Pertemuan 8 – Desain Database yang Baik (Konseptual)

1. Masalah duplikasi data
2. Normalisasi pengantar
3. Bentuk normal pertama (1NF)
4. Bentuk normal kedua (2NF)
5. Praktik: memperbaiki tabel perpustakaan

## Pertemuan 9 – Fitur Tambahan SQL (Praktikal)

1. Fungsi string (UPPER, LOWER, CONCAT)
2. Fungsi tanggal (NOW, YEAR, MONTH)
3. Fungsi kondisi (IF, CASE)
4. Praktik: laporan pinjaman bulan tertentu
5. Latihan mandiri: laporan keterlambatan

## Pertemuan 10 – View (Tabel Virtual) (Konseptual)

1. Apa itu View
2. Membuat View sederhana
3. Menggunakan View untuk laporan
4. Kapan menggunakan View
5. Praktik: View laporan pinjaman per bulan

## Pertemuan 11 – Keamanan Sederhana (Praktikal)

1. Membuat user baru
2. Memberi izin SELECT ke user tertentu
3. Memberi izin penuh ke admin
4. Simulasi login dengan user berbeda
5. Praktik: role `admin` dan `anggota`

## Pertemuan 12 – Backup & Restore (Praktikal)

1. Mengapa backup penting
2. Backup dengan `mysqldump`
3. Restore database dari file
4. Praktik: backup database perpustakaan
5. Latihan: simulasikan kehilangan data lalu restore

## Pertemuan 13 – Sedikit Tentang Performa (Konseptual)

1. Apa itu index (analogi daftar isi buku)
2. Membuat index sederhana
3. Melihat pengaruh index pada pencarian
4. Praktik: query dengan dan tanpa index
5. Latihan: tambahkan index ke kolom judul

## Pertemuan 14 – Otomatisasi Sederhana (Praktikal)

1. Apa itu prosedur (Stored Procedure)
2. Membuat prosedur sederhana
3. Apa itu trigger
4. Praktik: trigger update stok buku otomatis
5. Latihan: prosedur laporan harian

## Pertemuan 15 – Proyek Mini

1. Merancang database perpustakaan mini (final)
2. Mengisi data minimal 50 baris
3. Membuat laporan pinjaman bulanan
4. Membuat user admin & anggota dengan izin berbeda


---

Apakah kamu mau saya langsung mulai menulis **isi lengkap modul pertemuan 7** sesuai format markdown ini?
