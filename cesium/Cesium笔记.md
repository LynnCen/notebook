#                                            Cesium笔记

##### 官网地址：https://cesium.com/index.html

###      基础知识

##### 1.环境搭建

github： https://github.com/CesiumGS/cesium.git

下载运行 

推荐使用服务器 node.js

##### 2.官方demo API查看

官网示例在Apps\Sandcastle\gallery下

##### 3.cesium影像服务-在线服务

##### 4.在线服务的拓展

##### 5.地图发布

##### 6.图层功能

单张图片服务

##### 7.BaseLayerPicker使用

##### 8.cesium地形服务-在线服务

##### 9.cesium地形服务-本地地形数据处理

地形数据处理 地理空间数据云下载

##### 10.cesium地形服务-地形数据采样

##### 11.模型加载

##### 12.鼠标事件

##### 13.绘制对象entity

针对空间对象可视化cesium提供了两类接口：**entity**和**primitive**

entity是一个想对高级的接口 ；**primitive**则是相对复杂，包括geometry和apperance两部分

绘制点:

```javascript
viewer.entities.add({

positions:Cesium.Cartesian3.fromDegrees(-75.59777,40.03883),

point:{

pixelSize:10,

color:Cesium.Color.YELLOW

}

})
```

可以看出entity是通过viewer中的entities加载到场景中，enities是entity的集合对象。

position：点在场景中的位置（笛卡尔坐标系）

point：指明该entity对象为point类型，其中大小为10，颜色为黄色

相关还有其他的属性：id、name、availability、show、description、position、orientation、viewFrom、parent等其他表示entity类型。

可以在demo查看其他示例



**材质**

空间对象可视化，不仅需要知道对象的空间位置，还需要知道对象的显示样式。显示样式就是通过材质来控制，比如说颜色、透明度、纹理贴图、更高级的光照等等。我们常用到就是颜色和透明度。

```
ellipse:{

      semiMinorAxis:250000.0,

      semiMajorAxis:400000.0,

      material:Cesium.Color.RED.withAlpha(0.5),

    }
```

**填充和边框**

填充和边框共同组成了面状对象的样式，通过制定属性fill（默认为true）和outline（默认为false）来确定是否显示填充和边框，material对应填充样式，outlineColor和outlineWidth对应边框的颜色和宽度。

```javascript
 ellipse:{

     semiMinorAxis:300000.0,

     semiMajorAxis:300000.0,

     height:200000.0,

     fill:true,

     material:Cesium.Color.RED.withAlpha(0.5),

     outline:true, //必须设置height，否则ouline无法显示

     outlineColor:Cesium.Color.BLUE.withAlpha(0.5),

     outlineWidth:10.0//不能设置，固定为1

    }
```

**贴图**

通过设置material为图片url，可以将图片填充到对象中

```javascript
ellipse:{

        semiMinorAxis:250000.0,

        semiMajorAxis:400000.0,

        height:200000.0,

        fill:true,

        material:"./sampledata/images/globe.jpg",

        outline:true, //必须设置height，否则ouline无法显示

        outlineColor:Cesium.Color.BLUE.withAlpha(0.5),

        outlineWidth:10.0//windows系统下不能设置固定为1

      }
```

**垂直拉伸**

有时候我们需要将面在垂直方向进行拉伸形成体，通过extrudedHeight即可实现这种效果，形成的体积任然符合它拉伸面的地球曲率。

```javascript
extrudedHeight:400000.0,
```

**场景中entity管理**

viewer.entities属性实际上是一个EntityCollecton对象，是entity的一个集合，提供了add，remove，removeAll等接口来管理场景中的entity

**选择**

 

```
varhandler = new Cesium.ScreenSpaceEventHandler(viewer.scene.canvas);

    handler.setInputAction(function(movement) {

      varpick = viewer.scene.pick(movement.position);

      if(Cesium.defined(pick) && (pick.id.id === 'obj_id_110')) {

        ;

      }

    },Cesium.ScreenSpaceEventType.LEFT_CLICK);
```

##### 14 绘制对象-Primitive

Primitive方式绘制对象更接近webgl的底层，参考博客（https://blog.csdn.net/happyduoduo1/article/details/51868042）

**primitive由两部分组成**

（1）几何形状（**Geometry**）:定义了Primitive的结构，例如三角形，线条，点等。

（2）外观（**Appearance**）：定义Primitive的着色（Sharding），包括GLSL（OpenGL着色语言，OpenGL ShadingLanguage）顶点着色器和片段着色器（vertex and fragment shaders），以及渲染状态（render state）

主要支持一下几何图形

| 几何图形                      | 说明                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| BoxGeometry                   | 立方体                                                       |
| BoxOutlineGeometry            | 仅有轮廓的立方体                                             |
| CircleGeometry                | 圆形或者拉伸的圆形                                           |
| CircleOutlineGeometry         | 只有轮廓的圆形                                               |
| CorridorGeometry              | 走廊：沿着地表的多段线，且具有一定的宽度，可以拉伸到一定的高度 |
| CorridorOutlineGeometry       | 只有轮廓的走廊                                               |
| CylinderGeometry              | 圆柱、圆锥或者截断的圆锥                                     |
| CylinderOutlineGeometry       | 只有轮廓的圆柱、圆锥或者截断的圆锥                           |
| EllipseGeometry               | 椭圆或者拉伸的椭圆                                           |
| EllipseOutlineGeometry        | 只有轮廓的椭圆或者拉伸的椭圆                                 |
| EllipsoidGeometry             | 椭球体                                                       |
| EllipsoidOutlineGeometry      | 只有轮廓的椭球体                                             |
| RectangleGeometry             | 矩形或者拉伸的矩形                                           |
| RectangleOutlineGeometry      | 只有轮廓的矩形或者拉伸的矩形                                 |
| PolygonGeometry               | 多边形，可以具有空洞或者拉伸一定的高度                       |
| PolygonOutlineGeometry        | 只有轮廓的多边形                                             |
| PolylineGeometry              | 多段线，可以具有一定的宽度                                   |
| SimplePolylineGeometry        | 简单的多段线                                                 |
| PolylineVolumeGeometry        | 多段线柱体                                                   |
| PolylineVolumeOutlineGeometry | 只有轮廓的多段线柱体                                         |
| SphereGeometry                | 球体                                                         |
| SphereOutlineGeometry         | 只有轮廓的球体                                               |
| WallGeometry                  | 墙                                                           |
| WallOutlineGeometry           | 只有轮廓的墙                                                 |

**使用Geometry和Appearance 具有以下优势：**

（1）性能：绘制大量的Primitive时，可以将其合并为单个Geometry以减轻CPU负担，更好的使用CPU。合并Primitive由web worker线程执行，UI保持响应性

（2）灵活性：Geometry与Appearance解耦，两者可以分别进行修改

（3）低级别访问：易于编写GLSL顶点，片段着色器，使用自定义的渲染状态

**同时具有一下劣势：**

（1）需要编写更多的代码

（2）需要对图形编程有更多的理解，特别是OpenGL的知识

entity与primitive方式对比：

//entity方式

```javascript
viewer.entities.add({

rectangle:{

coordinates:Cesium.Rectangle.formDegrees(110.20,34.55,111.20,35.55),

marterial:new Cesium.StripeMaterialProperty({

evenColor:Cesium.Color.WHITE,

oddColor:Cesium.Color.BLUE,

repaet:5

})

}

})
```

 //primitive方式

```
    var instance = new Cesium.GeometryInstance({

      geometry: new Cesium.RectangleGeometry({

        rectangle: Cesium.Rectangle.fromDegrees(105.20, 30.55, 106.20, 31.55),

        vertexFormat:Cesium.EllipsoidSurfaceAppearance.VERTEXT_FORMAT

      })

    });

    viewer.scene.primitives.add(new Cesium.Primitive({

      geometryInstances: instance,

      appearance: new Cesium.EllipsoidSurfaceAppearance({

        material:Cesium.Material.fromType('Stripe')

      })

    }));
```

**Primitive的外观**：

Primitive由两个重要部分组成：几何图形实例（GeometryInstance）、外观（Appearances），一个Primitive只能有一个外观，而可以有多个实例。几何图形定义了结构，外观定义了每个像素被如何着色，外观可能使用材质（Material）。这些对象的关系如下图所示：

| 外观                       | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| MaterialAppearance         | 支持各种Geometry类型的外观，支持使用材质来定义着色           |
| EllipsoidSurfaceAppearance | MaterialAppearance的一个版本。假设几何图形与地表是平行的，并且依此来进行顶点属性（vertex attributes）的计算 |
| PerInstanceColorAppearance | 让每个实例使用自定义的颜色来着色                             |
| PolylineMaterialAppearance | 支持使用材质来着色多段线                                     |
| PolylineColorAppearance    | 使用每顶点或者每片段（per-vertex  or per-segment ）的颜色来着色多段线 |

外观定义了需要在GPU上执行的完整的GLSL顶点、片段着色器，通常不需要修改这一部分，除非需要定义自己的外观。

外观还定义了完整的render state，用于在绘制Primitive时控制GPU的状态，可以直接或者通过高层API来定义render state

```javascript
//下面的外观可用于定义一个Viewer不可进入的不透明盒子

    var appearance2 = new Cesium.PerInstanceColorAppearance({

      translucent: false,

      closed: true

   });

    //下面的代码效果同上

    var appearance3 = new Cesium.PerInstanceColorAppearance({

      renderState: {

        depthTest: {

          enabled: true

       },

        cull: {

          enabled: true,

          face: Cesium.CullFace.BACK

        }

      }

    });
```

一旦外观被创建，其render state就不可再变，但是其材质是可以替换的。另外Primitive的外观也是不可修改的。

大部分外观具有flat、faceForward属性，可以间接的控制GLSL着色器：

（1）flat：扁平化着色，不考虑光线的作用

（2）faceForward：布尔值，控制光照效果

**Geometry与Appearance的兼容性**

需要注意，不是所有外观和所有几何图形可以搭配使用，例如EllipsoidSurfaceAppearance与WallGeometry就不能搭配，原因是后者是垂直于地表的。

即使外观与几何图形兼容，它们还必须有匹配的顶点格式（vertex formats）—— 即几何图形必须具有外观可以作为输入的数据格式，在创建Geometry时可以提供VertexFormat。

为了简便，可以让Geometry计算所有顶点属性（vertex attributes），以使之适用于任何外观，但这样做效率较差：

```
var geometry = newCesium.RectangleGeometry( {

  vertexFormat :Cesium.VertexFormat.ALL

} );
```

而如果我们使用外观EllipsoidSurfaceAppearance，其实只需要知道位置：

```
var geometry = newCeisum.RectangleGeometry( {

  vertexFormat :Ceisum.VertexFormat.POSITION_ONLY

} );
```

大部分外观具有vertexFormat属性或者VERTEX_FORMAT 静态常量，创建形状时只需要使用这些顶点格式即可：

```
var geometry = newCeisum.RectangleGeometry( {

  vertexFormat :Ceisum.EllipsoidSurfaceAppearance.VERTEX_FORMAT

} );

var geometry2 = newCeisum.RectangleGeometry( {

  vertexFormat :Ceisum.PerInstanceColorAppearance.VERTEX_FORMAT

} );

var appearance = newCeisum.MaterialAppearance();

var geometry3 = newCeisum.RectangleGeometry( {

  vertexFormat :appearance.vertexFormat

} );
```

此外，两个形状必须具有匹配的vertexFormat，才能被合并到一个Primitive中。

##### 15.Primitive和entity的效率对比

前面总结完了entity与primitive两个接口的用法：entity用法简单，primitive用法复杂。我们会有这样的疑问：entity已经封装的如此完美，调用如此便捷，为何还要primitive接口呢？区别就是加载效率。primitive更接近webgl底层，没有entity各种各样的附加属性，因此在加载时效率会更高。为了直观感受两者区别，我们分别用entity和primitive方式绘制3150个圆。

**1 entity方式**

```
for (var lon = -180.0; lon < 180.0;lon += 4.0) {

      for(var lat = -70.0; lat < 70.0; lat += 4.0) {

        viewer.entities.add({

          position:Cesium.Cartesian3.fromDegrees(lon, lat),

          ellipse:{

            semiMinorAxis:10000.0,

            semiMajorAxis:10000.0,

            //height:200000.0,

            material:Cesium.Color.GREEN

          }

        });

      }

    }
```

峰值工作设置cpu(内存):1,022,712K

内存：763.300K

**2 Primitive方式**

```
 varinstances = [];

    for(var lon = -180.0; lon < 180.0; lon += 4.0) {

      for(var lat = -70.0; lat < 70.0; lat += 4.0) {

        varellipse = new Cesium.EllipseGeometry({

          center:Cesium.Cartesian3.fromDegrees(lon, lat),

          semiMajorAxis:10000.0,

          semiMinorAxis:10000.0,

          vertexFormat:Cesium.VertexFormat.POSITION_ONLY

        });

        vargeometry = Cesium.EllipseGeometry.createGeometry(ellipse);

        varellipseInstance = new Cesium.GeometryInstance({

          geometry:geometry,

          attributes:{

            color:Cesium.ColorGeometryInstanceAttribute.fromColor(Cesium.Color.RED)

          }

        });

        instances.push(ellipseInstance);

      }

    }

 

    viewer.scene.primitives.add(newCesium.Primitive({

      geometryInstances:instances,

      appearance:new Cesium.PerInstanceColorAppearance()

    }));
```

峰值工作设置cpu(内存):509,452K

内存：321,880K



我们可以看到primitive内存消耗远低于entity，加载时间也是远低于entity。

当添加对象超过1w时entity方式直接导致浏览器崩溃。



##### 16.3DTiles加载

传统的倾斜摄影、点云、三维模型数据都可以通过工具转换成3d tiles格式。由于数据原因我测试了倾斜摄影和三维模型两类数据。

**1 倾斜摄影**

倾斜摄影数据可以在smart3d软件中直接导出成cesium支持的3d tiles数据，后缀是**.b3dm**。

测试代码：

```javascript
 //1加载倾斜摄影

    var tileset = viewer.scene.primitives.add(new Cesium.Cesium3DTileset({

     url: './data/3dtiles-JX/tileset.json' //  ./data/3dtiles-lab/tileset.json

    }));

 

    tileset.readyPromise.then(function () {

      var boundingSphere = tileset.boundingSphere;

      viewer.camera.viewBoundingSphere(boundingSphere, new Cesium.HeadingPitchRange(0.0, -0.5, boundingSphere.radius));

      viewer.camera.lookAtTransform(Cesium.Matrix4.IDENTITY);

    }).otherwise(function (error) {

      throw (error);

    });
```

**2.三维模型**

模型数据如何制作，我没有接触过，直接使用官网上的测试数据，数据百度网盘地址3d tiles 模型数据：

测试代码：

```javascript
 //2加载模型

    var tileset = viewer.scene.primitives.add(new Cesium.Cesium3DTileset({

      url: './data/NewYork/tileset.json' //  ./data/3dtiles-lab/tileset.json

    }));

 

    tileset.readyPromise.then(function () {

      var boundingSphere = tileset.boundingSphere;

      viewer.camera.viewBoundingSphere(boundingSphere, new Cesium.HeadingPitchRange(0.0, -0.5, boundingSphere.radius));

      viewer.camera.lookAtTransform(Cesium.Matrix4.IDENTITY);

    }).otherwise(function (error) {

      throw (error);

    });
```

我们可以看到，不管是何种类型的3dtiles数据，加载接口都是统一的Cesium3DTileset，因为3d tiles只是一种数据规范，不管何种数据源经过处理后，满足这种规范就可以加载。

