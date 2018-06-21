---
layout: post
title : "Watershed Delineation in QGIS - Part II"
subtitle : "a conceptual and practical look at watershed delineation with FOSS software"
date: 2018-06-22 12:00:00
author: "Craig Dsouza"
header-img: "img/posts/wdq1/post-bg.jpg"
comments: true
tags: [Hydrology]
---

We ended the last post having split the area of Ken river's basin from the larger Ganga-Brahmaputra river basin. We did this because for further analyses we want to identify Ken river's watersheds and working within the smaller region allows the analyses to be more efficient. Moreover we also will attempt to use a higher resolution DEM (Cartosat) which is 30 m instead of 1 km to allow us to identify smaller streams. This means the new DEM has more than a thousand pixels for every pixel of the lower resolution GTOPO DEM. This has the potential to significantly increase the time taken for processing.

# 1. Data acquisition - Cartosat DEM
To download Cartosat imagery we need to note the Latitudinal and Longitudinal boundaries of Ken river basin, 78 to 81 deg E and 23 to 26 deg N. The site to download is [here](http://bhuvan.nrsc.gov.in/data/download/index.php). The version we are using is Cartosat Version 3 R1.

|![Downloaded Cartosat Imagery](/img/posts/wdq2/wdq_20.jpg)|
|:--:|
| 1. Merging of nine Cartosat images |

|![Comparison of Cartosat and GTOPO](/img/posts/wdq2/wdq_21.gif)|
|:--:|
| 2. Comparison of Cartosat and GTOPO |


# 2. Data Pre processing - Clipping, projecting and filling voids in Cartosat DEM
To start with we will clip the Cartosat DEM, but we first need a vector mask of Ken river basin. The output of Part I of this exercise gave us a raster mask. We use `r.to.vect` to convert raster to vector as shown below

|![Conversion of raster mask to vector](/img/posts/wdq2/wdq_22.jpg)|
|:--:|
| 3. Conversion of raster mask to vector |

We can then clip the Cartosat raster and project it to UTM Zone 44N as described in part I of the exercise to get the following result.

|![Clipped and projected Cartosat raster](/img/posts/wdq2/wdq_24.jpg)|
|:--:|
| 4. Conversion of raster mask to vector |

In the next step we want to fill voids in the DEM. This may be a long process because of the high resolution of the DEM, hence I would advise you to save your work before running the process. Fill sinks can be executed in the same way as it was shown in Part I.


# 3. Data Processing - Creation of watersheds and stream segments


