---
layout: datakepo
title: "Visualisasi Data: Text Mining Lirik Lagu Raisa"
short-title: "Text Mining Lirik Lagu Raisa"
meta-title: "Text Mining Lirik Lagu Raisa"
categories: datakepo
tags:
- Text Mining
type: post
img: "/images/datakepo/raisa/raisa.png"
published: true
word-count: false
---

Tanggal 3 September 2017 yang lalu, Raisa resmi berganti status. Selamat jalan Raisa! Selamat menempuh hidup baru!

Di episode DataKepo kali ini, gue bakalan melakukan sedikit analisis teks mengenai lirik lagu Raisa. Gue bakalan bermain dengan *package* R `tidytext` oleh [Julia Silge](https://juliasilge.com/) dan [David Robinson](http://varianceexplained.org/) yang sejalan dengan paham `tidyverse` oleh Hadley Wickham dan kawan-kawan. Di sini, gue bakal fokus ke visualisasi data teks meskipun ada banyak sekali analisis yang bisa dilakukan terkait dengan data teks seperti analisis sentimen, pemodelan topik, dan lain-lain. *Here we go!*

* TOC
{:toc}

## Data Preparation

Sebelum data disajikan ke meja makan, banyak persiapan yang mesti dilakukan. Pengambilan data dilakukan menggunakan `rvest` a.k.a. sup cantiknya R kalo di Python. Data diperoleh dari [liriklaguindonesia.net](https://liriklaguindonesia.net). Untuk lebih detail mengenai pengambilan data bisa ditengok di [sini](https://github.com/rasyidstat/datakepo-snippets/blob/master/raisa/lirik_extract.R)

```r
library(tidyverse)
library(tidytext)
library(extrafont)
# devtools::install_github("rasyidstat/mrSQ")
library(mrSQ)
loadfonts(quiet = TRUE)

# load data
load("data/df_raisa.Rda")
df <- df %>%
  select(-link, -source) 
sw <- readLines("sw.txt", warn=FALSE) %>%
  data_frame(word=.)
df
```

```r
## # A tibble: 792 x 5
##              title     p    pl     l
##              <chr> <int> <int> <int>
##  1 Anganku Anganmu     1     1     1
##  2 Anganku Anganmu     1     2     2
##  3 Anganku Anganmu     1     3     3
##  4 Anganku Anganmu     2     1     4
##  5 Anganku Anganmu     2     2     5
##  6 Anganku Anganmu     2     3     6
##  7 Anganku Anganmu     3     1     7
##  8 Anganku Anganmu     3     2     8
##  9 Anganku Anganmu     4     1     9
## 10 Anganku Anganmu     4     2    10
## # ... with 782 more rows, and 1 more variables: txt <chr>
```

Terdapat 29 lagu Raisa yang gue peroleh dengan total baris untuk semua lagu ada sebanyak 792 baris. 

## Data Visualization

```r
df_music <- df %>%
  mutate(word = stringr::str_count(txt, "\\w+"),
         title = gsub(" \\(.*", "", title)) %>%
  group_by(title) %>%
  summarise(p = max(p),
            l = max(l),
            w = sum(word))
df_music %>%
  top_n(15, w) %>%
  ggplot(aes(reorder(title, w), w)) + geom_col(fill="#74A9CF") + 
  coord_flip() + theme_din(grid="X") +
  labs(x=NULL, y=NULL) +
  scale_y_continuous(expand=c(0.01,0))
```

<img src="/images/datakepo/raisa/raisa-msc.png">

Plot di atas merupakan 15 lagu Raisa dengan jumlah kata terbanyak. Sesuai dengan judulnya, *Love You **Longer*** memiliki jumlah kata paling banyak dibandingkan lagu Raisa lainnya. 

### Word Cloud

```r
library(wordcloud)
df_token <- df %>%
  unnest_tokens(word, txt) %>%
  anti_join(sw) %>%
  anti_join(stop_words) %>%
  filter(nchar(word) >= 3, !grepl("you", word)) %>%
  mutate(word = ifelse(grepl("^[a-z]{3,}mu$", word), gsub("mu$", "", word), word),
         word = ifelse(grepl("^[a-z]{3,}ku$", word), gsub("ku$", "", word), word),
         word = ifelse(grepl("^[a-z]{3,}nya$", word), gsub("nya$", "", word), word))
df_word <- df_token %>%
  count(word) %>%
  arrange(-n)
wordcloud(df_word$word, df_word$n,
          random.order = FALSE,
          random.color = FALSE,
          max.words = 70,
          colors=brewer.pal(8, "BuPu"),
          family="DIN")
```

<img src="/images/datakepo/raisa/raisa.png">

Tidak bisa dipungkiri lagi, secara keseluruhan, cinta menjadi kata paling favorit. Secara lebih detail, apakah cinta menjadi kata paling favorit di setiap lagu?

```r
df_token %>%
  count(title, word) %>%
  group_by(title) %>%
  mutate(p = n/sum(n)) %>%
  top_n(1, n) %>%
  filter(word %in% c("cinta","love")) %>%
  arrange(-p) %>%
  mutate(p = scales::percent(p))
```

```
## # A tibble: 7 x 4
## # Groups:   title [7]
##                    title  word     n     p
##                    <chr> <chr> <int> <chr>
## 1            Could It Be  love    25   49%
## 2          Tentang Cinta cinta    33   33%
## 3 Apalah (Arti Menunggu) cinta    10 20.8%
## 4                Inginku cinta     5 10.4%
## 5         Cinta Sempurna cinta     5  9.8%
## 6             Kali Kedua cinta     5 8.33%
## 7        Love You Longer  love     6 7.41%
```

Dari 29 lagu, hanya 7 lagu dengan cinta/love sebagai kata yang paling sering muncul. *Tentang **Cinta***, sesuai judulnya, menoreh kata cinta paling banyak dibandingkan lagu lainnya. Dan setelah ditelusuri secara lebih mendalam, 15 dari 29 lagu mengandung kata cinta/love di dalamnya.

### tf-idf

*tf-idf* alias *term frequency-inverse document frequency* merupakan ukuran seberapa penting suatu kata terhadap sebuah dokumen dalam sekumpulan korpus. *tf* menunjukkan proporsi frekuensi kemunculan sebuah kata terhadap jumlah semua kata dalam sebuah dokumen. Sedangkan *idf* menunjukkan logaritma natural dari proporsi jumlah semua dokumen terhadap jumlah dokumen yang mengandung kata yang dimaksud. *tf-idf* merupakan perkalian antara keduanya. 

```r
df_token2 <- df %>%
  unnest_tokens(word, txt) %>%
  mutate(word = ifelse(grepl("^[a-z]{3,}mu$", word), gsub("mu$", "", word), word),
         word = ifelse(grepl("^[a-z]{3,}ku$", word), gsub("ku$", "", word), word),
         word = ifelse(grepl("^[a-z]{3,}nya$", word), gsub("nya$", "", word), word)) %>%
  count(title, word, sort=TRUE) %>%
  group_by(title) %>%
  mutate(total=sum(n)) %>%
  ungroup()

df_tfidf <- df_token2 %>%
  bind_tf_idf(word, title, n) %>%
  anti_join(sw) %>%
  anti_join(stop_words) %>%
  filter(nchar(word) >= 3, !grepl("you", word))

df_tfidf %>%
  mutate(title = gsub(" \\(.*", "", title)) %>%
  top_n(20, tf_idf) %>%
  arrange(-tf_idf) %>%
  ggplot(aes(reorder(word, tf_idf), tf_idf, fill=title)) + geom_col() +
  coord_flip() + theme_din(grid="X") +
  labs(x=NULL, y=NULL) +
  scale_y_continuous(expand=c(0.01,0)) +
  scale_fill_discrete(NULL) 
```

<img src="/images/datakepo/raisa/raisa-tf.png">

Dalam kasus di sini, dokumen merupakan lagu yang ada sebanyak 29. Plot di atas menampilkan 20 kata dengan *tf-idf* tertinggi dari semua lagu. 

### Network Graph

Ada banyak cara untuk memvisualisasikan *network graph* di R baik secara [dinamis maupun statis](http://kateto.net/network-visualization). Di sini, gue bakal fokus ke visualisasi *network graph* statis menggunakan `ggraph` karya [Thomas Lin Pedersen](http://www.data-imaginist.com/) yang sepaham dengan gaya `ggplot2`. 

```r
library(igraph)
library(ggraph)
# devtools::install_github("dgrtwo/widyr")
library(widyr)

df_pair <- df_token %>%
  pairwise_count(word, l, sort=TRUE)

df_graph <- df_pair %>%
  filter(n >= 10) %>%
  graph_from_data_frame()
V(df_graph)$freq <- degree(df_graph, mode = "all")

df_graph %>%
  ggraph(layout = "fr") +
  geom_edge_fan(aes(edge_width = n), alpha = 0.1, show.legend = FALSE) +
  geom_node_point(color = "#8C6BB1", aes(size = freq), show.legend = FALSE) +
  geom_node_text(aes(label = name, size = freq), vjust = 2, family = "DIN", show.legend = FALSE) +
  scale_size(range = c(3.5,6.5)) +
  theme_void()
```

<img src="/images/datakepo/raisa/raisa-nw.png">

Dalam kasus di sini, pasangan kata dibentuk pada tiap baris kalimat yang ada sebanyak 792 dengan menghilangkan *stopwords* terlebih dahulu. Sebagai contoh, pasangan kata yang terbentuk dari "budi pergi ke sekolah" adalah budi-pergi, budi-sekolah dan pergi-sekolah. Dengan menggunakan fungsi `pairwise_count` dalam *package* R `widyr` karya David Robinson, pasangan kata dapat dibentuk dengan mudah. 