---
layout: post
title : "Data Collection and Visualization for Non-profits"
subtitle : "ODK and Google Data Studio"
date: 2018-12-04 12:00:00
author: "Craig Dsouza"
header-img: "img/posts/imdaws/banner.png"
comments: true
tags: [ odk,surveys,dashboards ]
category: "blog"
---

This blog post is a step by step guide for going from data collection to visualization using Free or Open Source Software (FOSS). The hope is that this guide helps non-profits seeking to assess and communicate their impact with easy to use and free tools such as Open Data Kit and Google Data Studio.
 
# 1. Data Collection
A lot of non-profits are now familiar with the open source suite of software tools called '[Open Data Kit](https://opendatakit.org/)' or similar tools such as Kobo Toolbox. These tools make use of an open format called [xlsform](http://xlsform.org/en/) for survey form creation. The XLSform format supports a wide range of question types including photos, videos, geolocation, signature, text, integers, dates etc. Forms created using ODK, most importantly work offline, using the browser's cache to store responses if the internet connection is unstable and when the connection is restored the forms are uploaded. It is also simple to create XLSforms, a user familiar with Microsoft Excel or similar spreadsheet softwares can easily do so. This tutorial will not discuss at length the process of creating XLSforms since that is well documented on the project website. Instead this guide 
will discuss how one can go about building custom dashboards that continuously track the survey data being uploaded.

## 1.1 The Server
Data that is collected using ODK forms/Kobo forms can be collected either on an ODK Aggregate Server or Kobo Toolbox's own server. ODK Aggregate is more cumbersome to setup and 
it requires the data collector to have their own server space either on a cloud service provider or their own private server. Kobo Toolbox on the other hand removes this 
cumbersome requirement by providing server space for non-profits to submit their data to. The user thus is only required to setup a Kobo Toolbox account where they can login to 
visualize their data. While Kobo Toolbox does provide a great service it is limited when it comes to visualization of the data collected. Beyond the set of standard charts a user
cannot visualize their data differently. It is an appropriate choice for shorter term projects and small teams. It is also more appropriate for basic metric tracking but cannot 
allow for insights derived by combining multiple metrics. 

# 2. Data Visualization
An alternative to Kobo Toolbox, for more complex data collection systems is Google Data Studio. Data Studio is a new Google product, made open for all users since Sept of 2018.
Data Studio is a drag and drop dashboard creator which can access multiple data sources to display metrics. In this guide we will make a Data Studio dashboard to visualize data
collected with ODK Collect. The data will be submitted to Google Sheets instead of ODK Aggregate or Kobo Toolbox and Data Studio will read from Google Sheets.

|![Preview of IMD AWS Web App](/img/posts/imdaws/data-preview.PNG)|
|:--:|
| 1. Snapshot preview of IMD data |

I was curious about the general awareness about this app. Please click one of the poll options below to let me know.
[![](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20knew%20about%20the%20IMD%20AWS%20Web%20App)](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20knew%20about%20the%20IMD%20AWS%20Web%20App/vote)
[![](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20did%20not%20know%20about%20it)](https://api.gh-polls.com/poll/01CQ3QM0RNSVDXQEQJEAY364NG/I%20did%20not%20know%20about%20it/vote)



|![Preview of Old IMD AWS Web App](/img/posts/imdaws/old-imdaws.png)|
|:--:|
| 2. Snapshot preview of old IMD AWS Web App |

# 2. Challenges that remain


# 3. Potential uses of this data


# 4. Plans ahead


# 5. How you can contribute


Will continue to post updates on this effort below.. stay tuned..

# Updates

