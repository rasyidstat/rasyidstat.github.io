---
layout: datague
title: "2017 in Review: Let's Have a Lunch!"
short-title: "Let's Have a Lunch!"
meta-title: "Let's Have a Lunch!"
categories: datague
tags:
- Lunch
- Yearly
type: post
img: /images/datague/makan-siang/whom-free-1.png
published: true
related: false
period: 01/01/2017 - 30/11/2017
word-count: true
---

Di tahun 2017, selain merekap data transportasi, gue juga merekap data lainnya seperti data makan secara lebih rinci. Di tahun-tahun sebelumnya, gue sudah merekap data pengeluaran harian. Ke depannya, gue akan mengubah model data pengeluaran gue menjadi model data transaksional yang menjelaskan aktivitas harian gue secara lebih rinci dan proses perekapan dilakukan melalui perangkat *mobile*, kemudian data disinkronisasikan dan disimpan via *cloud*.

Bukan tidak mungkin di masa mendatang nanti, arah analisis data untuk pengambilan keputusan bukan untuk organisasi saja melainkan untuk individu, terlebih lagi dengan adanya IoT dan alat sensor yang merekam berbagai data di kehidupan kita secara otomatis.

> *I collect my own data because I am curious with the data of my life.*

## Intro

Di sini, gue akan fokus menggunakan data makan siang gue pas hari kerja. Ada tiga pertanyaan yang akan gue telusuri jawabannya yaitu:

- [Makan dengan siapa?](#makan-dengan-siapa)
- [Makan di mana?](#makan-di-mana)
- [Makan apa?](#makan-apa)

Data yang gue kumpulkan memiliki lima kolom yaitu: `date`, `food`, `place`, `with`, `price` dengan contoh isian nilai untuk kolom `with` yaitu "Kolega A, Kolega B, Kolega C" sedangkan kolom lainnya

- `date` tanggal makan siang
- `food` makanan yang dimakan (contoh: "Nasi Goreng", "Nasi Ayam Bakar")
- `place` lokasi makan siang (contoh: "Toko A Mall A")
- `with` kolega makan siang (contoh: "Kolega A, Kolega B, Kolega C", apabila makan sendiri diisi dengan "-")
- `price` harga makan siang dalam rupiah (contoh: 25000, 15000, apabila makan siang tanpa bayar diisi dengan 0)

Sedikut gambaran umum, dari tanggal 1 Januari 2017 - 30 November 2017, 65% makan siang pas hari kerja merupakan makan siang bareng. Teman makan siang bareng pas hari kerja ada sebanyak 39 kolega dengan 58 kombinasi unik.

<img src="/images/datague/makan-siang/intro-dist-1.png">

Saat makan siang bareng, gue paling banyak makan siang bareng dengan satu kolega saja.

<img src="/images/datague/makan-siang/intro-free-1.png">

Pas pertengahan tahun, dua bulan sebelum dan sesudah bulan puasa merupakan bulan di mana persentase makan siang bareng gue engga lebih dari 50%. Gue lebih sering makan siang bareng pas awal tahun dan akhir tahun.

Selanjutnya gue akan masuk ke inti pembahasan yaitu *siapa*, *di mana* dan *apa*.

## Makan dengan siapa?

<img src="/images/datague/makan-siang/whom-1.png">

Lima nama teratas merupakan kolega satu tim. Gue pernah makan dengan kolega yang sama lebih dari 60% dari semua makan siang bareng yang pernah gue lakukan di hari kerja. 

<img src="/images/datague/makan-siang/whom-free-1.png">

Semakin banyak teman yang ikut makan siang bareng, peluang buat makan gratis semakin tinggi? *Not at all*. Hal ini disebabkan karena makan siang bareng dengan jumlah anggota tim yang lebih banyak cenderung dikarenakan ada anggota yang ingin mentraktir dan mengajak anggota tim lebih banyak. 

Gue bisa saja membuat model untuk memprediksi apakah makan siang gue bakal gratis atau tidak berdasarkan variabel-variabel lain selain jumlah anggota tim yang ikut seperti siapa saja anggota tim yang ikut maupun tanggal dan hari makan siang.

<iframe src="/html/lunch/lunch_2017.html" width="100%" height="350px"></iframe>

Dari data makan siang yang gue peroleh, gue bisa membuat *network graph* kolega-kolega yang pernah makan siang bareng gue. Dari data makan siang yang gue peroleh terdapat dua kelompok makan siang di mana ada dua kolega dari CE yang menjadi penghubung ataupun irisan. Kasus visualisasi seperti ini bisa diterapkan untuk data lainnya seperti data CDR (*call detail records*), surel, sosial media, hubungan kata, pembelian barang (*market basket*) dan lain-lain. 

## Makan di mana?

<img src="/images/datague/makan-siang/where-1.png">

Ketika gue makan siang ga bareng yang lain, gue cenderung membeli di satu tempat tertentu dan sebisa mungkin tempatnya paling dekat dengan kantor.

## Makan apa?

<img src="/images/datague/makan-siang/what-1.png">

Empat jenis lauk makanan yang sering gue makan adalah ayam, telur, rendang dan ikan. Makanan dengan lauk ayam bisa beraneka ragam seperti ayam goreng, ayam bakar maupun sate ayam. Itulah mengapa frekuensinya jauh lebih banyak dibandingkan lauk makanan yang lain.

## Outro

Sebagai seorang antusias data, gue hanya penasaran dengan data-data yang bertebaran di hidup ini. Tidak terasa satu tahun sudah terlewati. Data merupakan fakta dan bisa menjadi sajian cerita yang unik dan menarik. Data pun bisa menjadi pelajaran untuk masa mendatang yang lebih baik.