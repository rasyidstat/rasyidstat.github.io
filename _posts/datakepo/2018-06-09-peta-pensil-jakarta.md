---
layout: datakepo
title: "Pencil Maps Pendatang Baru WNI ke DKI Jakarta"
short-title: "Pencil Maps Pendatang Baru WNI ke DKI Jakarta"
meta-title: "Pencil Maps Pendatang Baru WNI ke DKI Jakarta"
categories: datakepo
tags:
- Peta
type: post
img: "/images/datakepo/jkt-pencil/jakarta_pencil_map.png"
published: true
word-count: false
---

Terinspirasi dari tulisan [Colored Pencil Maps with R](https://rgeomatic.hypotheses.org/1333), gue pun tertarik untuk membuat serupa dengan menggunakan peta Indonesia. Agak sedikit berbeda dengan tulisan tersebut yang menggunakan visualisasi *base R*, di episode DataKepo kali ini, gue akan membuat peta ala pensil menggunakan `ggplot2`. 

Data yang akan gue pake adalah data jumlah pendatang baru WNI ke DKI Jakarta yang bisa diambil dari [sini](http://data.jakarta.go.id/dataset/jumlah-pendatang-baru-wni-dari-luar-dki-jakarta). 

## Persiapan

Beberapa `library` yang akan digunakan adalah:

```r
library(ggplot2)
library(nusantr)
library(sf)
library(dplyr)
library(cartography)
library(purrr)
library(ggsn)
```

`ggplot2` yang digunakan merupakan versi terbaru dengan fungsi `geom_sf` (akan tersedia di CRAN akhir Juni 2018 mendatang). `nusantr` digunakan untuk mendapatkan peta DKI Jakarta. `cartography` digunakan untuk mengubah peta biasa menjadi peta ala pensil. `ggsn` digunakan untuk menambahkan legenda skala peta menggunakan fungsi `scalebar` dan arah kompas menggunakan fungsi `north` yang akan ditambahkan ke elemen `ggplot2`.

```r
# data migrasi
metrics <- read.csv("http://data.jakarta.go.id/dataset/32b76674-064d-4d03-9429-58af35611a77/resource/4f6c0a04-b337-4a11-8641-6a03b2fe1a94/download/Data-penduduk-migrasi-masuk-DKI-Jakarta-per-kecamatan-2015.csv")
metrics <- metrics %>%
  group_by(kota_m = nama_kabupaten_atau_kota, kecamatan_m = nama_kecamatan) %>%
  summarise(cnt = sum(jumlah)) %>%
  ungroup() %>%
  filter(!grepl("seribu", tolower(kota_m)))

# data spasial/geometri
jkt <- id_map("jakarta", level = "kecamatan") %>%
  filter(kota != "Kepulauan Seribu") %>%
  st_transform("+proj=utm +zone=20 +datum=WGS84 +units=m +no_defs")

# penggabungan data
jkt <- jkt %>%
  mutate(kota_m = gsub("\\s+", "", toupper(kota)),
         kecamatan_m = gsub("\\s+", "", toupper(kecamatan))) %>%
  left_join(metrics %>%
              mutate(kota_m = gsub("\\s+", "", toupper(kota_m)),
                     kecamatan_m = gsub("\\s+", "", toupper(kecamatan_m)))) %>%
  select(-contains("_m")) %>%
  mutate(area = map_dbl(geometry, st_area),
         area = 10^-6 * area,
         cnt_per_km2 = cnt / area)

# data spasial/geometri ala pensil
jkt_pencil <- getPencilLayer(x = jkt, size = 250, lefthanded = FALSE)
jkt_pencil <- st_transform(jkt_pencil, 4326)
jkt <- st_transform(jkt, 4326)
jkt_center <- jkt %>%
  mutate(
    centroid = map(geometry, st_centroid),
    coord = map(centroid, st_coordinates),
    coord_x = map_dbl(coord, 1),
    coord_y = map_dbl(coord, 2)
  ) %>%
  as_tibble() %>%
  st_as_sf() %>%
  st_centroid()
```

Ada tiga `sf data.frame` yang dibentuk di atas yaitu:

* `jkt` data spasial poligon DKI Jakarta dengan metriks jumlah pendatang baru yang digabungkan dari data `metrics`
* `jkt_pencil` data spasial ala pensil yang didapatkan dari data `jkt`
* `jkt_center` data titik koordinat pusat tiap kecamatan yang diperoleh menggunakan fungsi `st_coordinates`

Dengan menggunakan fungsi `getPencilLayer`, data spasial poligon `jkt` akan berubah menjadi data spasial yang terdiri dari beberapa titik koordinat yang membentuk titik-titik seperti coretan pensil.

## Eksekusi

Dengan beberapa garis kode di bawah ini, kita sudah bisa mendapatkan visualisasi peta ala pensil.

```r
ggplot() +
  geom_sf(data = jkt_pencil, aes(color = cnt_per_km2)) + 
  geom_sf(data = jkt, fill = NA, color = "grey30", size = 0.1) +
  geom_sf(data = jkt_center, color = "grey30", size = 0.5) +
  ggrepel::geom_text_repel(data = jkt_center, 
                           aes(coord_x, coord_y, label = kecamatan),
                           color = "grey10",
                           family = "Nunito",
                           size = 3,
                           segment.colour = "grey30") +
  coord_sf(datum = NA) +
  mrsq::theme_nunito(legend = 4) +
  theme(axis.title = element_blank()) +
  labs(title = "Jumlah Pendatang Baru WNI ke DKI Jakarta",
       subtitle = "Tahun 2014",
       caption = "sumber: data.jakarta.go.id") +
  scale_color_gradient(expression(paste("Pendatang/", km^2)), 
                       low = "#DEEBF7", high = "#2171B5") +
  north(jkt, location = "bottomleft", scale = 0.1,
        anchor = c(x = 106.686, y = -6.362)) +
  scalebar(jkt, dist = 5, location = "bottomleft", 
           dd2km = TRUE, model = "WGS84", height = 0.02,
           box.fill = c("#9ECAE1", "grey60"), box.color = "grey100", 
           st.color = "grey30", family = "Nunito", st.size = 3)
```

<img src="/images/datakepo/jkt-pencil/jakarta_pencil_map.png">