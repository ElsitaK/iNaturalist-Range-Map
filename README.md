Range Map for Appalachian Brown (Satyrodes appalachia)
================
Elsita Kiekebusch
10/20/2019

Maps
----

Using the package "maps" to make some basic maps.

``` r
#messing around with maps
map(database="world", regions=".")
```

![](README_files/figure-markdown_github/mapfun-1.png)

``` r
map(database="usa", regions=".")
```

![](README_files/figure-markdown_github/mapfun-2.png)

``` r
#get the map data for later use
usa <- map_data("usa")
states <- map_data("state") #use this to make the state borders
world <- map_data("world")
```

iNaturalist
-----------

Using commands from package "rinat" to access data from the iNaturalist app and make some maps simply (without ggplot2).

``` r
#get the data, save it for further use
data <- get_inat_obs(taxon_name = "Appalachian Brown")
research.data <- data %>% filter(quality_grade == "research") #restricts to good quality observations

#MAKE MAP!
ABB.map <- inat_map(research.data, map = "usa", subregion = ".", plot = FALSE)

#improvements
ABB.map + borders("state") + theme_bw() + labs(x="Longitude", y="Latitude")
```

![](README_files/figure-markdown_github/iNaturalist-1.png)

Maps in ggplot2
---------------

Now making maps with ggplot2, much more control using the usual ggplot2 commands.

``` r
#only gives points
ggplot(research.data, aes(latitude,longitude)) +
  geom_point()
```

![](README_files/figure-markdown_github/ggplot2-1.png)

``` r
#basic map
ggplot() +
  geom_polygon(data = states, aes(x=long, y = lat, group = group), fill = NA, color = "black") + 
  geom_point(data = research.data, aes(longitude, latitude), color="black") +
  labs(x="Longitude", y="Latitude") +
  coord_cartesian(xlim=c(-100,-60)) +
  theme_bw()
```

![](README_files/figure-markdown_github/ggplot2-2.png)

Research Sites
--------------

With ggplot commands, I can add some research sites to my map and color them.

``` r
setwd('~/Documents/NCSU/SERDP/ABB MAP')
sites <- read.csv("Fake.Site.Coordinates.csv", stringsAsFactors = FALSE)
map.data <- research.data %>% select(latitude, longitude) #get lat, long coords only
map.data$Site <- "iNaturalist Observation" 
new.data <- rbind(sites,map.data)

#basic map
ggplot() +
  geom_polygon(data = states, aes(x=long, y = lat, group = group), fill = NA, color = "black") + 
  geom_point(data = new.data, aes(longitude, latitude, color=Site)) +
  labs(x="Longitude", y="Latitude") +
  coord_cartesian(xlim=c(-100,-60)) +
  theme_bw()
```

![](README_files/figure-markdown_github/sites-1.png)
