---
layout: post
title : "Watershed Delineation in QGIS"
subtitle : "a conceptual and practical look at watershed delineation with FOSS software"
date: 2018-06-20 12:00:00
author: "Craig Dsouza"
header-img: "img/blog-bg.jpg"
comments: true
tags: [Hydrology]
---

I'm going to begin with the assumption that you know what a watershed is. If you do, you would know that watersheds can vary in size. There is no strict definition for the threshold in sq.km. area or stream order that makes an area a watershed versus a micro-watershed, or a river basin. There is however general agreement that a micro-watershed < watershed < river basin in terms of area.

For the purposes of this post however we aren't concerned with the distinction between these definitions. Our goal is to take a freely available digital elevation model (DEM) and apply some techniques within QGIS (a FOSS software) to obtain 1) a watershed delineation and 2) a (theoretical) stream network. These two are immensely useful either to make maps for watershed planning or to use these features to build mobile/web applications. If some of these terms don't make sense at this point, don't worry, read on and use the images as a guide to understand the concepts involved.

We begin with a Digital Elevation Model (DEM). A DEM is basically just a raster file where the value of each pixel represents the elevation of that area of land above Mean Sea Level (MSL). The DEM we start with is a coarse 1 km resolution Global DEM obtained from [USGS Earth Explorer](https://earthexplorer.usgs.gov/). If you wish to skip the Earth Explorer website you can directly download the same from this [link](https://www.dropbox.com/s/rjz0htoieyobnfk/gt30e060n40_dem.zip?dl=0). When you open the file in QGIS this is what you see. The white in the image indicates the highest elevations, the black indicates elevations closer to sea level and grays indicate everything in between. 

![Global DEM TOPO 1km basic styling](/img/posts/wdq-1.png)

We can apply different styling palette, Singleband pseudocolor to get a better looking image. Also in the styling panel, change mode to 'Quantile' and click Classify. In the image below, blue represents the highest elevations and red the elevations near sea level. Yellow shades represent the middle elevations.

![Global DEM TOPO 1km custom styling](/img/posts/wdq-2.png)



