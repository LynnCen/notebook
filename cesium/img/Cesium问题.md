# Cesium 问题

#### 开启深度检测后，路线会有高程遮挡，不开启，雷达扫描和测距的线段会下沉

viewer.scene.globe.depthTestAgainstTerrain = true;

**路程会出现抗锯齿的情况**

![](../img/%E9%AB%98%E7%A8%8B%E9%81%AE%E6%8C%A1.PNG)

viewer.scene.globe.depthTestAgainstTerrain = false;

**雷达扫描和测距的线段会下沉**

![](../img/%E6%B5%8B%E8%B7%9D.PNG)

![](../img/%E6%89%AB%E6%8F%8F.PNG)

解决方案：

深度检测有必要开启的情况(默认不开启)

那么我们就要防止路线的遮挡，将每条线段的高度抬高。实现思路：

由于交通路线 geojson 格式通过 cesium 的 GeoJsonDataSource 这个类加载的，返回的是一个 promise，就已经直接加载好路线，但是 Geojson 中的位置信息是经纬度，没有高度，promise 成功返回的结果中有 entities 里面包含了每一个 entities，我们再点进去看每一个的信息，找到了*r.polyline.positions.\_value*，它是每一个 entity 的笛卡尔坐标系数组，通过 fir 循环拿到每一项，获取到椭球这个对象，使用椭球的*cartesianToCartographic*方法将笛卡尔转化成经纬度

-

```javascript
const* lineData = new Cesium.GeoJsonDataSource.load(lsuData)

  .then(*function* (lineData: any) {

   viewer.dataSources.add(lineData);

   *let* entities = lineData.entities.values;

   for (*let* o = 0; o < entities.length; o++) {

  *let* r = entities[o];

    *// 每一个entity的笛卡尔坐标系数组*

    *// let cartesian3Arr = r.polyline.positions._value*

    *// for (let i = 0; i < cartesian3Arr.length; i++) {*

    *//  let ellipsoid = viewer.scene.globe.ellipsoid;*

    *//  var cartographic = ellipsoid.cartesianToCartographic(cartesian3Arr[i]);*

    *//  var lat = Cesium.Math.toDegrees(cartographic.latitude);*

    *//  var lng = Cesium.Math.toDegrees(cartographic.longitude);*

    *//  cartesian3Arr[i] = Cesium.Cartesian3.fromDegreesArrayHeights(lat, lng, 10)*

    *// }*

    r.nameID = o;  *//给每条线添加一个编号，方便之后对线修改样式*

    r.polyline.width = 10; *//添加默认样式*

    r.polyline.material = new Cesium.PolylineGlowMaterialProperty({

     glowPower: 0.2, *//一个数字属性，指定发光强度，占总线宽的百分比。*

     color: Cesium.Color.ORANGERED.withAlpha(.6)

    })

   }
   return viewer.zoomTo(lineData)

  })
```
