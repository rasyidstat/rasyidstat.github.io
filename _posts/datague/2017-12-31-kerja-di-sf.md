---
layout: datague
title: "2017 in Review: Bermain dengan Data di SF"
short-title: "Bermain dengan Data di SF"
meta-title: "Bermain dengan Data di SF"
categories: datague
tags:
- Yearly
type: post
img: /images/datague/work-stat/query.png
published: true
related: false
period: 2017
word-count: true
---

Tidak terasa setahun sudah terlewati. Tahun 2017 adalah tahun yang penuh dengan berbagai macam data. Selain memang berprofesi sebagai tukang data, gue juga menyempatkan diri untuk bermain dengan data di luar profesi, salah satunya adalah data di kehidupan gue. Di awal tahun 2017, gue memang sudah berkomitmen untuk mengumpulkan berbagai macam data di kehidupan gue seperti data pengeluaran, transportasi, dan lain-lain. 

Di sini, gue akan menilik data gue di dunia kerja untuk menjawab apa aja yang udah gue kerjain di SF selama tahun 2017 ini.

## Berapa baris kode?

Ini metriks yang cukup *tricky* karena semakin banyak baris kode yang lu tulis, bukan berarti lu semakin produktif. Bisa jadi baris kode yang lu bikin banyak yang *spam* dan ga efisien. Namun seenggaknya, gue bisa mendapatkan gambaran yang tangibel dengan metriks yang terukur. Perlu digarisbawahi, gue hanya menghitung banyaknya baris kode untuk kodingan yang gue bikin lewat R doang (file berformat `.R` dan `.Rmd`), belum termasuk baris kode file berformat `.sql`, `.sh` dan lain sebagainya. Gue menghitung banyaknya baris kode tersebut dengan menggunakan *script* R di [sini](https://github.com/rasyidstat/codestats).

- **333** *file* yang terdiri dari **176** *file* R dan **157** *file* Rmd
- **27023** baris kode (**2205** atau 8.16% nya merupakan baris kode *comment*)
- **9,87** *chunk* dan **79,34** kata per *file* Rmd 

## `dplyr` *is my buddy*

Gue sering menggunakan `dplyr` untuk melakukan manipulasi data di R setelah mengambil data terlebih dahulu dari *database* menggunakan SQL. Gue sudah menggunakan **5383 *pipe*** alias `%>%` dari 333 kodingan R dan Rmd yang gue bikin di SF. Di bawah ini merupakan plot `dplyr` *verb* yang sering gue pake.

<img src="/images/datague/work-stat/dplyr.png">

## Berapa kali *query*?

Sejak Juli 2017, gue merekam setiap kali *query* dikirim via R melalui fungsi tambahan di modul *smartr*. Banyaknya *query* yang gue eksekusi hanya terhitung jika gue menggunakan R untuk mendapatkan data dari *database*, ga termasuk *query* yang ada di Hive CLI. 

Secara keseluruhan, gue sudah mengirimkan **4855** *query* yang berhasil dieksekusi via R sepanjang 3 Juli 2017 - 27 November 2017 (148 hari yang terdiri dari 102 hari aktif, 4 hari nonaktif dan 42 hari *weekend*). *Query* tersebut terdiri dari enam tipe yaitu: 

- `glance` *query* "select \* from tbl limit 10" atau "select count(\*) from tbl"
- `query` *query* umum
- `table` menunjukkan daftar tabel dalam *database*
- `usage` *query* khusus 
- `describe` untuk menunjukkan skema tabel
- `profile` *query* khusus

Gue kebanyakan menggunakan *query* secara umum dan mengecek isi tabel secara cepat menggunakan *query* `glance`, dengan detail yang bisa dilihat melalui plot di bawah ini.

<img src="/images/datague/work-stat/query-ovw.png">

Berikut merupakan plot banyaknya *query* umum harian yang totalnya ada sebanyak **2181** *query* dengan rata-rata 21,38 *query* per hari yang aktif.

<img src="/images/datague/work-stat/query.png">

*In a nutshell*, gue sudah menghasilkan >25 ribu baris kode R yang berisikan >4 ribu `%>%` dalam satu tahun dan >2 ribu *query* dalam satu semester di SF. Selain menjadi pengguna R, gue juga mencoba membuat *package* di R seperti `smartr` dan `nusantr` di tahun 2017. Gue harap gue bisa lebih aktif dan produktif lagi di tahun 2018 dengan menghasilkan lebih banyak baris kode dan *package* R.








