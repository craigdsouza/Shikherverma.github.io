---
layout: post
title : "Drought monitoring at a granular village level with Dugwells data"
subtitle : "an attempt at a proof of concept"
date: 2018-09-03 12:00:00
author: "Craig Dsouza"
header-img: "img/posts/ddm/overpass-export.png"
comments: true
tags: [ geohydrology,remoteSensing]
category: "blog"
---

# 1. Hypothesis
When satellite imagery is queried for the presence of water it is possible to identify individual pixels which correspond to dugwells.
Thus conversely, if a map of geolocated dugwells is available, it is possible to monitor them over time to understand the state of the shallow aquifer in the vicinity.

# 2. Challenges
While the hypothesis itself is simple enough, the reality may not be so simple. Dugwells in many places are used as temporary storage for water drawn from borewells.
Thus satellite imagery of these dugwells may give a false impression of water availability in the shallow aquifer. The challenge therefore becomes to separate the signal from the noise.

# 3. Goals
- Examine hypothesis and based on results obtained examine possibility of developing a proxy indicator for water security/scarcity at village level

# 4. FAQ
Why dugwells, when soil moisture is used for drought monitoring?
- I don't yet have a strong logic for this, other than a gut feeling that dugwells (presence/absence of water) seems to be a more intuitive way to communicate water scarcity rather than soil moisture.
However for a better scientific understanding we could study soil moisture in conjunction with dugwells.

How can you contribute?
- With ground observations. If you know of the pattern of use of a particular dugwell, when the well dries out, especially whether or not the dugwell is filled with water from surrounding borewells.
- Mark out dugwells in OpenStreetMap (See http://tasks.openstreetmap.in/project/99) This is a beginner level task and a fun way to learn GIS.
- Give me your feedback or thoughts on the hypothesis
- Help me code in Google Earth Engine to analyse dugwell data

# 5. Process documentation (rough notes, will keep adding more details)
Mark out dugwells for a village
Use overpass turbo to export kml - add code
geojson convert kml to shp
asset ingestion to GEE
- see individual feature error in geojson
