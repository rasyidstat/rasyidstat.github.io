---
layout: notes
title: "Package Development"
categories: notes
tags:
- dev
type: post
---

Sudah saatnya gue mengembangkan *package* di R, membagikannya di Github dan kalo bisa publikasikan di CRAN (Keran)! 

* `smartr` (current version 0.2.7, internal use only)
* `nusantr`
* `mrsq`
* `jakartapi`
* `qurancorpus`
* `liburrr`

Sejauh ini gue udah bikin *library* internal di kantor dengan nama `smartr` untuk memudahkan pekerjaan gue sehari-hari. 
Fungsinya adalah untuk koneksi *database*, visualisasi `ggplot2` dengan *font* DIN, referensi data cepat dan fungsi-fungsi kustom lainnya.
Gue pun punya dua opsi untuk mengambil data dari *database* yaitu dengan menggunakan fungsi biasa `sf_query` atau menggunakan fungsi `sf_query_` yang menangkap log transaksi kueri dan menyimpan data secara internal dengan format `feather` sehingga kalau kueri tersebut sudah pernah diinisiasi, gue ga perlu minta ulang data yang ada di Hadoop sono, tinggal baca data internal yang udah ditulis sebelumnya :))

```r
library(smartr)
df <- sf_query("SELECT * FROM db.tbl LIMIT 100")

# this will log query history and save the output data into "test.feather"
df <- sf_query_("SELECT * FROM db.tbl LIMIT 100", tbl="test")

# you also can pass parameters to the query
dtid <- "20170101"
df <- sf_query_("SELECT * FROM db.tbl WHERE dt='%s'", dtid, tbl="test_params")

# you can also do it with magrittr pipe
"SELECT * 
FROM db.tbl WHERE dt='%s'" %>%
sf_query(dtid, tbl="test_params") -> df
```





