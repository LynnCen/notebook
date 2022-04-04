# Cesium问题

#### 开启深度检测后，路线会有高程遮挡，不开启，雷达扫描和测距的线段会下沉

viewer.scene.globe.depthTestAgainstTerrain = true; 

**路程会出现抗锯齿的情况**

![](C:\Users\岑岑\Desktop\笔记\img\高程遮挡.PNG)

viewer.scene.globe.depthTestAgainstTerrain = false; 

**雷达扫描和测距的线段会下沉**

![](C:\Users\岑岑\Desktop\笔记\img\扫描.PNG)

![](C:\Users\岑岑\Desktop\笔记\img\测距.PNG)

解决方案：

深度检测有必要开启的情况(默认不开启)

那么我们就要防止路线的遮挡，将每条线段的高度抬高。实现思路：

由于交通路线geojson格式通过cesium的GeoJsonDataSource这个类加载的，返回的是一个promise，就已经直接加载好路线，但是Geojson中的位置信息是经纬度，没有高度，promise成功返回的结果中有entities 里面包含了每一个entities，我们再点进去看每一个的信息，找到了*r.polyline.positions._value*，它是每一个entity的笛卡尔坐标系数组，通过fir循环拿到每一项，获取到椭球这个对象，使用椭球的*cartesianToCartographic*方法将笛卡尔转化成经纬度

*

```javascript
const* lineData = new Cesium.GeoJsonDataSource.load(lsuData)

  .then(*function* (lineData: any) {

   viewer.dataSources.add(lineData);

   *let* entities = lineData.entities.values;

   for (*let* o = 0; o < entities.length; o++) {

​    *let* r = entities[o];

​    *// 每一个entity的笛卡尔坐标系数组*

​    *// let cartesian3Arr = r.polyline.positions._value*

​    *// for (let i = 0; i < cartesian3Arr.length; i++) {*

​    *//  let ellipsoid = viewer.scene.globe.ellipsoid;*

​    *//  var cartographic = ellipsoid.cartesianToCartographic(cartesian3Arr[i]);*

​    *//  var lat = Cesium.Math.toDegrees(cartographic.latitude);*

​    *//  var lng = Cesium.Math.toDegrees(cartographic.longitude);*

​    *//  cartesian3Arr[i] = Cesium.Cartesian3.fromDegreesArrayHeights(lat, lng, 10)*

​    *// }*

​    r.nameID = o;  *//给每条线添加一个编号，方便之后对线修改样式*

​    r.polyline.width = 10; *//添加默认样式*

​    r.polyline.material = new Cesium.PolylineGlowMaterialProperty({

​     glowPower: 0.2, *//一个数字属性，指定发光强度，占总线宽的百分比。*

​     color: Cesium.Color.ORANGERED.withAlpha(.6)

​    })

   }
   return viewer.zoomTo(lineData)

  })
```

