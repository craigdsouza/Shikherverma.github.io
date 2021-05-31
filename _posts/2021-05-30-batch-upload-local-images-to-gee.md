---
layout: post
title : "how to batch upload local images to gee"
subtitle : "IMD rainfall data"
date: 2021-05-30 06:53:00
author: "Craig Dsouza"
header-img: "img/banner-red.png"
comments: false
tags: [IMD, rainfall, GEE]
category: [blog, remote-sensing]
image_sliders:
  - [selector_name_of_slider]
---

The Indian Meteorological Department (IMD) has recently [published](https://imdpune.gov.in/Clim_Pred_LRF_New/Grided_Data_Download.html) a dataset of daily gridded rainfall (0.25 deg) for the entire time period from 1901 until 2020. The [reference paper](https://imdpune.gov.in/Clim_Pred_LRF_New/ref_paper_MAUSAM.pdf) for this dataset states that rainfall records from 6995 rain gage stations were used in it's preparation. A comparison of the dataset (named IMD4) suggests that it is comparable with other gridded rainfall datasets across the country and in regions such as the Western Sahyadris it is more realistic given that a higher density of rainfall stations was used. The method used to convert station data to gridded was the Inverse Distance Weighted (IDW) approach, (Shepard, 1968) using a minimum of 1 and maximum of 4 stations within a radial distance of 1.5 deg around the gridded pixel. 

Here we discuss accessing this data and publishing it on Google Earth Engine Image Collection to leverage it's utility against other remote sensing datasets.

|![final map layer](/img/posts/2021-05-30-how-to-batch-upload-local-images-to-gee/gee-map.PNG)|
|---|
|preview of final map layer|

*pre-requisites* : clone the following [github repo](https://github.com/craigdsouza/imdgrid) to your pc, then create a python virtual environment with the following requirements.txt . The packages listed here may be a bit excessive, alternatively you can install packages individually in your environment as required. The code here has been tested with Python 3.7

Table of Contents
=================
1. [Download Historical Data](#1-download-historical-data)
2. [Convert Grid to Tif](#2-convert-grid-to-tif)
3. [Split Tif](#3-split-tif)
4. [Upload Tif to GCP Bucket](#4-upload-tif-to-gcp-bucket)
5. [Subprocess loop to upload images](#5-subprocess-loop-to-upload-images)

# 1. Download Historical Data
|![imd website](/img/posts/2021-05-30-how-to-batch-upload-local-images-to-gee/imd-website.PNG)|
|---|
|imd source of dataset|

Data can be downloaded directly from the link provided above or programmatically using the python imdlib library. The [IMDHistoricalGrid script](https://github.com/craigdsouza/imdgrid/blob/master/IMDHistoricalGrid.py) uses the python imdlib library to download historical data programmatically from the IMD website. Pass the script the parameters 'rain', starting year and ending year according to which years you wish to download data for. Data is available for 1901 until 2020 and each file downloaded is 25MB.

```shell
C:Users\[Username]\Code\imdgrid> python IMDHistoricalGrid.py rain 2000 2001
```

This script downloads annual grd files in the following directory structure
- C > Users > [Username] > Data > imd > rain > 2000.grd, 2001.grd

[return to top](#table-of-contents)

# 2. Convert Grid to Tif
the [IMDHistoricalGrid2Tif script](https://github.com/craigdsouza/imdgrid/blob/master/IMDHistoricalGrid2Tif.py) uses the imdlib library and rasterio to read in grd files and output tif files. Pass the script the parameters 'rain', starting year and ending year as per your needs.

```shell
C:Users\[Username]\Code\imdgrid> python IMDHistoricalGrid2Tif.py rain 2000 2001
```

you are then left with annual tif files in the following directory structure
- C > Users > [Username] > Data > imd > rain > tif > 2000.tif

[return to top](#table-of-contents)

# 3. Split Tif
the [IMDHistoricalTif2Daily script](https://github.com/craigdsouza/imdgrid/blob/master/IMDHistoricalTif2Daily.py) splits the annual tif files into daily tif files and save to sub-directories with IMDHistoricalTif2Daily . Pass the script the parameters 'rain', starting year and ending year as per your needs.

```shell
C:Users\[Username]\Code\imdgrid> python IMDHistoricalTif2Daily.py rain 2000 2001
```

after running this script you are left with daily files in the following directory structure.

- C > Users > [Username] > Data > imd > rain > tif > 2000 > 20000101.tif,..,20001231.tif 

[return to top](#table-of-contents)

# 4. Upload Tif to GCP Bucket
First create a Google Cloud Project Bucket and set it's permissions to public, as described [here](https://cloud.google.com/storage/docs/access-control/making-data-public#buckets). 

|![bucket public access](/img/posts/2021-05-30-how-to-batch-upload-local-images-to-gee/bucket-public-access.PNG)|
|---|
|editing permissions on bucket to make it public |

Upload all files to the GCP bucket with the following command.

```shell
C:Users\[Username]\Code\imdgrid> gsutil -m cp *.tif gs://[bucketname]
```  

[return to top](#table-of-contents)

# 5. Subprocess loop to upload images
upload images to GEE using the [IMDHistoricalGCP2GEE script](https://github.com/craigdsouza/imdgrid/blob/master/IMDHistoricalGCP2GEE.py), which calls the earthengine CLI from within python to copy daily tif files from your GCP to GEE Assets. This script must be passed several parameters including year, bucketname, geeusername and geeimagecollectionpath.

```shell
C:Users\[Username]\Code\imdgrid> python IMDHistoricalGCP2GEE.py 2020 [bucketname] [geeusername] [gee-image-collection-path]
```

finally delete all images from the GCP Bucket after they have been copied to Google Earth Engine Assets

```shell
C:Users\[Username]\Code\imdgrid> gsutil -m rm gs://[bucketname]/**
```
Access image collection within your GEE code editor with the following demo script

```js
Map.addLayer(rainfall.mean(),{},'rainfall')
print("number of images in rainfall collection: ",rainfall.size())
print("first image date ",ee.Date(rainfall.first().get("system:time_start")))
print("last image date ",ee.Date(rainfall.sort("system:time_start",false).first().get("system:time_start")))
```

[return to top](#table-of-contents)

Attribution: much of the code here is inspired by [Ujaval Gandhi](https://github.com/spatialthoughts/projects/tree/master/imd) and [Qiusheng Wu](https://groups.google.com/g/google-earth-engine-developers/c/h5PZOmU_dfw/m/50MMDvOVAwAJ)