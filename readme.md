使用方式：
-  1.文件放在Source/Scene目录下

-  2.修改Cesium.js源码
  找到WebMapServiceImageryProvider,然后添加对应的WebFeatureServiceImageryProvider的代码
 *    (1)Cesium['WebFeatureServiceImageryProvider'] = Scene_WebFeatureServiceImageryProvider;
         Cesium['WebMapServiceImageryProvider'] = Scene_WebMapServiceImageryProvider;

 *    (2)'./Scene/WebFeatureServiceImageryProvider', './Scene/WebMapServiceImageryProvider'
  
 -  3.sandcastle中使用代码如下：

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8">
            <meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">  <!-- Use Chrome Frame in IE -->
            <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
            <meta name="description" content="Interactive 3D Tiles styling.">
            <meta name="cesium-sandcastle-labels" content="Showcases, 3D Tiles">
            <title>Point</title>
            <script src="../Apps/Sandcastle/Sandcastle-header.js"></script>
            <script src="../ThirdParty/requirejs-2.1.20/require.js"></script>
            <script type="text/javascript">
            require.config({
                baseUrl : '../Source',
                waitSeconds : 60
            });
            </script>
        </head>
        <body class="sandcastle-loading" data-sandcastle-bucket="bucket-requirejs.html">
        <style>
            @import url(../Apps/Sandcastle/templates/bucket.css);
        </style>
        <div id="cesiumContainer" class="fullSize"></div>
        <div id="loadingOverlay"><h1>Loading...</h1></div>

        <script src="../Source/Scene/WebFeatureServiceImageryProvider.js"></script>
        <script id="cesium_sandcastle_script">

        function startup(Cesium) {
                'use strict';
            var viewer = new Cesium.Viewer('cesiumContainer',{
                
            });
            
            //create the wfs getter to load a vector layer on the globe.
            var wfs = new Cesium.WebFeatureServiceImageryProvider({
                url : "http://192.168.10.167:9011/geoserver/xy",
                layers : "xy:aa-line",
                viewer : viewer
            });
        Sandcastle.finishedLoading();
        }
        if (typeof Cesium !== 'undefined') {
            startup(Cesium);
        } else if (typeof require === 'function') {
            require(['Cesium'], startup);
        }
        </script>
        </body>
        </html>

4.build
在源码根目录执行npm run minify
5.build后的使用方式
        <!DOCTYPE html>
        <html lang="en">
        <head>
        <!-- Use correct character set. -->
        <meta charset="utf-8">
        <!-- Tell IE to use the latest, best version. -->
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <!-- Make the application on mobile take up the full browser screen and disable user scaling. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
        <title>Hello World!</title>
        <script src="../Build/Cesium/Cesium.js"></script>
        
        <style>
            @import url(../Build/Cesium/Widgets/widgets.css);
            @import url(../Sandcastle/templates/bucket.css);
            #toolbar {
                background: rgba(42, 42, 42, 0.8);
                padding: 4px;
                border-radius: 4px;
            }
            #toolbar input {
                vertical-align: middle;
                padding-top: 2px;
                padding-bottom: 2px;
            }
            #toolbar .header {
                font-weight: bold;
            }
            html, body, #cesiumContainer {
                width: 100%; height: 100%; margin: 0; padding: 0; overflow: hidden;
            }
        </style>
        </head>
        <body>
        <div id="cesiumContainer"></div>
        <script>
            var viewer = new Cesium.Viewer('cesiumContainer');
            //create the wfs getter to load a vector layer on the globe.
            var wfs = new Cesium.WebFeatureServiceImageryProvider({
                url : "http://192.168.10.167:9011/geoserver/xy",
                layers : "xy:aa-line",
                viewer : viewer
            });
        </script>
        </body>
        </html>
