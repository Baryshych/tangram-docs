<!doctype html>
<!--
    Tangram: real-time WebGL rendering for OpenStreetMap

    http://github.com/tangrams/tangram

-->
<html lang="en-us">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Tangram Documentation</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.0.0-beta.2/leaflet.css" />
    <style>
        body {
            margin: 0px;
            border: 0px;
            padding: 0px;
        }

        #map {
            background: rgba(0, 0, 0, 0);
            height: 100%;
            width: 100%;
            position: absolute;
        }
    </style>
  </head>

  <body>
    <div id="map"></div>

    <!-- 3rd party libraries -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.0.0-beta.2/leaflet.js"></script>
    <!-- bog-standard leaflet URL hash -->
    <script src="src/leaflet-hash.js"></script>
    <!-- End of 3rd party libraries -->

    <!-- Main tangram library -->
    <script src="https://unpkg.com/tangram/dist/tangram.min.js"></script>

    <!-- Demo module -->
    <script type="text/javascript">
    map = (function () {
        'use strict';

        var map_start_location = [40.70531887544228, -74.00976419448853, 16];

        /*** URL parsing ***/

        // leaflet-style URL hash pattern:
        // ?style.yaml#[zoom],[lat],[lng]
        var url_hash = window.location.hash.slice(1).split('/');
        if (url_hash.length == 3) {
            map_start_location = [url_hash[1],url_hash[2], url_hash[0]];
            // convert from strings
            map_start_location = map_start_location.map(Number);
        }

        var style_file = 'scene.yaml';
        var url_search = window.location.search.slice(1);
        if (url_search.length > 0) {
            var ext = url_search.lastIndexOf('yaml');
            if (ext > -1) {
                style_file = url_search.substr(0, ext + 4);
            }
        }

        /*** Map ***/

        var map = L.map('map',
            {'keyboardZoomOffset': .05}
        );

        var layer = Tangram.leafletLayer({
            scene: style_file,
            attribution: '<a href="https://github.com/tangrams/tangram" target="_blank">Tangram</a> | &copy; OSM contributors'
        });

        window.layer = layer;
        var scene = layer.scene;
        window.scene = scene;

        map.setView(map_start_location.slice(0, 2), map_start_location[2]);
        var hash = new L.Hash(map);

        window.addEventListener('load', function () {
            // Scene initialized
            layer.addTo(map);
        });

        return map;

    }());
    </script>

    <script>
      (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
      (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
      m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
      })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

      ga('create', 'UA-47035811-1', 'auto');
      ga('send', 'pageview');
    </script>

  </body>
</html>
