---
layout: datakepo
title: "Kenalan Sama Nakama, Yuk!"
categories: datakepo
tags:
- R
- data viz
type: post
img: /images/kenalan-sama-nakama-yuk/1.png
pbulished: false
---

Pada episode DataKepo kali ini, kita akan berkenalan dengan Nakama, julukan dari pegawai Tokopedia. Saya pun melakukan analisis dan eksplorasi data yang diperoleh dari laman Tokopedia, tepatnya di [https://www.tokopedia.com/team/](https://www.tokopedia.com/team/). 

Analisis mengenai topik ini sudah pernah diinisiasi hampir satu tahun yang lalu (September 2016). Topik ini kembali saya angkat melalui canel DataKepo dengan menggunakan data terbaru (Maret 2017). Analisis terdahulu dapat disaksikan pada laman [ini](). Melihat kodingan saya kala itu, saya masih menggunakan **base R** dengan sedikit sentuhan `dplyr`. Anehnya, saya harus menggunakan `dplyr::` karena terjadi konflik antar **package** (entahlah ada konflik apa kala itu). Visualisasi data yang saya gunakan pun masih menggunakan `ggplot2` mentah, berbeda dengan sekarang yang menggunakan **font** maupun warna sehingga visualisasi menjadi lebih mantap.

Saya merupakan tipe **data scientist** yang terkadang lebih condong untuk mendapatkan datanya terlebih dahulu baru bertanya dengan berbagai kemungkinan, bukan bertanya terlebih dahulu baru mencari data yang relevan. Dengan data yang diperoleh dari laman [https://www.tokopedia.com/team/](https://www.tokopedia.com/team/), sejauh mana kita bisa "kepo" hal-hal mengenai Nakama? 

## Nakama boys or Nakama girls?

Dari data yang diperoleh, tidak ada keterangan apakah sang Nakama merupakan seorang pria maupun wanita. Analisis terdahulu sempat terhenti di bagian **Gender Prediction**. Kala itu, saya ingin memprediksi jenis kelamin Nakama berdasarkan nama lengkap mereka. Untuk membuat model tersebut, saya membutuhkan data nama  Indonesia yang sudah dilabel untuk laki-laki maupun perempuan sebagai **data training**. Fitur yang bisa diekstrak bisa berdasarkan frekuensi kemunculan kata atau bahkan tokenisasi hingga ke bagian terkecil yaitu dari pola huruf per huruf. Alternatif lainnya adalah menggunakan API untuk memberikan skoring untuk tiap kata-kata pada nama yang selanjutnya dirata-rata secara berimbang maupun tidak. 

Saya pun menemukan alternatif lain yaitu dengan menggunakan URL foto Nakama yang tersedia di laman tersebut. Dengan menggunakan API dari Microsoft, saya bisa memprediksi jenis kelamin berdasarkan foto. Selain itu, ada berbagai keluaran lainnya yang didapatkan selain jenis kelamin seperti umur, skoring ekspresi wajah seperi marah, senang, sedih maupun netral. Alangkah baiknya memang apabila saya bisa membuat model sendiri. Sempat terpikir, keluaran yang didapatkan bisa jauh lebih dalam seperti mengetahui apakah dalam foto tersebut seseorang menggunakan kacamata atau tidak, menggunakan hijab atau tidak bagi yang perempuan, dan lain-lain. Saya yakin dengan prediksi yang didapatkan untuk menentukan jenis kelamin, namun tidak untuk umur, karena manusia biasa pun memang sangat sulit untuk memprediksi umur hanya berdasarkan tampilan saja.

## Kapan tanggal resmi calon Nakama menjadi Nakama?

## Apa hobi dan kesukaan Nakama?






