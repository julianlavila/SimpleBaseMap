Simple base map
===============

Sometimes we need to put points on a simple map that shows the
geographic location of sampling points, stations, or species for which
we know the geographic coordinates. This script allows you to easily
view points by creating a data frame of locations.

first load required libraries
-----------------------------

``` r
library("ggplot2")
library("sf")
library("rnaturalearth")
library("rnaturalearthdata")
library("ggspatial")
theme_set(theme_bw())
```

\#base map

``` r
world <- ne_countries(scale = "medium", returnclass = "sf")
class(world)
```

    ## [1] "sf"         "data.frame"

``` r
world_points<- st_centroid(world) #countries centroids
world_points <- cbind(world, st_coordinates(st_centroid(world$geometry)))
sites <- data.frame(longitude = c(88.8,88.8,88.8), # dataframe of points coordinates
latitude = c(20.8,20.4,19.8))
sites
```

    ##   longitude latitude
    ## 1      88.8     20.8
    ## 2      88.8     20.4
    ## 3      88.8     19.8

``` r
ggplot(data = world) +
geom_sf()+
coord_sf(xlim = c(78, 96), ylim = c(9, 26), expand = FALSE)+ # adjust to study area
geom_text(data= world_points,aes(x=X, y=Y, label=name), # text color can be adjusted
color = "darkblue", fontface = "bold", check_overlap = FALSE) +
annotate(geom = "text", x = 88, y = 15, label = "Bay of Bengal", # Optional text
fontface = "italic", color = "grey22", size = 6, alpha=0.4)+
annotate(geom = "text", x = 88, y = 22, label = "Example", #remove this code line
fontface = "italic", color = "cyan4", size = 23, alpha=0.4)+ #remove this code line
annotate(geom = "text", x = 88, y = 14, label = "Example", #remove this code line
fontface = "italic", color = "cyan4", size = 23, alpha=0.4)+ #remove this code line
geom_point(data = sites, aes(x = longitude, y = latitude), size = 1.5,
shape = 23, fill = "darkred")+ #Color and shape of points can be adjusted
annotation_scale(location = "bl", width_hint = 0.5,tick_height = 0.4) +
annotation_north_arrow(location = "bl", which_north = "true",
height = unit(0.55, "cm"),
width = unit(0.55, "cm"),
pad_x = unit(0.75, "cm"),
pad_y = unit(10, "cm"),
style = north_arrow_orienteering)+
labs(title="Bay of Bengal", #adjust to necessity
caption = "made by: Julián Avila-Jiménez",
x="Longitude",
y="Latiude")
```

![](SimpleBaseMap_files/figure-markdown_github/unnamed-chunk-2-1.png)
