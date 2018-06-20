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

My goal in this exercise is to delineate watersheds for the Ken river basin in Central India. The watersheds created can then be used within the Humanitarian (HOT) OSM Tasking Manager to crowdsource digitizing of actual stream lines (but more on this later). We know that the Ken river is a tributary to the Yamuna river which in turn is a tributary of the Ganga river in northern India. Hence for context we load vector layers seen below. The first image shows the [states of India](http://projects.datameet.org/maps/#state-boundaries) and the second image shows larger river basins of Asia, downloaded from [USGS Hydrosheds](https://hydrosheds.cr.usgs.gov/dataavail.php)- See 30 sec SHAPE (Drainage Basins). The styling options to pay attention to are 'Outline', 'Outline width' and 'Fill style' - No brush.

![States of India - DEM in background](/img/posts/wdq-3.png)


![States of India - River basins overlaid](/img/posts/wdq-4.png)

The image above gives us a preview of what we want. The USGS Hydrosheds river basins are generally larger river basins. Some smaller river basins such as Ken River are swallowed up and only the larger Ganga-Brahmaputra river basin can be seen. Next we clip out the larger Ganga-Brahmaputra river basin and use this as a starting point. We do this by clicking on the river basins layer to make it active, then clicking on 'Select feature' in the middle of the menu on the top and then clicking anywhere in the Ganga basin, which makes it turn yellow. Then right click on the river basins file in the layers panel and click 'Save as'. Check the option for 'Save only selected features', select a location to save and say OK. Once this is done you can remove the larger Asia river basins file from the Layers panel and you will only see the Ganga-Brahmaputra basin.

![Clip out Ganga-Brahmaputra river basin](/img/posts/wdq-5.png)

![Only Ganga-Brahmaputra river basin remains](/img/posts/wdq-6.png)

We now clip the large DEM we started with to the boundaries of the Ganga-Brahmaputra basin. Make the DEM layer active (click on it in the Layers panel), then go to Raster > Extraction > Clipper and you see the menu below. 

![Raster Clip](/img/posts/wdq-7.png)

Select a location to save output file, then click on Mask layer and select 'Ganga-Brahmaputra' basin as your mask layer. Click OK and you see this error, which says 'Ring self intersection'. This basically means that the vertices of the polygon we're working with overlap in one or more places which makes the polygon invalid for this operation.

![Raster Clip Error](/img/posts/wdq-8.png)

Normally this would be more cumbersome to fix but I was able to locate the overlapping vertices, zoom in on it, click on the editing toolbar (just above browser panel) and nudge one vertex aside to correct the overlap. It turns out these two were the only overlapping ones and after this the clip operation worked successfully.

![Overlap correction](/img/posts/wdq-9.png)

![Clipped raster](/img/posts/wdq-10.png)





