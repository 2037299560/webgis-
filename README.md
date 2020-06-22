# webgis-基础代码
基于arcgis server js api的基础功能代码


## 第二章  Javascript API基础

#### **Arcgis Server api for javascript 的 hello world示例**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>第一个JavaScript API应用</title>
    <link rel="stylesheet" href="http://js.arcgis.com/3.9/js/esri/css/esri.css">
    <style>
        html, body, #mapDiv {
            padding: 0;
            margin: 0;
            height: 100%;
        }
    </style>
    <script src="http://js.arcgis.com/3.9/"></script>
    <script type="text/javascript">        
        require(
		["esri/map", "esri/layers/ArcGISTiledMapServiceLayer", "dojo/domReady!"], function (Map, ArcGISTiledMapServiceLayer) {
            // 以下是创建地图与加入底图的代码
            var map = new Map("mapDiv");
            var agoServiceURL = "http://server.arcgisonline.com/ArcGIS/rest/services/ESRI_StreetMap_World_2D/MapServer";
            var agoLayer = new ArcGISTiledMapServiceLayer(agoServiceURL, { displayLevels: [0, 1, 2, 3, 4, 5, 6, 7] });
            map.addLayer(agoLayer);
        });
    </script>
</head>
<body class="tundra">
    <div id="mapDiv"></div>
</body>
</html>

```

**关键点**:

   - 引入css样式文件和 js api文件

   - 设置body标签的class

   - 设置html，body，以及自定义的id为mapDiv的元素的css样式

     







## 第三章 页面布局设计

#### **布局小部件综合使用**

```html
<!DOCTYPE html>
<html>
<head>
    <title>页面布局</title>
    <link rel="stylesheet" href="http://js.arcgis.com/3.9/js/dojo/dijit/themes/tundra/tundra.css" />
    <link rel="stylesheet" href="http://js.arcgis.com/3.9/js/esri/css/esri.css" />
    <style>       
        html, body, #main{	
            width: 100%;
            height: 100%;
            margin: 0;
        }
    </style>
	<script type="text/javascript">
	    dojoConfig = {
	        isDebug: true,
	        async: true
	    };
    </script>
    <script src="http://js.arcgis.com/3.9/"></script>
    <script type="text/javascript">
        require(["dojo/parser", "esri/map", "esri/layers/ArcGISTiledMapServiceLayer",
            "dijit/layout/BorderContainer", "dijit/layout/ContentPane",
            "dijit/layout/AccordionContainer", "dojo/domReady!"],
            function (parser, Map, ArcGISTiledMapServiceLayer) {
                parser.parse();

                var map = new Map("mapDiv");
                var agoServiceURL = "http://server.arcgisonline.com/ArcGIS/rest/services/ESRI_StreetMap_World_2D/MapServer";
                var agoLayer = new ArcGISTiledMapServiceLayer(agoServiceURL, { displayLevels: [0, 1, 2, 3, 4, 5, 6, 7] });
                map.addLayer(agoLayer);
            });
    </script>
</head>
<body class="tundra">
    <div data-dojo-type="dijit/layout/BorderContainer" data-dojo-props="design:'headline',gutters:false"  id="main">
        <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'top'" 
            style="background-color: #b39b86; height: 10%;">
            放置单位或公司标志、本应用系统名称等
        </div>
        <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'left', splitter:'true'"
            style="background-color: #acb386; width: 100px;">
            一般为菜单
        </div>
        <div id="mapDiv" data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'center'"
            style="background-color: #f5ffbf; padding: 10px;">            
        </div> 
        <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'right', splitter:'true'"
            style="background-color: #acb386; width: 100px;">
            <div id="accordionContainer" data-dojo-type="dijit/layout/AccordionContainer" 
                style="padding: 0px; overflow: hidden; z-index: 29;">
                <div data-dojo-type="dijit/layout/ContentPane" title="查询" style="overflow: hidden;">
                    <div id="findServicesDiv">
                    </div>
                </div>
                <div id="identifyResultsPane" data-dojo-type="dijit/layout/ContentPane" style="overflow: hidden;" title="查询结果">
                    <div id="resultsDiv">
                    </div>
                    <br>
                </div>
                <div id="parcelResultsPane" data-dojo-type="dijit/layout/ContentPane" title="缓冲区分析">
                </div>
                <div data-dojo-type="dijit/layout/ContentPane" style="width: 100%" title="图层控制">
                    <br>
                    <div id="layerConfigDiv">
                    </div>
                </div>
            </div>
        </div>

        <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'bottom', splitter:'true'"
            style="background-color: #b39b86; height: 50px;">
            版权信息等
        </div>
    </div>
</body>
</html>
```

**使用要点：**

- 要在require函数中引入相关模块
- 在require函数中引入parser模块，并调用`paser.parse()`来解析页面中的dojo元素
- 在body元素中设置相应布局部件的属性



## 第四章 地图与图层

## 第五章 空间参考系统与几何对象

#### **绘制几何对象**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=device-width,user-scalable=no">
    
    <meta name="viewport" content="initial-scale=1, maximum-scale=1,user-scalable=no">
    <title>Maps Toolbar</title>
    
    <link rel="stylesheet" href="https://js.arcgis.com/3.32/dijit/themes/nihilo/nihilo.css">
    <link rel="stylesheet" href="https://js.arcgis.com/3.32/esri/css/esri.css">
    <style>
      html, body, #mainWindow {
        font-family: sans-serif; 
        height: 100%; 
        width: 100%; 
      }
      html, body {
        margin: 0; 
        padding: 0;
      }
      #header {
        height: 80px; 
        overflow: auto;
        padding: 0.5em;
      }
    </style>
    
    <script src="https://js.arcgis.com/3.32/"></script>
    <script>
      var map, toolbar, symbol, geomTask;

      require([
        "esri/map", 
        "esri/toolbars/draw",
        "esri/graphic",

        "esri/symbols/SimpleMarkerSymbol",
        "esri/symbols/SimpleLineSymbol",
        "esri/symbols/SimpleFillSymbol",

        "dojo/parser", "dijit/registry",

        "dijit/layout/BorderContainer", "dijit/layout/ContentPane", 
        "dijit/form/Button", "dijit/WidgetSet", "dojo/domReady!"
      ], function(
        Map, Draw, Graphic,
        SimpleMarkerSymbol, SimpleLineSymbol, SimpleFillSymbol,
        parser, registry
      ) {
        parser.parse();

        map = new Map("map", {
          basemap: "streets",
          center: [-15.469, 36.428],
          zoom: 3
        });
        
        map.on("load", createToolbar);

        registry.forEach(function(d) {
          if ( d.declaredClass === "dijit.form.Button" ) {
            d.on("click", activateTool);
          }
        });

        function activateTool() {
          var tool = this.label.toUpperCase().replace(/ /g, "_");
          toolbar.activate(Draw[tool]);
          map.hideZoomSlider();
        }

        function createToolbar(themap) {
          toolbar = new Draw(map);
          toolbar.on("draw-end", addToMap);
        }

        function addToMap(evt) {
          var symbol;
          toolbar.deactivate();
          map.showZoomSlider();
          switch (evt.geometry.type) {
            case "point":
            case "multipoint":
              symbol = new SimpleMarkerSymbol();
              break;
            case "polyline":
              symbol = new SimpleLineSymbol();
              break;
            default:
              symbol = new SimpleFillSymbol();
              break;
          }
          var graphic = new Graphic(evt.geometry, symbol);
          map.graphics.add(graphic);
        }
      });
    </script>
  </head>
  <body class="nihilo">

  <div id="mainWindow" data-dojo-type="dijit/layout/BorderContainer" data-dojo-props="design:'headline'">
    <div id="header" data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'top'">
      <span>Draw:<br /></span>
      <button data-dojo-type="dijit/form/Button">Point</button>
      <button data-dojo-type="dijit/form/Button">Multi Point</button>
      <button data-dojo-type="dijit/form/Button">Line</button>
      <button data-dojo-type="dijit/form/Button">Polyline</button>
      <button data-dojo-type="dijit/form/Button">Polygon</button>
      <button data-dojo-type="dijit/form/Button">Freehand Polyline</button>
      <button data-dojo-type="dijit/form/Button">Freehand Polygon</button>
      
      <button data-dojo-type="dijit/form/Button">Arrow</button>
      <button data-dojo-type="dijit/form/Button">Triangle</button>
      <button data-dojo-type="dijit/form/Button">Circle</button>
      <button data-dojo-type="dijit/form/Button">Ellipse</button>
    </div>
    <div id="map" data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'center'"></div>
  </div>

  </body>
</html>
```

**几何绘图工具使用步骤**：

1. 地图加载的时候创建绘图工具条
2. 在绘图按钮的单机事件中激活对应的绘图工具（例如在"点"按钮的单击事件中激活绘制点的工具)
3. 在工具条的绘制完毕事件中将绘制的几何图形添加到地图的graphics图层中



## 第六章 符号与图形

## 第七章 要素图层与专题图

#### **范围专题图**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>范围专题图</title>
    <link rel="stylesheet" href="http://js.arcgis.com/3.9/js/esri/css/esri.css">
    <style>
        html, body, #map {
            height: 100%;
            margin: 0;
        }
        #info {
            position: absolute;
            right: 0;
            top: 0;
            font: 14px sans-serif;
            background: #fff;
            width: 160px;
            text-align: center;
            border-radius: 0 0 0 10px;
        }
    </style>
    <script src="http://js.arcgis.com/3.9/"></script>
    <script>
        var map;
        require(["esri/map", "esri/layers/ArcGISTiledMapServiceLayer", "esri/layers/FeatureLayer",
          "esri/InfoTemplate", "esri/renderers/ClassBreaksRenderer", "esri/symbols/SimpleFillSymbol", "esri/dijit/Legend",
          "esri/Color", "dojo/domReady!"
        ], function (Map, ArcGISTiledMapServiceLayer, FeatureLayer,
          InfoTemplate, ClassBreaksRenderer, SimpleFillSymbol, Legend, Color
        ) {
            map = new Map("map");

            var baseMapUrl = "http://services.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer";
            var baseMap = new ArcGISTiledMapServiceLayer(baseMapUrl);
            map.addLayer(baseMap);

            var layerUrl = "http://services.arcgis.com/BG6nSlhZSAWtExvp/ArcGIS/rest/services/Demographics_World_Simp/FeatureServer/0";
            var layer = new FeatureLayer(layerUrl, {
                infoTemplate: new InfoTemplate("${CNTRY_NAME}", "${*}"),
                mode: FeatureLayer.MODE_ONDEMAND,
                outFields: ["*"]
            });

            var symbol = new SimpleFillSymbol();
            symbol.setColor(new Color([150, 150, 150, 0.5]));

            var renderer = new ClassBreaksRenderer(symbol, "POP2007");
            renderer.addBreak(0, 10000000, new SimpleFillSymbol().setColor(new Color([56, 168, 0, 0.5])));
            renderer.addBreak(10000000, 50000000, new SimpleFillSymbol().setColor(new Color([139, 209, 0, 0.5])));
            renderer.addBreak(50000000, 100000000, new SimpleFillSymbol().setColor(new Color([255, 255, 0, 0.5])));
            renderer.addBreak(100000000, 500000000, new SimpleFillSymbol().setColor(new Color([255, 128, 0, 0.5])));
            renderer.addBreak(500000000, Infinity, new SimpleFillSymbol().setColor(new Color([255, 0, 0, 0.5])));

            layer.setRenderer(renderer);
            map.addLayer(layer);

            layer.on("load", function () {
                
                var legend = new Legend({
                    map: map,
                    layerInfos: [{
                        layer: layer,
                        title: "各国人口"
                    }]
                }, "legend");
                legend.startup();
            });
        });
    </script>
</head>
<body>
    <div id="map"></div>
    <div id="info">
      <div id="legend"></div>
    </div>
</body>
</html>  
```

**使用步骤：**

1. 创建要素图层
2. 创建对应渲染器，并设置渲染器的参数，主要用到了符号（例如 MarkerSymbol, LineSymbol, FillSymbol）
3. 为要素图层设置渲染器：`setRenderer(renderer)`
4. 将要素图层添加至map地图上：`map.addLayer(featurelayer)`



## 第八章 空间分析

####  **图形查询属性** 

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>属性查询图形</title>
    <link rel="stylesheet" href="http://js.arcgis.com/3.9/js/dojo/dijit/themes/claro/claro.css">
    <link rel="stylesheet" href="http://js.arcgis.com/3.9/js/esri/css/esri.css">
    <link rel="stylesheet" href="Layout.css" />
    <script src="http://js.arcgis.com/3.9/"></script>
    <script>
        var map, identifyTask, identifyParams;
        var pointSym, lineSym, polygonSym;
        var layer2results, layer1results, layer0results;

        require(["dojo/parser", "dijit/registry", "esri/map", "esri/layers/ArcGISDynamicMapServiceLayer", "esri/toolbars/draw",
            "esri/symbols/SimpleMarkerSymbol", "esri/symbols/SimpleLineSymbol", "esri/symbols/SimpleFillSymbol", "esri/Color",
            "esri/tasks/IdentifyTask", "esri/tasks/IdentifyParameters", "esri/geometry/screenUtils",
            "dijit/form/Button", "dijit/layout/TabContainer", "dijit/layout/ContentPane", "dojo/domReady!"],
            function (parser, registry, Map, ArcGISDynamicMapServiceLayer, Draw,
                SimpleMarkerSymbol, SimpleLineSymbol, SimpleFillSymbol, Color,
                IdentifyTask, IdentifyParameters, screenUtils) {

                parser.parse();

                map = new Map("mapDiv");
                var url = "http://sampleserver1.arcgisonline.com/ArcGIS/rest/services/Specialty/ESRI_StatesCitiesRivers_USA/MapServer";
                var agoLayer = new ArcGISDynamicMapServiceLayer(url);
                map.addLayer(agoLayer);
                map.on("load", initIdentify);

                var redColor = new Color([255, 0, 0]);
                var halfFillYellow = new Color([255, 255, 0, 0.5]);
                pointSym = new SimpleMarkerSymbol(SimpleMarkerSymbol.STYLE_DIAMOND, 10,
                    new SimpleLineSymbol(SimpleLineSymbol.STYLE_SOLID, redColor, 1),
                    halfFillYellow);
                lineSym = new SimpleLineSymbol(SimpleLineSymbol.STYLE_DASHDOT, redColor, 2);
                polygonSym = new SimpleFillSymbol(SimpleFillSymbol.STYLE_SOLID,
                    new SimpleLineSymbol(SimpleLineSymbol.STYLE_SOLID, redColor, 2),
                    halfFillYellow);

                map.infoWindow.on("show", function () {
                    registry.byId("tabs").resize();
                });

                var tb = new Draw(map);
                tb.on("draw-end", doIdentify);

                registry.forEach(function (d) {
                    if (d.declaredClass === "dijit.form.Button") {
                        d.on("click", activateTool);
                    }
                });

                function activateTool() {
                    var tool = null;
                    switch (this.label) {
                        case "点":
                            tool = "POINT";
                            break;
                        case "线":
                            tool = "POLYLINE";
                            break;
                        case "多边形":
                            tool = "POLYGON";
                            break;
                    }
                    tb.activate(Draw[tool]);
                    map.hideZoomSlider();
                }

                function initIdentify(evt) {
                    // 实例化IdentifyTask
                    identifyTask = new IdentifyTask(url);
                    // IdentifyTask参数设置
                    identifyParams = new IdentifyParameters();
                    // 冗余范围
                    identifyParams.tolerance = 3;
                    // 返回地理元素
                    identifyParams.returnGeometry = true;
                    // 进行Identify的图层
                    identifyParams.layerIds = [2, 1, 0];
                    // 进行Identify的图层为全部
                    identifyParams.layerOption = IdentifyParameters.LAYER_OPTION_ALL;

                    // 设置infoWindow的大小
                    evt.map.infoWindow.resize(415, 200);
                    // 设置infoWindow的标题头
                    evt.map.infoWindow.setTitle("Identify结果");
                    evt.map.infoWindow.setContent(registry.byId("tabs").domNode);
                }

                // 进行Identify
                function doIdentify(evt) {
                    // 清除上一次的高亮显示
                    map.graphics.clear();
                    // Identify的geometry
                    identifyParams.geometry = evt.geometry;
                    // Identify范围
                    identifyParams.mapExtent = map.extent;
                    identifyTask.execute(identifyParams, function (idResults) {
                        addToMap(idResults, evt.geometry);
                    });
                }

                // 在infoWindow中显示Identify结果
                function addToMap(idResults, geometry) {
                    layer2results = { displayFieldName: null, features: [] };
                    layer1results = { displayFieldName: null, features: [] };
                    layer0results = { displayFieldName: null, features: [] };
                    for (var i = 0, il = idResults.length; i < il; i++) {
                        var idResult = idResults[i];
                        if (idResult.layerId === 2) {
                            if (!layer2results.displayFieldName) {
                                layer2results.displayFieldName = idResult.displayFieldName;
                            }
                            layer2results.features.push(idResult.feature);
                        } else if (idResult.layerId === 1) {
                            if (!layer1results.displayFieldName) {
                                layer1results.displayFieldName = idResult.displayFieldName;
                            }
                            layer1results.features.push(idResult.feature);
                        } else if (idResult.layerId === 0) {
                            if (!layer0results.displayFieldName) {
                                layer0results.displayFieldName = idResult.displayFieldName;
                            }
                            layer0results.features.push(idResult.feature);
                        }
                    }
                    registry.byId("layer2Tab").setContent(layerTabContent(layer2results, "layer2results"));
                    registry.byId("layer1Tab").setContent(layerTabContent(layer1results, "layer1results"));
                    registry.byId("layer0Tab").setContent(layerTabContent(layer0results, "layer0results"));

                    // 设置infoWindow显示
                    var firstPt;
                    if (geometry.type == "point")
                        firstPt = geometry;
                    else
                        firstPt = geometry.getPoint(0, 0);
                    var screenPoint = screenUtils.toScreenPoint(map.extent, map.width, map.height, firstPt);
                    map.infoWindow.show(screenPoint, map.getInfoWindowAnchor(screenPoint));
                }

                function layerTabContent(layerResults, layerName) {
                    var content = "<i>选中要素数目为：" + layerResults.features.length + "</i>";
                    switch (layerName) {
                        case "layer2results":
                            content += "<table border='1'><tr><th>ID</th><th>州名</th><th>面积</th></tr>";
                            for (var i = 0, il = layerResults.features.length; i < il; i++) {
                                content += "<tr><td>" + layerResults.features[i].attributes['FID'] + " <a href='#' onclick='showFeature(" + layerName + ".features[" + i + "]); return false;'>(显示)</a></td>";
                                content += "<td>" + layerResults.features[i].attributes['STATE_NAME'] + "</td>";
                                content += "<td>" + layerResults.features[i].attributes['AREA'] + "</td>";
                            }
                            content += "</tr></table>";
                            break;
                        case "layer1results":
                            content += "<table border='1'><tr><th>ID</th><th>名称</th></tr>";
                            for (var i = 0, il = layerResults.features.length; i < il; i++) {
                                content += "<tr><td>" + layerResults.features[i].attributes['FID'] + " <a href='#' onclick='showFeature(" + layerName + ".features[" + i + "]); return false;'>(显示)</a></td>";
                                content += "<td>" + layerResults.features[i].attributes['NAME'] + "</td>";
                            }
                            content += "</tr></table>";
                            break;
                        case "layer0results":
                            content += "<table border='1'><tr><th>ID</th><th>名称</th><th>州名</th><th>人口</th></tr>";
                            for (var i = 0, il = layerResults.features.length; i < il; i++) {
                                content += "<tr><td>" + layerResults.features[i].attributes['FID'] + " <a href='#' onclick='showFeature(" + layerName + ".features[" + i + "]); return false;'>(显示)</a></td>";
                                content += "<td>" + layerResults.features[i].attributes['CITY_NAME'] + "</td>";
                                content += "<td>" + layerResults.features[i].attributes['STATE_NAME'] + "</td>";
                                content += "<td>" + layerResults.features[i].attributes['POP1990'] + "</td>";
                            }
                            content += "</tr></table>";
                            break;
                    }
                    return content;
                }
            });

        // 高亮显示选中元素
        function showFeature(feature) {
            map.graphics.clear();
            var symbol;
            // 将几何对象加入到地图中
            switch (feature.geometry.type) {
                case "point":
                    symbol = pointSym;
                    break;
                case "polyline":
                    symbol = lineSym;
                    break;
                case "polygon":
                    symbol = polygonSym;
                    break;
            }

            feature.setSymbol(symbol);
            map.graphics.add(feature);
        }

    </script>
</head>

<body class="claro">
    <div id="mainWindow" data-dojo-type="dijit/layout/BorderContainer" data-dojo-props="design:'headline',gutters:false"
        style="width:100%; height:100%;">
        <!--标题区域-->
        <div id="header" data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'top'" style="height:50px;">
            属性查询图形
        </div>

        <!--中间地图区域-->
        <div id="mapDiv" data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'center'"
            style="margin:5px;">
        </div>

        <!--右边为属性查询与结果显示区域-->
        <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'right'"
            style="width:35%;margin:5px;background-color:whitesmoke;">
            输入查询文本
            <input id="searchText" value="KS" type="text" />
            <input id="findBtn" type="button" value="查 找" />
            <div id="contentsContainer"></div>
        </div>
    </div>
</body>

</html>
```

**查询类工具使用步骤：**

1. 创建相应的工具（需要对应服务的url）
2. 创建参数对象并填充参数(图形或者是文本参数)
3. 使用对应的函数执行查询并处理结果

#### **地理编码**

```javascript
require(["dojo/on", "esri/map", "esri/geometry/Extent", "esri/SpatialReference", "esri/layers/ArcGISTiledMapServiceLayer", "esri/InfoTemplate",
    "esri/graphic", "esri/geometry/Multipoint", "esri/symbols/SimpleMarkerSymbol", "esri/symbols/TextSymbol", "esri/Color", "esri/tasks/locator", "dojo/domReady!"],
    function (on, Map, Extent, SpatialReference, ArcGISTiledMapServiceLayer, InfoTemplate,
        Graphic, Multipoint, SimpleMarkerSymbol, TextSymbol, Color, Locator) {
        var extent = new Extent(-95.271, 38.933, -95.228, 38.976, new SpatialReference({ wkid: 4326 }))
        var map = new esri.Map("map", {
            extent: extent
        });

        var streetMap = new ArcGISTiledMapServiceLayer("http://server.arcgisonline.com/ArcGIS/rest/services/ESRI_StreetMap_World_2D/MapServer");
        map.addLayer(streetMap);

        var locator = new Locator("http://geocode.arcgis.com/arcgis/rest/services/World/GeocodeServer");
        //locator.on("address-to-locations-complete", showResults);

        on(document.getElementById("locateBtn"), "click", function () {
            locate();
        });

        function locate() {
            map.graphics.clear();
            var address = {
                SingleLine: document.getElementById("address").value
            };
            var options = {
                address: address,
                outFields: ["Loc_name"]
            };
            locator.addressToLocations(options, showResults);
        }

        function showResults(candidates) {
            var candidates;
            var symbol = new SimpleMarkerSymbol();
            var infoTemplate = new InfoTemplate("位置", "地址：${address}<br />得分：${score}<br />匹配源：${locatorName}");

            symbol.setStyle(SimpleMarkerSymbol.STYLE_DIAMOND);
            symbol.setColor(new Color([255, 0, 0, 0.75]));

            var points = new Multipoint(map.spatialReference);

            for (var i = 0, il = candidates.length; i < il; i++) {
                var candidate = candidates[i];
                if (candidate.score > 70) {
                    var attributes = { address: candidate.address, score: candidate.score, locatorName: candidate.attributes.Loc_name };
                    var graphic = new Graphic(candidate.location, symbol, attributes, infoTemplate);
                    map.graphics.add(graphic);
                    map.graphics.add(new Graphic(candidate.location, new TextSymbol(attributes.address).setOffset(0, 8)));
                    points.addPoint(candidate.location);
                }
            }
            map.setExtent(points.getExtent().expand(3));
        }
});
```

**关键步骤：**

- 获取查询所需要的参数（比如目标地址，或者目标位置）
- 使用参数进行查询



#### **视域分析**

```javascript
require(["dojo/parser", "esri/map", "esri/geometry/Extent", "esri/SpatialReference", "esri/layers/ArcGISTiledMapServiceLayer",
    "esri/graphic", "esri/symbols/SimpleMarkerSymbol", "esri/symbols/SimpleLineSymbol", "esri/symbols/SimpleFillSymbol", "esri/Color",
    "esri/tasks/Geoprocessor", "esri/tasks/LinearUnit", "esri/tasks/FeatureSet", "dojo/domReady!"],
    function (parser, Map, Extent, SpatialReference, ArcGISTiledMapServiceLayer,
        Graphic, SimpleMarkerSymbol, SimpleLineSymbol, SimpleFillSymbol, Color,
        Geoprocessor, LinearUnit, FeatureSet) {
        parser.parse();

        var extent = new Extent(-122.7268, 37.4557, -122.1775, 37.8649, new SpatialReference({ wkid: 4326 }))
        var map = new esri.Map("map", {
            extent: extent
        });

        var streetMap = new ArcGISTiledMapServiceLayer("http://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer");
        map.addLayer(streetMap);

        var gp = new Geoprocessor("http://sampleserver1.arcgisonline.com/ArcGIS/rest/services/Elevation/ESRI_Elevation_World/GPServer/Viewshed");
        gp.setOutputSpatialReference({ wkid: 102100 });

        map.on("click", computeViewShed);

        function computeViewShed(evt) {
            map.graphics.clear();
            var pointSymbol = new SimpleMarkerSymbol();
            pointSymbol.setSize(15);
            pointSymbol.setOutline(new SimpleLineSymbol(SimpleLineSymbol.STYLE_SOLID, new Color([255, 0, 0]), 1));
            pointSymbol.setColor(new dojo.Color([0, 255, 0, 0.25]));

            var graphic = new Graphic(evt.mapPoint, pointSymbol);
            map.graphics.add(graphic);

            var features = [];
            features.push(graphic);
            var featureSet = new FeatureSet();
            featureSet.features = features;
            var vsDistance = new LinearUnit();
            vsDistance.distance = 15000;
            vsDistance.units = "esriMeters";
            var params = { "Input_Observation_Point": featureSet, "Viewshed_Distance": vsDistance };
            gp.execute(params, drawViewshed);
        }

        function drawViewshed(results, messages) {
            var polySymbol = new SimpleFillSymbol();
            polySymbol.setOutline(new SimpleLineSymbol(SimpleLineSymbol.STYLE_SOLID, new Color([0, 0, 0, 0.5]), 1));
            polySymbol.setColor(new Color([255, 127, 0, 0.7]));
            var features = results[0].value.features;
            for (var f = 0, fl = features.length; f < fl; f++) {
                var feature = features[f];
                feature.setSymbol(polySymbol);
                map.graphics.add(feature);
            }
        }
});
```

