# GES 486 Project: Part 1 

# What Is The Chosen Topic?
Interested in seeing what neighborhoods in Baltimore City have been influenced by street flooding over a period of four years.

# What Data Is Being Used And How Can This Be Accesssed?
I looked up the 311 Customer Service Requests and filtered the results by SRType: WW- Storm Flooded Street for the following four years: [2017](https://data.baltimorecity.gov/datasets/311-customer-service-requests-2017/explore), [2018](https://data.baltimorecity.gov/datasets/311-customer-service-requests-2018/explore), [2019](https://data.baltimorecity.gov/datasets/311-customer-service-requests-2019/explore) , and [2020](https://data.baltimorecity.gov/datasets/311-customer-service-requests-2020/explore). 

# Processing & Transformations Needed For Spatial Analysis
There were a few necessary transformations that needed to be done to the data in order to use it how I wanted to. First, I filtered out the data entries that did not have a corresponding location provided by latitude and longitude. I used st_as_if in order to assign the data to a spatial dataset. Once the data was assigned spatially, I did a spatial join so that I could show where the reports were in relation to the neighborhood data set (which was downloaded from Eli Pousson’s GitHub site: https://github.com/elipousson/mapbaltimore/tree/main/data). Before completing this join, I had to make sure that the neighborhood layer was in the same coordinate reference system as the rest of the data that I had planned to use. I did the spatial join between the “flood_20**_spatial” (where ** is the year used) and “neighboods_projected” data frames. I then wanted to take a count of the reports per neighborhood and did so by using the count(as_tibble) function in R. I did this for each year. 

# Analysis That Will Be Performed On Datasets
The following code was used to produce this information.

# Results; What Outputs Are Being Created And How Are They Related To Our Class?
I created a series of 4 maps for 2017, 2018, 2019, and 2020 to show where the flooded reports were. I then used that information to create a GIF.
