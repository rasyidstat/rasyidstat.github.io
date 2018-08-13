---
layout: datakepo
title: "Eksplorasi Peta Rute Transjakarta 2018 menggunakan sf"
short-title: "Rute Transjakarta 2018"
meta-title: "Peta Rute Transjakarta"
categories: datakepo
tags:
- Peta
type: post
img: "/images/datakepo/tj/tj_route.png"
published: true
word-count: false
---

Di episode DataGue kali ini, gue akan mengeksplorasi data rute Transjakarta, moda transportasi bus di Jakarta dan sekitarnya. Dari tahun 2017, gue sudah penasaran mengenai data transportasi di Indonesia. Gue pun memulai pencarian data di situs [Jakarta Smart City](http://smartcity.jakarta.go.id/) dan [data.jakarta.go.id](http://data.jakarta.go.id/), sebuah situs data terbuka mengenai Jakarta. Sayangnya, data yang diberikan sudah kadaluarsa, tidak lengkap dan tidak sesuai harapan gue.

Barulah akhir-akhir ini, gue menemukan data Transjakarta sesuai harapan gue berdasarkan rekomendasi dari teman. Gue pun mendapatkan data rute Transjakarta dari laman Trafi di [sini](https://www.trafi.com/id/jakarta/transjakarta) per tanggal 25 Juni 2018. Gue pun perlu membersihkan data tersebut dan sekarang sudah bisa diakses di *[nusantr](https://github.com/rasyidstat/nusantr)* versi >=0.3.3 yang dapat dipasang dengan mudah via R melalui `devtools::instal_github("rasyidstat/nusantr")`.

Ada dua data *dataset* Transjakarta di *nusantr* yaitu:

* `transjakarta` data halte Transjakarta beserta posisi *longitude* dan *latitude*

```
transjakarta
#> # A tibble: 1,555 x 6
#>   halte_id         halte_name  latitude longitude corridor_cnt schedule_id
#>   <chr>            <chr>          <dbl>     <dbl>        <int> <list>     
#> 1 idjkb_1-10 Sari… Sarinah        -6.19      107.           10 <chr [10]> 
#> 2 idjkb_1-11 Bank… Bank Indon…    -6.18      107.            9 <chr [9]>  
#> 3 idjkb_1-12 Monas Monumen Na…    -6.18      107.           16 <chr [16]> 
#> 4 idjkb_1-13 Harm… Harmoni        -6.17      107.           18 <chr [18]> 
#> 5 idjkb_1-14 Sawa… Sawah Besar    -6.16      107.            5 <chr [5]>  
#> # ... with 1,550 more rows
```

* `transjakarta_route` data trayek Transjakarta beserta rute dan geometri dalam bentuk *sf data frame*

```
transjakarta_route
#> Simple feature collection with 482 features and 11 fields
#> geometry type:  LINESTRING
#> dimension:      XY
#> bbox:           xmin: 106.6 ymin: -6.396 xmax: 107 ymax: -6.091
#> epsg (SRID):    4326
#> proj4string:    +proj=longlat +datum=WGS84 +no_defs
#> # A tibble: 482 x 12
#>   transport_id schedule_id corridor_id corridor_name corridor_color
#>   <chr>        <chr>       <chr>       <chr>         <chr>         
#> 1 idjkb_brt    idjkb_1     1           Blok M - Kota D02027        
#> 2 idjkb_brt    idjkb_1     1           Blok M - Kota D02027        
#> 3 idjkb_brt    idjkb_1     1           Blok M - Kota D02027        
#> 4 idjkb_brt    idjkb_1     1           Blok M - Kota D02027        
#> 5 idjkb_brt    idjkb_1     1           Blok M - Kota D02027        
#> # ... with 477 more rows, and 7 more variables: route_id <chr>,
#> #   route_name <chr>, direction <int>, validity <chr>, is_main <lgl>,
#> #   is_main_reverse <lgl>, geometry <LINESTRING [°]>
```

## Visualisasi Data

Berikut adalah visualisasi rute Transjakarta yang dibuat dengan menggunakan `sf` dan `ggplot2` dalam beberapa baris kode di bawah ini.

```
library(nusantr) # version 0.3.3 get tj route
library(tidyverse)
library(sf)
library(mrsq)

# get route
tj <- transjakarta_route %>%
  filter(direction == 1,
         is_main == TRUE)

# color palette
tj_color <- paste0("#", tj$corridor_color)
names(tj_color) <- tj$corridor_id
bg_color <- "gray10"

# viz route data
ggplot() +
  geom_sf(data = tj, aes(color = corridor_id)) +
  coord_sf(datum = NA) +
  theme_nunito() +
  guides(color = FALSE) +
  scale_color_manual(values = tj_color) +
  labs(caption = "Rasyid Ridha (rasyidridha.com)
       source: trafi.com\ndate: 25 June 2018") +
  theme(panel.background = element_rect(fill = bg_color, color = bg_color),
        plot.background = element_rect(fill = bg_color, color = bg_color),
        strip.background = element_rect(fill = bg_color, color = bg_color),
        plot.caption = element_text(color = "white"))
```

<img src="/images/datakepo/tj/tj_route.png">	

### 10 Rute Terpanjang

Dari `dataset` Transjakarta, kita bisa mendapatkan rute terpanjang maupun terpendek menggunakan fungsi `st_length` dalam *package* `sf` untuk mengukur jarak trayek.

```
# longest / shortest route
tj <- tj %>%
  mutate(route_length = as.numeric(st_length(geometry)) * 10^-3)
tj_top10 <- tj %>%
  top_n(10, route_length)
tj_bottom10 <- tj %>%
  top_n(-10, route_length)

# viz top
tj_color <- paste0("#", tj_top10$corridor_color)
names(tj_color) <- tj_top10$corridor_id

tj_top10 %>%
  mutate(corridor_name = paste0(corridor_name, " [", corridor_id, "]")) %>%
  ggplot(aes(reorder(corridor_name, route_length), route_length, fill = corridor_id)) +
  geom_col() +
  coord_flip() +
  theme_nunito(grid = "") +
  labs(x = NULL, y = NULL) +
  guides(fill = FALSE) +
  scale_fill_manual(values = tj_color) +
  geom_text(aes(label = paste(round(route_length, 2), "km")),
            color = "white",
            family = "Neo Sans Pro",
            hjust = 1.2) +
  theme(axis.text.x = element_blank())
```

<img src="/images/datakepo/tj/tj_top10.png">

Rute terpanjang memiliki jarak 44,84 kilometer! yaitu rute Bekasi Timur - Kalideres. Apakah anda pernah mencoba halte tersebut? Atau bahkan mencobanya dari halte paling awal hingga paling akhir?

Sepanjang apakah jika kita memvisualisasikan 10 rute terpanjang tersebut ke dalam peta?

```
ggplot() +
  geom_sf(data = tj, color = "grey95") +
  geom_sf(data = tj_top10, aes(color = corridor_id)) +
  coord_sf(datum = NA) +
  theme_nunito() +
  guides(color = FALSE) +
  scale_color_manual(values = tj_color)
```

<img src="/images/datakepo/tj/tj_route_top10.png">

### 10 Rute Terpendek

Bagaimana dengan rute terpendek? Sependek apakah?

<img src="/images/datakepo/tj/tj_bottom10.png">

Rute terpendek memiliki jarak 1,88 km yang merupakan bus Transjakarta yang mengelilingi di kawasan Tanah Abang. 10 rute terpendek pun kebanyakan diisi oleh bus Transjakarta kecil dan pengumpan, kebanyakan merupakan trayek untuk rusun.

Sependek apakah jika kita memvisualisasikan 10 rute terpendek tersebut ke dalam peta?

<img src="/images/datakepo/tj/tj_route_bottom10.png">

Selain visualisasi di atas, ada beberapa pertanyaan lain yang mungkin bisa kita jawab seperti:

1. Kota atau daerah mana yang memiliki halte terbanyak?
2. Rute mana yang melewati kota atau daerah terbanyak?
3. Berapakah rata-rata jarak antar halte satu trayek?
4. Berapakah rata-rata jarak antar halte?
5. Halte mana yang memiliki trayek terbanyak?

Ke depannya, gue juga akan menggunakan `dataset` Transjakarta untuk memvisualisasikan rute-rute Transjakarta yang pernah gue naiki melalui canel DataGue.

*Be data-driven!*


