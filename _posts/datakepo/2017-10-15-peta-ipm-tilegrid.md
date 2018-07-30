---
layout: datakepo
title: "Tile Grid Maps Indeks Pembangunan Manusia Indonesia per Provinsi"
short-title: "Tile Grid Maps IPM Indonesia per Provinsi"
meta-title: "Tile Grid Maps IPM Indonesia per Provinsi"
categories: datakepo
tags:
- Peta
type: post
img: "/images/datakepo/ipm-tilegrid/ipm-2016-peta-bin.png"
published: true
word-count: false
---

Di episode DataKepo kali ini, gue bakal mengulang kembali visualisasi data yang pernah gue buat sebelumnya di [sini]({{ site.url }}/blog/game-of-data/). Kali ini gue akan membuat visualisasi peta ala *tile grid* seperti yang dibikin oleh tim Lokadata Beritagar via Tableau di [sini](https://public.tableau.com/profile/beritagar#!/vizhome/PertumbuhanEkonomidesktop_0/Dashboard2). Data yang gue gunakan adalah data Indeks Pembangunan Manusia Indonesia per provinsi tahun 2016 yang didapatkan dari laman BPS di [sini](https://www.bps.go.id/linkTableDinamis/view/id/1211).

## *Tile Grid* di R

Ada beberapa *package* di R untuk mempermudah visualisasi peta ala *tile grid*, salah satunya `statebins` yang bisa dilihat di [sini](https://github.com/hrbrmstr/statebins). Sayangnya, peta hanya tersedia untuk daerah Amerika Serikat. Setelah menelaah isi kodingan *package* tersebut, ternyata membuat visualisasi peta ala *tile grid* tidaklah sulit. Gue butuh `geom_tile` yang ada di `ggplot2` sekaligus posisi provinsi X dan Y yang gue dapatkan dari [sini](https://public.tableau.com/profile/beritagar#!/vizhome/PertumbuhanEkonomidesktop_0/Dashboard2). 

Terlebih dahulu, gue akan membaca data IPM yang pernah gue taruh di Github gue.

```r
library(tidyverse)
# devtools::install_github("rasyidstat/nusantr")
library(nusantr)

df_ipm <- read_csv("https://raw.githubusercontent.com/rasyidstat/GoD/master/ipm/data/ipm_provinsi.csv") %>%
  gather(key=tahun, value=ipm, -1) %>%
  select(provinsi=1, everything()) %>%
  mutate(provinsi=case_when(grepl("Jakarta", .$provinsi)~"DKI Jakarta",
                            grepl("Yogyakarta", .$provinsi)~"DI Yogyakarta",
                            TRUE~.$provinsi),
         provinsi=gsub("Kep.", "Kepulauan", provinsi),
         tahun=as.numeric(tahun),
         ipm=as.numeric(ipm)) %>%
  mutate(ipm=ifelse(ipm==0, NA, ipm))
```

*package* `nusantr` yang baru-baru ini gue updet berguna sebagai data referensi `provinsi` yang berisi mengenai informasi posisi provinsi yang dinyatakan dalam kolom X dan Y.  FYI, terdapat fungsi lain yang ada di situ seperti fungsi untuk menerjemahkan nomor KTP alias NIK ke jenis kelamin, tanggal lahir dan lokasi.

Dengan beberapa baris kode di bawah ini, peta ala *tile grid* dapat dibuat dengan mudah. *Voila!*

```r
df_ipm %>%
  filter(tahun==2016) %>%
  inner_join(provinsi, by=c("provinsi"="provinsi2")) %>%
  ggplot(aes(X, Y, fill=ipm, label=provinsi_abb)) +
  geom_tile(size=6, width=0.95, height=0.95) + geom_text(family="DIN", color="white") +
  labs(title="Peta Indeks Pembangunan Manusia di Indonesia",
       subtitle="Per Provinsi (Tahun 2016)",
       caption="Visualisasi data oleh: Rasyid Ridha (@rasyidstat)
       http://rasyidridha.com
       Sumber data: BPS",
       x=NULL, y=NULL) +
  mrSQ::theme_din(legend = 3) +
  theme(panel.border = element_blank(),
        panel.grid = element_blank(),
        panel.background = element_blank(),
        axis.ticks = element_blank(),
        axis.text = element_blank(),
        legend.direction = "horizontal") +
  viridis::scale_fill_viridis("IPM")
```

<img src="/images/datakepo/ipm-tilegrid/ipm-2016-peta-bin.png">

Referensi lainnya mengenai *tile grid maps* bisa dilihat di [sini](https://forumone.com/ideas/good-data-visualization-practice-tile-grid-maps-0). 