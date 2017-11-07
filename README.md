---
Title: "Web Services and APIs"
Author: "John Fay"
Date: "Fall 2017"
---

# Using APIs

In 2003 I attended an ESRI workshop at a Hyatt Hotel in downtown San Francisco. ESRI wanted to demonstrate some new powerful network server technology – their Internet Mapping System or [ArcIMS](http://en.wikipedia.org/wiki/ArcIMS). Their presentation flopped because the hotel’s network was not as fast as they had expected and drawing the data on screen was painfully slow. Using networked data was an interesting idea, but my takeaway was that it was always best to have all the resources you needed to get a spatial analysis done stored on the local desktop. 

Now, of course, high speed internet access is almost taken for granted. If you don’t have a smartphone, tablet, or ultraportable laptop with a data plan, then it’s likely that a free Wi-Fi hotspot is not too far away. And the data transfer speeds are far superior to what was available a decade ago. What was lacking in the ESRI presentation I attended years ago is now abundant, and what we see is a huge growth in web-enabled or “cloud-based” computing. 

The impact on spatial analysis is that we no longer need local copies of data on our machines to run analyses. Instead, we can simply connect to various data sources located on remote servers and query, clip, overlay, or mask these datasets directly. This offers the huge advantage of centralized management of these datasets; with everyone tapping into the same central dataset, you ensure that edits to the dataset are propagated to all users. It also makes doing spatial analyses on local machines much more lightweight since we don’t have to download entire datasets just to do analyses that require a small section of them.
In the past few years, however, servers have been going beyond just providing live links to data: they are now providing services. In other words, instead of using the tools contained in desktop ArcGIS we can use tools served up in the Cloud. If we want to calculate the average slope within a specified distance of certain streams, we can instruct a set of servers to find the elevation and stream data, to buffer the stream, calculate the slope with that buffered distance and report the average. All the processing is done on the server(s) hosting the service, meaning we don’t even need a beefy computer to get it done; a smart phone will do! 

In this tutorial we explore the application of web services to spatial analysis. It’s an exciting and rapidly evolving field: what’s “cutting edge” this year is likely to be old news the next. However, getting in on this technology at an early stage should provide you with a more robust understanding of how it works and thus how you can utilize its power most effectively. At this stage in the development of web services and cloud-based GIS, however, you’ll need to be prepared for bugs and limited documentation. It’s all part of the excitement of being at the forefront of technology. 

In this tutorial we begin simply, looking at web services from within web browsers. From there we explore how these services can be controlled via scripts (e.g. Python). Then we examine more advanced interfaces to web services, i.e. via APIs and applications that are integrated with web services (e.g. ArcGIS.com and Desktop ArcGIS). 



## Part 1. Exploring Web Services with a web browser

HTTP web services are programmatic ways of sending and receiving data from remote servers using the operations of HTTP directly. In its simplest form, it's a URL we send to our web browser that acts as a command, complete with arguments. 

#### An example:

To explain this concept better, try this: 

* Go to the NWIS web site that we used to download data for one of our first tutorials:
  http://waterdata.usgs.gov/nwis 
* Click on the **Current Conditions** button, then the **Build Current Conditions Table** link.
* Next, check **Site Number** and click **Submit**
* In the next page, enter the site number **02085070**, accept all the other defaults, and click the **Submit** button at the very bottom of the page.
* In the next page, click the link in the <u>Site Number</u> column of the table.
* In the next page, change 'Output Format' to <u>Tab-Separated</u> and 'Days' to **1**. Clear out the dates in the "Begin date" and "End date" boxes, then hit `GO`.

The result should be a text screen in your browser listing all data collected in the last 24 hours for site 02085070 - Eno River Near Durham. More importantly, take a look at the URL for the current page:

[http://waterdata.usgs.gov/nwis/uv?cb_00060=on&cb_00065=on&format=rdb&site_no=02085070&period=1&begin_date=&end_date=](http://waterdata.usgs.gov/nwis/uv?cb_00060=on&cb_00065=on&format=rdb&period=1&begin_date=&end_date=&site_no=02085070)

Edit this URL, changing the site_no to from 02085070 to 02087183. You'll see that the page now lists data for site 02087183 - Neuse River near Falls Lake, NC. 

***In other words, this URL is essentially a command string sent to the NWIS server with arguments embedded within it!*** 

Generally speaking arguments in a Web Service URL follow the '?' and are separated '&'. We can then parse this URL into the parts listed below. We can deduce what they might mean through some keen observation (e.g. look at the web page used to generate the URL), and perhaps through experimentation (i.e. change the values and see what happens). 

| component                      | meaning                                  |
| ------------------------------ | ---------------------------------------- |
| http://waterdata.usgs.gov/nwis | the web-service provider                 |
| uv                             | the service provided                     |
| cb_                            | 00060=on	include discharge data in the output |
| cb_                            | 00065=on	include gage height data in the output |
| format=rdb                     | list the output as a tab-separated       |
| period=1                       | days to include in the output table      |
| site_no=02085070               | the site number                          |



#### Other examples:

Another, more documented example of this can be seen in USGS web services hosted at the USGS’ Biodiversity Information Serving Our Nation (BISON), which provides its own API:
https://bison.usgs.gov/doc/api.jsp 

The BISON API provides a number of services and documents their parameters. For example, copy and paste their example URL in a browser: 

http://bison.usgs.gov/api/search.json?species=Bison%20bison&type=scientific_name&start=0&count=1

You see the result is in JSON format, and you can probably guess how you might edit this URL to return results for a different species. (Try “Cryptobranchus alleganiensis” – aka the hellbender). 

The web site documents various parameters you can invoke in the URL to filter the records returned. These include a filter for the county FIPS code. We can utilize this to return a set of record for a specific county, e.g. Durham (FIPS 37063)

Try this URL, which returns the first 10,000 records in Durham County. 
http://bison.usgs.gov/api/search.json?count=10000&countyFIPS=37063   

With some editing you could shape this data into a table which you could import into GIS…



### Using HTTP Services with Python and the `requests` module

Python allows us to programmatically extract data from web services such as the above NWIS and BISON examples using its `requests `module. This module works similarly to the `urllib` module (in fact it is built on top of it), bit it also includes a few features that facilitate using web services and APIs. 

I've created a few notebooks demonstrating these concepts

* `11-NWIS-discharge-data-as-API.ipynb` - 
* `12-Exploring-the-BISON-API`
* ​

