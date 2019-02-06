---
title: "README"
author: "Team 8"
date: "2/6/2019"
output: 
      html_document:
        keep_md: True
---



# STAT585_LAB1

## Team Members
- Li Jing
- Zhang Zerui
- Qiao Yang


## Buid a book

It said it has no package called "emo".

I found the package homepage on GitHub: https://github.com/hadley/emo. It provides the installation method: `devtools::install_github("hadley/emo")`.

'pseudo-class'


## Weather station


```r
library(dplyr)
library(readr)
library(ggplot2)
library(stringr)
library(ggmap)

ushcn2 <- read_fwf("ushcn-v2.5-stations.txt", fwf_cols(ID = c(1, 11), LATITUDE = c(13, 20), LONGITUDE = c(22, 30), ELEVATION = c(32, 37), STATE = c(39, 40), NAME = c(42, 71), COMPONENT1 = c(73, 78), COMPONENT2 = c(80, 85), COMPONENT3 = c(87, 92), UTC = c(94, 95)))

us <- map_data("state")
statename <- data.frame(STATE = state.abb, region = tolower(state.name), stringsAsFactors = FALSE)
us2 <- us %>%
  left_join(statename,"region") %>%
  left_join(unique(ushcn2[,c(5,10)]),by="STATE")

theme_set(theme_bw(16))
ushcn2 %>%
  ggplot(aes(x=LONGITUDE, y=LATITUDE)) +
  geom_polygon(data=us2, aes(x=long, y=lat, group = group, fill=UTC),colour="black") +
  #geom_polygon(aes(x=LONGITUDE, y=LATITUDE, group=UTC, fill=UTC)) +
  scale_fill_gradient(low = "yellow", high = "orange") +
  geom_point(aes(colour=ELEVATION)) + 
  scale_color_continuous(low="blue", high="red") +
  ggtitle("US map with elevation and time zone")
```

![](README_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

  
## More weather data

```r
raw <- untar("ushcn.tavg.latest.raw.tar.gz", list = T)
paste0("There are ",length(raw)," of files in the zip file")
```

```
## [1] "There are 1219 of files in the zip file"
```

```r
ID <- ushcn2 %>%
  filter(str_detect(NAME, fixed("fort dodge", ignore_case = T)), str_detect(STATE, "IA")) %>%
  .$ID %>% .[1]
paste0("The file location and name of Fort Dodge, IA is '", str_detect(raw, ID) %>% raw[.],"'")
```

```
## [1] "The file location and name of Fort Dodge, IA is './ushcn.v2.5.5.20190130/USH00132999.raw.tavg'"
```




=======
## Link
[https://github.com/lijing28101/STAT585_LAB1](https://github.com/lijing28101/STAT585_LAB1)
>>>>>>> a4082a4fd6525e094fe4566ca42d0892c0650d93
  

