# Finding A Relationship Between Trees and Other Census Data in Baltimore City
This map was created in R Studio and is representative of the number of trees planted and neighborhoods within Baltimore City.

# Data
The data used for this was provided by a given [geojson file](Trees in Baltimore/data) from class about trees in Baltimore City and the get_acs function in R.

# Processing & Transformations
I used R Studio for processing.

# Analysis
I initially tried to make a comparison of trees and median age per neighborhood to see if there was a correlation between trees and either older or younger populations. Since the trees are points, I had to find a way to relay it to geometry that was provided by the get_acs function 
      baltimorecity_age <- get_acs(
        geography = "tract", 
        variables = c("B01002_001"), #medianage
        state = "MD",
        county = "Baltimore City",
        geometry = TRUE

I then calculated the area of each tract so that I can get an idea of trees/area, which is a better representation than just the number of trees per neighborhood since some are more densely populated than others.

I did a spatial join (st_join) and used the count function (count(as_tibble)) to count the number of trees per tract (info provided by the GEOID value)

# Results
What **outputs** will you be creating and how are they directly connected to the class? Explain your bin folder.
I created a map that showed the distrubtion of trees in Baltimore City as it relates to meters^2.
