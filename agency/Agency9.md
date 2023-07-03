# Agency9

[TOC]



#### 项目的搭建和引入


引入mapsweb.js 依赖包
<script type="text/javascript" language="javascript" src="mapsweb/mapsweb.nocache.js"></script>



`var maps ;`
**//共同写在onMapsReady()函数下面 页面加载的时候调用****映射就绪时自动调用-在init（）之后**
`function onMapsReady() {`
**//实例化maps对象 (需要一个容器用来存装载体,同时需要一个example-setting.json的文件)**

**example-setting.json文件用来存放 a9许可证和许可密钥**

   `maps = new Agency9.Maps("mapContainer", "example-settings.json");`
**//然后调用maps下面的onStarted方法下面的done方法 把下面写的init函数传递过去**
   `maps.onStarted().done(init);`

  `}`

**再来编写我们init函数**

`function init(){`

**在init中先添加我们所需要的地块terrain**

**实例化terrain对象 向A3XTerrainLayer方法中传递两个参数 一个是terrain 另外一个是地块的URL**

`var terrain = new Agency9.Layer.A3XTerrainLayer("terrain", "https://storage.agency9.io/devportal-data/danderyd/150615/terrain/a3x/");`

**再执行maps下面的addLayer方法 把刚刚实例化的terrain作为参数传递过去**

`maps.addLayer(terrain);` 

`}`

**至此我们的3D地图和地形已经创建完毕*

#### **常用名词**

layer（层）；featureLayer（特征层）；addlayer (添加层)；terrain(地块) ；addFeature（添加功能）；addVertex（添加顶点）；getVertices(获取顶点)；getGeoLocation（获取点击的地理位置）;

*

#### 常用的方法：

bindEvent（）：

将由此点生成的事件绑定到JavaScript函数回调。

支持的事件类型有：“click”、“mouseDown”、“mouseUp”、“mouseEnter”、“mouseLeave”、“mouseDrag”。

生成事件时，FeatureEvent会自动传递给回调函数。

getGeoLocation（）：

地理位置是用经度、纬度和海拔来描述的地理位置。



####        点point

**首先实例化一个特征层** 

`var featureLayer = new Agency9.Layer.FeatureLayer("featurelayer");`

**为我们的点添加一些样式style**  **向ProjectedFeatureStyle方法中传一个我们的样式对象**

`var style = new Agency9.Style.ProjectedFeatureStyle({"pointSize": 100,"pointColor":"#0000ff55"});`

**然后把我们的样式传给我们的featureLayer特征层**

`featureLayer.setStyle(style);`

**再把特征层渲染到我们的maps下面 需要调用maps的addlayer方法**

`maps.addLayer(featureLayer)`

**我们的基本工作已经做完，再来创建我们的point(点)对象**

`var point = new Agency9.Feature.Point(152465, 6586459);`

**把我们的点point传递给我们的featureLayer特征层**，**需要调用featureLayer下的addFeature方法**

`featureLayer.addFeature(point)`



#### 自定义点

**我们可以对点的样式以及位置的修改**，**按照a9官网的教程，需要添加jq插件的GUI组件 ，我们这里只单单实现的鼠标可以自由点，**

***将鼠标单击事件绑定到地形。***

`terrain.bindEvent()`

**并将a9的鼠标单击事件传给回调函数**

`terrain.bindEvent('terrain',function(a9MouseEvent){`

***我们使用单击地形的坐标创建一个新点。*****getGeoLocation()方法可以获取当前的三维坐标系**

 `var point = new Agency9.Feature.Point(a9MouseEvent.getGeoLocation());`

`})`

**显式设置新点的样式  新点上的“有效”样式是默认样式之间的合并，后者被layerStyle覆盖**

`var pointStyle = new Agency9.Style.ProjectedFeatureStyle();`

​       `zIndex++;`

​       `pointStyle.setzIndex(zIndex);`

​       `pointStyle.setPointSize('60');`

​       `pointStyle.setOpacity('0.5');`

​       `pointStyle.setPointColor(new Agency9.Color('pink'))`

**设置好了的样式添加进点的样式里面**

`point.setStyle(pointStyle);`

**再添加进特征层**

`featureLayer.addFeature(point)`



#### 线line

**有两个点可以构成一条线** **（也可以是多个点）**

**首先定义一个状态量 startNewLine = true 用来判断是否开始新线段的开始**

##### **一 设置默认的线段点**

**先设置线的样式** 创建线的特征层

`var lineLayer = new Agency9.Layer.FeatureLayer("lineLayer");`

   `var lineLayerStyle = new Agency9.Style.ProjectedFeatureStyle({`

​    `"lineWidth": 25,`

​    `"lineColor": "#ffff00"`

   `})`

   `lineLayer.setStyle(lineLayerStyle);`

   `maps.addLayer(lineLayer);`

**再添加各个点，连接成线**

`var line;`

**实例化一个demoLine对象(a9.Feature.line()方法)**

   `var demoLine = new Agency9.Feature.Line();`

**在向其中添加addVertex顶点**

   `demoLine.addVertex(152390.85018333726, 6586561.7661167765);`

   `demoLine.addVertex(152233.97006867296, 6586757.4447951745);`

   `demoLine.addVertex(152182.01034470944, 6586833.752078245);`

   `demoLine.addVertex(152142.9956241825, 6586921.835794493);`

   `demoLine.addVertex(152124.43895469635, 6587022.128776855);`

   `lineLayer.addFeature(demoLine);`



##### **二 自定义线段**

**添加地形的点击事件**

`terrain.bindEvent("click",function(){`

***获取当前的坐标***

var clickedGeo = a9MouseEvent.getGeoLocation();

**先对状态进行判断，如果是true就把点连成线并把状态改成false**

`var lineStyle = new Agency9.Style.ProjectedFeatureStyle();`

​     `zIndex++;`

​     `lineStyle.setzIndex(zIndex);`

​     `lineStyle.setLineColor(new Agency9.Color('#fff'));`

​     `lineStyle.setLineWidth('10');`

​     `line = new Agency9.Feature.Line();`

​     `line.setStyle(lineStyle);`

​     `line.addVertex(clickedGeo);`

​     `lineLayer.addFeature(line);`

​     `startNewLine = false`

`})``else{`

`line.addVertex(clickedGeo);`

`}`



​    `if (line.getVertices().length < 2) {`

​     `var point = new Agency9.Feature.Point(clickedGeo);`

​     `pointLayer.addFeature(point);`

​    `} else {`

​     `pointLayer.clearFeatures(); *// Empty the layer*`

​    `}`



#### Polygon（多边形）

##### **一 设置默认多边形层**

**首先实例化多边形层**

`var polygonLayer = new Agency9.Layer.FeatureLayer("polygonlayer");`

**设置polygonLayerStyle多边形的样式**

`var polygonLayerStyle = new Agency9.Style.ProjectedFeatureStyle({`

​    `"polygonColor": "pink"`

   `});`

**渲染进相应的图层**

`polygonLayer.setStyle(polygonLayerStyle);`

   `maps.addLayer(polygonLayer);`

**绘制图多边形图层并渲染进图层**

`var demoPolygon = new Agency9.Feature.Polygon();`

   `demoPolygon.addVertex(152385, 6586404);`

   `demoPolygon.addVertex(152685, 6586404);`

   `demoPolygon.addVertex(152385, 6586704);`

   `polygonLayer.addFeature(demoPolygon); *// Don't forget to add it to the layer*`

##### 二 自定义多边形图层

`var polygon`

**为地形图层添加点击事件**

 `terrain.bindEvent("click", function (a9MouseEvent) {`

**点击获取当前的地理位置**

​    `var clickedGeo = a9MouseEvent.getGeoLocation();`

**对状态进行判断**

​    `if (startNewPolygon) {`

**设置配置的多边形图层的样式**

​     `var polygonStyle = new Agency9.Style.ProjectedFeatureStyle();`

​     `var color = new Agency9.Color('#00d2ff');`

​     `polygonStyle.setPolygonColor(color);`

​     `zIndex++;`

​     `polygonStyle.setzIndex(zIndex); **//确保新多边形投影在旧多边形的顶部**

​     `polygon = new Agency9.Feature.Polygon();`

​     `polygon.setStyle(polygonStyle);`

​     `polygon.addVertex(clickedGeo);`**//将第一个顶点添加到多边形**

​     `polygonLayer.addFeature(polygon);`**//多边形只有在有3个或更多顶点时才可见**

​     `startNewPolygon = false`

​    `} else {`

​     `polygon.addVertex(clickedGeo);`**//向多边形添加下一个顶点（我们现在有两个或多个顶点）**

​    `}`

**//当多边形的顶点少于3个时，显示单击指示点。**

**//隐藏当多边形变得可见时。**

​    `if (polygon.getVertices().length < 3) {`

​     `var point = new Agency9.Feature.Point(clickedGeo);`

​     `pointLayer.addFeature(point);`

​    `} else {`

​     `pointLayer.clearFeatures();`**//清空图层**

​    `}`

   `})`

#### PointStyle和LineStyle

待定。。。。

#### 相机Camera

**场景模拟 Fly to terrain overview（飞行至地形概述）Fly to clicked position（飞到单击的位置）Instant move to clicked position（即时移动到单击位置）**

**为这个三个功能分别添加单击响应事件** 

 `terrain.bindEvent('click', function (a9MouseEvent) {`

​    `if (point !== null) {`

​     `featureLayer.removeFeature(point); *// Remove the old point*`

​    `}`

​    `clickedGeo = a9MouseEvent.getGeoLocation();`

​    `point = new Agency9.Feature.Point(clickedGeo); *// Show point where we clicked*`

​    `featureLayer.addFeature(point);`

​    `clickedLong = clickedGeo.getLongitude();`

​    `clickedLat = clickedGeo.getLatitude();`

​    `clickedAlt = clickedGeo.getAltitude();`

   `});`

**//当摄像机停止移动时，运行camerastop函数**

**//所以每次都可以更新HTML覆盖**

`maps.getCamera().bindEvent('stop', cameraStopped);`

**//使用相机位置和注视位置更新HTML覆盖**

`function cameraStopped(event) {`

   `var pos = event.getCamera().getPosition();`

   `var lookAt = event.getCamera().getLookAt();`

  `}`

##### Fly to clicked position（飞到单击的位置）

**//动画移动到单击的地理位置-当用户单击HTML覆盖中的按钮时调用**

`function flyToClickedPos() {`

   `if (clickedGeo !== null) {`

​    `maps.getCamera().flyTo(new Agency9.GeoLocation(clickedLong, clickedLat - 150, clickedAlt + 150), clickedGeo);`

   `}`

  `}`

##### move to clicked position（即时移动到单击位置）

*//在给定层的字符串id的情况下，飞到层概述位置。*

*//计算层内部边界的中心位置，并设置*

*//照相机和相应的观察。还可以修改摄影机上的“maxZoom”*

*//这样你就不能缩小超过图层的最大边时间*

`function flyToLayerOverview(layerId) {`

   `var innerBounds = maps.getLayerById(layerId).getInnerBounds();`

   `var xSide = innerBounds[2] - innerBounds[0];`

   `var ySide = innerBounds[3] - innerBounds[1];`

   `var xCenter = innerBounds[0] + xSide * 0.5;`

   `var yCenter = innerBounds[1] + ySide * 0.5;`

   `var largestSide = xSide;`

   `if (ySide > largestSide) { largestSide = ySide }`

   `var SIDE_MULTIPLIER = 1.5;`

   `maps.getCamera().setMaxZoom(largestSide * SIDE_MULTIPLIER);`

   `var newCamPos = new Agency9.GeoLocation(xCenter, yCenter, largestSide * SIDE_MULTIPLIER);`

   `var newLookAt = new Agency9.GeoLocation(xCenter, yCenter + 0.00001, 0)`

   `maps.getCamera().flyTo(newCamPos, newLookAt);`

  `}`



##### Fly to terrain overview（飞行至地形概述）

`function flyToTerrainOverview() {`

   `flyToLayerOverview("terrain");`

  `}`

#### PlaceMarks and Balloons

##### **页面建构HTML部分**

设置三个容器div 分别用来存放需要展示的内容 具体的样式设定在style中

##### 创建一个point1点

**创建点之后 获取当前我们需要在气球上展示的内容的demo节点元素**

`var point1 = new Agency9.Feature.Point(153319.44397980915, 6586605.613156633, 4.3288750967057785);`

   `point1.setAltitudeMode(Agency9.Feature.ALTITUDE_MODE_ABSOLUTE);`

   `var text = document.getElementById('balloon-text-only')`

   `*// console.log(text.outerHTML)*`

   `balloonTextOnly = new Agency9.Balloon(text.outerHTML); *// 从这个文件底部的隐藏气球div读取HTML*`

   `balloonTextOnly.setVisible(false);*//Visibility默认为true，我们希望最初隐藏它*`

   `balloonTextOnly.setOffsetX(BALLOON_OFFSET_X);*//调整引出序号的X位置*`

   `balloonTextOnly.setOffsetY(BALLOON_OFFSET_Y);*//调整引出序号的Y位置*`

**然后再把点附着在我们的点上**

 `point1.setBalloon(balloonTextOnly)*//把气球系在点上*`

**添加鼠标移入移出事件**

*`//当鼠标在气球上方时，使其可见*`

   `point1.bindEvent('mouseenter', function () {`

​    `balloonTextOnly.setVisible(true)`

   `});`

   `*//当鼠标不在气球上方时将其隐藏*`

   `point1.bindEvent("mouseleave", function () {`

​    `balloonTextOnly.setVisible(false)`

   `});`

##### 创建point2

*`// 创建另一个点以附加另一个引出序号*`

   `var point2 = new Agency9.Feature.Point(152457.59797036526, 6586799.055207573, 66.43515716075204);`

   `point2.setAltitudeMode(Agency9.Feature.ALTITUDE_MODE_ABSOLUTE);`

   `var img = document.getElementById('balloon-popup-image')`

   `balloonPopupImage = new Agency9.Balloon(img.outerHTML);`

   `balloonPopupImage.setVisible(false);`

   `balloonPopupImage.setOffsetX(BALLOON_OFFSET_X);`

   `balloonPopupImage.setOffsetY(BALLOON_OFFSET_Y);`

**并把气球添加在点上**

   `point2.setBalloon(balloonPopupImage);`

**给我们的point2添加点击事件**

`point2.bindEvent("click", function () {`

​    `var isVisible = balloonPopupImage.isVisible();`

​    `balloonPopupImage.setVisible(!isVisible);`

   `});`

##### 创建point3

同以上的创建我们的point3点 

`var point3 = new Agency9.Feature.Point(152398.1653185724, 6586289.275179138, 14.989928342774109);`

   `point3.setAltitudeMode(Agency9.Feature.ALTITUDE_MODE_ABSOLUTE);`

   `var video = document.getElementById('balloon-popup-video')`

   `balloonPopupVideo = new Agency9.Balloon(video.outerHTML);`

   `balloonPopupVideo.setVisible(false);`

   `balloonPopupVideo.setOffsetX(BALLOON_OFFSET_X);`

   `balloonPopupVideo.setOffsetY(BALLOON_OFFSET_Y);`

   `point3.setBalloon(balloonPopupVideo);`

**point3的点击事件**

 `point3.bindEvent("click", function () {`

​    `var isVisible = balloonPopupVideo.isVisible();`

​    `balloonPopupVideo.setVisible(!isVisible);`

   `});`

##### 创建FeatureLayer并添加点（带有附加的引出序号）

`var featureLayer = new Agency9.Layer.FeatureLayer("featurelayer");`

   `featureLayer.addFeature(point1)`

   `featureLayer.addFeature(point2)`

   `featureLayer.addFeature(point3)`

**特征创建样式图层-点成为位置标记**

`var placemarkStyle = new Agency9.Style.PlacemarkStyle({`

​    `"url": './img/user_location.png',`

​    `"width": 20,`

​    `"height": 20,`

​    `"pivotY": 0,*//旋转：0-围绕图像底部中心，0.5-围绕图像中心，1-围绕图像顶部中心*`

​    `"autoScale": true *//如果设置为true，请在缩小时放大位置标记，以便更容易发现*`

   `})`

   `featureLayer.setStyle(placemarkStyle);`

`maps.addLayer(featureLayer);`



#### 自定义标签气球

 `var maps;`

  `var balloonTextonly;`

  `var BALLOON_OFFSET_X = 0;*// 将值增大到可向右移动引出序号*`

  `var BALLOON_OFFSET_Y = 20;*// 增加值可向上移动引出序号*`

  `var zIndex = 0;`

**准备onMapsReady函数**

  `function onMapsReady() {`

   `maps = new Agency9.Maps('mapContainer', 'example-settings.json')`

   `maps.onStarted().done(init);`



  `}`

**初始化init函数**

  `function init() {`

**添加地形**

   `var terrain = new Agency9.Layer.A3XTerrainLayer('terrain', "https://storage.agency9.io/devportal-data/danderyd/150615/terrain/a3x/");`

   `maps.addLayer(terrain);`

**在init函数作用域中创建featurelayer对象以及placemarkStyle 配置样式**

   `var featureLayer = new Agency9.Layer.FeatureLayer("featurelayer");`

   `var placemarkStyle = new Agency9.Style.PlacemarkStyle({`

​    `"url": './img/user_location.png',`

​    `"width": 20,`

​    `"height": 20,`

​    `"pivotY": 0,*//旋转：0-围绕图像底部中心，0.5-围绕图像中心，1-围绕图像顶部中心*`

​    `"autoScale": true *//如果设置为true，请在缩小时放大位置标记，以便更容易发现*`

   `})`

**为地形添加绑定事件，我们可以点一个点就添加一个标签**


   `terrain.bindEvent('click', function (a9MouseEvent) {`

​    `var point = new Agency9.Feature.Point(a9MouseEvent.getGeoLocation());`

​    `*// var pointStyle = new Agency9.Style.ProjectedFeatureStyle();*`

​    `*// zIndex++;*`

​    `*// pointStyle.setzIndex(zIndex);*`

​    `point.setAltitudeMode(Agency9.Feature.ALTITUDE_MODE_ABSOLUTE);`

​    `var text = document.getElementById('balloon-text-only')`

​    `balloonTextonly = new Agency9.Balloon(text.outerHTML);`

​    `balloonTextonly.setVisible(false);`

​    `balloonTextonly.setOffsetX(BALLOON_OFFSET_X);`

​    `balloonTextonly.setOffsetY(BALLOON_OFFSET_Y);`

​    `point.setBalloon(balloonTextonly);`

​    `featureLayer.addFeature(point);`

​    `featureLayer.setStyle(placemarkStyle);`

​    `maps.addLayer(featureLayer);`

   `});`

#### model1

**场景：**在地形上放置一个模型，并且可以移动和拖拽

**同上以上步骤加载地图maps 和地形terrain**

**首先我们需要一个模型阅读器来阅读模型**

**ModelReader**是**A3X**格式的三维模型的读取器。

`var modelReader = new Agency9.Model.ModelReader();`

*使用**modelReader**从硬盘读取模型以获取*

**给定模型文件的URL，读取A3X格式的三维模型。成功读取模型后，模型将自动传递给done（）函数回调。**

`modelReader.read("3d-models/dino.a3x").done(function (model) {`

​    `var point = model.createFeature();*////将模型转换为a9的特征点对象*`

​    `*//在这里，我们设置了高度模式，以便我们的模型被放置在地面上。*`

​    `*//默认模式为高度模式*`

 `point.setAltitudeMode(Agency9.Feature.ALTITUDE_MODE_RELATIVE_TO_GROUND);`

​       `point.setGeoLocation(new Agency9.GeoLocation(152442.60172571658, 6586390.42175299));`

   `*// 绑定事件，以便可以通过单击并按住拖动在地图上围绕三维模型移动。*`

​    `*//要想做到这一点，首先，将相机锁定在mousedown上。`*

​    `point.bindEvent('mouseDown', function (event) {`

​     `maps.getCamera().setLocked(true);`

​    `});`

`*`//当鼠标拖动的时候*``

​    `point.bindEvent("mouseDrag", function (event) {`

`*//获取到当前位置*`

​     `var geoLoc = event.getGeoLocation();`

`*//判断当前位置是否存在 获取到新的地理位置并付给点*`

​     `if (geoLoc !== null) {`

​      `var newLong = geoLoc.getLongitude();`

​      `var newLat = geoLoc.getLatitude();`

​      `point.setGeoLocation(new Agency9.GeoLocation(newLong, newLat, 0));`

​     `}`

​    `});`

`*//当鼠标松开的时候 取消锁定摄像机*`

​    `point.bindEvent('mouseUp', function (event) {`

​     `maps.getCamera().setLocked(false);`

​    `});`

`//别忘了添加层`

​    `modelLayer.addFeature(point);`

`失败的回调函数`

   `}).fail(function (error) {`

​    `var hasNext;`

​    `do {`

​     `console.log("errorCode: " + error.getErrorCode());`

​     `console.log("message: " + error.getMessage());`

​     `console.log("hasNext: " + error.hasNext());`

​     `console.log("toString: " + error.toString());`

​     `hasNext = error.hasNext();`

​     `error = error.next();`

​    `} while (hasNext)`

   `});`

`最后将modelLayer添加到maps上`

`maps.addLayer(modelLayer);`



#### model2

**场景：**如何将三维模型的副本添加到地图中

同意上步骤创建maps 以及 terrain

**创建一个层用来放置模型**

`var modelLayer = new Agency9.Layer.FeatureLayer('modelLayer');`

`缩小模型的配置`

   `var modelScale = [`

​    `0.03,*//x-axis scale*`

​    `0.03,`

​    `0.03`

   `];`

`*// Constructs a ModelStyle using settings from the jso parameter.*`

 `var layerStyle = new Agency9.Style.ModelStyle({ "scale": modelScale });`

`我们需要一个模型阅读器来阅读模型`

   `var modelReader = new Agency9.Model.ModelReader();`

`读取模型`

 `modelReader.read('"3d-models/duck.a3x').done(function (a9Model) {`

`//Sets a Model to this ModelStyle.`

​    `layerStyle.setModel(a9Model);`

   `}).fail(function (error) {`

​    `var hasNext;`

​    `do {`

​     `console.log('errorCode' + error.getErrorCode());`

​     `console.log('message' + error.getMessage());`

​     `console.log('hasNext' + error.hasNext());`

​     `console.log('toString' + error.toString());`

​     `hasNext = error.hasNext();`

​     `error = error.next();`

​    `} while (hasNext)`

   `})`

`添加完模型得layer Style需要传给我们放置模型的层上`

 `modelLayer.setStyle(layerStyle);`

`var startX = 152398.11637268058;``//开始鸭子模型的x坐标`

   `var startY = 6586240.36819196;``//开始鸭子模型的y坐标`

   `var modelCount = 8;` `//鸭子的总数`

   `var modelDistance = 50;``//相邻鸭子之间的间距`

`循环遍历出这些鸭子`

   `for (let i = 0; i < modelCount; i++) {`

​    `var point = new Agency9.Feature.Point(startX, startY - i * modelDistance);`

​    `point.setAltitudeMode(Agency9.Feature.ALTITUDE_MODE_RELATIVE_TO_GROUND);`

​    `modelLayer.addFeature(point);`

   `}`

   `maps.addLayer(modelLayer);`



#### model3

**场景：How to create a forest using only one 3D-model.**

同上 maps treeain

`*// 创建放置模型的层*`

   `var modelLayer = new Agency9.Layer.FeatureLayer("modelLayer");`

   `*//创建模型的样式*`

   `var layerStyle = new Agency9.Style.ModelStyle();`

   `*//模型的阅读器*`

   `var modelReader = new Agency9.Model.ModelReader();`

`//读取模型`

   `modelReader.read("3d-models/tree.a3x").done(function (a9Model) {`

​    `layerStyle.setModel(a9Model);`

   `}).fail(function (error) {`

​    `var hasNext;`

​    `do {`

​     `console.log("errorCode: " + error.getErrorCode());`

​     `console.log("message: " + error.getMessage());`

​     `console.log("hasNext: " + error.hasNext());`

​     `console.log("toString: " + error.toString());`

​     `hasNext = error.hasNext();`

​     `error = error.next();`

​    `} while (hasNext);`

   `});`

`modelLayer.setStyle(layerStyle);`

`//生成点并为每个点添加一个单独的模型样式。`

   `*//同时设置旋转和缩放到我们称为treeStyle的模型样式*`

   `*//所以每棵树都会有不同的比例和旋转角度。*`

   `*//每个点都将有一个“有效样式”，根据样式计算*`

   `*//设置为上面的图层，样式设置为点本身。*`

   `for (let i = 0; i < cols; i++) {`

​    `for (let j = 0; j < rows; j++) {`

​     `*//为了简单起见，在网格中生成点。*`

​     `*//当然，如果你愿意，你也可以随机化他们的位置。*`

​     `var point = new Agency9.Feature.Point(startX + i * treeDistance, startY + j * treeDistance);`

​     `*//在这里，我们设置了高度模式，以便我们的模型被放置在地面上。*`

​     `*//默认模式为高度模式*`

​     `point.setAltitudeMode(Agency9.Feature.ALTITUDE_MODE_RELATIVE_TO_GROUND);`

`设置树的样式`

​     `var treeStyle = new Agency9.Style.ModelStyle();`

`随机生成树旋转的角度deg`

​     `var treeRotation = *Math*.random() * 360;*//degrees*`

`随机生成树的宽度`

​     `var treeWidth = 5 + *Math*.random() * 8;`

`随机生成树的高度`

​     `var treeHeight = 10 + *Math*.random() * 30;`‘

`将样式添加到配置`

​     `treeStyle.setRotation(treeRotation);`

​     `treeStyle.setScale(treeWidth, treeWidth, treeHeight)`

​     `point.setStyle(treeStyle);`

​     `modelLayer.addFeature(point);`



​    `}`

   `}

**

**注意 一个layer只能读取一个model模型 如果你把一个style赋值给一个layer  同时没有set你的point 那么这个point 的style就会是这个layer的style**





#### shapefiles

**场景:此示例演示如何从形状文件加载形状，将形状转换为要素并在地图上显示它们。**

`*//创建一个特征层*`

   `var featureLayer = new Agency9.Layer.FeatureLayer('featureLayer');`

 `//我们在本例中选择ProjectedFeatureStyle ,`

   `*//因为我们知道我们的形状文件由多边形组成。这些将转换为Agency9特征多边形对象。*`

   `var layerStyle = new Agency9.Style.ProjectedFeatureStyle({`

​    `"polygonColor": 'ff00ff'`

   `});`

 `featureLayer.setStyle(layerStyle);`



`创建能够读取形状文件的ShapeReader`

   `var shapeReader = new Agency9.Shape.ShapeReader();`

`//读取文件`

`shapeReader.read("shapefiles/danderyd-buildings.shp").done(function (shape) {`

​    `var featureList = null;`

​    `featureList = shape.getFeatures(false);// false - don't read attributes from danderyd-buildings.dbf (we will in 13. ExtrudeStyle example)`

​    `*//把特征添加到特征层*`

​    `if (featureList !== null) {`

​     `featureLayer.addFeatures(featureList);`

​     `maps.addLayer(featureLayer)`  

​    `} else {`

​     `console.log('NO FEATURE IN FEATURELIST');`

​    `}`

​    `*//仅当发生错误时才会运行失败-例如形状文件的URL不正确*`

   `}).fail(function (error) {`

​    `var hasNext;`

​    `do {`

​     `console.log("errorCode: " + error.getErrorCode());`

​     `console.log("message: " + error.getMessage());`

​     `console.log("hasNext: " + error.hasNext());`

​     `console.log("toString: " + error.toString());`

​     `hasNext = error.hasNext();`

​     `error = error.next();`

​    `} while (hasNext)`

   `})`