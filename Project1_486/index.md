---
title: "Change over time in flood reports, 2017-2020"
output: pdf_document
---
```{r loading_libraries} 
library(sf)
library(ggplot2)
library(mapbaltimore)
library(dplyr)
library(baltimoredata)
library(themedubois)
library(gganimate)
library(ggthemes)
library(gifski)
library(readr)
library(tidyr)
library(gapminder)
library(plotly)
library(ggplot2)
library(gapminder)
library(magick)
library(gganimate)
```

```{r flooded streets 2017, message=FALSE, warning=FALSE}
#this file can be downloaded from the Open Baltimore site and is already filtered to be strtype swm: street flood
floodreports_2017 <- read.csv(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/311_Customer_Service_Requests_2017.csv")

str(floodreports_2017)
st_crs(floodreports_2017) #checking the CRS of the csv file...none assigned so we can adjust accordingly

#this is filtering out areas without lat/long information and will assign the data as a spatial dataset using st_as_sf
flood_2017_filtermissing <- floodreports_2017 %>% filter(!is.na(latitude))
flood_2017_spatial <- st_as_sf(flood_2017_filtermissing, coords = c("longitude", "latitude"), crs = 4326) #CRS for lat/long

#I was unable to figure out how to load this within R, so I downloaded the .rda file from: https://github.com/elipousson/mapbaltimore/tree/main/data...this has the NAD83/HARN Maryland CRS 2804
load(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/neighborhoods.rda")

plot(neighborhoods$geometry) #plotting the neighborhoods file from the baltimoredata package to see how it looks

neighborhoods_projected <- st_transform(neighborhoods, crs = 4326) #projecting this so that it can be used in a join later...the join objects seemed to need the same CRS in order to work

# onlyresidential <- neighborhoods &&
#   group_by(type) want to group by type and only look into residential areas but not working 

#attempting spatial join by geometry
reportsneighborhoods2017_join <- st_join(flood_2017_spatial, neighborhoods_projected, join = st_within) #think i need to get rid of the null fields
reports_inneighborhoods2017_count <- count(as_tibble(reportsneighborhoods2017_join), neighborhood) 

info2017 <- left_join(neighborhoods_projected, reports_inneighborhoods2017_count, by = c("name" = "neighborhood")) #this joins the info so we have the total number in each neighborhood for 2017

info2017_filtermissing <- info2017%>% filter(!is.na(n)) #filtering out null values

#plotting the neighborhoods with repotrs in 2017
ggplotmap2017 <- ggplot(data = info2017_filtermissing, aes(fill = n)) +
  geom_sf() +
  scale_fill_distiller(palette = "RdPu",
                       direction = 1) +
  labs(title = "2017 Flooded Street Reports",
       caption = "311 data provided by Open Baltimore; Neighborhoods provided by Eli Pousson via GitHub",
       fill = "total reports") +
  theme_void()

plot(ggplotmap2017)
```



```{r flooded_streets_2018, message=FALSE, warning=FALSE}
#this file can be downloaded from the Open Baltimore site and is already filtered to be sttype swm: street flood
floodreports_2018 <- read.csv(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/311_Customer_Service_Requests_2018.csv")

str(floodreports_2018)
st_crs(floodreports_2018) #checking the CRS of the csv file...none assigned so we can adjust accordingly

#this is filtering out areas without lat/long information and will assign the data as a spatial dataset using st_as_sf
flood_2018_filtermissing <- floodreports_2018 %>% filter(!is.na(latitude))
flood_2018_spatial <- st_as_sf(flood_2018_filtermissing, coords = c("longitude", "latitude"), crs = 4326) #CRS for lat/long

#I was unable to figure out how to load this within R, so I downloaded the .rda file from: https://github.com/elipousson/mapbaltimore/tree/main/data...this has the NAD83/HARN Maryland CRS 2804
load(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/neighborhoods.rda")

plot(neighborhoods$geometry) #plotting the neighborhoods file from the baltimoredata package to see how it looks

neighborhoods_projected <- st_transform(neighborhoods, crs = 4326) #projecting this so that it can be used in a join later...the join objects seemed to need the same CRS in order to work

# onlyresidential <- neighborhoods &&
#   group_by(type) want to group by type and only look into residential areas but not working 

#attempting spatial join by geometry
reportsneighborhoods2018_join <- st_join(flood_2018_spatial, neighborhoods_projected, join = st_within) #think i need to get rid of the null fields
reports_inneighborhoods2018_count <- count(as_tibble(reportsneighborhoods2018_join), neighborhood) 

info2018 <- left_join(neighborhoods_projected, reports_inneighborhoods2018_count, by = c("name" = "neighborhood")) #this joins the info so we have the total number in each neighborhood for 2018

plot(info2018)

info2018_filtermissing <- info2018%>% filter(!is.na(n)) #filtering out null values

#plotting neighborhoods with 2018 flooding reports
ggplotmap2018 <- ggplot(data = info2018_filtermissing, aes(fill = n)) +
  geom_sf() +
  scale_fill_distiller(palette = "RdPu",
                       direction = 1) +
  labs(title = "2018 Flooded Street Reports",
       caption = "311 data provided by Open Baltimore; Neighborhoods provided by Eli Pousson via GitHub",
       fill = "total reports") +
  theme_void()

plot(ggplotmap2018)

```

```{r flooded streets 2019, message=FALSE, warning=FALSE}
#this file can be downloaded from the Open Baltimore site and is already filtered to be sttype swm: street flood
floodreports_2019 <- read.csv(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/311_Customer_Service_Requests_2019.csv")

str(floodreports_2019)
st_crs(floodreports_2019) #checking the CRS of the csv file...none assigned so we can adjust accordingly

#this is filtering out areas without lat/long information and will assign the data as a spatial dataset using st_as_sf
flood_2019_filtermissing <- floodreports_2019 %>% filter(!is.na(latitude))
flood_2019_spatial <- st_as_sf(flood_2019_filtermissing, coords = c("longitude", "latitude"), crs = 4326) #CRS for lat/long

#I was unable to figure out how to load this within R, so I downloaded the .rda file from: https://github.com/elipousson/mapbaltimore/tree/main/data...this has the NAD83/HARN Maryland CRS 2804
load(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/neighborhoods.rda")

plot(neighborhoods$geometry) #plotting the neighborhoods file from the baltimoredata package to see how it looks

neighborhoods_projected <- st_transform(neighborhoods, crs = 4326) #projecting this so that it can be used in a join later...the join objects seemed to need the same CRS in order to work

# onlyresidential <- neighborhoods &&
#   group_by(type) want to group by type and only look into residential areas but not working 

#attempting spatial join by geometry
reportsneighborhoods2019_join <- st_join(flood_2019_spatial, neighborhoods_projected, join = st_within) #think i need to get rid of the null fields
reports_inneighborhoods2019_count <- count(as_tibble(reportsneighborhoods2019_join), neighborhood) 

info2019 <- left_join(neighborhoods_projected, reports_inneighborhoods2019_count, by = c("name" = "neighborhood")) #this joins the info so we have the total number in each neighborhood for 2018

plot(info2019)

info2019_filtermissing <- info2019%>% filter(!is.na(n)) #filtering out null values

#plotting neighborhoods with 2019 street flood reports
ggplotmap2019 <- ggplot(data = info2019_filtermissing, aes(fill = n)) +
  geom_sf() +
  scale_fill_distiller(palette = "RdPu",
                       direction = 1) +
  labs(title = "2019 Flooded Street Reports",
       caption = "311 data provided by Open Baltimore; Neighborhoods provided by Eli Pousson via GitHub",
       fill = "total reports") +
  theme_void()

plot(ggplotmap2019)
```

```{r flooded streets 2020, message=FALSE, warning=FALSE}
#this file can be downloaded from the Open Baltimore site and is already filtered to be sttype swm: street flood
floodreports_2020 <- read.csv(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/311_Customer_Service_Requests_2020.csv")

str(floodreports_2020)
st_crs(floodreports_2020) #checking the CRS of the csv file...none assigned so we can adjust accordingly

#this is filtering out areas without lat/long information and will assign the data as a spatial dataset using st_as_sf
flood_2020_filtermissing <- floodreports_2020 %>% filter(!is.na(latitude))
flood_2020_spatial <- st_as_sf(flood_2020_filtermissing, coords = c("longitude", "latitude"), crs = 4326) #CRS for lat/long

#I was unable to figure out how to load this within R, so I downloaded the .rda file from: https://github.com/elipousson/mapbaltimore/tree/main/data...this has the NAD83/HARN Maryland CRS 2804
load(file = "/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/neighborhoods.rda")

plot(neighborhoods$geometry) #plotting the neighborhoods file from the baltimoredata package to see how it looks

neighborhoods_projected <- st_transform(neighborhoods, crs = 4326) #projecting this so that it can be used in a join later...the join objects seemed to need the same CRS in order to work

# onlyresidential <- neighborhoods &&
#   group_by(type) want to group by type and only look into residential areas but not working 

#attempting spatial join by geometry
reportsneighborhoods2020_join <- st_join(flood_2020_spatial, neighborhoods_projected, join = st_within) #think i need to get rid of the null fields
reports_inneighborhoods2020_count <- count(as_tibble(reportsneighborhoods2020_join), neighborhood) 

info2020 <- left_join(neighborhoods_projected, reports_inneighborhoods2020_count, by = c("name" = "neighborhood")) #this joins the info so we have the total number in each neighborhood for 2018

plot(info2020)

info2020_filtermissing <- info2020%>% filter(!is.na(n))

ggplotmap2020 <- ggplot(data = info2020_filtermissing, aes(fill = n)) +
  geom_sf() +
  scale_fill_distiller(palette = "RdPu",
                       direction = 1) +
  labs(title = "2020 Flooded Street Reports",
       caption = "311 data provided by Open Baltimore; Neighborhoods provided by Eli Pousson via GitHub",
       fill = "total reports") +
  theme_void()

plot(ggplotmap2020)
```

```{r}
#this is adding new columns to each of the files that show neighborhood and number of reports, for 2017 to 2020.

reports_inneighborhoods2017_count$year <- 2017
reports_inneighborhoods2018_count$year <- 2018
reports_inneighborhoods2019_count$year <- 2019
reports_inneighborhoods2020_count$year <- 2020
```

```{r}
combined1718 <- left_join(reports_inneighborhoods2018_count, reports_inneighborhoods2017_count, by  = "neighborhood") #joined in this way bc 2018 had more neighborhoods

combined1920 <- left_join(reports_inneighborhoods2019_count, reports_inneighborhoods2020_count, by  = "neighborhood") #joining these two years together

combined_allyears <- left_join(combined1718, combined1920, by  = "neighborhood") #had to do a bunch of joins in order to combine things with different rows...

write.csv(combined_allyears,"/Users/Tyrah/Documents/adv_GIS_classwork/Project1_687/combined_allyears.csv", row.names = FALSE)

#I wanted to have a data frame with only 3 rows: neighborhood, n, and year! so this rbind function is combining by row

combined_allyears_byrow <- rbind(reports_inneighborhoods2018_count, reports_inneighborhoods2017_count,reports_inneighborhoods2019_count, reports_inneighborhoods2020_count)
cat("Combined by rows: ", "\n")

```
```{r}
library(gganimate)

combined_allyears_byrow %>%
  ggplot(aes(neighborhood,n)) +
  geom_col() +
  scale_y_continuous(limits = c(0, 100), breaks = seq(0, 70, by = 5)) +
  theme_minimal() +
  ## gganimate functionality starts here
  labs(x = "Neighborhoods", y = "Total Submitted Reports", title = "{frame_time}") +
  transition_time(year) +
  ease_aes("linear")

anim_save("flooded-street-reports.gif")
