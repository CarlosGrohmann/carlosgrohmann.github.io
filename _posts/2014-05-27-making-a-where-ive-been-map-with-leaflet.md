---
layout: post
title: Making a "where I've been" map with Leaflet
author: CarlosGrohmann
date: 2014-05-27
categories: 
tags: 
permalink: http://carlosgrohmann.com/making-a-where-ive-been-map-with-leaflet/
published: true
---


In my website I have a map of some (most? all?) of the places I've been ([here](/gmaps.html)). When I first started with this idea (in 2011, I guess), I used [GoogleMaps API](https://developers.google.com/maps/) and all the points of the map had to be defined in the HTML file, which was very annoying and not very good in terms of maintenance.  Then I discovered [MapBox](https://www.mapbox.com) and [TileMill](https://www.mapbox.com/tilemill/), and my life changed. TileMill is a very nice software for creating maps, and you can export the results to your free MapBox account, and have your map on-line in no time. The problem is: if I wanted to update the 'database' of places, I'd have to re-import it into TileMill, re-export the project and re-import it into my MapBox account. Too much hassle. So I decided to try something different. I wanted to be able to save/export my 'database' (a simple spreadsheet) whenever I updated it, and the map would also be updated without additional steps. After a little bit of Googling, I had a few options: Leaflet, Mapbox and CartoDB. [Leaflet](http://leafletjs.com) is an open-source JavaScript library for mobile-friendly maps, MapBox is a well-know an open source mapping platform and provide the JavaScript library mapbox.js (a [plugin](https://www.mapbox.com/help/mapboxjs-a-leaflet-plugin/) that extends Leaflet's functionalities), while [CartoDB](https://cartodb.com) is "a cloud-based solution for all your mapping needs". Both MapBox and CartoDB offer free accounts (with limits on storage/views/etc). With CartoDB it is really simple to import a CSV file (or a lot of other formats) into a table, create a beautiful visualisation of the data, and make it public, share it, etc. I think that CartoDB is easier to maintain than MapBox, since they offer some nice tools to simply update your data (table). But for me, it has the same disadvantage of MapBox: I have to login to the site and upload/update my data. I didn't want to keep my data in another server. I prefer to upload a text file to the same server where my site is hosted. Even if that requires converting (from, say, CSV to GeoJSON or whatever), because I can put all that into a script. And I like scripts. [They save us time.](http://xkcd.com/1319/) So I decided to use Leaflet. I'll show you how I did it. First, the entire HTML code: 

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    

    



Now let's see what each part does. The first thing to do is to call the needed libraries. In the `head` section of your file, insert these lines to access `leaflet.js`, `JQuery` and the leaflet plugin `Leaflet.label`. 

    

    

    

    

    

    

    

    

    

    

    

    

    



Next we need to define some basic properties for our map: 

    

    

    

    

    

    

    

    

    



Now the fun begins. In my case, I opt for using a GeoJSON file with the places I've been. To create this file you need to: 1 - export it from spreadsheet as CSV 2 - open it in [QGIS](https://www.qgis.org) 3 - save as GeoJSON 4 - simple, right? OK, now that we have the GeoJSON file, we need to do some tricks to open it directly (you can make it a JavaScript file by manually editing it, by it goes against our initial idea). The trick is to reference it in the HTML as a relative link (I got the idea from this [post](http://lyzidiamond.com/posts/osgeo-august-meeting/)). In this example, the `places.geojson` file is in the same directory as the hmtl file. 

    

    

    

    

    



Good. Time to create the base layers of the map. I used two layers from MapBox, one from NaturalEarth (for the smaller zooms) and the other is from my personal account (you should have your account as well).
