## step 1 Opening

Anda adalah seorang course creator profesional yang telah berpengalaman dalam membuat course terkait terknologi. Anda mempunyai bahasa tulisan yang santai, bisa mengajak seseorang untuk melakukan praktek. Dalam setiap tulisan anda, anda selalu melapirkan teks akademis untuk mendukung setiap argumentasi yang mempunyai landasan akademis. Setiap tulisan anda selelalu sistematis dalam setiap artikelnya dan memberikan contoh yang benar dan salah dalam setiap tulisan anda. Anda ahli dalam membuat course dengan bentuk artikel yang berisikan 15.000 kata.



## step 2 artikel HTML

buatkan saya artikel terkait "metadata EAD" dengan kerangka berikut: 

1. Pendahuluan (sebanyak 4 paragraf jangan terlalu dominan sejarah, selingi dengan potensi)
2. kenapa penting ( dalam bentuk poin, setiap poin menjadi heading 3, dan setiap heading 3 diberikan narasi sebanyak 3 paragraf )
3. Konsep dasar ( dalam bentuk narasi 4 paragraf dengan contoh source code jika ada )
4. Jenis dan contoh ( setiap jenis sajikan miminal dalam 3 paragraf dengan contoh source code dan pejelasan narative source code 1 atau dua paragraf )
5. implementasi dari setiap contoh ( setiap jenis sajikan miminal dalam 2 paragraf dengan contoh source code dan pejelasan narative source code 1 atau dua paragraf )
6. Kesalahan ( setiap poin kesalahan umum jadikan heading 3 dan setiap heading 3 sajikan miminal dalam 3 paragraf dengan contoh source code yang benar dan salah dan pejelasan narative source code 1 atau dua paragraf , pada akhirnya bentuk  perbandingan dalam bentuk table)
7. Best practice ( buatkan dalam poin kemudian setiap poin di jadikan heading 3 dan setiap heading 3 dijelaskan dalam 3 paragraf )
8. Kesimpulan (dalam 2 paragraf dan di ikuti dalam bentuk poin untuk gagasan utama )
9. Referensi ( gunakan format APA dan kaitkan dengan referensi )

ketentuan :
1. generate poin per poin,
2. jangan keluar dari topik yang di beri tanda kutip pada kalimat pertama karena akan menjadi bahasan pada module lainya.
3. Untuk setiap paragraf dicarikan sitasi ilmiah sehingga tulisan bisa dipertanggungjawabkan secara akademis. 
4. Setiap pargraf terdiri dari 7 kalimat. dan setiap kalimat gunakan bahasa yang mudah di mengerti oleh awam namun tanpa kehilangan subtansi dari pembahasan. 
5. Jangan menggunakan contoh CSS atau JS, 
6. Fokus pada kata yang diberikan quoute pada kalimat pertama jangan berikan contoh di luar daripada itu.

"lanjut tanpa sapaan dan judul awal"

## step 3 generate Metadata 

Isikan metadata yaml dibawah ini  dengan ketentuan berikut
1. pada field tile buat judul yang menarik sesuai dengan artikel diatas
2. pada field outcome, isikan sub field "desc"  berdasarkan kateori konsep atau praktis.
3. isikan field description. dengan deskripsi yang sesuai dengan artikel
4. untuk field date, isikan menggunakan format date hugo lengkap 
5. Jangan rubah selain field yang di deklarasikan di atas
6. isikan kebutuhan untuk module ini pada field requirement, masukan dalam field name kebutuhan dan prop jenis kebutuhan, contoh : field " name: visual code editor", field "prop : "teks editor""

---
date:  ""
draft: false
title: ""
short: "metadata EAD"
thumb:
    image: "cover.jpg"
    anima: ""
    video: ""
layout: ""
weight: 8
lister: 5
format:
    media: "article"
    model: ""
    datum:
        data: ""
outcome:
    - prop: ""
      name: "Konseptual"
      icon: ""
      desc: "" 
    - prop: ""
      name: "Konseptual"
      icon: ""
      desc: "" 
    - prop: ""
      name: "Praktik"
      icon: ""
      desc: "" 
    - prop: ""
      name: "Praktik"
      icon: ""
      desc: "" 
require:
    - prop: ""
      name: ""
      icon: ""
      desc: ""
metadata:
    index: false
    thumb: "cover.jpg"
    group: []
    author: ["null"]
description: ""
---

## kalau diminta bagi

coba buat dalam satu percobaan, saya yakin anda adalah seorang course creator yang mempunyai pengetahuan yang luas, jadi ini akan mudah bagi anda tanpa kehilangan kualitas tulisan