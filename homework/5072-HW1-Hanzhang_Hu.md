``` r
library("rjson")
```

    ## Warning: 程辑包'rjson'是用R版本4.1.2 来建造的

``` r
library("tidyverse")
```

    ## Warning: 程辑包'tidyverse'是用R版本4.1.1 来建造的

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.1.1     v forcats 0.5.1

    ## Warning: 程辑包'ggplot2'是用R版本4.1.2 来建造的

    ## Warning: 程辑包'tibble'是用R版本4.1.2 来建造的

    ## Warning: 程辑包'tidyr'是用R版本4.1.1 来建造的

    ## Warning: 程辑包'readr'是用R版本4.1.2 来建造的

    ## Warning: 程辑包'dplyr'是用R版本4.1.1 来建造的

    ## Warning: 程辑包'forcats'是用R版本4.1.1 来建造的

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library("stringr")
library("plyr")
```

    ## ------------------------------------------------------------------------------

    ## You have loaded plyr after dplyr - this is likely to cause problems.
    ## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
    ## library(plyr); library(dplyr)

    ## ------------------------------------------------------------------------------

    ## 
    ## 载入程辑包：'plyr'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     arrange, count, desc, failwith, id, mutate, rename, summarise,
    ##     summarize

    ## The following object is masked from 'package:purrr':
    ## 
    ##     compact

``` r
library("ggwordcloud")
```

    ## Warning: 程辑包'ggwordcloud'是用R版本4.1.2 来建造的

# About this GitHub Project

I found [Chinese poetry
database](https://github.com/chinese-poetry/chinese-poetry) on
GitHub.This is a database full of Chinese ancient poetry. China has five
thousand years of history, so does Chinese poetry. Chinese ancient
poetry is a treasure of Chinese culture and even a treasure of the whole
world. We should carry forward Chinese ancient poetry. I really like

Although there are printed collections, most people do not have the
physical books. The electronic version is convenient for copying, so
this open source database was born. This database is distributed in JSON
format, making it easy for people to start their own projects.

### This Chinese poetry database includes:

-   The composition of Chinese Poetry Database:
    -   Poetry from Tang Dynasty
        -   55,000 poems
    -   Poetry from Song Dynasty
        -   280,000 poems
    -   Poetry from other dynasties
        -   no numeric information

### Why I choose this GitHub Project

Recently, I’m reading the one of Four Great Classical Novel, Three
Kingdoms. I really like that novel, and I really like Cao Cao, who is
the most wisdom man in the novel. So I chose the Cao Cao’s poetry in my
report, I analyzed Cao Cao peotry’s main title to know more about his
poems.

# About Three Kingdoms

### The History

The Three Kingdoms (simplified Chinese: 三国时代) from 220 to 280 AD was
the tripartite division of China among the states of Wei, Shu, and Wu.
The Three Kingdoms period started with the end of the Han dynasty and
was followed by the Jin dynasty.

### The Novel

Three Kingdoms (simplified Chinese) is a 14th-century historical novel
attributed to Luo Guanzhong. It is set in the turbulent years towards
the end of the Han dynasty and the Three Kingdoms period in Chinese
history, starting in 169 AD and ending with the reunification of the
land in 280 by Western Jin. The novel is based primarily on the Records
of the Three Kingdoms (三国志), written by Chen Shou.

# About Caocao

Cao Cao (Chinese: 曹操),courtesy name Mengde, was a Chinese poet,
statesman, and warlord. He was the penultimate grand chancellor of the
Eastern Han dynasty who rose to great power in the final years of the
dynasty. As one of the central figures of the Three Kingdoms period, he
laid the foundations for what was to become the state of Cao Wei and was
posthumously honored as “Emperor Wu of Wei” although he never officially
claimed the title Emperor of China or proclaimed himself “Son of Heaven”
during his lifetime

### Reference

1.  [Three Kingdoms](https://en.wikipedia.org/wiki/Three_Kingdoms)
2.  [Romance of the Three
    Kingdoms](https://en.wikipedia.org/wiki/Romance_of_the_Three_Kingdoms)
3.  [Cao Cao](https://en.wikipedia.org/wiki/Cao_Cao),
    <https://en.wikipedia.org/wiki/Cao_Cao>

``` r
json_file <- "https://raw.githubusercontent.com/chinese-poetry/chinese-poetry/master/caocaoshiji/caocao.json"
caocao_data <- fromJSON(paste(readLines(json_file), collapse=""))
```

    ## Warning in readLines(json_file): 读'https://raw.githubusercontent.com/chinese-
    ## poetry/chinese-poetry/master/caocaoshiji/caocao.json'时最后一行未遂

``` r
# extract title from Caocao's poems and create the title dataframe
title <- sapply(caocao_data, function(x) x [[1]])
title_frame <- tibble(title)

# use word() function from "stringr" package to keep the main poem titles and eliminate the sub titles，
# which eliminate the sub title after a space
main_titles <- word(title_frame[[1]], 1)

# count the frequency of the main title
title_freq <- count(word(title_frame[[1]], 1))

# arrange the frequency in a descending way
title_sort <- arrange(title_freq, desc(freq))
names(title_sort)[names(title_sort) == "x"] <- "Main_Title"
names(title_sort)[names(title_sort) == "freq"] <- "Frequency"
```

``` r
library(knitr)
```

    ## Warning: 程辑包'knitr'是用R版本4.1.1 来建造的

``` r
kable(title_sort, align = "lccrr")
```

| Main_Title | Frequency |
|:-----------|:---------:|
| 步出夏门行 |     3     |
| 气出唱     |     3     |
| 善哉行     |     3     |
| 短歌行     |     2     |
| 秋胡行     |     2     |
| 董逃歌词   |     1     |
| 度关山     |     1     |
| 对酒       |     1     |
| 观沧海     |     1     |
| 龟虽寿     |     1     |
| 蒿里行     |     1     |
| 精列       |     1     |
| 苦寒行     |     1     |
| 陌上桑     |     1     |
| 却东西门行 |     1     |
| 塘上行     |     1     |
| 薤露行     |     1     |
| 谣俗词     |     1     |

``` r
ggplot(title_sort, aes(x=Main_Title, y=Frequency, fill=Frequency)) + 
  geom_bar(stat = "identity") +
  theme(axis.text.x = element_text(angle = 0, vjust = 0.5, hjust=1)) + 
  coord_flip() +
  labs(title = "The Frequency of Caocao Poems' Titles",
              y = "Frequency", x = "Main Title") +
  scale_color_gradient(low = "blue", high = "orange")
```

![](5072-HW1-Hanzhang_Hu_files/figure-markdown_github/unnamed-chunk-5-1.png)

``` r
ggplot(title_sort, aes(label = Main_Title, size = Frequency, color = Frequency)) +
  geom_text_wordcloud() +
  scale_size_area(max_size = 10) +
  theme_minimal() + 
  scale_color_gradient(low = "blue", high = "orange") 
```

![](5072-HW1-Hanzhang_Hu_files/figure-markdown_github/unnamed-chunk-6-1.png)
