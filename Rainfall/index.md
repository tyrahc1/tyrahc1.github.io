---
name: Tyrah Cobb-Davis
title: "rainovertime_2019"
output: pdf_document
---

## Loading the necessary library packages needed.

```{r}
library(dplyr)
library(tmap)
library(ggplot2)
library(cowplot)
library(magick)
library(sf)
library(readr)
library(tidyr)
```

## Uploading rainfall data and the corresponding lat/long locations from another data file

```{r}
#this is pulling in the rainfall data for 2019, information collected every 15min
rainfall_data <- read_csv("/Users/Tyrah/adv GIS classwork/finalproject_687/BaltCity2019_Tyrah_finalproject.csv")

#this is the location of the pixels collecting the rainfall data
pixels_latlong <- read_csv("/Users/Tyrah/adv GIS classwork/finalproject_687/Balt_latlong.csv")

# this is getting the geometries of the pixels
# pixel_location <- st_as_sf(pixels_latlong, coords = c("longitude", "latitude"),  crs = 4326)
# plot(pixel_location$geometry)
```

```{r}
rainfall_data_transposed <- rainfall_data %>% pivot_longer(cols = 3:244, names_to = "gridnum", values_to = "rainmm")

#had to change this column to be of character type (original a double), so that it could join with the gridnum column in another dataframe
pixels_latlong$PixelNumber <- as.character(pixels_latlong$PixelNumber)

rainwithlatlong <- left_join(rainfall_data_transposed, pixels_latlong, by = c("gridnum" = "PixelNumber")) #joining the transposed rainfall data to lat long to give it geometry

rainwithlatlong$gridnum <- as.vector(rainwithlatlong$gridnum)

#if i dont use the removed zeros in the group by can also use rainwithlatlong
removed_zeros <- filter(rainwithlatlong, rainmm > 0)
dayandpixel <- removed_zeros %>% group_by(Date, gridnum, latitude, longitude)
# sumbyday_rain <- dayandpixel %>% summarise(
#   sum = sum(rainmm)
# )

by_gridnum <- rainwithlatlong %>% group_by(gridnum, latitude, longitude)
sum_rain <- by_gridnum %>% summarise(
  rain = sum(rainmm),
  mean = mean(rainmm)
)

rain_sf <- st_as_sf(sum_rain, coords = c("longitude", "latitude"),  crs = 4326)
rain_proj <- rain_sf %>% st_transform(3857)

totalrain_plot <- tm_shape(rain_sf) +
  tm_dots(group ="rain", col = "rain", size = 1, palette = "-Spectral") + 
  tm_layout( outer.margins = rep(0.06, 6),inner.margins = rep(0.08, 8), main.title = "Total Amount of Rainfall(mm) per Pixel (2019)", main.title.size = .89)
totalrain_plot
```

## This is breaking down the rain data by corresponding month

```{r}
#https://stackoverflow.com/questions/28335715/r-how-to-filter-subset-a-sequence-of-dates
#Create date object
dayandpixel$Date <- as.Date(dayandpixel$Date)

april <- filter(dayandpixel, between(Date, as.Date("2019-04-01"), as.Date("2019-05-1")))
  april_rain <- april %>% group_by(gridnum, latitude, longitude)
  totalapril <- april_rain%>% summarise(
    rain = sum(rainmm))
  
april_sf <- st_as_sf(totalapril, coords = c("longitude", "latitude"),  crs = 4326)

april_rain <- tm_shape(april_sf) +
  tm_dots(group ="rain", col = "rain", size = 1, palette = "-viridis", style="cont", breaks = c(0, 50, 100, 150, 200, 250)) + 
  tm_layout( outer.margins = rep(0.06, 6),inner.margins = rep(0.08, 8), main.title = "Total Amount of Rainfall(mm) in April (2019)", main.title.size = .5)
april_rain

may <- filter(dayandpixel, between(Date, as.Date("2019-05-01"), as.Date("2019-06-1")))
  may_rain <- may %>% group_by(gridnum, latitude, longitude)
  totalmay <- may_rain%>% summarise(
    rain = sum(rainmm))
  
may_sf <- st_as_sf(totalmay, coords = c("longitude", "latitude"),  crs = 4326)

may_rain <- tm_shape(may_sf) +
  tm_dots(group ="rain", col = "rain", size = 1, palette = "-viridis", style="cont", breaks = c(0, 50, 100, 150, 200, 250)) + 
  tm_layout( outer.margins = rep(0.06, 6),inner.margins = rep(0.08, 8), main.title = "Total Amount of Rainfall(mm) in May (2019)", main.title.size = .5)
may_rain
  
june <- filter(dayandpixel, between(Date, as.Date("2019-06-01"), as.Date("2019-07-1")))
  june_rain <- may %>% group_by(gridnum, latitude, longitude)
  totaljune <- june_rain%>% summarise(
    rain = sum(rainmm))
  
  june_sf <- st_as_sf(totaljune, coords = c("longitude", "latitude"),  crs = 4326)

june_rain <- tm_shape(june_sf) +
  tm_dots(group ="rain", col = "rain", size = 1, palette = "-viridis", style="cont", breaks = c(0, 50, 100, 150, 200, 250)) + 
  tm_layout( outer.margins = rep(0.06, 6),inner.margins = rep(0.08, 8), main.title = "Total Amount of Rainfall(mm) in June (2019)", main.title.size = .5)
june_rain

july <- filter(dayandpixel, between(Date, as.Date("2019-07-01"), as.Date("2019-08-1")))
  july_rain <- july %>% group_by(gridnum, latitude, longitude)
  totaljuly <- july_rain%>% summarise(
    rain = sum(rainmm))
  
july_sf <- st_as_sf(totaljuly, coords = c("longitude", "latitude"),  crs = 4326)

july_rain <- tm_shape(july_sf) +
  tm_dots(group ="rain", col = "rain", size = 1, palette = "-viridis", style="cont", breaks = c(0, 50, 100, 150, 200, 250)) + 
  tm_layout( outer.margins = rep(0.06, 6),inner.margins = rep(0.08, 8), main.title = "Total Amount of Rainfall(mm) in July (2019)", main.title.size = .5)
july_rain

august <- filter(dayandpixel, between(Date, as.Date("2019-08-01"), as.Date("2019-09-1")))
  aug_rain <- august %>% group_by(gridnum, latitude, longitude)
  totalaug <- aug_rain%>% summarise(
    rain = sum(rainmm))
  
aug_sf <- st_as_sf(totalaug, coords = c("longitude", "latitude"),  crs = 4326)

aug_rain <- tm_shape(aug_sf) +
  tm_dots(group ="rain", col = "rain", size = 1, palette = "-viridis", style="cont", breaks = c(0, 50, 100, 150, 200, 250)) + 
  tm_layout( outer.margins = rep(0.06, 6),inner.margins = rep(0.08, 8), main.title = "Total Amount of Rainfall(mm) in August (2019)", main.title.size = .5)
aug_rain

september <- filter(dayandpixel, between(Date, as.Date("2019-09-01"), as.Date("2019-10-1")))
  sept_rain <- september %>% group_by(gridnum, latitude, longitude)
  totalsept <- sept_rain%>% summarise(
    rain = sum(rainmm))
sept_sf <- st_as_sf(totalsept, coords = c("longitude", "latitude"),  crs = 4326)

sept_rain <- tm_shape(sept_sf) +
  tm_dots(group ="rain", col = "rain", size = 1, palette = "-viridis", style="cont", breaks = c(0, 50, 100, 150, 200, 250)) +
  tm_layout( outer.margins = rep(0.06, 6),inner.margins = rep(0.08, 8), main.title = "Total Amount of Rainfall(mm) in Sept (2019)", main.title.size = .5)
sept_rain
```

## This is to show total rain (mm) over time from April to September 2019

```{r message=FALSE, warning=FALSE}
tmap_save(filename = "april.png", tm=april_rain,width=4,height=4,units="in",scale=1)
aprilgif <- image_read("april.png")

tmap_save(filename = "may.png", tm=may_rain,width=4,height=4,units="in",scale=1)
maygif <- image_read("may.png")

tmap_save(filename = "june.png", tm=june_rain,width=4,height=4,units="in",scale=1)
junegif <- image_read("june.png")

tmap_save(filename = "july.png", tm=july_rain,width=4,height=4,units="in",scale=1)
julygif <- image_read("july.png")

tmap_save(filename = "aug.png", tm=aug_rain,width=4,height=4,units="in",scale=1)
auggif <- image_read("aug.png")

tmap_save(filename = "sept.png", tm=sept_rain,width=4,height=4,units="in",scale=1)
septgif <- image_read("sept.png")

#this is putting each file together
img <- c(aprilgif, maygif, junegif, julygif, auggif, septgif)

image_append(image_scale(img, "x300"))

#this is actually creating the gif
my.animation<-image_animate(image_scale(img, "400x400"), fps = 1, dispose = "previous")
image_write(my.animation, "rainperpixel.gif")
```
