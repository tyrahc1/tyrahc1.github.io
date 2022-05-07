# Project 3: 

# Understanding Baltimore City Demographics By Census Tracts
This map was created in R Studio and is representative of the black population in Baltimore City by census tract.

# Data
The data used for this was provided by the tidycensus library and ACS data generated in R Studio.

bmorecity_race2 <- get_acs(
      geography = "tract",
      state = "MD",
      county = "Baltimore City",
      variables = c(White = "B03002_003",
                    Black = "B03002_004",
                    Native = "B03002_005",
                    Asian = "B03002_006",
                    Hispanic = "B03002_012"),
      summary_var = "B03002_001",
      geometry = TRUE
 ) %>%
    mutate(percent = 100 * (estimate / summary_est))
Getting data from the 2015-2019 5-year ACS
Downloading feature geometry from the Census website.  To cache shapefiles for use in future sessions
8
Getting data from the 2015-2019 5-year ACS
Using FIPS code '24' for state 'MD'
Using FIPS code '510' for 'Baltimore city'
library(tmap)
bmore_black <- filter(bmorecity_race2,
                        variable == "Black")
tm_shape(bmore_black) + tm_polygons()

tm_shape(bmore_black,
         projection = sf::st_crs(6488)) +
tm_polygons(col = "percent",
                  palette = "Oranges",
                  title = "ACS estimate",
                  legend.hist = TRUE) +
tm_layout(title = "Percent Black\nby Census tract",
                frame = FALSE,
                legend.outside = TRUE,
                bg.color = "grey70",
                legend.hist.width = 5)

# Processing & Transformations
I used R Studio for processing.

