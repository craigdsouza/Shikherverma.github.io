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

An alternative to Kobo Toolbox, for more complex data collection systems is Google Sheets. We will use Google Data Studio, a new Google product, made open for all users since Sept of 2018 to visualize the data collected on Google Sheets. Data Studio is a versatile drag and drop dashboard creator which can access multiple data sources to display metrics. 

## 1.2 Managing Permissions


# 2. Data Visualization


# 3. Challenges that remain





