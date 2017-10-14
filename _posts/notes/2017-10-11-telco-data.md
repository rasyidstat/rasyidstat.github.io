---
layout: notes
title: "Data Science in Telecommunication"
categories: notes
tags:
- reference
type: post
publish: false
---

Sudah setahun lebih gue berurusan dengan data di industri telekomunikasi. Gue menganalisis data telco untuk pertama kalinya pada bulan Agustus 2016 yang lalu. Kala itu, gue harus berpikir keras bagaimana menggunakan laptop RAM 8GB untuk melakukan analisis data transaksi yang besarnya bisa lebih dari 100GB. Gue sedikit terkejut melihat besaran datanya karena sebelum-sebelumnya gue cuma mainin data yang besarnya engga lebih dari 10GB. Apalagi pas gue masih duduk di bangku kuliah dulu yang datanya bahkan ga nyampe 1MB. Terjun langsung di dunia telco pada Desember 2016, 100GB belum ada apa-apanya dibandingkan dengan besaran data yang bisa mencapai skala TB. Gue pun berkata, "Oh ini toh yang namanya Big Data yang sering diomongi orang pas awal-awal 2015 dulu".

Inilah pekerjaan sehari-hari gue, jadi kuli data besar. Gue harus melakukan transformasi data dari suatu bentuk ke bentuk lainnya agar selanjutnya bisa gue olah di R tercinta gue.

## Customer Experience Modeling

## Mobility and Spatial Analytics

Analisis mobilitas. Ini yang gue suka dari data telco! Meskipun bukan posisi pas kayak GPS namun seenggaknya hal tersebut bisa menggambarkan mobilitas pengguna dan preferensinya. Selain posisi yang seutuhnya tidak pas sesuai lokasi TKP, terkadang pengguna tidak melulu aktif menggunakan telepon selulernya sehingga terdapat *missingness of data* di suatu titik atau rentang waktu tertentu. Ini memang menjadi sebuah tantangan dan menjadi sebuah penelitian tersendiri di dunia telco. Ya, ada banyak sekali penelitian menarik dengan menggunakan data telco bahkan ada konferensi tahunannya yang bernama NetMob (*Conference on the Analysis of Mobile Phone Datasets*)

- Prediksi lokasi tempat tinggal dan tempat kerja pengguna
- Prediksi lokasi pengguna pas mudik
- Prediksi tingkat kemacetan
- Prediksi pengguna naik apa pas di jalan (KRL, busway, mobil pribadi, jalan kaki, dll.)
- Prediksi apakah di suatu lokasi ada acara tertentu (bisa juga disanding dengan analisis anomali)
- Prediksi musiman pas musim liburan, lokasi mal, bandara, tempat wisata, dll.

## Customer 360

Dengan perpaduan antara data engineer/architect, ML engineer/data scientist dan software engineer/web developer, gue kira dasbor Customer 360 akan menjadi sesuatu yang sangat wow! Kalo dari segi data engineering, untuk membuat data customer 360, data didapatkan dari berbagai macam sumber dan kemudian disatukan dalam sebuah database dokumen kayak MongoDB. Peran data scientist adalah memperkaya data yang ada dengan cara melakukan prediksi atau clustering/segmentation. Kombinasi data engineer dan data scientist di sini sangatlah penting sebelum data disajikan dalam sebuah produk data yang dibuat oleh software engineer atau web developer. Setelah itu pengguna bisnis bisa menggunakan produk data untuk kepentingan bisnis seperti melakukan *campaign*, *targeted ads* dan berbagai hal lainnya yang masih perlu dilakukan integrasi. Tidak berhenti di situ, tiap *campaign* yang dilakukan selalu dilakukan pengukuran berikutnya (*AB Testing*). Dari data, aksi dan kembali lagi muncul data baru yang kemudian akan berguna untuk aksi berikutnya, dan selanjutnya diiterasi terus-menerus. *This is the art on how to use your data*, *you play with your data, team up with your team to solve a problem, explore, act, monitor and improve!*

https://www.slideshare.net/cloudera/using-big-data-to-drive-customer-360

Di industri telekomunikasi, customer 360 bisa berisi informasi seperti:

- Demografi pelanggan (bisa dilakukan prediksi apabila ada data yang belum diketahui)
- Histori pembayaran (dapat digunakan untuk menentukan tingkat keuangan pengguna)
- Preferensi (berdasarkan segmentasi yang sudah dibuat)
- Penggunaan data (ditinjau dari aplikasi, waktu penggunaan, tingkah laku dan lain-lain)
- Mobilitas
- Skoring *churn*
- Umur pemakaian, tenor, ARPU, *lifetime value*
- Data *customer care*, media sosial (sentimen, hobi)
- *Social network* dengan pelanggan lain

Referensi lain:
- [http://stats.research.att.com/research.php](http://stats.research.att.com/research.php)
- [https://www.slideshare.net/cloudera/using-big-data-to-drive-customer-360](https://www.slideshare.net/cloudera/using-big-data-to-drive-customer-360)



