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

The pdf files themselves are available for all 360 something tehsils in Maharashtra state on the website of [Maharashtra Remote Sensing Application Centre](https://mrsac.gov.in/MRSAC/map/map). In the dropdown box select 'Taluka wise Village Maps'

|![Screenshot of PDF of Village Map](/img/posts/2020-11-21-village-boundaries-from-pdf/TH-ABD-AURANGABAD.PNG)|
|:--:|
| Screenshot of PDF of Village Map |

This blog post is a work in progress and the roadmap I imagine is as follows. Do reach out if you know of a better way to do this or just to give feedback in general!

| Find python library suited for extraction of data from PDFs                 | ✅ | [`PyMuPDF`](https://pymupdf.readthedocs.io/en/latest/) |
| Extract Village names & positions as a vector file from PDF                 | ✅ |
| Extract Village boundaries as a vector file from pdf                        | ✅ |
| Study how to georeference vector file | Automated georeferencing?           |    |
| Georeference vector files                                                   |    |
| Correct topology errors in vector boundaries so we have complete polygons   |    |
| Spatial join of the vector file with village names to the vector boundaries |    |


Table of Contents
=================
1. [Find Python Library for PDFs](#1-find-python-library-for-pdfs)
2. [Extracting village names and positions](#2-extracting-village-names-and-positions)
3. [Extracting village boundaries](#3-extracting-village-boundaries)
4. [Study how to georeference vector files](#4-how-to-georeference-vector-files)
5. [Georeference vector files](#5-georeference-vector-files)
6. [Correct topology errors in vector boundaries](#6-correct-topology-errors-in-vector-boundaries)
7. [Spatial joins](#7-spatial-joins)


# 1. Find Python Library for PDFs
En route to choosing a suitable library to work with I found out a little bit about the structure of PDF documents themselves. 
I chose the library [`PyMuPDF`](https://pymupdf.readthedocs.io/en/latest/) because of it's comprehensive documentation, collection of examples and page reading and manipulation capabilities, both in raster (for images) and vector (for text and drawings) formats.

## 1.1 Installation/Imports
Install PyMuPDF with `pip install pymupdf` and import the required libraries, including PyMuPDF, which for some historical reasons must be imported with the name `fitz`

```
import os, sys       # python built in libraries for file handling
import fitz          # this imports the PyMuPDF library
```

Set up project directories as required
```
projFol = os.path.dirname(os.getcwd())
dataFol = os.path.join(projFol,"data")
imagesFol = os.path.join(projFol,"images")

print("Current working directory:\n",os.getcwd())    
# Current working directory: 
  C:\Users\Craig D\Code\pdftogis\notebooks 
print("\nProj folder:\n",projFol,"\nData folder;\n",dataFol)
# Proj folder: 
  C:\Users\Craig D\Code\pdftogis
# Data folder:
  C:\Users\Craig D\Code\pdftogis\data

filename = os.path.join(dataFol,'village-maps\\TH_ABD_Aurangabad.pdf')
print(filename)      # C:\Users\Craig D\Code\pdftogis\data\village-maps\TH_ABD_Aurangabad.pdf
```

the version of PyMuPDF I'm using is 1.18.3 
versions prior to this do not have the `getDrawings()` method which is essential to what we're attempting here

```
print(fitz.__doc__)
# PyMuPDF 1.18.3: Python bindings for the MuPDF 1.18.0 library.
# Version date: 2020-11-09 07:36:17.
# Built for Python 3.6 on win32 (64-bit).
```

## 1.2 Examination of the PDF

```
doc = fitz.open(filename)
print(doc.metadata.keys())  # dict_keys(['format', 'title', 'author', 'subject', 'keywords', 'creator', 'producer', 'creationDate', 'modDate', 'encryption'])
print(doc.pageCount)   # 1
```

The metadata.keys() method lists out the metadata variables of the document, which isn't super important for the purposes of this post. Since the pdf has only a single page we can load it in a variable `page` of the class Page which has numerous useful methods to access relevant elements of the page.

```
page = doc.loadPage(0)
dir(page)    # lists the methods and attributes of the Page class, including `getText()` , `getDrawings()` and more
```
As mentioned in the docs, 
> Page handling is at the core of MuPDF’s functionality.
> - You can render a page into a raster or vector (SVG) image, optionally zooming, rotating, shifting or shearing it.
> - You can extract a page’s text and images in many formats and search for text strings.
> - For PDF documents many more methods are available to add text or images to pages.

we'll explore some of this functionality in the remainder of this blogpost

[return to top](#table-of-contents)

# 2. Extracting Village names and positions
To get all of the text from the pdf along with the bounding boxes they lie within, use the `getText()` method of the page object, passing it the argument 'blocks'. This returns an output containing the lower left and upper right x,y coordinates of the position of each text string in the pdf.

```
text = page.getText('blocks')
for name in text[10:12]:
    print("\n",name)
#(515.4502563476562, 97.36946105957031, 559.2020874023438, 107.29110717773438, 'Phulambri\n', 10, 0)
# (171.25669860839844, 200.58009338378906, 217.4857940673828, 210.50173950195312, 'Khuldabad\n', 11, 0)
```

while this is the default format, what we ideally want is just a single x,y coordinate of each name, so that the names and coordinates give us a point gis layer, which can be merged with the polygon layer of the village boundaries. we can get this by simply taking the mean of the two x,y coordinates we already have

```
pointlayer = []

for name in text:
    x1,y1 = list(name)[0:2]
    x2,y2 = list(name)[2:4]
    villname = list(name)[4].replace("\n","")
    x0 = (x1+x2)/2
    y0 = (y1+y2)/2
    pointlayer.append((x0,y0,villname))

print(pointlayer[10:12])
```

this still leaves us with names that are clearly labels from the pdf file and not village names, but that's a worry for another day.

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
