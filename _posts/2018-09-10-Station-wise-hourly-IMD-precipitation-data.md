---
layout: post
title : "Station-wise,Hourly rainfall data from IMD now available"
subtitle : "see it to believe it!"
date: 2018-09-10 12:00:00
author: "Craig Dsouza"
header-img: "img/posts/imdaws/banner.PNG"
comments: true
tags: [ hydrology,precipitation]
---

# 1. In Brief
The Indian Meteorological Department (IMD) which has for long refrained from openly publishing its station-wise precipitation data is now doing so.
I recently came across their new web app: [http://aws.imd.gov.in/](http://aws.imd.gov.in/), which allows users to select a state and view a
table of hourly precipitation for all stations in the state. Preview can be seen in Fig.1 below.  

|![Preview of IMD AWS Web App](/img/posts/imdaws/data-preview.PNG)|
|:--:|
| 1. Snapshot preview of IMD data |

I was curious about the general awareness about this app. Please click one of the poll options below to let me know.
[![](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20knew%20about%20the%20IMD%20AWS%20Web%20App)](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20knew%20about%20the%20IMD%20AWS%20Web%20App/vote)
[![](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20did%20not%20know%20about%20it)](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20did%20not%20know%20about%20it/vote)

If anyone already knew of the existence of this web app do let me know how you found out about it. Curious to know how IMD publishes new updates such as this.
The user interface is very intuitive and gives a smooth experience overall. The data itself doesn't take too long to load. A version of this
web app was also available more than 2 years ago, after which it was taken down. That however had a much less user friendly, slower interface.

|![Preview of Old IMD AWS Web App](/img/posts/imdaws/old-imdaws.png)|
|:--:|
| 2. Snapshot preview of old IMD AWS Web App |

# 2. Challenges that remain
A couple of issues still remain however. One, although the web app provides the names of the stations, it doesn't provide a location. There are a few
ways this issue can be resolved, perhaps by scraping the locations of the stations from the 'Data on map' page of the web app. Or by matching station names
available here with other lists of station names (with geolocation) available on the IMD's main website. Secondly, an issue that remains is that the data 
provided here can only be accessed one week at a time. This is a design feature presumably to keep users from viewing excessively large tables of data that
may cause the web app to crash. 

Over the weekend I attempted a bit of web scraping for the first time to resolve this second issue. The code shared in this 
[repository](https://github.com/craigdsouza/getRainfallData) allows users to automate the process of visiting the IMD web app and saving station data, 
one week at a time. The plan right now is to run this code once a week and save each batch of new data that is generated and archive the records as a public 
commons. You can access the saved data [here](https://github.com/craigdsouza/getRainfallData/tree/master/data) . The acronyms, ARG, AWS, AGRO you see are 
the different types of weather stations operated by IMD. According to their page, there are 1351, 573 and 128 stations of each kind respectively.

# 3. Potential uses of this data
For those wondering 'what's the big deal?', it is! Hourly precipitation data opens up a whole range of possibilities for improving our understanding of the water
cycle, studying phenomenon such as floods at a more granular level, examining droughts with 'dry spell analysis', more accurately mapping how precipitation affects
base flows, and much much more.

# 4. Plans ahead
The next thing on the to-do list is to write a bit of code to access older data, that isn't directly accessible using the web app interface. Another goal is to
develop heatmap visualizations, and integrate them into the [Open Water Data](http://water-data-web-app.appspot.com/) project, perhaps something like 
[this](https://www.patrick-wied.at/static/heatmapjs/) Anyone wishing to collaborate on this please leave me a message! 

# 5. How you can contribute
If you wish to contribute but not with the code, you still can. Please share this [tool](http://aws.imd.gov.in/), and the [data archive](https://github.com/craigdsouza/getRainfallData/tree/master/data)
widely, blog about it, attribution to the source would be much appreciated! Developing use cases for this data would be a great bonus. I am particularly keen on organizing 
workshops around learning to use the data and developing preliminary analysis, if that interests you ping me. If you have your own weather stations please share that data too. 
The hope here is that creating more shared commons data repositories can help build capacity and break institutional silos.

Will continue to post updates on this effort below.. stay tuned..


