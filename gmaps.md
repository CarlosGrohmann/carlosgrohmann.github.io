---
layout: page
title: Map
subtitle: 
published: true
---
Map created with <a href="http://leafletjs.com" target="_blank">Leaflet</a>, using base layer data from <a href="https://www.mapbox.com" target="_blank">MapBox</a>.
There's a <a href="http://carlosgrohmann.com/blog/?p=787" target="_blank">post on my blog</a> explaining how to make this map.

For a more immersive experience, there's a full-screen version <a href="/gmaps_full.html" target="_blank">here</a> (opens in a new tab/window).
<div>
<h4>Some interesting places I've been</h4>

<!-- JQuery -->
<script src="http://code.jquery.com/jquery-1.10.2.min.js"></script>

<!-- Leaflet stuff -->
<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.css" />
<script src="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.js"></script>

<!-- Leaflet Label plugin -->
<script src='https://api.tiles.mapbox.com/mapbox.js/plugins/leaflet-label/v0.2.1/leaflet.label.js'></script>
<link href='https://api.tiles.mapbox.com/mapbox.js/plugins/leaflet-label/v0.2.1/leaflet.label.css' rel='stylesheet' />

<!-- /* LeafLet map props*/ -->
<style type="text/css">
#map { height: 450px; width: 650px;}
</style>

<!-- LeafLet map  - relative link -->
<div id="map"></div>
<!-- places.geojson -->
<link rel="points" type="application/json" href='/places.geojson'>
<!-- <script src='places.geojson' type="text/javascript"></script> http://{s}.tiles.mapbox.com/v4/mapbox.natural-earth-2/{z}/{x}/{y}.png-->

<!--  -->
<!-- https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png -->
<!-- https://{s}.tiles.mapbox.com/v4/mapbox.satellite/${z}/${x}/${y}.png -->



<script>
    // create map
    var map = L.map('map').setView([15, 0], 1);
    // Basic MBox map - zooms 0-4
    mapbox_simple = L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
            maxZoom: 4,
            minZoom: 0,
            id: 'mapbox.streets',
            accessToken: 'pk.eyJ1IjoiY2FybG9zZ3JvaG1hbm4iLCJhIjoiOGNmS3ptYyJ9.WzKUdXGmEgbl4C5EdQMhMw',
            attribution: '&copy; Tiles Courtesy of <a href="https://www.mapbox.com" title="MapBox" target="_blank">MapBox</a>',
            }).addTo(map);
    // MapBox Terrain - zooms 5-18
    MBTerrain = L.tileLayer('http://{s}.tiles.mapbox.com/v3/carlosgrohmann.ibb4756i/{z}/{x}/{y}.png', {
            maxZoom: 18,
            minZoom: 5,
            attribution: '&copy; Tiles Courtesy of <a href="https://www.mapbox.com" title="MapBox" target="_blank">MapBox</a>',
            }).addTo(map);                
    // color itens coording to properties
    function getColor(category) {
        return category == "airport"  ?   '#002E63' : 
               category == "place"    ?   '#FF7E00' :
                                         '#000';
    }
    // Attaching a GeoJSON file with relative link: (from: http://lyzidiamond.com/posts/osgeo-august-meeting/)
      $.getJSON($('link[rel="points"]').attr("href"), function(data) {
        var geoJsonLayer = L.geoJson(data, {
            onEachFeature: function (feature, layer) {
                var desc = feature.properties.Title
                layer.bindLabel(desc)
            },
            pointToLayer: function (feature, latlng) {
                return L.circleMarker(latlng, {
                radius: 3,
                Label: getColor(feature.properties.Title), 
                fillColor: getColor(feature.properties.category), 
                color: "#000",
                weight: 0.5,
                opacity: 0.8,
                fillOpacity: 0.8,})
            },
        });
        geoJsonLayer.addTo(map);
      });
</script>

&nbsp;
&nbsp;
&nbsp;
