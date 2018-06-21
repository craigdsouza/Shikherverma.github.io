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

We now clip the large DEM we started with to the boundaries of the Ganga-Brahmaputra basin. This will make further operations faster and interpretation of the output images simpler. For this, make the DEM layer active (click on it in the Layers panel), then go to Raster > Extraction > Clipper and you see the menu below. 

![Raster Clip](/img/posts/wdq-7.png)

Select a location to save output file, then click on Mask layer and select 'Ganga-Brahmaputra' basin as your mask layer. Click OK and you see this error, which says 'Ring self intersection'. This basically means that the vertices of the polygon we're working with overlap in one or more places which makes the polygon invalid for this operation.

![Raster Clip Error](/img/posts/wdq-8.PNG)

Normally this would be more cumbersome to fix but I was able to locate the overlapping vertices, zoom in on it, click on the editing toolbar (just above browser panel) and nudge one vertex aside to correct the overlap. It turns out these two were the only overlapping ones and after this the clip operation worked successfully.

![Overlap correction](/img/posts/wdq-9.png)

![Clipped raster](/img/posts/wdq-10.png)

We now need the raster to be projected to a Projected Coordinate System (PCS) for the purposes of accurate analysis. The coordinate system we're using now, WGS 1984 is good for viewing maps but does not provide the required accuracy for analysis. (I may do a more detailed post on PCS later). The PCS we will use is UTM Zone 44N (EPSG code 32644). To assign a projection the steps are Raster > Projections > Assign Projection. 

![Projection](/img/posts/wdq-11.png)

Reading through the literature (and Stack Exchange) you will see that there are two possibilities the QGIS toolbox provides for hydrological analyses (Grass or Saga). While in some cases, the two approaches can be comparable, I chose to work with the algorithms in the Grass toolbox based on this review of the two, ['Channel network derivation from Digital Elevation Models - An evaluation of Open Source Approaches'](https://www.researchgate.net/publication/304137316_Channel_Network_Derivation_from_Digital_Elevation_Models_-_An_Evaluation_of_Open_Source_Approaches). The basic workflow is as shown below. In the first step, we separate out just the Ken River Basin from the larger Ganga Brahmaputra river basin. Then subsequently we split the Ken into smaller watersheds.

![Workflow](/img/posts/wdq-12.png)

One more pre-processing step required is 'Filling sinks in the DEM'. Some DEMs have artefacts or sinks where the elevation is such that water would flow into these sinks against hydrological rules. These sinks need to be corrected. For this we use `r.fill.dir` in the toolbox,  with just a single input, the projected 'Ganga-Brahmaputra basin' DEM. We only select 'Depressionless DEM' as the output desired and uncheck the other outputs.

![Fill sinks](/img/posts/wdq-13.png)

Our DEM is now ready for final processing. We use the all encompassing `r.watersheds` tool. This tool takes as its input the Depressionless DEM and can generate as its output, Flow Accumulation raster (number of cells draining through each cell), Drainage Direction raster, watershed basins, stream segments and more. In this first step however all we require is the Flow Accumulation and Flow Direction rasters. We can specify 'Minimum size of exterior watershed basin' as 100000. This indicates the minimum number of cells a basin must be composed of. Irrelevant since we don't need watershed basins delineated in this step, however a number must be specified. Any number too low may cause QGIS to crash. Besides this, also check 'Use positive flow accumulation even for likely underestimates' since without this we get negative flow accumulation values. I think this is because the raster is a coarse resolution (1 km). See these steps and resultant rasters below.

![r.watershed](/img/posts/wdq-14.png)

![drainage-direction](/img/posts/wdq-15.png)

![flow-accumulation](/img/posts/wdq-16.png)

We must now identify the point of confluence between the Ken river and Yamuna to separate out the Ken basin. I initially tried inserting a Google basemap with the Open Layers plugin but the map tiles are not available in our projected reference system (UTM Zone 44N). Hence I used a combination of the projected DEM and the flow accumulation map to pinpoint the confluence and created a point shapefile to mark out the location.

![confluence-pinpoint](/img/posts/wdq-17.gif)

Now we use the `r.water.outlet` tool. This tool takes two inputs, the Drainage direction raster generated with  `r.watersheds` and the coordinates of the confluence. To ensure the coordinates are in the same coordinate system as UTM Zone 44N we change the project coordinate system by clicking in the bottom right corner. 

![r.water.outlet](/img/posts/wdq-18.png)

![ken-basin](/img/posts/wdq-19.png)








