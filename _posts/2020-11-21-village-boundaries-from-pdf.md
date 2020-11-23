---
layout: post
title : "Extracting Village Boundaries from PDF"
subtitle : "pdf to gis"
date: 2020-11-21 19:13:00
author: "Craig Dsouza"
header-img: "img/posts/2020-11-21-village-boundaries-from-pdf/banner.png"
comments: false
tags: [ pdf, gis, scraping ]
category: [blog, geography]
image_sliders:
  - [selector_name_of_slider]
---

Much of the valuable geospatial data required for studies in the development sector is not openly available as spatial data but instead embedded in pdf files. In this post we'll examine the possibility of extracting one such spatial dataset (village boundaries and their attributes) from a pdf map of a tehsil (sub-district). This spatial data can then be read and analysed in gis softwares such as QGIS.

The pdf files themselves are available for all 360 something tehsils in Maharashtra state on the website of ![Maharashtra Remote Sensing Application Centre](https://mrsac.gov.in/MRSAC/map/map). In the dropdown box select 'Taluka wise Village Maps'

|![Screenshot of PDF of Village Map](/img/posts/2020-11-21-village-boundaries-from-pdf/TH-ABD-AURANGABAD.PNG)|
|:--:|
| Screenshot of PDF of Village Map |

This blog post is a work in progress and the roadmap I imagine is as follows. Do reach out if you know of a better way to do this or just to give feedback in general!

| Find python library suited for extraction of data from PDFs                 | ✅ |
| Extract Village names & positions as a vector file from PDF                 | ✅ |
| Extract Village boundaries as a vector file from pdf                        | ✅ |
| Study how to georeference vector file | Automated georeferencing?           |    |
| Georeference vector files                                                   |    |
| Correct topology errors in vector boundaries so we have complete polygons   |    |
| Spatial join of the vector file with village names to the vector boundaries |    |


Table of Contents
=================
1. [Find Python Library for PDFs](#1-find-python-library-for-pdfs)


# 1. Find Python Library for PDFs
En route to choosing a suitable library to work with I found out a little bit about the structure of PDF documents themselves. 
I chose the library [`PyMuPDF`](https://pymupdf.readthedocs.io/en/latest/) because of it's comprehensive documentation and collection of examples.

## Installation/Imports
Install PyMuPDF with `pip install pymupdf` and import the required libraries, including PyMuPDF, which for some odd reason must be imported with the name `fitz`

```
import os, sys
import fitz
```
## Examination of PDF

```
doc = fitz.open(filename)
print(doc.pageCount)
```

Since the pdf has only a single page we can load it in a variable `page` of the class Page which has numerous useful methods to access relevant elements of the page. 

```
page = doc.loadPage(0)
dir(page)
```


[return to top](#table-of-contents)

# 2. Extracting Village names and positions
To get all of the text from the pdf along with the position use the following, which returns an output as shown below.


[return to top](#table-of-contents)

# 3. Extracting Village boundaries
Boundaries in the PDF are stored as drawings which can be extracted using the class Path. Drawings however represent more than just the village boundaries in the PDF. Every line on the page represents a *drawing*. The challenge was to separate out signal (village boundaries) from the noise (every other line)

```
paths = page.getDrawings()
```

[return to top](#table-of-contents)

# 4. How to georeference vector files
https://gisforthought.com/georeferencing-vector-data-using-qgis-and-ogr2ogr/
https://gis.stackexchange.com/questions/33208/georeferencing-vector-layer-with-control-points-using-qgis (look for long answer at bottom)
https://gdal.org/programs/ogr2ogr.html

[return to top](#table-of-contents)

# 5. Georeference vector files



[return to top](#table-of-contents)

# 6. Correct topology errors in vector boundaries


[return to top](#table-of-contents)

# 7. Spatial joins


[return to top](#table-of-contents)
